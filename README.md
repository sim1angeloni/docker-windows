# windows-dev

Dockerfiles to build development environments under Windows.

## Getting Started

There are multiple Dockerfiles in this repository, for different purposes:
* Windows 10 v1903 (win1903)
    * MS Visual Studio 2017 (msvc2017)
* generic-devenv

### msvc2017

This Dockerfile is used to build a docker image that contains Microsoft Visual Studio Build Tools for C++ development. Its entrypoint activates the development environment directly calling `vcvarall` in 64bits mode.

The details of the VS installation is controlled via `.vsconfig`. It installs:
* VC Tools required packages
* Universal Apps required packages
* Windows SDK 10.0.17763.0
* ATL and MFC for x64 and x86, classical and Spectre mitigated versions
* Universal CRT SDK
* [CMake](https://cmake.org)
* [Ninja](https://ninja-build.org)

A guide to Install BuildTools in Docker for VS 2017 is accessible [here](https://docs.microsoft.com/en-us/visualstudio/install/build-tools-container?view=vs-2017) and the command line options for the installer are [here](https://docs.microsoft.com/en-us/visualstudio/install/use-command-line-parameters-to-install-visual-studio?view=vs-2017).
The list of Visual Studio workloads and components is available [here](https://docs.microsoft.com/en-us/visualstudio/install/workload-component-id-vs-build-tools?view=vs-2017).

To use this Dockerfile runs the following:
```
git clone https://github.com/sim1angeloni/windows-dev.git
cd windows-dev\win1903\msvc2017
docker build . -t windows/1903/msvc:2017
```

### generic-devenv

This Dockerfile uses as a base one of the images built with Microsoft Visual Studio Build Tools and it installs various utilities on top:
* [Git](https://git-scm.com)
* [Perforce](https://www.perforce.com)
* [Conan](https://conan.io)

To use this Dockerfile runs the following:
```
git clone https://github.com/sim1angeloni/windows-dev.git
cd windows-dev\generic-devenv
docker build . -t windows/1903/devenv:latest
```

**IMPORTANT**: Change the `FROM` line in the Dockerfile to match your base image requirements.

## Notes

### .NET Framework images

The official Microsoft documentation to install Visual Studio under a Windows docker container explains to use Windows Server Core images with .NET Framework preinstalled.

However I noticed that these images come with preintalled parts of Visual Studio tools too, and when the `vs_BuildTools.exe` is run again, it finds the previous installation and fails to continue the job. Also, because VS is already installed, you can't control which version of the Build Tools executable you want to use.

I decided to use pure Windows 10 images instead.

### Windows container version compatibility

Because Windows Server containers and the underlying host share a single kernel, the container's base image OS version must match that of the host. If the versions are different, the container may start, but full functionally isn't guaranteed. If you want to run a container based on a newer Windows build, it can only run on the newer host build.

[Here](https://docs.microsoft.com/en-us/virtualization/windowscontainers/deploy-containers/version-compatibility) you can check if the Windows 10 or Windows Server Core version that you are choosing for the container is compatible with your host.

You can change the Windows image used in the Dockerfiles updating the `FROM` line. Find below the list of the official Microsoft images. I suggest using either Windows 10 or Windows Server Core.

### Microsoft images

* [Windows 10 images](https://hub.docker.com/_/microsoft-windows)
* [Windows Server Core images](https://hub.docker.com/_/microsoft-windows-servercore)
* [.NET Framework SDK images](https://hub.docker.com/_/microsoft-dotnet-framework-sdk)

### DNS

Make sure you set the DNS for your Docker installation, otherwise the containers will not be able to execute an `nslookup` and also, installing packages with `pip` will fail.

You can set the DNS editing the JSON file of the Docker daemon. If you are using Docker Desktop for Windows, open the Settings page and go to the Docker Engine tab. Add the following code:

```
"dns": [
  "8.8.8.8",
  "8.8.4.4"
]
```

Here I am using the Google DNS servers, but you can use your preferred DNS.

## Contributing

Please submit a pull request to contribute to this repository and add new versions.

## Authors

* **Simone Angeloni** - *Initial work* - [@sim1angeloni](https://github.com/sim1angeloni)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
