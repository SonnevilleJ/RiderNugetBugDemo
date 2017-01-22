# RiderNugetBugDemo
This repo can be used to reproduce NuGet-related bugs in [JetBrains Rider](https://www.jetbrains.com/rider) version #RS-163.12057

**Please note:** This demo assumes a TeamCity server is configured to host a NuGet package source with guest authentication at `http://localhost:9000/guestAuth/app/nuget/v1/FeedService.svc`. Please ensure TeamCity is configured before beginning, or use another valid TeamCity NuGet feed in the steps below.

## Steps to reproduce [RIDER-4152](https://youtrack.jetbrains.com/issue/RIDER-4152):
1. Clone this repo: https://github.com/SonnevilleJ/RiderNugetBugDemo
1. Open RiderNugetBugDemo.sln in Rider.
1. In the Solution Explorer panel, right click the RiderNugetBugDemo project to reveal the context menu.
1. Click `Manage NuGet Packages`.

* **Expected behavior**: The NuGet panel appears.
* **Observed behavior**: The NuGet panel does not appear.

## Steps to reproduce [RIDER-4151](https://youtrack.jetbrains.com/issue/RIDER-4151):
1. Clone this repo: https://github.com/SonnevilleJ/RiderNugetBugDemo
1. Open RiderNugetBugDemo.sln in Rider.
1. Open the NuGet panel.
1. Click the Sources tab in the NuGet panel, then click the NuGet.Config file to open it in the editor. 
1. Add the following line in the `<packageSources>` block: `<add key="TeamCity" value="http://localhost:9000/guestAuth/app/nuget/v1/FeedService.svc/" />`
1. Save and close the NuGet.Config file.
1. Verify the new package source is listed on the Packages tab of the NuGet panel. Ensure the box is unchecked.
1. Select a NuGet package and try to install it into the RiderNugetBugDemo.

* **Expected behavior**: The NuGet package is installed into the project using the https://www.nuget.org package source
* **Observed behavior**: The NuGet package fails to install. The error message mentions an error while retrieving package metadata from the TeamCity source added earlier.
