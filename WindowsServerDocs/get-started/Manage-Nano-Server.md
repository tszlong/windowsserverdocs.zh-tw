---
title: 管理 Nano Server
description: 更新、服務封裝、網路追蹤、效能監控
ms.prod: windows-server
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/06/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 599d6438-a506-4d57-a0ea-1eb7ec19f46e
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 132f4e1966b332cd6bb6e21402984db7ceed4497
ms.sourcegitcommit: d599eea5203f95609fb21801196252d5dd9f2669
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/07/2019
ms.locfileid: "72005220"
---
# <a name="manage-nano-server"></a>管理 Nano Server

>適用於：Windows Server 2016

> [!IMPORTANT]
> 從 Windows Server 1709 版開始，Nano Server 僅以[容器基礎 OS 映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)的形式來提供。 請查看 [Nano Server 的變更](nano-in-semi-annual-channel.md)以了解這代表的意義。   

Nano Server 是從遠端進行管理。 完全沒有本機登入功能，也不支援終端機服務。 不過，您可以透過各種不同的選項從遠端管理 Nano Server，包括 Windows PowerShell、Windows Management Instrumentation (WMI)、Windows 遠端管理和緊急管理服務 (EMS)。  

若要使用任何遠端管理工具，您可能需要知道 Nano Server 的 IP 位址。 查明 IP 位址的一些方法包括：  
  
-   使用 Nano 修復主控台 (如需詳細資訊，請參閱本主題的＜使用 Nano Server 修復主控台＞一節)。  
  
-   將序列纜線連線到電腦並使用 EMS。  
  
-   您可以使用設定時指派給 Nano Server 的電腦名稱，透過 Ping 取得 IP 位址。 例如，`ping NanoServer-PC /4`。  
  
## <a name="using-windows-powershell-remoting"></a>使用 Windows PowerShell 遠端執行功能  
若要使用 Windows PowerShell 遠端執行功能管理 Nano Server，您需要將 Nano Server 的 IP 位址新增至管理電腦的信任主機清單、將您使用的帳戶新增為 Nano Server 的系統管理員，並啟用 CredSSP (如果想要使用該功能)。  

> [!NOTE]
> 如果目標 Nano Server 和您的管理電腦位於相同的 AD DS 樹系 (或具有信任關係的樹系) 中，則不應該將 Nano Server 新增至信任主機清單；您可以使用 Nano Server 的完整網域名稱連線到 Nano Server，例如︰PS C:\>Enter-PSSession -ComputerName nanoserver.contoso.com -Credential (Get-Credential)
  
  
若要將 Nano Server 新增至信任主機清單，請在提升權限的 Windows PowerShell 命令提示字元中執行下列命令：  
  
`Set-Item WSMan:\localhost\Client\TrustedHosts "<IP address of Nano Server>"`  
  
若要啟動遠端 Windows PowerShell 工作階段，請啟動提升權限的本機 Windows PowerShell 工作階段，然後執行下列命令：  
  
  
```  
$ip = "<IP address of Nano Server>"  
$user = "$ip\Administrator"  
Enter-PSSession -ComputerName $ip -Credential $user  
```  
  
  
您現在可在 Nano Server 上正常執行 Windows PowerShell 命令。  
  
> [!NOTE]  
> 並非所有 Windows PowerShell 命令都可用於此版本的 Nano Server。 若要查看有哪些命令可用，請執行 `Get-Command -CommandType Cmdlet`  
  
透過命令 `Exit-PSSession` 停止遠端工作階段  
  
## <a name="using-windows-powershell-cim-sessions-over-winrm"></a>透過 WinRM 使用 Windows PowerShell CIM 工作階段  
您可以在 Windows PowerShell 中使用 CIM 工作階段和執行個體，透過 Windows 遠端管理 (WinRM) 執行 WMI 命令。  
  
在 Windows PowerShell 命令提示字元中執行下列命令來啟動 CIM 工作階段：  
  
  
```  
$ip = "<IP address of the Nano Server\>"  
$user = $ip\Administrator  
$cim = New-CimSession -Credential $user -ComputerName $ip  
```  
  
  
建立工作階段之後，您可以執行各種 WMI 命令，例如：  
  
  
```  
Get-CimInstance -CimSession $cim -ClassName Win32_ComputerSystem | Format-List *  
Get-CimInstance -CimSession $Cim -Query "SELECT * from Win32_Process WHERE name LIKE 'p%'"  
```  
  
  
## <a name="windows-remote-management"></a>Windows 遠端管理  
您可以在 Nano Server 上透過 Windows 遠端管理 (WinRM) 從遠端執行程式。 若要使用 WinRM，請先在提升權限的命令提示字元中使用下列命令來設定服務並設定字碼頁：  
  
```
winrm quickconfig
winrm set winrm/config/client @{TrustedHosts="<ip address of Nano Server>"}
chcp 65001
```
  
現在您可以在 Nano Server 上從遠端執行命令。 例如：  

```
winrs -r:<IP address of Nano Server> -u:Administrator -p:<Nano Server administrator password> ipconfig
```
  
如需 Windows 遠端管理的詳細資訊，請參閱 [Windows 遠端管理 (WinRM) 概觀](https://technet.microsoft.com/library/dn265971.aspx)。  
   
   
  
## <a name="running-a-network-trace-on-nano-server"></a>在 Nano Server 上執行網路追蹤  
 Nano Server 中無法使用 netsh trace、Tracelog.exe 和 Logman.exe。 若要擷取網路封包，您可以使用下列 Windows PowerShell Cmdlet：  
   
   
```  
New-NetEventSession [-Name]  
Add-NetEventPacketCaptureProvider -SessionName  
Start-NetEventSession [-Name]  
Stop-NetEventSession [-Name]  
```  
如需這些 Cmdlet 的詳細說明，請參閱 [Network Event Packet Capture Cmdlets in Windows PowerShell](https://technet.microsoft.com/library/dn268520(v=wps.630).aspx) (Windows PowerShell 中的網路事件封包擷取 Cmdlet)  

## <a name="installing-servicing-packages"></a>安裝服務套件  
如果您想安裝服務套件，請使用 -ServicingPackagePath 參數 (您可以傳遞路徑陣列到 .cab 檔案)：  
  
`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -ServicingPackagePath \\path\to\kb123456.cab`  
  
服務封裝或 Hotfix 通常會以包含 .cab 檔案的 KB 項目下載。 請遵循下列步驟來解壓縮 .cab 檔案，再使用 -ServicingPackagePath 參數進行安裝：  
  
1.  下載服務套件 (從關聯的知識庫文章或 [Microsoft Update Catalog](https://catalog.update.microsoft.com/v7/site/home.aspx))。 將它儲存至本機目錄或網路共用，例如︰C:\ServicingPackages  
2.  建立您要在其中儲存解壓縮之服務套件的資料夾。  範例︰c:\KB3157663_expanded  
3.  開啟 Windows PowerShell 主控台，然後使用 `Expand` 命令來指定服務套件的 .msu 檔案路徑，包括 `-f:*` 參數和解壓縮服務套件的目的地路徑。  例如：`Expand "C:\ServicingPackages\Windows10.0-KB3157663-x64.msu" -f:* "C:\KB3157663_expanded"`  
  
    展開的檔案應該類似如下：  
C:>dir C:\KB3157663_expanded   
磁碟機 C 的磁碟區是作業系統  
磁碟區序號為 B05B-CC3D  
   
      C:\KB3157663_expanded 的目錄  
   
      04/19/2016  01:17 PM    \<DIR>          .  
      04/19/2016  01:17 PM    \<DIR&gt;          .  
        04/17/2016  12:31 AM               517 Windows10.0-KB3157663-x64-pkgProperties.txt  
04/17/2016  12:30 AM        93,886,347 Windows10.0-KB3157663-x64.cab  
04/17/2016  12:31 AM               454 Windows10.0-KB3157663-x64.xml  
04/17/2016  12:36 AM           185,818 WSUSSCAN.cab  
               4 File(s)     94,073,136 bytes  
               2 Dir(s)  328,559,427,584 bytes free  
4.  執行 `New-NanoServerImage` 並提供 -ServicingPackagePath 參數，以指向此目錄中的 .cab 檔案，例如：`New-NanoServerImage -DeploymentType Guest -Edition Standard -MediaPath \\Path\To\Media\en_us -BasePath .\Base -TargetPath .\NanoServer.wim -ServicingPackagePath C:\KB3157663_expanded\Windows10.0-KB3157663-x64.cab`  

## <a name="managing-updates-in-nano-server"></a>在 Nano 伺服器中管理更新

目前您可以使用 Windows Management Instrumentation (WMI) 的 Windows Update 提供者來尋找適用的更新清單，然後安裝所有或部分更新。 如果您使用 Windows Server Update Services (WSUS)，您也可以設定 Nano Server 連絡 WSUS 伺服器，以取得更新。  

在所有情況下，請先建立 Nano Server 電腦的遠端 Windows PowerShell 工作階段。 這些範例使用 *$sess* 代表工作階段；如果您要使用其他元素，請視需要取代該元素。  


### <a name="view-all-available-updates"></a>檢視所有可用的更新  
---  
您可以使用下列命令來取得適用更新的完整清單：  
```  
$sess = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  

$scanResults = Invoke-CimMethod -InputObject $sess -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=0";OnlineScan=$true}  
```  
**注意：**  
如果沒有更新可用，此命令會傳回下列錯誤：  
```  
Invoke-CimMethod : A general error occurred that is not covered by a more specific error code.  

At line:1 char:16  

+ ... anResults = Invoke-CimMethod -InputObject $sess -MethodName ScanForUp ...  

+                 ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~  

    + CategoryInfo          : NotSpecified: (MSFT_WUOperatio...-5b842a3dd45d")  

   :CimInstance) [Invoke-CimMethod], CimException  

    + FullyQualifiedErrorId : MI RESULT 1,Microsoft.Management.Infrastructure.  

   CimCmdlets.InvokeCimMethodCommand  
```  

### <a name="install-all-available-updates"></a>安裝所有可用的更新  
---  
您可以使用下列命令，同時偵測、下載及安裝**所有**可用的更新：  

```  
$sess = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  

$scanResults = Invoke-CimMethod -InputObject $sess -MethodName ApplyApplicableUpdates  

Restart-Computer  
```  
**注意：**  
Windows Defender 將會防止安裝更新。 若要解決此問題，請解除安裝 Windows Defender、安裝更新，然後重新安裝 Windows Defender。 或者，您可以將更新下載至另一部電腦、將更新複製到 Nano Server，然後使用 DISM.exe 套用更新。  


### <a name="verify-installation-of-updates"></a>確認安裝更新  
---  
您可以使用下列命令來取得目前安裝的更新清單：  
```  
$sess = New-CimInstance -Namespace root/Microsoft/Windows/WindowsUpdate -ClassName MSFT_WUOperationsSession  

$scanResults = Invoke-CimMethod -InputObject $sess -MethodName ScanForUpdates -Arguments @{SearchCriteria="IsInstalled=1";OnlineScan=$true}  
```  

**注意：**  
這些命令會列出安裝項目，但不會在輸出中特別指出「已安裝」。 如果您需要輸出加入該指示 (例如針對報表)，您可以執行  
```PowerShell
Get-WindowsPackage -Online
```

### <a name="using-wsus"></a>使用 WSUS  
---  
上述命令會在網際網路上查詢 Windows Update 和 Microsoft Update 服務，並下載更新。 如果您使用 WSUS，您可以在 Nano Server 上設定登錄機碼，以改用 WSUS 伺服器。  
  
請參閱 [Configure Automatic Updates in a Non-Active Directory Environment](https://technet.microsoft.com/library/cc708449(v=ws.10).aspx) (在非 Active Directory 環境中設定自動更新) 中的「Windows Update Agent Environment Options Registry Keys」(Windows Update 代理程式環境選項登錄機碼) 表格  
  
您至少應該設定 **WUServer** 和 **WUStatusServer** 登錄機碼，但視您如何實作 WSUS，可能需要其他值。 您一律可以藉由檢查相同環境中的另一部 Windows Server，來確認這些設定。  

為您的 WSUS 設定這些值之後，上一節中的命令會查詢該伺服器的更新，並使用它作為下載來源。  

### <a name="automatic-updates"></a>自動更新  
---  
目前，自動更新安裝的方式是將上述步驟轉換成本機 Windows PowerShell 指令碼，然後建立排程的工作來執行此指令碼，並依照排程重新啟動系統。


## <a name="performance-and-event-monitoring-on-nano-server"></a>Nano Server 上的效能和事件監視
[comment]: # (從 Venkat Yalla。)
Nano Server 完全支援 [Windows 事件追蹤](https://aka.ms/u2pa0i) (ETW) 架構，但某些用來管理追蹤和效能計數器的熟悉工具目前不適用於 Nano Server。 不過，Nano Server 具有可完成最常見效能分析案例的工具和 Cmdlet。

任何 Window Server 安裝上的高階工作流程會保持不變 -- 在目標 (Nano Server) 電腦上執行低負荷的追蹤，並使用 [Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/hardware/hh448170.aspx)、[Message Analyzer](https://www.microsoft.com/download/details.aspx?id=44226) 等工具，在另一部電腦上離線後續處理所產生的追蹤檔案及 (或) 記錄檔。

> [!NOTE]
> 若要回顧如何使用 PowerShell 遠端執行功能來轉送檔案，請參閱 [How to copy files to and from Nano Server](https://aka.ms/nri9c8) (如何將檔案複製到 Nano Server 及從中複製檔案)。

下列各節列出最常見的效能資料收集活動，以及在 Nano Server 上完成這些活動的支援方法。

### <a name="query-available-event-providers"></a>查詢可用的事件提供者
[Windows Performance Recorder](https://msdn.microsoft.com/library/hh448229.aspx) 是可查詢可用事件提供者的工具，如下所示：
```
wpr.exe -providers
```

您可以依感興趣的事件類型篩選輸出。 例如：
```
PS C:\> wpr.exe -providers | select-string "Storage"

       595f33ea-d4af-4f4d-b4dd-9dacdd17fc6e                              : Microsoft-Windows-StorageManagement-WSP-Host
       595f7f52-c90a-4026-a125-8eb5e083f15e                              : Microsoft-Windows-StorageSpaces-Driver
       69c8ca7e-1adf-472b-ba4c-a0485986b9f6                              : Microsoft-Windows-StorageSpaces-SpaceManager
       7e58e69a-e361-4f06-b880-ad2f4b64c944                              : Microsoft-Windows-StorageManagement
       88c09888-118d-48fc-8863-e1c6d39ca4df                              : Microsoft-Windows-StorageManagement-WSP-Spaces
```

### <a name="record-traces-from-a-single-etw-provider"></a>記錄單一 ETW 提供者的追蹤
您可以使用新的[事件追蹤管理 Cmdlet](https://technet.microsoft.com/library/dn919247.aspx) 來執行這項動作。 以下是工作流程範例：

建立及啟動追蹤，並指定檔案名稱以儲存事件。
```
PS C:\> New-EtwTraceSession -Name "ExampleTrace" -LocalFilePath c:\etrace.etl
```

將提供者 GUID 新增至追蹤。 使用 ```wpr.exe -providers``` 作為 GUID 翻譯的提供者名稱。 
```
PS C:\> wpr.exe -providers | select-string "Kernel-Memory"

       d1d93ef7-e1f2-4f45-9943-03d245fe6c00                              : Microsoft-Windows-Kernel-Memory

PS C:\> Add-EtwTraceProvider -Guid "{d1d93ef7-e1f2-4f45-9943-03d245fe6c00}" -SessionName "ExampleTrace"
```

移除追蹤 -- 這會停止追蹤工作階段，並將事件排清至關聯的記錄檔。
```
PS C:\> Remove-EtwTraceSession -Name "ExampleTrace"

PS C:\> dir .\etrace.etl

    Directory: C:\

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        9/14/2016  11:17 AM       16515072 etrace.etl
```
> [!NOTE]
> 此範例示範如何將單一追蹤提供者新增至工作階段，但您也可以在具有不同提供者 GUID 的追蹤工作階段上多次使用 ```Add-EtwTraceProvider``` Cmdlet，以啟用多個來源的追蹤。 另一個替代方式是使用如下所述的 ```wpr.exe``` 設定檔。

### <a name="record-traces-from-multiple-etw-providers"></a>記錄多個 ETW 提供者的追蹤
[Windows Performance Recorder](https://msdn.microsoft.com/library/hh448229.aspx) 的 ```-profiles``` 選項可同時啟用多個提供者的追蹤。 您可以選擇一些內建設定檔，例如 CPU、網路和 DiskIO：
```
PS C:\Users\Administrator\Documents> wpr.exe -profiles 

Microsoft Windows Performance Recorder Version 10.0.14393 (CoreSystem)
Copyright (c) 2015 Microsoft Corporation. All rights reserved.

        GeneralProfile              First level triage
        CPU                         CPU usage
        DiskIO                      Disk I/O activity
        FileIO                      File I/O activity
        Registry                    Registry I/O activity
        Network                     Networking I/O activity
        Heap                        Heap usage
        Pool                        Pool usage
        VirtualAllocation           VirtualAlloc usage
        Audio                       Audio glitches
        Video                       Video glitches
        Power                       Power usage
        InternetExplorer            Internet Explorer
        EdgeBrowser                 Edge Browser
        Minifilter                  Minifilter I/O activity
        GPU                         GPU activity
        Handle                      Handle usage
        XAMLActivity                XAML activity
        HTMLActivity                HTML activity
        DesktopComposition          Desktop composition activity
        XAMLAppResponsiveness       XAML App Responsiveness analysis
        HTMLResponsiveness          HTML Responsiveness analysis
        ReferenceSet                Reference Set analysis
        ResidentSet                 Resident Set analysis
        XAMLHTMLAppMemoryAnalysis   XAML/HTML application memory analysis
        UTC                         UTC Scenarios
        DotNET                      .NET Activity
        WdfTraceLoggingProvider     WDF Driver Activity
```

如需建立自訂設定檔的詳細指引，請參閱 [WPR.exe 文件](https://msdn.microsoft.com/library/windows/hardware/hh448223.aspx)。

### <a name="record-etw-traces-during-operating-system-boot-time"></a>記錄作業系統開機期間的 ETW 追蹤
使用 ```New-AutologgerConfig``` Cmdlet 可收集系統開機期間的事件。 其使用方式與 ```New-EtwTraceSession``` Cmdlet 非常類似，但最快只能在下次開機時才能啟用新增至自動記錄工具設定的提供者。 整體工作流程如下所示：

首先，建立新的自動記錄工具設定。
```
PS C:\> New-AutologgerConfig -Name "BootPnpLog" -LocalFilePath c:\bootpnp.etl 
```

將 ETW 提供者新增至此設定。 此範例使用核心 PnP 提供者。 再次叫用 ```Add-EtwTraceProvider```，然後指定相同的自動記錄工具名稱但不同的 GUID，來啟用多個來源的開機追蹤收集。
```
Add-EtwTraceProvider -Guid "{9c205a39-1250-487d-abd7-e831c6290539}" -AutologgerName BootPnpLog
```

這不會立即啟動 ETW 工作階段，而是設定一個工作階段在下次開機時啟動。 重新開機之後，具有自動記錄工具設定名稱的新 ETW 工作階段會自動啟動，並啟用已新增的追蹤提供者。 Nano Server 開機之後，下列命令會在將記錄的事件排清至關聯的追蹤檔案之後停止追蹤工作階段：
```
PS C:\> Remove-EtwTraceSession -Name BootPnpLog
```

若要防止在下次開機時自動建立其他追蹤工作階段，請如下所示移除自動記錄工具設定：
```
PS C:\> Remove-AutologgerConfig -Name BootPnpLog
```

若要收集多部系統或一部無磁碟系統上的開機和安裝追蹤，請考慮使用[安裝與開機事件收集](../administration/get-started-with-setup-and-boot-event-collection.md)。

### <a name="capture-performance-counter-data"></a>擷取效能計數器資料
通常，您可以使用 Perfmon.exe GUI 監視效能計數器資料。 在 Nano Server 上，使用對等的 ```Typeperf.exe``` 命令列。 例如：

查詢可用的計數器 - 您可以篩選輸出，輕鬆地找出感興趣的輸出。
```
PS C:\> typeperf.exe -q | Select-String "UDPv6"

\UDPv6\Datagrams/sec
\UDPv6\Datagrams Received/sec
\UDPv6\Datagrams No Port/sec
\UDPv6\Datagrams Received Errors
\UDPv6\Datagrams Sent/sec
```

這些選項可讓您指定收集計數器值的次數和間隔。 在下列範例中，每隔 3 秒會收集 5 次處理器閒置時間。
```
PS C:\> typeperf.exe "\Processor Information(0,0)\% Idle Time" -si 3 -sc 5

"(PDH-CSV 4.0)","\\ns-g2\Processor Information(0,0)\% Idle Time"
"09/15/2016 09:20:56.002","99.982990"
"09/15/2016 09:20:59.002","99.469634"
"09/15/2016 09:21:02.003","99.990081"
"09/15/2016 09:21:05.003","99.990454"
"09/15/2016 09:21:08.003","99.998577"
Exiting, please wait...
The command completed successfully.
```

其他命令列選項可讓您指定設定檔中感興趣的效能計數器名稱、將輸出重新導向至記錄檔，以及執行其他工作。 如需詳細資訊，請參閱 [typeperf.exe 文件](https://technet.microsoft.com/library/bb490960.aspx)。

您也可以使用 Perfmon.exe 的圖形化介面，從遠端處理 Nano Server 目標。 將效能計數器新增至檢視時，在電腦名稱中指定 Nano Server 目標，而不是預設的 *<Local computer>* 。

### <a name="interact-with-the-windows-event-log"></a>與 Windows 事件記錄檔互動

Nano Server 支援 ```Get-WinEvent``` Cmdlet，該 Cmdlet 在本機和遠端電腦上提供 Windows 事件記錄檔篩選和查詢功能。 [Get-WinEvent 文件頁面](https://technet.microsoft.com/library/hh849682.aspx)提供詳細的選項和範例。 此簡單範例會擷取「系統」  記錄檔過去兩天所註明的「錯誤」  。
```
PS C:\> $StartTime = (Get-Date) - (New-TimeSpan -Day 2)
PS C:\> Get-WinEvent -FilterHashTable @{LogName='System'; Level=2; StartTime=$StartTime} | select TimeCreated, Message

TimeCreated           Message
-----------           -------
9/15/2016 11:31:19 AM Task Scheduler service failed to start Task Compatibility module. Tasks may not be able to reg...
9/15/2016 11:31:16 AM The Virtualization Based Security enablement policy check at phase 6 failed with status: {File...
9/15/2016 11:31:16 AM The Virtualization Based Security enablement policy check at phase 0 failed with status: {File...
```

Nano Server 也支援 ```wevtutil.exe```，以擷取事件記錄檔和發行者的相關資訊。 如需詳細資訊，請參閱 [wevtutil.exe 文件](https://aka.ms/qvod7p)。 

### <a name="graphical-interface-tools"></a>圖形化介面工具
[網頁伺服器管理工具](https://blogs.technet.microsoft.com/servermanagement/2016/08/17/deploy-setup-server-management-tools/)可用來從遠端管理 Nano Server 目標，並使用網頁瀏覽器呈現 Nano Server 事件記錄檔。 最後，MMC 嵌入式管理單元事件檢視器 (eventvwr.msc) 也可用來檢視記錄檔 -- 只要在電腦上使用桌面開啟檔案，並將它指向遠端 Nano Server 即可。




## <a name="using-windows-powershell-desired-state-configuration-with-nano-server"></a>搭配 Nano Server 使用 Windows PowerShell 預期狀態設定  
  
您可以使用 Windows PowerShell 預期狀態設定 (DSC)，將 Nano Server 當做目標節點來管理。 目前，您只能使用 push 模式的 DSC，來管理執行 Nano Server 的節點。 並非所有 DSC 功能都能搭配 Nano Server 使用。  
  
如需完整詳細資訊，請參閱[在 Nano Server 上使用 DSC](https://msdn.microsoft.com/powershell/dsc/nanoDsc)。  
