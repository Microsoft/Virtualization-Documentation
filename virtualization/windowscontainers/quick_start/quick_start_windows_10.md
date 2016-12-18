---
title: Windows Container on Windows 10
description: Container deployment quick start
keywords: docker, containers
author: enderb-ms
ms.date: 09/26/2016
ms.topic: article
ms.prod: windows-containers
ms.service: windows-containers
ms.assetid: bb9bfbe0-5bdc-4984-912f-9c93ea67105f
---

# Windows Containers on Windows 10

The exercise will walk through basic deployment and use of the Windows container feature on Windows 10 Professional or Enterprise (Anniversary Edition). After completion, you will have installed the container role, and deployed a simple Hyper-V container. Before starting this quick start, familiarize yourself with basic container concepts and terminology. This information can be found on the [Quick Start Introduction](./quick_start.md).

This quick start is specific to Hyper-V containers on Windows 10. Additional quick start documentation can be found in the table of contents on the left hand side of this page.

**Prerequisites:**

- One physical computer system running Windows 10 Anniversary Edition (Professional or Enterprise).   
- This quick start can be run on a Windows 10 virtual machine however nested virtualization will need to be enabled. More information can be found in the [Nested Virtualization Guide](https://msdn.microsoft.com/en-us/virtualization/hyperv_on_windows/user_guide/nesting).

> You must install critical updates for Windows Containers to work. 
> To check your OS version, run `winver.exe`, and compare the version shown to [Windows 10 update history](https://support.microsoft.com/en-us/help/12387/windows-10-update-history). 
> Make sure you have 14393.222 or later before continuing.

## 1. Install Container Feature

The container feature needs to be enabled before working with Windows containers. To do so run the following command in an **elevated** PowerShell session.

If you recieve an error saying `Enable-WindowsOptionalFeature` does not exist, double check that you are running PowerShell as Administrator.

```none
Enable-WindowsOptionalFeature -Online -FeatureName containers -All
```

Because Windows 10 only supports Hyper-V containers, the Hyper-V feature must also be enabled. To enable the Hyper-V feature using PowerShell, run the following command in an elevated PowerShell session.

```none
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```

When the installation has completed, reboot the computer.

```none
Restart-Computer -Force
```

> If you were previously using Hyper-V Containers on Windows 10 with the Technical Preview 5 container base images, be sure to re-enable OpLocks. Please run the following  command:  `Set-ItemProperty -Path 'HKLM:SOFTWARE\Microsoft\Windows NT\CurrentVersion\Virtualization\Containers' -Name VSmbDisableOplocks -Type DWord -Value 0 -Force`

## 2. Install Docker

Docker is required in order to work with Windows containers. Docker consists of the Docker Engine, and the Docker client. For this exercise, both will be installed. Run the following commands to do so.

Download the Docker engine and client as a zip archive.

```none
Invoke-WebRequest "https://test.docker.com/builds/Windows/x86_64/docker-1.13.0-rc4.zip" -OutFile "$env:TEMP\docker-1.13.0-rc4.zip" -UseBasicParsing
```

Expand the zip archive into Program Files, the archive contents is already in docker directory.

```none
Expand-Archive -Path "$env:TEMP\docker-1.13.0-rc4.zip" -DestinationPath $env:ProgramFiles
```

Add the Docker directory to the system path.

```none
# For quick use, does not require shell to be restarted.
$env:path += ";c:\program files\docker"

# For persistent use, will apply even after a reboot.
[Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\Program Files\Docker", [EnvironmentVariableTarget]::Machine)
```

To install Docker as a Windows service, run the following.

```none
dockerd --register-service
```

Once installed, the service can be started.

```none
Start-Service Docker
```

## 3. Install Base Container Images

Windows containers are deployed from templates or images. Before a container can be deployed, a container base OS image needs to be downloaded. The following commands will download the Nano Server base image.

Pull the Nano Server base image.

```none
docker pull microsoft/nanoserver
```

Once the image is pulled, running `docker images` will return a list of installed images, in this case the Nano Server image.

```none
docker images

REPOSITORY             TAG                 IMAGE ID            CREATED             SIZE
microsoft/nanoserver   latest              105d76d0f40e        4 days ago          652 MB
```

For in depth information on Windows container images see, [Managing Container Images](../management/manage_images.md).

> Please read the Windows Containers OS Image EULA which can be found here – [EULA](../Images_EULA.md).

## 4. Deploy Your First Container

For this simple example a ‘Hello World’ container image will be created and deployed. For the best experience run these commands in an elevated Windows CMD shell or PowerShell.

> Windows PowerShell ISE does not work for interactive sessions with containers. Even though the container is running, it will appear to hang.

First, start a container with an interactive session from the `nanoserver` image. Once the container has started, you will be presented with a command shell from within the container.  

```none
docker run -it microsoft/nanoserver cmd
```

Inside the container we will create a simple ‘Hello World’ script.

```none
powershell.exe Add-Content C:\helloworld.ps1 'Write-Host "Hello World"'
```   

When completed, exit the container.

```none
exit
```

You will now create a new container image from the modified container. To see a list of containers run the following and take note of the container id.

```none
docker ps -a
```

Run the following command to create the new ‘HelloWorld’ image. Replace <containerid> with the id of your container.

```none
docker commit <containerid> helloworld
```

When completed, you now have a custom image that contains the hello world script. This can be seen with the following command.

```none
docker images
```

Finally, to run the container, use the `docker run` command.

```none
docker run --rm helloworld powershell c:\helloworld.ps1
```

The outcome of the `docker run` command is that a Hyper-V container was created from the 'HelloWorld' image, a sample 'Hello World' script was then executed (output echoed to the shell), and then the container stopped and removed.
Subsequent Windows 10 and container quick starts will dig into creating and deploying applications in containers on Windows 10.

## Next Steps

[Windows Containers on Windows Server](./quick_start_windows_server.md)
