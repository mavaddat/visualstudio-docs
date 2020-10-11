---
title: wsl-install
description: devinit tool wsl-install.
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
# wsl-install

The `wsl-install` tool is used to install Linux distros for the [Windows Subsystem for Linux](/windows/wsl/) (WSL).

The `wsl-install` tool requires WSL 2 to already be enabled on Windows. If for some reason WSL2 is not enabled, you can enable WSL2 by using the [windowsfeature-enable](tool-windowsfeature-enable.md) tool and the feature name `Microsoft-Windows-Subsystem-Linux`.

## Usage

If both the `input` and `additionalOptions` properties are omitted or empty, then the tool will follow the [default](#default-behavior) behavior detailed below.

| Name                                             | Type   | Required | Value                                                             |
|--------------------------------------------------|--------|----------|-------------------------------------------------------------------|
| **comments**                                     | string | No       | Optional comments property. Not used.                             |
| [**input**](#input)                              | string | Yes      | The distro to install. See [Input](#input) below for details.     |
| [**additionalOptions**](#additional-options)     | string | No       | See [Additional options](#additional-options) below for details.  |

### Input

The URI for the AppX application distribution package (`.appx`) containing the distro to deploy. The URI must point to a `.appx` archive that contains a single `install.tar.gz` either in archive root, or inside of an inner `.appx` archive. Examples of supported distros include:

| Distro                          | Uri                                                           |
|---------------------------------|---------------------------------------------------------------|
| Ubuntu 20.04                    | https://aka.ms/wslubuntu2004                                  |
| Ubuntu 18.04                    | https://aka.ms/wsl-ubuntu-1804                                |
| Ubuntu 16.04                    | https://aka.ms/wsl-ubuntu-1604                                |
| Debian GNU/Linux                | https://aka.ms/wsl-debian-gnulinux                            |
| Kali Linux                      | https://aka.ms/wsl-kali-linux-new                             |
| OpenSUSE Leap 42                | https://aka.ms/wsl-opensuse-42                                |
| SUSE Linux Enterprise Server 12 | https://aka.ms/wsl-sles-12                                    |

> [!NOTE]
> ARM Linux distros are not currently supported in GitHub Codespaces.

### Additional options

Multiple Additional options are supported:

| Name                      | Type      | Required | Value                                                                                                                                                                                    |
|---------------------------|-----------|----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| --wsl-version             | string    | No       | WSL version to use. The default value is 2.                                                                                                                                  |
| --post-create-command     | string    | No       | The command to execute inside of the Linux distro once the installation is complete. The command should be formatted as a single word, or wrapped in quotes. The default is no command.  |

### Default behavior

The Default behavior of the `wsl-install` tool is to error as the `input` property, the distro to install, is required.

## Example usage

```json
{
    "$schema": "https://json.schemastore.org/devinit.schema-2.0",
    "run": [
        {
            "comments": "Example that will install Ubuntu 20.04.",
            "tool": "wsl-install",
            "input": "https://aka.ms/wslubuntu2004"
        },
        {
            "comments": "Example that will install Ubuntu 20.04 using WSL2, and echo 'Hello from Ubuntu!' after installing.",
            "tool": "wsl-install",
            "input": "https://aka.ms/wslubuntu2004",
            "additionalOptions": "--wsl-version 2 --post-create-command 'echo Hello from Ubuntu!'"
        },
        {
            "comments": "Example that will install Ubuntu 20.04 using WSL2, and configure it with various packages.",
            "tool": "wsl-install",
            "input": "https://aka.ms/wslubuntu2004",
            "additionalOptions": "--wsl-version 2 --post-create-command 'apt-get update && apt-get install g++ gcc g++-9 gcc-9 cmake gdb ninja-build zip rsync -y'"
        }
    ]
}
```