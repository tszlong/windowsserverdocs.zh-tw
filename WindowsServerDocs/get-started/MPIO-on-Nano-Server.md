---
title: Nano Server 上的 MPIO
description: 在 Nano 上設定多重路徑 I/O (MPIO)
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.date: 09/06/2017
ms.technology: server-nano
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fbef4d91-e18c-4f1b-952f-a9a7ad46cd74
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 15d9ebfe72744ed26239587ef297b06361233425
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874039"
---
# <a name="mpio-on-nano-server"></a>Nano Server 上的 MPIO

>適用於：Windows Server 2016

> [!IMPORTANT]
> 從 Windows Server 版本 1709 開始，Nano Server 僅以[容器基礎 OS 映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)的形式來提供。 請查看 [Nano Server 的變更](nano-in-semi-annual-channel.md)以了解這代表的意義。 

本主題介紹如何在 Windows Server 2016 的 Nano Server 安裝中使用多重路徑 I/O (MPIO)。 如需 Windows Server 之多重路徑 I/O (MPIO) 的一般資訊，請參閱[多重路徑 I/O 概觀](https://technet.microsoft.com/library/cc725907.aspx)。  

## <a name="using-mpio-on-nano-server"></a>在 Nano Server上使用多重路徑 I/O (MPIO)  
您可以在 Nano Server上使用多重路徑 I/O (MPIO)，但具有下列差異：  
  
-   僅支援 MSDSM。  
  
-   系統會動態選擇負載平衡原則，而且無法修改。 此原則具有下列特性：  
  
    -   預設值 -- 循環配置資源 (主動/主動)  
  
    -   SAS HDD -- LeastBlocks  
  
    -   ALUA -- 以子集循環配置資源  
  
-   ALUA 陣列的路徑狀態 (主動/被動) 會從目標陣列挑選。  
  
-   存放裝置會依匯流排類型 (例如 FC、iSCSI 或 SAS) 宣告。 當 Nano Server 上安裝 MPIO 時，在設定 MPIO 以宣告及管理特定磁碟之前，磁碟仍會公開為重複值 (每個路徑可使用一個磁碟)。 本主題中的範例指令碼將會宣告或取消宣告 MPIO 的磁碟。

- 不支援 iSCSI 開機。
  
您可以使用下列 Windows PowerShell Cmdlet 來啟用 MPIO：  
  
`Enable-WindowsOptionalFeature -Online -FeatureName MultiPathIO`  
  
此範例指令碼可讓呼叫者藉由變更特定登錄機碼，來宣告或取消宣告 MPIO 的磁碟。 雖然您可以將其他存放裝置新增至這些機碼來進行宣告，但不建議直接操作這些機碼。  
  
```  
#  
#  Copyright (c) 2015 Microsoft Corporation.  All rights reserved.  
#    
#  THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY   
#  OF ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED   
#  TO THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A    
#  PARTICULAR PURPOSE  
#  
  
<#  
.Synopsis  
    This powershell script allows you to enable Multipath-IO support using Microsoft's  
    in-box DSM (MSDSM) for storage devices attached by certain bus types.  
  
    After running this script you will have to either:  
    1. Disable and then re-enable the relevant Host Bus Adapters (HBAs); or  
    2. Reboot the system.  
  
.Description  
  
.Parameter BusType  
    Specifies the bus type for which the claim/unclaim should be done.  
  
    If omitted, this parameter defaults to "All".  
  
    "All" - Will claim/unclaim storage devices attached through Fibre Channel, iSCSI, or SAS.  
  
    "FC" - Will claim/unclaim storage devices attached through Fibre Channel.  
  
    "iSCSI" - Will claim/unclaim storage devices attached through iSCSI.  
  
    "SAS" - Will claim/unclaim storage devices attached through SAS.  
  
.Parameter Server  
    Allows you to specify a remote system, either via computer name or IP address.  
  
    If omitted, this parameter defaults to the local system.  
  
.Parameter Unclaim  
    If specified, the script will unclaim storage devices of the bus type specified by the  
    BusType parameter.  
  
    If omitted, the script will default to claiming storage devices instead.  
  
.Example  
MultipathIoClaim.ps1  
  
Claims all storage devices attached through Fibre Channel, iSCSI, or SAS.    
  
.Example  
MultipathIoClaim.ps1 FC  
  
Claims all storage devices attached through Fibre Channel.  
  
.Example  
MultipathIoClaim.ps1 SAS -Unclaim  
  
Unclaims all storage devices attached through SAS.  
  
.Example  
MultipathIoClaim.ps1 iSCSI 12.34.56.78  
  
Claims all storage devices attached through iSCSI on the remote system with IP address 12.34.56.78.    
  
#>  
[CmdletBinding()]  
param  
(    
    [ValidateSet('all','fc','iscsi','sas')]  
    [string]$BusType='all',  
  
    [string]$Server="127.0.0.1",  
  
    [switch]$Unclaim   
)  
  
#  
# Constants  
#  
$type = [Microsoft.Win32.RegistryHive]::LocalMachine  
[string]$mpioKeyName = "SYSTEM\CurrentControlSet\Control\MPDEV"  
[string]$mpioValueName = "MpioSupportedDeviceList"  
[string]$msdsmKeyName = "SYSTEM\CurrentControlSet\Services\msdsm\Parameters"  
[string]$msdsmValueName = "DsmSupportedDeviceList"  
  
[string]$fcHwid = "MSFT2015FCBusType_0x6   "  
[string]$sasHwid = "MSFT2011SASBusType_0xA  "  
[string]$iscsiHwid = "MSFT2005iSCSIBusType_0x9"  
  
#  
# Functions  
#  
  
function AddHardwareId  
{  
    param  
    (  
        [Parameter(Mandatory=$True)]  
        [string]$Hwid,  
  
        [string]$Srv="127.0.0.1",  
  
        [string]$KeyName="SYSTEM\CurrentControlSet\Control\MultipathIoClaimTest",  
  
        [string]$ValueName="DeviceList"  
    )  
  
    $regKey = [Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey($type, $Srv)  
    $key = $regKey.OpenSubKey($KeyName, 'true')  
    $val = $key.GetValue($ValueName)  
    $val += $Hwid  
    $key.SetValue($ValueName, [string[]]$val, 'MultiString')  
}  
  
function RemoveHardwareId  
{  
    param  
    (  
        [Parameter(Mandatory=$True)]  
        [string]$Hwid,  
  
        [string]$Srv="127.0.0.1",  
  
        [string]$KeyName="SYSTEM\CurrentControlSet\Control\MultipathIoClaimTest",  
  
        [string]$ValueName="DeviceList"  
    )  
  
    [string[]]$newValues = @()  
    $regKey = [Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey($type, $Srv)  
    $key = $regKey.OpenSubKey($KeyName, 'true')  
    $values = $key.GetValue($ValueName)  
    foreach($val in $values)  
    {  
        # Only copy values that don't match the given hardware ID.  
        if ($val -ne $Hwid)  
        {  
            $newValues += $val  
            Write-Debug "$($val) will remain in the key."  
        }  
        else  
        {  
            Write-Debug "$($val) will be removed from the key."  
        }  
    }  
    $key.SetValue($ValueName, [string[]]$newValues, 'MultiString')  
}  
  
function HardwareIdClaimed  
{  
    param  
    (  
        [Parameter(Mandatory=$True)]  
        [string]$Hwid,  
  
        [string]$Srv="127.0.0.1",  
  
        [string]$KeyName="SYSTEM\CurrentControlSet\Control\MultipathIoClaimTest",  
  
        [string]$ValueName="DeviceList"  
    )  
  
    $regKey = [Microsoft.Win32.RegistryKey]::OpenRemoteBaseKey($type, $Srv)  
    $key = $regKey.OpenSubKey($KeyName)  
    $values = $key.GetValue($ValueName)  
    foreach($val in $values)  
    {  
        if ($val -eq $Hwid)  
        {  
            return 'true'  
        }  
    }  
  
    return 'false'  
}  
  
function GetBusTypeName  
{  
    param  
    (  
        [Parameter(Mandatory=$True)]  
        [string]$Hwid  
    )  
  
    if ($Hwid -eq $fcHwid)  
    {  
        return "Fibre Channel"  
    }  
    elseif ($Hwid -eq $sasHwid)  
    {  
        return "SAS"  
    }  
    elseif ($Hwid -eq $iscsiHwid)  
    {  
        return "iSCSI"  
    }  
  
    return "Unknown"  
}  
  
#  
# Execution starts here.  
#  
  
#  
# Create the list of hardware IDs to claim or unclaim.  
#  
[string[]]$hwids = @()  
  
if ($BusType -eq 'fc')  
{  
    $hwids += $fcHwid  
}  
elseif ($BusType -eq 'iscsi')  
{  
    $hwids += $iscsiHwid  
}  
elseif ($BusType -eq 'sas')  
{  
    $hwids += $sasHwid  
}  
elseif ($BusType -eq 'all')  
{  
    $hwids += $fcHwid  
    $hwids += $sasHwid  
    $hwids += $iscsiHwid  
}  
else  
{  
    Write-Host "Please provide a bus type (FC, iSCSI, SAS, or All)."  
}  
  
$changed = 'false'  
  
#  
# Attempt to claim or unclaim each of the hardware IDs.  
#  
foreach($hwid in $hwids)  
{      
    $busTypeName = GetBusTypeName $hwid  
  
    #  
    # The device is only considered claimed if it's in both the MPIO and MSDSM lists.  
    #  
    $mpioClaimed = HardwareIdClaimed $hwid $Server $mpioKeyName $mpioValueName  
    $msdsmClaimed = HardwareIdClaimed $hwid $Server $msdsmKeyName $msdsmValueName  
    if ($mpioClaimed -eq 'true' -and $msdsmClaimed -eq 'true')  
    {  
        $claimed = 'true'  
    }  
    else  
    {  
        $claimed = 'false'  
    }  
  
    if ($mpioClaimed -eq 'true')  
    {  
        Write-Debug "$($hwid) is in the MPIO list."  
    }  
    else  
    {  
        Write-Debug "$($hwid) is NOT in the MPIO list."  
    }  
  
    if ($msdsmClaimed -eq 'true')  
    {  
        Write-Debug "$($hwid) is in the MSDSM list."  
    }  
    else  
    {  
        Write-Debug "$($hwid) is NOT in the MSDSM list."  
    }  
  
    if ($Unclaim)  
    {  
        #  
        # Unclaim this hardware ID.  
        #   
        if ($claimed -eq 'true')  
        {              
            RemoveHardwareId $hwid $Server $mpioKeyName $mpioValueName  
            RemoveHardwareId $hwid $Server $msdsmKeyName $msdsmValueName  
            $changed = 'true'  
            Write-Host "$($busTypeName) devices will not be claimed."  
        }  
        else  
        {  
            Write-Host "$($busTypeName) devices are not currently claimed."  
        }  
  
    }  
    else  
    {  
        #  
        # Claim this hardware ID.  
        #       
        if ($claimed -eq 'true')  
        {              
            Write-Host "$($busTypeName) devices are already claimed."  
        }  
        else  
        {  
            AddHardwareId $hwid $Server $mpioKeyName $mpioValueName  
            AddHardwareId $hwid $Server $msdsmKeyName $msdsmValueName  
            $changed = 'true'  
            Write-Host "$($busTypeName) devices will be claimed."  
        }  
    }  
}  
  
#  
# Finally, if we changed any of the registry keys remind the user to restart.  
#  
if ($changed -eq 'true')  
{  
    Write-Host "The system must be restarted for the changes to take effect."  
}  
```  
  


