---
title: 開發 Nano Server 的 PowerShell Cmdlet
description: '移轉 CIM、.NET Cmdlet、C++ '
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b4267f0-1c91-4a40-9262-5daf4659f686
author: jaimeo
ms.author: jaimeo
ms.date: 09/06/2017
ms.localizationpriority: medium
ms.openlocfilehash: 4c669db414c4f12b6145a26a75b83449f43e8918
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887679"
---
# <a name="developing-powershell-cmdlets-for-nano-server"></a>開發 Nano Server 的 PowerShell Cmdlet

>適用於：Windows Server 2016

> [!IMPORTANT]
> 從 Windows Server 版本 1709 開始，Nano Server 僅以[容器基礎 OS 映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)的形式來提供。 請查看 [Nano Server 的變更](nano-in-semi-annual-channel.md)以了解這代表的意義。 
  
## <a name="overview"></a>總覽  
Nano Server 預設會在所有 Nano Server 安裝中包含 PowerShell Core。 PowerShell Core 是縮減版 PowerShell，建置在 .NET Core 上，並於 Nano Server 和 Windows IoT 核心版等縮減版 Windows 上執行。 PowerShell Core 的運作方式與其他 PowerShell 版本 (例如 Windows Server 2016 上執行的 Windows PowerShell) 的運作方式相同。 不過，縮減版 Nano Server 表示並非 Windows Server 2016 中的所有 PowerShell 功能都可供 Nano Server 上的 PowerShell Core 使用。  
  
如果您有想在 Nano Server 上執行的現成 PowerShell Cmdlet，或要開發新的 Cmdlet 以用於該目的，本主題包含的秘訣和建議有助於簡化工作。  

## <a name="powershell-editions"></a>PowerShell 版本  
  
  
從 5.1 版開始，PowerShell 提供代表各種功能集和平台相容性的不同版本。  
  
- **Desktop Edition:** 在.NET Framework 上建置，並提供與指令碼和模組在完整使用量的 Server Core 等的 Windows 和 Windows 桌面版本上執行的 PowerShell 版本的相容性。  
- **Core Edition:** 建置在.NET Core，並提供與指令碼和模組的縮減版 Nano Server 等的 Windows 和 Windows IoT 上執行的 PowerShell 版本相容性。  
  
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
Get-Module -ListAvailable | ? CompatiblePSEditions -Contains "Desktop"  
  
    Directory: C:\Program Files\WindowsPowerShell\Modules  
  
  
ModuleType Version    Name                                ExportedCommands  
---------- -------    ----                                ----------------  
Manifest   1.0        ModuleWithPSEditions  
  
Get-Module -ListAvailable | ? CompatiblePSEditions -Contains "Core" | % CompatiblePSEditions  
Desktop  
Core  
  
```  
指令碼作者可在 #requires 陳述式上使用 PSEdition 參數，只允許指令碼在相容的 PowerShell 版本上執行。  
```powershell  
Set-Content C:\script.ps1 -Value "#requires -PSEdition Core  
Get-Process -Name PowerShell"  
Get-Content C:\script.ps1  
#requires -PSEdition Core  
Get-Process -Name PowerShell  
  
C:\script.ps1  
C:\script.ps1 : The script 'script.ps1' cannot be run because it contained a "#requires" statement for PowerShell editions 'Core'. The edition of PowerShell that is required by the script does not match the currently running PowerShell Desktop edition.  
At line:1 char:1  
+ C:\script.ps1  
+ ~~~~~~~~~~~~~  
    + CategoryInfo          : NotSpecified: (script.ps1:String) [], RuntimeException  
    + FullyQualifiedErrorId : ScriptRequiresUnmatchedPSEdition  
```  
  
  
## <a name="installing-nano-server"></a>安裝 Nano Server  
[安裝 Nano Server](Getting-Started-with-Nano-Server.md) (也就是本文的最上層主題) 中提供在虛擬或實體機器上安裝 Nano Server 的快速入門和詳細步驟。  
  
> [!NOTE]  
> 若要在 Nano Server 上正確開發，使用 New-NanoServerImage 的 -Development 參數來安裝 Nano Server 會很有用。 這會啟用未簽署驅動程式的安裝、複製偵錯工具二進位檔、開啟偵錯用的連接埠、啟用測試簽署，以及啟用 AppX 套件的安裝，而不需要開發人員授權。 例如:   
>  
>`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -Development`  
  
## <a name="determining-the-type-of-cmdlet-implementation"></a>決定 Cmdlet 實作的類型  
PowerShell 支援 Cmdlet 的一些實作類型，而您所使用的類型會決定在 Nano Server 上正確建立或移植所需的程序和工具。 支援的實作類型包括：  
* CIM - 包含 CIM (WMIv2) 提供者上分層的 CDXML 檔案   
* .NET - 包含實作 Managed Cmdlet 介面的 .NET 組件，通常以 C# 撰寫   
* PowerShell 指令碼 - 包含以 PowerShell 語言撰寫的指令碼模組 (.psm1) 或指令碼 (.ps1)   
  
如果您不確定要移植之現有 Cmdlet 所使用的實作，請安裝您的產品或功能，然後在下列其中一個位置尋找 PowerShell 模組資料夾：   
  
* %windir%\system32\WindowsPowerShell\v1.0\Modules   
* %ProgramFiles%\WindowsPowerShell\Modules   
* %UserProfile%\Documents\WindowsPowerShell\Modules   
* \<您的產品安裝位置 >   
    
 在這些位置確認下列詳細資料：  
 * CIM Cmdlet 的副檔名為 .cdxml。  
 * .NET Cmdlet 的副檔名為 .dll，或已安裝 .psd1 檔案所列 GAC 之 RootModule、ModuleToProcess 或 NestedModules 欄位下的組件。  
* PowerShell 指令碼 Cmdlet 的副檔名為 .psm1 或 .ps1。   
  
## <a name="porting-cim-cmdlets"></a>移植 CIM Cmdlet  
一般而言，這些 Cmdlet 應該可在 Nano Server 中正常運作，而不需要進行任何轉換。 不過，如果尚未移植基礎 WMI v2 提供者，您必須先將它移植才能在 Nano Server 上執行。  
  
### <a name="building-c-for-nano-server"></a>建置適用於 Nano Server 的 C++  
若要讓 C++ DLL 在 Nano Server 上正常運作，請針對 Nano Server (而不是特定版本) 加以編譯。  
  
如需在 Nano Server 上開發 C++ 的先決條件和逐步解說，請參閱 [Developing Native Apps on Nano Server](http://blogs.technet.com/b/nanoserver/archive/2016/04/27/developing-native-apps-on-nano-server.aspx) (在 Nano Server 上開發原生應用程式)。  
  
  
## <a name="porting-net-cmdlets"></a>移植 .NET Cmdlet  
Nano Server 支援大部分的 C# 程式碼。 您可以使用 [ApiPort](https://github.com/Microsoft/dotnet-apiport) 掃描是否有不相容的 API。  
  
### <a name="powershell-core-sdk"></a>Powershell Core SDK  
[PowerShell Gallery](https://www.powershellgallery.com/packages/Microsoft.PowerShell.NanoServer.SDK/) (PowerShell 資源庫) 中提供模組 "Microsoft.PowerShell.NanoServer.SDK"，使用以 Nano Server 隨附的 CoreCLR 和 PowerShell Core 版本為目標的 Visual Studio 2015 Update 2 來加速 .NET Cmdlet 的開發。 您可以使用 PowerShellGet 搭配下列命令來安裝模組：  
  
`Find-Module Microsoft.PowerShell.NanoServer.SDK -Repository PSGallery | Install-Module -Scope <scope>`  
  
PowerShell Core SDK 模組所公開的 Cmdlet，可設定正確的 CoreCLR 和 PowerShell Core 參考組件、在 Visual Studio 2015 中建立以這些參考組件為目標的 C# 專案，以及在 Nano Server 電腦上設定遠端偵錯工具，讓開發人員可以偵錯 Nano Server 上以 Visual Studio 2015 遠端執行的 .NET Cmdlet。  
  
PowerShell Core SDK 模組需要 Visual Studio 2015 Update 2。 如果您未安裝 Visual Studio 2015，您可以安裝 [Visual Studio Community 2015](https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx)。  
  
SDK 模組也相依於要在 Visual Studio 2015 中安裝的下列功能：  
  
- Windows 及 Web 程式開發 -> 通用 Windows 應用程式開發工具 -> 工具 (1.3.1) 及 Windows 10 SDK  
  
使用 SDK 模組之前，請先檢閱您的 Visual Studio 安裝，以確保符合這些先決條件。 請確定您在 Visual Studio 安裝期間選取並安裝上述功能，或修改您現有的 Visual Studio 2015 安裝以安裝此功能。  
  
PowerShell Core SDK 模組包含下列 Cmdlet：  
- New-NanoCSharpProject:建立新的 Visual StudioC#以 CoreCLR 和 PowerShell Core 包含在 Nano Server 的 Windows Server 2016 版本為目標的專案。  
- Show-SdkSetupReadMe:在 [檔案總管] 中開啟 SDK 根資料夾，並開啟 README.txt 檔案手動進行安裝。  
- Install-remotedebugger︰安裝和設定 Nano Server 電腦上的 Visual Studio 遠端偵錯工具。  
- Start-remotedebugger︰在執行 Nano Server 的遠端電腦上啟動遠端偵錯工具。  
- Stop-remotedebugger︰在執行 Nano Server 的遠端電腦上停止遠端偵錯工具。  
  
如需如何使用這些 Cmdlet 的詳細資訊，請在安裝並匯入模組之後，在每個 Cmdlet 上執行 Get-Help，如下所示：  
  
`Get-Command -Module Microsoft.PowerShell.NanoServer.SDK | Get-Help -Full`   
  
  
### <a name="searching-for-compatible-apis"></a>搜尋相容的 API  
  
您可以在 API 類別目錄中搜尋 .NET Core，或反組譯 Core CLR 參考組件。 如需 .NET API 之平台可攜性的詳細資訊，請參閱 [Platform Portability](https://github.com/Microsoft/dotnet-apiport/blob/master/docs/HowTo/PlatformPortability.md)  
  
### <a name="pinvoke"></a>PInvoke  
在 Nano Server 使用的 Core CLR 中，已將一些基本 DLL (例如 kernel32.dll 和 advapi32.dll) 分割成多個 API 集合，因此您必須確定 PInvoke 參考正確的 API。 任何不相容都會顯示為執行階段錯誤。  
  
如需 Nano Server 支援的原生 API 清單，請參閱 [Nano Server API](https://msdn.microsoft.com/library/mt588480(v=vs.85).aspx)。  
  
### <a name="building-c-for-nano-server"></a>建置適用於 Nano Server 的 C#  
  
在 Visual Studio 2015 中使用 `New-NanoCSharpProject` 建立 C# 專案之後，您可以直接在 Visual Studio 中按一下 [組建] 功能表，然後選取 [建置專案] 或 [建置方案] 加以建置。 產生的組件會以 Nano Server 隨附的正確 CoreCLR 和 PowerShell Core 為目標，而且您只能將這些組件複製到執行 Nano Server 的電腦，再加以使用。  
  
### <a name="building-managed-c-cppcli-for-nano-server"></a>建置適用於 Nano Server 的 Managed C++ (CPP/CLI)  
CoreCLR 不支援 Managed C++。 移植到 CoreCLR 時，請在 C# 中重寫 Managed C++ 程式碼，並透過 PInvoke 進行所有原生呼叫。  
  
## <a name="porting-powershell-script-cmdlets"></a>移植 PowerShell 指令碼 Cmdlet  
  
PowerShell Core 具有與其他 PowerShell 版本完全相同的PowerShell 語言，包括在 Windows Server 2016 和 Windows 10 上執行的版本。 不過，將 PowerShell 指令碼 Cmdlet 移植到 Nano Server 時，請注意下列因素：  
* 是否相依於其他 Cmdlet？ 如果是的話，這些 Cmdlet 是否可在 Nano Server 上使用？ 如需哪些 Cmdlet 無法使用的資訊，請參閱 [Nano Server 上的 PowerShell](PowerShell-on-Nano-Server.md)。  
* 如果相依於在執行階段載入的組件，是否仍然可以運作？   
* 如何從遠端偵錯指令碼？   
* 如何從 WMI .Net 移轉至 MI .Net？  
  
### <a name="dependency-on-built-in-cmdlets"></a>內建 Cmdlet 的相依性  
並非 Windows Server 2016 中的所有 Cmdlet 都可在 Nano Server 上使用 (請參閱 [Nano Server 上的 PowerShell](PowerShell-on-Nano-Server.md))。 最佳做法是設定 Nano Server 虛擬機器，並探索您需要的 Cmdlet 是否可用。 若要這樣做，請執行 `Enter-PSSession` 以連線到目標 Nano Server，然後執行 `Get-Command -CommandType Cmdlet, Function` 以取得可用 Cmdlet 的清單。  
  
### <a name="consider-using-powershell-classes"></a>考慮使用 PowerShell 類別  
Nano Server 支援 Add-Type 以編譯內嵌 C# 程式碼。 如果您要撰寫新的程式碼或移植現有的程式碼，您也可以考慮使用 PowerShell 類別來定義自訂類型。 您可以將 PowerShell 類別用於屬性包案例及列舉。 如果您需要執行 PInvoke，請透過 C# 使用 Add-Type 或在先行編譯組件中執行。  
下列範例示範如何使用 Add-Type：  
  
```  
Add-Type -ReferencedAssemblies ([Microsoft.Management.Infrastructure.Ciminstance].Assembly.Location) -TypeDefinition @'  
public class TestNetConnectionResult  
{  
   // The compute name  
   public string ComputerName = null;  
   // The Remote IP address used for connectivity  
   public System.Net.IPAddress RemoteAddress = null;  
}  
'@  
# Create object and set properties  
$result = New-Object TestNetConnectionResult  
$result.ComputerName = "Foo"  
$result.RemoteAddress = 1.1.1.1  
  
```  
 下列範例示範如何在 Nano Server 上使用 PowerShell 類別：  
   
```  
class TestNetConnectionResult    
{    
   # The compute name  
  [string] $ComputerName    
  
  #The Remote IP address used for connectivity    
  [System.Net.IPAddress] $RemoteAddress  
}  
# Create object and set properties  
$result = [TestNetConnectionResult]::new()  
$result.ComputerName = "Foo"  
$result.RemoteAddress = 1.1.1.1  
  
```  
  
### <a name="remotely-debugging-scripts"></a>從遠端偵錯指令碼  
  
若要從遠端偵錯指令碼，請從 PowerShell ISE 使用 `Enter-PSsession` 連線到遠端電腦。 進入工作階段之後，您就可以執行 `psedit <file_path>`，隨即會在您的本機 PowerShell ISE 中開啟此檔案的複本。 然後，您可以設定中斷點來偵錯指令碼，就像是在本機執行一樣。 此外，您對此檔案所做的任何變更都會儲存在遠端版本中。   
  
### <a name="migrating-from-wmi-net-to-mi-net"></a>從 WMI .NET 移轉至 MI .NET  
  
[WMI.NET](https://msdn.microsoft.com/library/mt481551(v=vs.110).aspx)不支援，因此使用舊版 API 的所有 cmdlet 必須都移轉到支援的 WMI API:[MI。NET](https://msdn.microsoft.com/library/dn387184(v=vs.85).aspx)。 您可以直接透過 C# 或透過 CimCmdlets 模組中的 Cmdlet 來存取 MI .NET。   
  
### <a name="cimcmdlets-module"></a>CimCmdlets 模組  
  
Nano Server 不支援 WMI v1 Cmdlet (例如 Get-WmiObject)， 但支援 CimCmdlets 模組中的 CIM Cmdlet (例如 Get-CimInstance)。 CIM Cmdlet 會非常緊密地對應至 WMI v1 Cmdlet。 例如，Get-WmiObject 使用非常類似的參數來與 Get-CimInstance 建立關聯。 方法引動過程語法稍有不同，但已透過 Invoke-CimMethod 適當記載。 輸入參數時請小心。 MI .NET 對於方法參數類型有更嚴格的需求。  
  
### <a name="c-api"></a>C# API  
  
WMI .NET 包裝 WMIv1 介面，而 MI .NET 包裝 WMIv2 (CIM) 介面。 公開的類別可能會不同，但基礎作業非常類似。 您可以列舉或取得物件的執行個體，並對其叫用作業以完成工作。   
  
  


