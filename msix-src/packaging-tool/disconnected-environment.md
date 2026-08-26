---
title: Using the MSIX Packaging Tool in a disconnected environment
description: This article describes how to acquire all of the assets required for the MSIX Packaging Tool if you are in a disconnected environment.
ms.date: 08/26/2026
ms.topic: concept-article
keywords: msix
---

# Using the MSIX Packaging Tool in a disconnected environment

While we make it super easy for users to acquire the MSIX Packaging Tool through the Microsoft Store, we know that not everyone has access to the Store or to a connected environment where they want to perform conversions. So this topic is about using the tool in a disconnected mode. The info here applies only to our public releases; not to our [Insider program](insider-program.md) releases.

## Get the MSIX Packaging Tool

You can download the latest version of the offline package below.

> [!div class="button" style="text-align: left;" width="150px;"] 
> [Download 1.2024.405.0 MSIX Packaging Tool](https://download.microsoft.com/download/e/2/e/e2e923b2-7a3a-4730-969d-ab37001fbb5e/MSIXPackagingtoolv1.2024.405.0.msixbundle)
If you encounter issues with the offline copy of the packaging tool, then download the offline copy of the license for the tool below.

> [!div class="button" style="text-align: left;" width="150px;"] 
> [Offline copy of the license](https://download.microsoft.com/download/e/2/e/e2e923b2-7a3a-4730-969d-ab37001fbb5e/MSIXPackagingtoolv1.2024.405.0.License.xml)
After you have the offline version of the tool, you can use [PowerShell](/powershell/module/dism/add-appxprovisionedpackage) to add the app package and license to your machine.

### Example of offline installation

```
PS C:\> Add-AppxProvisionedPackage -Path C:\offline -PackagePath C:\MSIX\MyPackage.msix -LicensePath C:\MSIX\MyLicense.xml
```

The MSIX Packaging Tool driver is delivered as the `Msix.PackagingTool.Driver~~~~0.0.1.0` [Feature on Demand (FOD)](/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities). On a connected device, Windows can acquire the capability from Windows Update:

```cmd
DISM /Online /Add-Capability /CapabilityName:Msix.PackagingTool.Driver~~~~0.0.1.0
```

If Windows Update is disabled or controlled by Windows Server Update Services (WSUS) or Configuration Manager, review [how FOD source policy differs by Windows version](/windows/deployment/update/fod-and-lang-packs). Windows 11, version 22H2 and later can receive FOD content through on-premises Unified Update Platform (UUP). Earlier supported configurations might require Windows Update or an alternate source.

### Install the driver from offline media

FOD packages are serviced components and must match the Windows release and architecture on the conversion device. Don't use the Windows 11, version 21H2 driver CAB with later Windows 11 releases. Instead, use the media that corresponds to the installed Windows release:

| Conversion device | Offline source |
| --- | --- |
| Windows 11 | Matching Windows 11 Languages and Optional Features ISO |
| Windows 10, version 2004 and later | Windows 10, version 2004 Features on Demand ISO |

The [Features on Demand media table](/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#features-on-demand-media) lists the correct media for other Windows releases. Acquire the ISO through a channel available to your organization:

- Volume licensing customers can [download volume licensing products](/microsoft-365/commerce/licenses/download-vl-products) from the Microsoft 365 admin center.
- OEMs and system builders can obtain the Languages and Optional Features ISO from the [Microsoft OEM site](https://go.microsoft.com/fwlink/?LinkId=131359) or [Device Partner Center](https://devicepartner.microsoft.com/).

Mount the matching ISO and use it directly as the FOD repository. In an elevated Command Prompt, replace `<source>` with the mounted drive or repository path:

```cmd
DISM /Online /Add-Capability /CapabilityName:Msix.PackagingTool.Driver~~~~0.0.1.0 /Source:<source> /LimitAccess
```

`/LimitAccess` prevents DISM from contacting Windows Update or WSUS. If you build a reduced repository instead of using the mounted ISO, follow the [FOD repository guidance](/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#fod-repositories); don't copy an individual CAB without its required repository metadata.
