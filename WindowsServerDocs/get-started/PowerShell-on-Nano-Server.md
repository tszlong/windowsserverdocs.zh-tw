---
title: Nano 伺服器上的 PowerShell
description: Nano Server 上縮減 PowerShell 功能的差異
manager: DonGill
ms.topic: article
ms.assetid: 9b25b939-1e2c-4bed-a8d3-2a8e8e46b53d
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.date: 08/07/2020
ms.openlocfilehash: 08ea28459ae58673c0c0e61310cb4bd4adcda675
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97942704"
---
# <a name="powershell-on-nano-server"></a>Nano 伺服器上的 PowerShell

> 適用於：Windows Server 2016

> [!IMPORTANT]
> 從 Windows Server 1709 版開始，Nano Server 僅以[容器基礎 OS 映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)的形式來提供。 請查看 [Nano Server 的變更](nano-in-semi-annual-channel.md)以了解這代表的意義。

## <a name="powershell-editions"></a>PowerShell 版本

從 5.1 版開始，PowerShell 提供代表各種功能集和平台相容性的不同版本。

- **Desktop Edition：** 建置在 .NET Framework 上，並與目標為完整版 Windows (Server Core 和 Windows Desktop 等) 上執行之 PowerShell 版本的指令碼和模組相容。
- **Core Edition：** 建置在 .NET Core 上，並與目標為縮減版 Windows (Nano Server 和 Windows IoT 等) 上執行之 PowerShell 版本的指令碼和模組相容。

$PSVersionTable 的 PSEdition 屬性會顯示正在執行的 PowerShell 版本。
```powershell
$PSVersionTable

Name                           Value
----                           -----
PSVersion                      5.1.14300.1000
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
CLRVersion                     4.0.30319.42000
BuildVersion                   10.0.14300.1000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```

模組作者可以使用 CompatiblePSEditions 模組資訊清單金鑰，來宣告其模組與一或多個 PowerShell 版本相容。 只有 PowerShell 5.1 或更新版本才支援此金鑰。
```powershell
New-ModuleManifest -Path .\TestModuleWithEdition.psd1 -CompatiblePSEditions Desktop,Core -PowerShellVersion 5.1
$moduleInfo = Test-ModuleManifest -Path \TestModuleWithEdition.psd1
$moduleInfo.CompatiblePSEditions
Desktop
Core

$moduleInfo | Get-Member CompatiblePSEditions

   TypeName: System.Management.Automation.PSModuleInfo

Name                 MemberType Definition
----                 ---------- ----------
CompatiblePSEditions Property   System.Collections.Generic.IEnumerable[string] CompatiblePSEditions {get;}

```
取得可用的模組清單時，您可以依 PowerShell 版本篩選清單。
```powershell
Get-Module -ListAvailable | ? CompatiblePSEditions -Contains Desktop

    Directory: C:\Program Files\WindowsPowerShell\Modules


ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Manifest   1.0        ModuleWithPSEditions

Get-Module -ListAvailable | ? CompatiblePSEditions -Contains Core | % CompatiblePSEditions
Desktop
Core

```
指令碼作者可在 #requires 陳述式上使用 PSEdition 參數，只允許指令碼在相容的 PowerShell 版本上執行。
```powershell
Set-Content C:\script.ps1 -Value #requires -PSEdition Core
Get-Process -Name PowerShell
Get-Content C:\script.ps1
#requires -PSEdition Core
Get-Process -Name PowerShell

C:\script.ps1
C:\script.ps1 : The script 'script.ps1' cannot be run because it contained a #requires statement for PowerShell editions 'Core'. The edition of PowerShell that is required by the script does not match the currently running PowerShell Desktop edition.
At line:1 char:1
+ C:\script.ps1
+ ~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (script.ps1:String) [], RuntimeException
    + FullyQualifiedErrorId : ScriptRequiresUnmatchedPSEdition
```

## <a name="differences-in-powershell-on-nano-server"></a>Nano Server 上的 PowerShell 差異
Nano Server 預設會在所有 Nano Server 安裝中包含 PowerShell Core。 PowerShell Core 是縮減版 PowerShell，建置在 .NET Core 上，並於 Nano Server 和 Windows IoT 核心版等縮減版 Windows 上執行。 PowerShell Core 的運作方式與其他 PowerShell 版本 (例如 Windows Server 2016 上執行的 Windows PowerShell) 的運作方式相同。 不過，縮減版 Nano Server 表示並非 Windows Server 2016 中的所有 PowerShell 功能都可供 Nano Server 上的 PowerShell Core 使用。


**Nano Server 中無法使用的 Windows PowerShell 功能**
* ADSI、ADO 和 WMI 類型介面卡
* Enable-PSRemoting、Disable-PSRemoting (預設會啟用 PowerShell 遠端執行功能；請參閱[安裝 Nano Server](Getting-Started-with-Nano-Server.md) 的＜使用 Windows PowerShell 遠端執行功能＞一節)。
* 排程的工作和 PSScheduledJob 模組
* 用於加入網域的電腦 Cmdlet { Add | Remove } (如需將 Nano Server 加入網域的不同方法，請參閱[安裝 Nano Server](Getting-Started-with-Nano-Server.md) 的＜將 Nano Server 加入網域＞一節)。
* Reset-ComputerMachinePassword、Test-ComputerSecureChannel
* 設定檔 (您可以使用 `Set-PSSessionConfiguration` 新增連入遠端連線的啟動指令碼)
* 剪貼簿 Cmdlet
* 事件記錄檔 Cmdlet { Clear | Get | Limit | New | Remove | Show | Write } (請改用 New-WinEvent 和 Get-WinEvent Cmdlet)。
* Get-PfxCertificate Cmdlet
* TraceSource Cmdlet { Get | Set }
* 計數器 Cmdlet { Get | Export | Import }
* 某些與網路相關的 Cmdlet { New-WebServiceProxy、Send-MailMessage、ConvertTo-Html }
* 使用 PSDiagnostics 模組記錄和追蹤
* Get-HotFix (若要取得及管理 Nano Server 上的更新，請參閱[管理 Nano Server](Manage-Nano-Server.md))。
* 隱含遠端 Cmdlet { Export-PSSession | Import-PSSession }
* New-PSTransportOption
* PowerShell 交易和交易 Cmdlet { Complete | Get | Start | Undo | Use }
* PowerShell 工作流程基礎結構、模組和 Cmdlet
* Out-Printer
* Update-List
* WMI v1 Cmdlet：Get-WmiObject、Invoke-WmiMethod、Register-WmiEvent、Remove-WmiObject、Set-WmiInstance (請改用 CimCmdlets 模組)。

## <a name="using-windows-powershell-desired-state-configuration-with-nano-server"></a>搭配 Nano Server 使用 Windows PowerShell 預期狀態設定

您可以使用 Windows PowerShell 預期狀態設定 (DSC)，將 Nano Server 當做目標節點來管理。 目前，您只能使用 push 模式的 DSC，來管理執行 Nano Server 的節點。 並非所有 DSC 功能都能搭配 Nano Server 使用。

如需完整詳細資訊，請參閱[在 Nano Server 上使用 DSC](/powershell/scripting/dsc/getting-started/nanodsc)。
