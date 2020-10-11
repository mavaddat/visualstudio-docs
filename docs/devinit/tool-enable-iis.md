---
title: enable-iis
description: devinit tool enable-iis.
ms.date: 08/28/2020
ms.topic: reference
author: andysterland
ms.author: andster
manager: jillfra
ms.workload:
- multiple
monikerRange: ">= vs-2019"
ms.prod: visual-studio-windows
ms.technology: devinit
---
# enable-iis

The `enable-iis` tool is used to enable IIS features and install the [ASP.NET Core Module](/aspnet/core/host-and-deploy/aspnet-core-module) for ASP.NET development with IIS.

## Usage

If both the `input` and `additionalOptions` properties are omitted or empty, then the tool will follow the [default](#default-behavior) behavior detailed below.

| Name                                             | Type   | Required | Value                                                                               |
|--------------------------------------------------|--------|----------|-------------------------------------------------------------------------------------|
| **comments**                                     | string | No       | Optional comments property. Not used.                                               |
| [**input**](#input)                              | string | No       | Not used.                                                                           |
| [**additionalOptions**](#additional-options)     | string | No       | Not used.                                                                           |

### Input

Not used.

### Additional options

Not used.

### Default behavior

The default behavior of the `enable-iis` tool is to enable IIS features: IIS-WebServer, IIS-WebServerRole, IIS-WebSockets, and IIS-WebAuthentication, and then install the latest version of the ASP.NET hosting bundle that includes the ASP.NET Core Module. 

## Example usage

```json
{
    "$schema": "./devinit.schema-2.0.json",
    "run": [
        {
            "comments": "Example that will enable IIS features and install the latest ASP.NET hosting bundle.",
            "tool": "enable-iis"
        },
    ]
}
```