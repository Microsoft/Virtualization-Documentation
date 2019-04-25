---
title: Windows Containers FAQ
description: Windows Server containers FAQ
keywords: docker, containers
author: PatrickLang
ms.date: 05/02/2016
ms.topic: article
ms.prod: windows-containers
ms.service: windows-containers
ms.assetid: 25de368c-5a10-40a4-b4aa-ac8c9a9ca022
---
# Frequently asked questions

## General

### What is WCOW? What is LCOW?

WCOW is an abbreviation for Windows containers on Windows and LCOW is an abbreviation for Linux containers on Windows.

### What is the difference between Linux and Windows Server Containers?

Linux and Windows Server containers are similar in that they both implement similar technologies within their kernel and core operating system. The difference comes from the platform and workloads that run within the containers.  

When a customer uses Windows Server containers, they can integrate with existing Windows technologies such as .NET, ASP.NET, PowerShell, and more.

### As a developer, do I have to rewrite my app for each type of container?

No. Windows container images are common across both Windows Server containers and Hyper-V isolation. The choice of container type is made when you start the container. From a developer standpoint, Windows Server Containers and Hyper-V isolation are two flavors of the same thing. They offer the same development, programming and management experience, are open and extensible and include the same level of integration and support with Docker.

A developer can create a container image using a Windows Server container and deploy it in Hyper-V isolation or vice-versa without any changes other than specifying the appropriate runtime flag.

Windows Server containers offer greater density and performance for when speed is key, such as lower spin up time and faster runtime performance compared to nested configurations. Hyper-V isolation, true to its name,  offers greater isolation, ensuring that code running in one container can't compromise or impact the host operating system or other containers running on the same host. This is useful for multitenant scenarios with requirements for hosting untrusted code, including SaaS applications and compute hosting.

### What are the prerequisites for running containers on Windows?

Containers were introduced to the platform with Windows Server 2016. To use containers, you'll need either Windows Server 2016 or Windows 10 Anniversary update (version 1607) or newer.

### Can I run Windows containers in process-isolated mode on Windows 10 Enterprise or Professional?

Beginning with the Windows 10 October 2018 update, we no longer disallow a user from running a Windows container with process isolation. However, you must directly request for process isolation by using the `--isolation=process` flag when running your containers via `docker run`.

If this is something you're interested in, you need to make sure your host is running Windows 10 build 17763+ and you have a docker version with Engine 18.09 or newer.

> [!WARNING]
> This feature is only meant for development/testing. You should continue to use Windows Server as the host for production deployments.
>
> By using this feature, you must also ensure that your host and container version tags match, otherwise the container may fail to start or can exhibit undefined behavior.

## Windows container management

### How do I make my container images available on air-gapped machines?

The Windows container base images contain artifacts whose distribution is restricted by license. When you build on these images and push them to a private or public registry, you'll notice the base layer is never pushed. Instead, we use the concept of a foreign layer which points to the real base layer residing in Azure cloud storage.

This can present an issue when you have an air-gapped machine that can only pull images from the address of your private container registry. Attempts to follow the foreign layer to get the base image would fail in this case. To override the foreign layer behavior, you can use the `--allow-nondistributable-artifacts` flag in the Docker daemon.

> [!IMPORTANT]
> Usage of this flag shall not preclude your obligation to comply with the terms of the Windows container base image license; you must not post Windows content for public or third-party redistribution. Usage within your own environment is allowed.

## Microsoft's open ecosystem

### Is Microsoft participating in the Open Container Initiative (OCI)?

To guarantee the packaging format remains universal, Docker recently organized the Open Container Initiative (OCI), aiming to ensure container packaging remains an open and foundation-led format with Microsoft as one of the founding members.

> [!TIP]
> Have a recommendation for an addition to the FAQ? Open a new feedback issue in the comments section or use GitHub to open a pull request against these docs!
