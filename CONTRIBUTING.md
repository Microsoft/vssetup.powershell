Contributing
============

## Prerequisites

This project uses the following software. Newer versions may work but backward compatibility must be maintained.

* [Visual Studio 2015](https://www.visualstudio.com/en-us/downloads/visual-studio-2015-downloads-vs.aspx)

### Optional

Some projects require optional software to open or otherwise use in Visual Studio. They are not required to build the solution using MSBuild.

* [NuProj Package Project](https://marketplace.visualstudio.com/items?itemName=NuProjTeam.NuGetPackageProject)

## Coding

This project uses a Git flow model releasing from the `master` branch with development based on and stabilize in the `develop` branch. You can view current build status in the [README](README.md) document.

These cmdlets use the setup configuration API. [Documentation][docs] is available on MSDN, as well as [samples][samples] of how the APIs can be used.

Code analysis and style cop rules are defined for this solution, but are currently not enforced during build for performance reasons or treated as errors. This may change in the future. Please resolve any build warnings in the code editor or **Error List** window.

If you add any commands please update the `Tags` property of the _VSSetup.nuproj_ project as appropriate. This project is used instead of `Publish-Module` from the _PowerShellGet_ module because it works better with the build systems and can be tested on developer machines without also publishing.

## Building

Before you can build this project from the command line with MSBuild or within Visual Studio, you must restore packages including the [embeddable interop types][interop].

* In Visual Studio, make sure Package Restore is enabled.
* On the command line and assuming _nuget.exe_ is in your `PATH`, in the solution directory run: `nuget restore`

Note again that to build the full solution in Visual Studio some optional software may be required.

### Updating Packages

Please note that the types in _Microsoft.VisualStudio.Setup.Configuration.Interop_ are embeddable and Visual Studio will use this setting by default for both the source and test projects. However, after updating the package reference in the test project you must disable "Embed Interop Types" and enable "Copy Local" for the _Microsoft.VisualStudio.Setup.Configuration.Interop_ assembly or any tests that mock the interfaces will fail.

This is because only types and methods referenced are included, and any methods prior to referenced methods in VTBL order are stubbed and will not match the proper method names, thus failing any tests with mocks of embedded interfaces. By referencing the assembly locally instead of embedding, all types are left intact and mocks will correctly match all names despite those types being embedded in the source project, since COM-imported type names are equivalent based on their GUIDs.

## Testing

All available tests are discovered after a complete build in Test Explorer within Visual Studio.

On the command line, you can run the following commands from the solution directory. Replace `<version>` with whatever version was downloaded.

```batch
nuget install xunit.runner.console -outputdirectory packages
packages\xunit.runner.console.<version>\tools\xunit.runner.console test\VSSetup.PowerShell.Test\bin\Debug\Microsoft.VisualStudio.Setup.PowerShell.Test.dll
```

If your machine supports it, you can install [Docker for Windows][docker], switch to Windows containers, and test in isolated containers for runtime behavior. You can run unit tests and integration tests together.

```batch
tools\test.cmd
```

For a faster development process, you can run `docker-compose run test` from the _docker_ directory to start an interactive PowerShell session in a container running the Visual Studio Remote Debugger. The build output and test scripts are mounted into the container. If you rebuild or modify the tests the container is automatically updated. You can also start the container detached and run tests faster without having to restart the container.

```batch
cd docker
docker-compose up -d

REM Repeat following command as often as desired.
docker-compose exec test powershell.exe -c invoke-pester c:\tests -enableexit

docker-compose stop
```

### Debugging

You can run `docker-compose up -d` from the _docker_ directory to start an interactive shell for exploratory testing. If no other commands are passed when starting the container, the Visual Studio Remote Debugger will launch by default. Remote debugging services are discoverable on your private network.

1. Click *Debug -> Attach to Process*
2. Change *Transport* to "Remote (no authentication)"
3. Click *Find*
4. Click *Select* on the container (host name will be the container name)
5. Select "powershell" under *Available Processes*
6. Click *Attach*

If you know the host name (by default, the container name) or IP address (depending on your network configuration for the container), you can type it into the *Qualifier* directory along with port 4022, e.g. "172.22.0.1:4022".

## Pull Requests

We welcome pull requests for both bug fixes and new features that solve a common enough problem to benefit the community. Please note the following requirements.

1. Code changes for bug fixes and new features are accompanied by new tests or, only if required, modifications to existing tests. Modifying existing tests when not required may introduce regressions.
2. All tests must pass. We have automated PR builds that will verify any PRs before they can be merged, but you are encouraged to run all tests in your development environment prior to pushing to your remote.

Thank you for your contributions!

  [docker]: https://www.docker.com/products/overview
  [samples]: https://aka.ms/setup/configuration/samples
  [docs]: https://aka.ms/setup/configuration/docs
  [interop]: https://aka.ms/setup/configuration/interop
