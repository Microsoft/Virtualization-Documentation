---
title: Windows Container Requirements
description: Windows Container Requirements.
keywords: metadata, containers
author: taylorb-microsoft
ms.date: 09/26/2016
ms.topic: deployment-article
ms.prod: windows-containers
ms.assetid: 3c3d4c69-503d-40e8-973b-ecc4e1f523ed
---
# Windows container requirements

This guide lists the requirements for a Windows container Host.

## OS requirements

- The Windows container feature is only available on Windows Server 2016 (Core and with Desktop Experience), Windows 10 Professional and Enterprise (Anniversary Edition) and later.
- The Hyper-V role must be installed before running Hyper-V isolation
- Windows Server Container hosts must have Windows installed to c:\. This restriction does not apply if only Hyper-V isolated containers will be deployed.

## Virtualized container hosts

If a Windows container host will be run from a Hyper-V virtual machine, and will also be hosting Hyper-V isolation, nested virtualization will need to be enabled. Nested virtualization has the following requirements:

- At least 4 GB RAM available for the virtualized Hyper-V host.
- Windows Server 2019, Windows Server version 1803, Windows Server version 1709, Windows Server 2016, or Windows 10 on the host system, and Windows Server (Full, Core) in the virtual machine.
- A processor with Intel VT-x (this feature is currently only available for Intel processors).
- The container host VM will also need at least two virtual processors.

## Supported base images

Windows containers are offered with four container base images: Windows Server Core, Nano Server, Windows, and IoT Core. Not all configurations support both OS images. This table details the supported configurations.

<table border="1" style="background-color:FFFFCC;border-collapse:collapse;border:1px solid FFCC00;color:000000;width:75%" cellpadding="5" cellspacing="5">
<thead>
<tr valign="top">
<th><center>Host Operating System</center></th>
<th><center>Windows Server container</center></th>
<th><center>Hyper-V isolation</center></th>
</tr>
</thead>
<tbody>
<tr valign="top">
<td><center>Windows Server 2016 / 2019 (Standard or Datacenter)</center></td>
<td><center>Server Core, Nano Server, Windows</center></td>
<td><center>Server Core, Nano Server, Windows</center></td>
</tr>
<tr valign="top">
<td><center>Nano Server<a href="#warn-1">*</a></center></td>
<td><center> Nano Server</center></td>
<td><center>Server Core, Nano Server, Windows</center></td>
</tr>
<tr valign="top">
<td><center>Windows 10 Pro / Enterprise</center></td>
<td><center>Not Available</center></td>
<td><center>Server Core, Nano Server, Windows</center></td>
</tr>
<tr valign="top">
<td><center>IoT Core</center></td>
<td><center>IoT Core</center></td>
<td><center>Not Available</center></td>
</tr>
</tbody>
</table>

> [!WARNING]  
> Starting with Windows Server version 1709, Nano Server is no longer available as a container host.

### Memory requirements

Restrictions on available memory to containers can be configured though [resource controls](https://docs.microsoft.com/en-us/virtualization/windowscontainers/manage-containers/resource-controls) or by overloading a container host.  The minimum amount of memory required to launch a container and run basic commands (ipconfig, dir, and so on) are listed below.

>[!NOTE]
>These values don't take into account resource sharing between containers or requirements from the application running in the container.  For example a host with 512MB of free memory can run multiple Server Core containers under Hyper-V isolation because those containers share resources.

#### Windows Server 2016

| Base image  | Windows Server container | Hyper-V isolation    |
| ----------- | ------------------------ | -------------------- |
| Nano Server | 40 MB                     | 130 MB + 1 GB Pagefile |
| Server Core | 50 MB                     | 325 MB + 1 GB Pagefile |

#### Windows Server version 1709

| Base image  | Windows Server container | Hyper-V isolation    |
| ----------- | ------------------------ | -------------------- |
| Nano Server | 30 MB                     | 110 MB + 1 GB Pagefile |
| Server Core | 45 MB                     | 360 MB + 1 GB Pagefile |

### Base image differences

How does one choose the right base image to build upon? While you are free to build with whatever you wish, these are the general guidelines for each image:

- [Windows Server Core](https://hub.docker.com/_/microsoft-windows-servercore): If your application needs the full .NET framework, this is the best image to use.
- [Nano Server](https://hub.docker.com/_/microsoft-windows-nanoserver): For applications that only require .NET Core, Nano Server will provide a much slimmer image.
- [Windows](https://hub.docker.com/_/microsoft-windowsfamily-windows): You may find your application depends on a component or .dll that is missing in Server Core or Nano Server images, such as GDI libraries. This image carries the full dependency set of Windows.
- [IoT Core](https://hub.docker.com/_/microsoft-windows-iotcore): This image is purpose-built for [IoT applications](https://developer.microsoft.com/en-us/windows/iot). You should use this container image when targeting an IoT Core host.

For most users, Windows Server Core or Nano Server will be the most appropriate image to use. The following are things to keep in mind as you think about building on top of Nano Server:

- The servicing stack was removed
- .NET Core is not included (though you can use the [.NET Core Nano Server image](https://hub.docker.com/r/microsoft/dotnet/))
- PowerShell was removed
- WMI was removed
- Starting with Windows Server version 1709 applications run under a user context, so commands that require administrator privileges will fail. You can specify the container administrator account via the --user flag (such as docker run --user ContainerAdministrator) however in the future we intend to fully remove administrator accounts from NanoServer.

These are the biggest differences and not an exhaustive list. There are other components not called out which are absent as well. Keep in mind that you can always add layers on top of Nano Server as you see fit. For an example of this check out the [.NET Core Nano Server Dockerfile](https://github.com/dotnet/dotnet-docker/blob/master/2.1/sdk/nanoserver-1803/amd64/Dockerfile).