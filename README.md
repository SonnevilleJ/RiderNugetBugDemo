# RiderBugDemo
This repo can be used to reproduce bugs in [JetBrains Rider](https://www.jetbrains.com/rider).

## Steps to reproduce [RIDER-4152](https://youtrack.jetbrains.com/issue/RIDER-4152):
1. Clone this repo: https://github.com/SonnevilleJ/RiderNugetBugDemo
1. Open RiderNugetBugDemo.sln in Rider.
1. Ensure the NuGet panel is not visible.
1. In the Solution Explorer panel, right click the RiderNugetBugDemo project to reveal the context menu.
1. Click `Manage NuGet Packages`.

**Expected behavior**: The NuGet panel appears.<br>

**Observed behavior**: The NuGet panel does not appear.

## Steps to reproduce [RIDER-4151](https://youtrack.jetbrains.com/issue/RIDER-4151):
**Please note:** This assumes a TeamCity server is configured to host a NuGet package source with guest authentication at `http://localhost:9000/guestAuth/app/nuget/v1/FeedService.svc`. Please ensure TeamCity is configured before beginning, or use another valid TeamCity NuGet feed in the steps below.

1. Clone this repo: https://github.com/SonnevilleJ/RiderNugetBugDemo
1. Open RiderNugetBugDemo.sln in Rider.
1. Open the NuGet panel.
1. Click the Sources tab in the NuGet panel, then click the NuGet.Config file to open it in the editor. 
1. Add the following line in the `<packageSources>` block: `<add key="TeamCity" value="http://localhost:9000/guestAuth/app/nuget/v1/FeedService.svc/" />`
1. Save and close the NuGet.Config file.
1. Close the project and reopen it to force Rider to reload the NuGet.Config file.
1. Verify the new package source is listed on the Packages tab of the NuGet panel. Ensure the box is unchecked.
1. Select a NuGet package and try to install it into the RiderNugetBugDemo.

**Expected behavior**: The NuGet package is installed into the project using the https://www.nuget.org package source<br>

**Observed behavior**: The NuGet package fails to install. The error message mentions an error while retrieving package metadata from the TeamCity source added earlier.

## Steps to reproduce [RIDER-4254](https://youtrack.jetbrains.com/issue/RIDER-4254)
1. Clone this repo: https://github.com/SonnevilleJ/RiderNugetBugDemo
1. Open RiderNugetBugDemo.sln in Rider.
1. Open Class1.cs
1. Add an unused import to the class, for instance, `using System.Linq;`
1. Press Ctrl + K (Cmd + K on Mac) to open the commit window.
1. Ensure the checkbox to `Optimize Imports` is checked, then click the Commit button.
1. Observe the Class1.cs file and the commit diff in the VCS Log. The unused import is still present.

**Expected behavior**: The unused import is removed from the file and does not appear in the commit history.<br>

**Observed behavior**: The unused import still exists in the file and appears in the commit history.

## Steps to reproduce [RIDER-4466](https://youtrack.jetbrains.com/issue/RIDER-4466)
**Please note:** This defect has two parts. Complete both parts in order.

### Part 1
1. In the Solution Explorer panel, right-click the project and choose `New` > `Directory`. Enter a name with a hyphen, such as `DirectoryWithIllegalNamespaceChars-1234`.
1. Right click the new directory and choose `New` > `Class`. Enter a valid class name, such as `Example`.
1. Open the new class file in the editor.

**Expected behavior**: The namespace of the class should not contain the hyphen character, because hyphens are illegal characters in .NET namespaces. For instance, a directory named `DirectoryWithIllegalNamespaceChars-1234` should instead be named `DirectoryWithIllegalNamespaceChars1234`.

**Observed behavior**: The namespace of the class does in fact have the hyphen character. Rider correctly flags the error.

### Part 2
1. Use the same procedure in step 4 to create another new class. When naming this class, provide a name with a hyphen, such as `Example-2`.
1. Open the new class file in the editor.

**Expected behavior**: Neither the namespace of the class nor the class itself should contain the hyphen character. For instance, a class named `Example-2` should be named `Example2` in code.

**Observed behavior**: The namespace of the class does not have the hyphen character; however, the class name *does* have the illegal character. Rider correctly flags the error.

## Steps to reproduce [RIDER-4681](https://youtrack.jetbrains.com/issue/RIDER-4681)
**Please note:** This defect is similar or identical to [RIDER-4312](https://youtrack.jetbrains.com/issue/RIDER-4312)

1. Clone this repo: https://github.com/SonnevilleJ/RiderNugetBugDemo
1. Open RiderNugetBugDemo.sln in Rider.
1. Open Class2.cs
1. Observe Class2.cs contains both an interface `IClass2` and a class `Class2`.
1. Perform a "Move" refactoring on the `IClass2` interface: right-click the `IClass2` text and choose `Refactoring` > `Move...` or press F6 with the cursor on the `IClass2` name.
1. Choose `Move To Folder`
1. Enter the `Interfaces` folder (or any other existing folder) and press `Next`.

**Expected behavior**: The `IClass2` interface is moved to a new file `IClass2.cs` inside the `Interfaces` folder and `Class2` remains inside `Class2.cs` in the original location.

**Observed behavior**: Both the `IClass2` interface and the `Class2` class are moved to corresponding files inside the destination folder specified in the last step.

## Steps to reproduce [RIDER-4767](https://youtrack.jetbrains.com/issue/RIDER-4767)
1. Clone this repo: https://github.com/SonnevilleJ/RiderNugetBugDemo
1. Open RiderNugetBugDemo.sln in Rider.
1. Open Class1.cs
1. Ensure Chrome or another application has a window open in the background. Switch back to Rider.
1. In the body of `Class1` type `System.` and pause to allow the autocomplete dialog to appear.
1. While the autocomplete dialog is open, press Alt + Tab to switch to the other application.

**Expected behavior**: The autocomplete dialog closes, or is otherwise not visible when the other app has focus.

**Observed behavior**: The autocomplete dialog is visible above the other application.
