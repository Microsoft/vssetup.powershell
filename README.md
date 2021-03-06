Visual Studio Setup PowerShell Module
=====================================

![build status: master](https://devdiv.visualstudio.com/DevDiv/_apis/build/status/Setup/Setup-VSSetup.PowerShell-CI?branchName=master&label=master)
![build status: develop](https://devdiv.visualstudio.com/DevDiv/_apis/build/status/Setup/Setup-VSSetup.PowerShell-CI?branchName=develop&label=develop)
[![github release](https://img.shields.io/github/release/Microsoft/VSSetup.PowerShell.svg?logo=github&logoColor=white)](https://github.com/Microsoft/VSSetup.PowerShell/releases/latest)
[![github releases: all](https://img.shields.io/github/downloads/Microsoft/VSSetup.PowerShell/total.svg?logo=github&logoColor=white&label=github)](https://github.com/Microsoft/VSSetup.PowerShell/releases)
[![PowerShell Gallery](https://img.shields.io/powershellgallery/dt/VSSetup.svg?logo=powershell&logoColor=white&label=gallery)](https://powershellgallery.com/packages/VSSetup)

This PowerShell module contains cmdlets to query instances of Visual Studio 2017 and newer. It also serves as a more useful sample of using the Setup Configuration APIs than the previously [published samples][samples] though those also have samples using VB and VC++.

## Installing

With Windows Management Framework 5.0 or newer (which installs PowerShell and comes with Windows 10), or [PowerShellGet][psget] for PowerShell 3.0 or 4.0, you can download and install this module easily.

```powershell
Install-Module VSSetup -Scope CurrentUser
```

To install for all users, pass `AllUsers` instead of `CurrentUser`, or just leave the `-Scope` parameter out of the command entirely.

You can also download the ZIP package from the [Releases][releases] page on this project site and extract to a directory named _VSSetup_ under a directory in your `$env:PSMODULEPATH`.

```powershell
Expand-Archive VSSetup.zip "$([Environment]::GetFolderPath("MyDocuments"))\WindowsPowerShell\Modules\VSSetup"
```

## Using

You can query all usable instances of Visual Studio and other products installed by the Visual Studio installer.

```powershell
Get-VSSetupInstance
```

To get the instance for a specific installation directory, you can run the following.

```powershell
Get-VSSetupInstance 'C:\Program Files (x86)\Microsoft Visual Studio\2017\Community'
```

If you want to select the latest instance that contains the .NET desktop development workload, you can pipe all instances - usable or not - to `Select-VSSetupInstance` that provides more fine grain control over which instances you enumerate.

```powershell
Get-VSSetupInstance -All | Select-VSSetupInstance -Require 'Microsoft.VisualStudio.Workload.ManagedDesktop' -Latest
```

## Feedback

To file issues or suggestions, please use the [Issues][issues] page for this project on GitHub.

## License

This project is licensed under the [MIT license](LICENSE.txt).

## Code of Conduct

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

  [issues]: https://github.com/Microsoft/vssetup.powershell/issues
  [psget]: http://go.microsoft.com/fwlink/?LinkID=746217
  [releases]: https://github.com/Microsoft/vssetup.powershell/releases
  [samples]: https://aka.ms/setup/configuration/samples
