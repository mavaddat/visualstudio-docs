---
title: windowsfeature-enable
description: devinit tool windowsfeature-enable.
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
# windowsfeature-enable

The `windowsfeature-enable` tool is used to enable Windows features.

## Usage

| Name                                             | Type   | Required | Value                                                                    |
|--------------------------------------------------|--------|----------|--------------------------------------------------------------------------|
| **comments**                                     | string | No       | Optional comments property. Not used.                                    |
| [**input**](#input)                              | string | Yes      | The Windows feature to install. See [Input](#input) below for details.   |
| [**additionalOptions**](#additional-options)     | string | No       | See [Additional options](#additional-options) below for details.         |

### Input

The `input` property should be the `name` of the `windows feature` to install. A list of available features can be found by running the `Get-WindowsFeature` PowerShell cmd.

### Additional-Options

None.

### Default behavior

The Default behavior of the `windowsfeature-enable` tool is to error, as `input` is required.

## Example usage

```json
{
    "$schema": "https://json.schemastore.org/devinit.schema-2.0",
    "run": [
        {
            "comments": "Installs IIS.",
            "tool": "windowsfeature-enable",
            "input": "web-server",
        },
        {
            "comments": "Installs the .NET Framework 3.5.",
            "tool": "windowsfeature-enable",
            "input": "net-framework-features"
        },
        {
            "comments": "Installs the .NET Framework 4.5.",
            "tool": "windowsfeature-enable",
            "input": "net-framework-45-features"
        },
        {
            "comments": "Installs Windows Subsystem for Linux 2.0.",
            "tool": "windowsfeature-enable",
            "input": "Microsoft-Windows-Subsystem-Linux"
        }
    ]
}
```
