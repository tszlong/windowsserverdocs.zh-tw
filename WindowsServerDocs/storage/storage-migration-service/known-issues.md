---
title: 儲存體遷移服務的已知問題
description: 儲存體遷移服務的已知問題和疑難排解支援，例如如何收集 Microsoft 支援服務的記錄。
author: nedpyle
ms.author: nedpyle
manager: tiaascs
ms.date: 02/10/2020
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.openlocfilehash: a9759f0ea8835c8e07bcd298b75024e3ee29c9ed
ms.sourcegitcommit: b5c12007b4c8fdad56076d4827790a79686596af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78856342"
---
# <a name="storage-migration-service-known-issues"></a>儲存體遷移服務的已知問題

本主題包含使用[儲存體遷移服務](overview.md)來遷移伺服器時的已知問題的解答。

儲存體遷移服務的發行分為兩個部分： Windows Server 中的服務，以及 Windows 系統管理中心內的使用者介面。 此服務適用于 Windows Server、長期維護通道，以及 Windows Server、半年通道、雖然 Windows 系統管理中心可供個別下載。 我們也會定期包含 Windows Server 累計更新中的變更（透過 Windows Update 發行）。 

例如，Windows Server 1903 版包含儲存體遷移服務的新功能和修正，其也適用于 Windows Server 2019 和 Windows Server 1809 版，安裝[KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)。

## <a name="collecting-logs"></a>如何在使用 Microsoft 支援服務時收集記錄檔

儲存體遷移服務包含 Orchestrator 服務和 Proxy 服務的事件記錄檔。 Orchestrator 伺服器一律會同時包含事件記錄檔，以及已安裝 proxy 服務的目的地伺服器包含 proxy 記錄。 這些記錄檔位於：

- 應用程式和服務記錄檔 \ Microsoft \ Windows \ StorageMigrationService
- 應用程式和服務記錄檔 \ Microsoft \ Windows \ StorageMigrationService-Proxy

如果您需要收集這些記錄以進行離線查看或傳送至 Microsoft 支援服務，GitHub 上有開放原始碼 PowerShell 腳本可供使用：

 [儲存體遷移服務協助程式](https://aka.ms/smslogs) 

請參閱讀我檔案以取得使用方式。

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>除非管理 Windows Server 2019，否則存放裝置遷移服務不會顯示在 Windows 系統管理中心

使用1809版的 Windows 管理中心來管理 Windows Server 2019 orchestrator 時，您看不到儲存體遷移服務的工具選項。 

Windows 系統管理中心儲存體遷移服務延伸模組的版本系結只會管理 Windows Server 2019 1809 版或更新版本的作業系統。 如果您使用它來管理舊版的 Windows Server 作業系統或 insider preview，則不會顯示此工具。 此行為是設計使然。 

若要解決此問題，請使用或升級至 Windows Server 2019 build 1809 或更新版本。

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>儲存體遷移服務轉換驗證失敗，錯誤為「目的地電腦上的權杖篩選原則拒絕存取」

執行轉換驗證時，您會收到「失敗：存取目的地電腦上的權杖篩選原則」錯誤。 即使您為來源和目的電腦提供正確的本機系統管理員認證，也會發生這種情況。

此問題已在[KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)更新中修正。 

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-or-windows-server-2019-essentials-edition"></a>儲存體遷移服務未包含在 Windows Server 2019 評估或 Windows Server 2019 Essentials 版本中

使用 Windows 系統管理中心連線到[Windows server 2019 評估版](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)或 windows Server 2019 Essentials edition 時，不會有管理儲存體遷移服務的選項。 儲存體遷移服務也不包含在角色和功能中。

此問題是由 Windows Server 2019 和 Windows Server 2019 Essentials 評估媒體中的服務問題所造成。 

若要解決此問題以進行評估，請安裝零售、MSDN、OEM 或大量授權版本的 Windows Server 2019，而不要啟動它。 如果沒有啟用，所有版本的 Windows Server 都會在評估模式下運作達180天。 

我們已在較新版本的 Windows Server 中修正此問題。  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>儲存體遷移服務超時下載傳輸錯誤 CSV

使用 Windows Admin Center 或 PowerShell 下載傳輸作業詳細的錯誤-僅限 CSV 記錄檔時，您會收到錯誤：

 >   傳輸記錄檔-請檢查防火牆中允許的檔案共用。 ：傳送到 net.tcp：//localhost： 28940/sms/service/1/transfer 的此要求作業未在設定的超時時間內收到回復（00:01:00）。 分配給此作業的時間，可能是較長逾時的一部分。 這可能是因為服務仍然在處理作業，或服務無法傳送回覆訊息。 請考慮增加作業超時（藉由將通道/proxy 轉換成 IcoNtextchannel.localaddress 並設定 OperationTimeout 屬性），並確定服務能夠連接到用戶端。

此問題是因為儲存體遷移服務所允許的預設一分鐘時間內，無法篩選出的傳輸檔案數量非常大。 

若要解決這個問題：

1. 在 orchestrator 電腦上，使用 Notepad.exe 編輯 *%SYSTEMROOT%\SMS\Microsoft.StorageMigration.Service.exe.config*檔案，將 "sendTimeout" 從其1分鐘的預設值變更為10分鐘

   ```
     <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:01:00"
   ```

2. 重新開機 orchestrator 電腦上的「儲存體遷移服務」服務。 
3. 在 orchestrator 電腦上，啟動 Regedit.exe
4. 找到並按一下以下的登錄子機碼： 

   `HKEY_LOCAL_MACHINE\Software\Microsoft\SMSPowershell`

5. 在 [編輯] 功能表中指向 [新增]，然後按一下 [DWORD 值]。 
6. 輸入 "WcfOperationTimeoutInMinutes" 作為 DWORD 的名稱，然後按 ENTER。
7. 在 [WcfOperationTimeoutInMinutes] 上按一下滑鼠右鍵，然後按一下 [修改]。 
8. 在 [基本資料] 方塊中，按一下 [十進位]
9. 在 [數值資料] 方塊中，輸入 "10"，然後按一下 [確定]。
10. 結束 [登錄編輯程式]。
11. 嘗試再次下載僅限錯誤的 CSV 檔案。 

我們想要在較新版本的 Windows Server 2019 中變更此行為。  

## <a name="validation-warnings-for-destination-proxy-and-credential-administrative-privileges"></a>目的地 proxy 和認證系統管理許可權的驗證警告

驗證傳送作業時，您會看到下列警告：

 > **認證具有系統管理許可權。**
 > 警告：無法從遠端使用動作。
 > **目的地 proxy 已註冊。**
 > 警告：找不到目的地 proxy。

如果您尚未在 Windows Server 2019 目的地電腦上安裝儲存體遷移服務 Proxy 服務，或目的地電腦是 Windows Server 2016 或 Windows Server 2012 R2，則此行為是依設計而定。 我們建議您遷移至已安裝 proxy 的 Windows Server 2019 電腦，以大幅改善傳輸效能。  

## <a name="certain-files-do-not-inventory-or-transfer-error-5-access-is-denied"></a>某些檔案不會進行清查或傳輸，錯誤5「拒絕存取」

從來源清查或傳輸檔案到目的地電腦時，使用者移除系統管理員群組許可權的檔案無法遷移。 檢查儲存體遷移服務-Proxy 調試顯示：

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          2/26/2019 9:00:04 AM
    Event ID:      10000
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      srv1.contoso.com
    Description:

    02/26/2019-09:00:04.860 [Error] Transfer error for \\srv1.contoso.com\public\indy.png: (5) Access is denied.
    Stack Trace:
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.OpenFile(String fileName, DesiredAccess desiredAccess, ShareMode shareMode, CreationDisposition creationDisposition, FlagsAndAttributes flagsAndAttributes)
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile(String path)
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile(FileInfo file)
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.InitializeSourceFileInfo()
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.Transfer()
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.TryTransfer()   

此問題是由儲存體遷移服務中的程式碼缺失所造成，其中不會叫用備份許可權。 

若要解決此問題，請在[KB4490481 （OS 組建17763.404）](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481)在 orchestrator 電腦和目的地電腦（如果已安裝 proxy 服務）上安裝 Windows Update。 請確定來源遷移使用者帳戶是來源電腦上的本機系統管理員，以及儲存體遷移服務協調器。 請確定目的地遷移使用者帳戶是目的地電腦上的本機系統管理員，以及儲存體遷移服務協調器。 

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>使用儲存體遷移服務預置資料時，DFSR 雜湊不相符

當使用儲存體遷移服務將檔案傳輸到新的目的地，然後將 DFS 複寫（DFSR）設定為透過 preseeded 複寫或 DFSR 資料庫複製來複寫該資料與現有的 DFSR 伺服器時，所有檔案都會遇到雜湊不相符，而且會重新複寫。 在使用 SMS 傳送資料流程、安全性資料流程、大小和屬性之後，這些資料流程會完全相符。 使用 ICACLS 或 DFSR 資料庫複製 debug 記錄來檢查檔案會顯示：

來源檔案：

  icacls d:\test\Source：

  icacls d:\test\thatcher.png/save out .txt/t thatcher .png D:AI （A;;FA;;;BA）（A;; 0x1200a9;;;DD）（A;; 0x1301bf;;;DU）（A; ID; FA;;;BA）（A; ID; FA;;;SY）（A; ID; 0x1200a9;;;BU

目的地檔案：

  icacls d:\test\thatcher.png/save out .txt/t thatcher .png D:AI （A;;FA;;;BA）（A;; 0x1301bf;;;DU）（A;; 0x1200a9;;;DD）（A; ID; FA;;;BA）（A; ID; FA;;;SY）（A; ID; 0x1200a9;;;BU）**S： PAINO_ACCESS_CONTROL**

DFSR Debug 記錄檔：

    20190308 10:18:53.116 3948 DBCL  4045 [WARN] DBClone::IDTableImportUpdate Mismatch record was found. 

    Local ACL hash:1BCDFE03-A18BCE01-D1AE9859-23A0A5F6 
    LastWriteTime:20190308 18:09:44.876 
    FileSizeLow:1131654 
    FileSizeHigh:0 
    Attributes:32 

    Clone ACL hash:**DDC4FCE4-DDF329C4-977CED6D-F4D72A5B** 
    LastWriteTime:20190308 18:09:44.876 
    FileSizeLow:1131654 
    FileSizeHigh:0 
    Attributes:32 

[KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)更新已修正此問題

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transferring-from-windows-server-2008-r2"></a>從 Windows Server 2008 R2 傳輸時，發生錯誤「無法在任何端點上轉移存放裝置」

嘗試從 Windows Server 2008 R2 來源電腦傳輸資料時，不會傳輸任何資料，而且您會收到錯誤訊息：  

    Couldn't transfer storage on any of the endpoints.
    0x9044

如果您的 Windows Server 2008 R2 電腦未使用 Windows Update 的所有重大和重要更新進行完整修補，則預期會發生此錯誤。 無論儲存體遷移服務為何，基於安全考慮，我們一律建議修補 Windows Server 2008 R2 電腦，因為該作業系統並未包含較新版本 Windows Server 的安全性改進。

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-and-check-if-the-source-device-is-online---we-couldnt-access-it"></a>「無法在任何端點上轉移儲存體」和「檢查來源裝置是否在線上-我們無法存取它」錯誤。

嘗試從來源電腦傳送資料時，部分或所有共用不會傳輸，摘要錯誤如下：

    Couldn't transfer storage on any of the endpoints.
    0x9044

檢查 SMB 傳輸詳細資料會顯示錯誤：

    Check if the source device is online - we couldn't access it.

檢查 StorageMigrationService/Admin 事件記錄檔會顯示：

    Couldn't transfer storage.

    Job: Job1
    ID:  
    State: Failed
    Error: 36931
    Error Message: 

   指引：檢查詳細的錯誤，並確定已符合傳輸需求。 傳送作業無法傳輸任何來源和目的地電腦。 這可能是因為協調器電腦無法連線到任何來源或目的地電腦，可能是因為防火牆規則或遺失許可權所致。

檢查 StorageMigrationService-Proxy/Debug 記錄檔會顯示：

    07/02/2019-13:35:57.231 [Error] Transfer validation failed. ErrorCode: 40961, Source endpoint is not reachable, or doesn't exist, or source credentials are invalid, or authenticated user doesn't have sufficient permissions to access it.
    at Microsoft.StorageMigration.Proxy.Service.Transfer.TransferOperation.Validate()
    at Microsoft.StorageMigration.Proxy.Service.Transfer.TransferRequestHandler.ProcessRequest(FileTransferRequest fileTransferRequest, Guid operationId)    

這是一個程式碼缺失，如果您的遷移帳戶至少沒有 SMB 共用的讀取權限，就會產生資訊清單。 此問題先在累計更新[4520062](https://support.microsoft.com/help/4520062/windows-10-update-kb4520062)中修正。 

## <a name="error-0x80005000-when-running-inventory"></a>執行清查時發生錯誤0x80005000

安裝[KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)並嘗試執行清查之後，清查會失敗，並出現錯誤：

    EXCEPTION FROM HRESULT: 0x80005000
  
    Log Name:      Microsoft-Windows-StorageMigrationService/Admin
    Source:        Microsoft-Windows-StorageMigrationService
    Date:          9/9/2019 5:21:42 PM
    Event ID:      2503
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      FS02.TailwindTraders.net
    Description:
    Couldn't inventory the computers.
    Job: foo2
    ID: 20ac3f75-4945-41d1-9a79-d11dbb57798b
    State: Failed
    Error: 36934
    Error Message: Inventory failed for all devices
    Guidance: Check the detailed error and make sure the inventory requirements are met. The job couldn't inventory any of the specified source computers. This could be because the orchestrator computer couldn't reach it over the network, possibly due to a firewall rule or missing permissions.
  
    Log Name:      Microsoft-Windows-StorageMigrationService/Admin
    Source:        Microsoft-Windows-StorageMigrationService
    Date:          9/9/2019 5:21:42 PM
    Event ID:      2509
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      FS02.TailwindTraders.net
    Description:
    Couldn't inventory a computer.
    Job: foo2
    Computer: FS01.TailwindTraders.net
    State: Failed
    Error: -2147463168
    Error Message: 
    Guidance: Check the detailed error and make sure the inventory requirements are met. The inventory couldn't determine any aspects of the specified source computer. This could be because of missing permissions or privileges on the source or a blocked firewall port.
  
    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          2/14/2020 1:18:21 PM
    Event ID:      10000
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      2019-rtm-orc.ned.contoso.com
    Description:
    02/14/2020-13:18:21.097 [Erro] Failed device discovery stage SystemInfo with error: (0x80005000) Unknown error (0x80005000)   
  
當您以使用者主體名稱（UPN）的形式提供遷移認證（例如 'meghan@contoso.com'）時，儲存體遷移服務中的程式碼缺失會導致此錯誤。 儲存體遷移服務協調器服務無法正確剖析此格式，這會導致在 KB4512534 和19H1 中針對叢集遷移支援新增的網域查閱發生失敗。

若要解決此問題，請以 domain\user 格式提供認證，例如 ' Contoso\Meghan '。

## <a name="error-serviceerror0x9006-or-the-proxy-isnt-currently-available-when-migrating-to-a-windows-server-failover-cluster"></a>錯誤「ServiceError0x9006」或「目前無法使用 proxy」。 遷移至 Windows Server 容錯移轉叢集時

嘗試傳輸叢集檔案伺服器的資料時，您會收到如下的錯誤： 

    Make sure the proxy service is installed and running, and then try again. The proxy isn't currently available.
    0x9006
    ServiceError0x9006,Microsoft.StorageMigration.Commands.UnregisterSmsProxyCommand

如果檔案伺服器資源從其原始的 Windows Server 2019 叢集擁有者節點移至新節點，而該節點上未安裝儲存體遷移服務 Proxy 功能，就會發生此錯誤。

因應措施是將目的地檔案伺服器資源移回您第一次設定傳輸配對時所使用的原始擁有者叢集節點。

另一個因應措施：

1. 在叢集中的所有節點上安裝儲存體遷移服務 Proxy 功能。
2. 在 orchestrator 電腦上執行下列儲存體遷移服務 PowerShell 命令： 

   ```PowerShell
   Register-SMSProxy -ComputerName *destination server* -Force
   ```
## <a name="error-dll-was-not-found-when-running-inventory-from-a-cluster-node"></a>從叢集節點執行清查時發生「找不到 Dll」錯誤

嘗試使用儲存體遷移服務執行清查，並將目標設為 Windows Server 容錯移轉叢集一般使用檔案伺服器來源時，您會收到下列錯誤：

    DLL not found
    [Error] Failed device discovery stage VolumeInfo with error: (0x80131524) Unable to load DLL 'Microsoft.FailoverClusters.FrameworkSupport.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)   

若要解決此問題，請在執行儲存體遷移服務協調器的伺服器上安裝「容錯移轉叢集管理工具」（RSAT-叢集-管理）。 

## <a name="error-there-are-no-more-endpoints-available-from-the-endpoint-mapper-when-running-inventory-against-a-windows-server-2003-source-computer"></a>針對 Windows Server 2003 來源電腦執行清查時，發生「端點對應程式中沒有其他可用的端點」錯誤

嘗試對 Windows Server 2003 來源電腦使用儲存體遷移服務協調器執行清查時，您會收到下列錯誤：

    There are no more endpoints available from the endpoint mapper  

[KB4537818](https://support.microsoft.com/help/4537818/windows-10-update-kb4537818)更新已解決此問題。

## <a name="uninstalling-a-cumulutative-update-prevents-storage-migration-service-from-starting"></a>卸載 cumulutative 更新可防止儲存體遷移服務啟動

卸載 Windows Server 累積更新可能會導致儲存體遷移服務無法啟動。 若要解決此問題，您可以備份和刪除儲存體遷移服務資料庫：

1.  開啟提升許可權的 cmd 提示字元，其中您是儲存體遷移服務 orchestrator 伺服器上的系統管理員成員，並執行：

     ```
     TAKEOWN /d y /a /r /f c:\ProgramData\Microsoft\StorageMigrationService
     
     MD c:\ProgramData\Microsoft\StorageMigrationService\backup

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService\* /grant Administrators:(GA)

     XCOPY c:\ProgramData\Microsoft\StorageMigrationService\* .\backup\*

     DEL c:\ProgramData\Microsoft\StorageMigrationService\* /q

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService  /GRANT networkservice:F /T /C

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService /GRANT networkservice:(GA) /T /C
     ```
   
2.  啟動儲存體遷移服務服務，這會建立新的資料庫。

## <a name="error-clusctl_resource_netname_repair_vco-failed-against-netname-resource-and-windows-server-2008-r2-cluster-cutover-fails"></a>錯誤「針對網路服務名稱的 CLUSCTL_RESOURCE_NETNAME_REPAIR_VCO 失敗」和 Windows Server 2008 R2 叢集轉換失敗

嘗試在 Windows Server 2008 R2 叢集來源上執行剪下時，切換會停滯在階段「重新命名來源電腦 ...」而且您會收到下列錯誤：

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          10/17/2019 6:44:48 PM
    Event ID:      10000
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      WIN-RNS0D0PMPJH.contoso.com
    Description:
    10/17/2019-18:44:48.727 [Erro] Exception error: 0x1. Message: Control code CLUSCTL_RESOURCE_NETNAME_REPAIR_VCO failed against netName resource 2008r2FS., stackTrace:    at Microsoft.FailoverClusters.Framework.ClusterUtils.NetnameRepairVCO(SafeClusterResourceHandle netNameResourceHandle, String netName)
       at Microsoft.FailoverClusters.Framework.ClusterUtils.RenameFSNetName(SafeClusterHandle ClusterHandle, String clusterName, String FsResourceId, String NetNameResourceId, String newDnsName, CancellationToken ct)
       at Microsoft.StorageMigration.Proxy.Cutover.CutoverUtils.RenameFSNetName(NetworkCredential networkCredential, Boolean isLocal, String clusterName, String fsResourceId, String nnResourceId, String newDnsName, CancellationToken ct)    [d:\os\src\base\dms\proxy\cutover\cutoverproxy\CutoverUtils.cs::RenameFSNetName::1510]

這個問題是由舊版 Windows Server 中遺失的 API 所造成。 目前沒有任何方法可以遷移 Windows Server 2008 和 Windows Server 2003 叢集。 您可以在 Windows Server 2008 R2 叢集上執行清查和傳輸，而不會發生問題，然後手動變更叢集的來源檔案伺服器資源網路名稱和 IP 位址，然後變更目的地叢集網路名稱和 IP，以手動執行切換要與原始來源相符的位址。 

## <a name="cutover-hangs-on-38-mapping-network-interfaces-on-the-source-computer-when-using-dhcp"></a>在來源電腦上的「38% 對應網路介面已停止回應」使用 DHCP 時 

當嘗試在來源電腦上執行剪下時，將來源電腦設定為在一或多個網路介面上使用新的靜態（而非 DHCP） IP 位址時，剪下會停滯在「來源 comnputer 上的「38% 對應網路介面」階段 ...」而且您會在 SMS 事件記錄檔中收到下列錯誤：

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Admin
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          11/13/2019 3:47:06 PM
    Event ID:      20494
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      orc2019-rtm.corp.contoso.com
    Description:
    Couldn't set the IP address on the network adapter.

    Computer: fs12.corp.contoso.com
    Adapter: microsoft hyper-v network adapter
    IP address: 10.0.0.99
    Network mask: 16
    Error: 40970
    Error Message: Unknown error (0xa00a)

    Guidance: Confirm that the Netlogon service on the computer is reachable through RPC and that the credentials provided are correct.

檢查來源電腦顯示原始 IP 位址無法變更。 

如果您在 Windows 系統管理中心的 [設定轉換] 畫面上選取 [使用 DHCP]，則只有在指定新的靜態 IP 位址、子網和閘道時，才會發生此問題。 

[KB4537818](https://support.microsoft.com/help/4537818/windows-10-update-kb4537818)更新已解決此問題。

## <a name="slower-than-expected-re-transfer-performance"></a>比預期的重新傳輸效能慢

完成傳輸之後，如果來源伺服器上的資料同時發生變更，則在傳輸時間執行後續的重新傳輸時，您可能不會看到更大的改善。

傳輸非常大量的檔案和嵌套資料夾時，這是預期的行為。 資料的大小不相關。 我們先對[KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)中的這項行為進行改良，並繼續優化傳輸效能。 若要進一步調整效能，請參閱[優化清查和傳輸效能](https://docs.microsoft.com/windows-server/storage/storage-migration-service/faq#optimizing-inventory-and-transfer-performance)。

## <a name="data-does-not-transfer-user-renamed-when-migrating-to-or-from-a-domain-controller"></a>在網域控制站上進行遷移時，資料不會傳輸、使用者重新命名

開始從或到網域控制站的傳輸之後：

 1. 不會遷移任何資料，也不會在目的地上建立任何共用。
 2. Windows 系統管理中心顯示紅色錯誤符號，沒有錯誤訊息
 3. 一或多個 AD 使用者和網域本機群組已變更其名稱和/或 Windows 前2000的登入屬性
 4. 您會在 SMS orchestrator 上看到事件3509：
 
        Log Name:      Microsoft-Windows-StorageMigrationService/Admin
        Source:        Microsoft-Windows-StorageMigrationService
        Date:          1/10/2020 2:53:48 PM
        Event ID:      3509
        Task Category: None
        Level:         Error
        Keywords:      
        User:          NETWORK SERVICE
        Computer:      orc2019-rtm.corp.contoso.com
        Description:
        Couldn't transfer storage for a computer.

        Job: dctest3
        Computer: dc02-2019.corp.contoso.com
        Destination Computer: dc03-2019.corp.contoso.com
        State: Failed
        Error: 53251
        Error Message: Local accounts migration failed with error System.Exception: -2147467259
           at Microsoft.StorageMigration.Service.DeviceHelper.MigrateSecurity(IDeviceRecord sourceDeviceRecord, IDeviceRecord destinationDeviceRecord, TransferConfiguration config, Guid proxyId, CancellationToken cancelToken)

如果您嘗試使用儲存體遷移服務從網域控制站進行遷移，並使用 [遷移使用者和群組] 選項來重新命名或重複使用帳戶，這是預期的行為。 而不是選取 [不要傳輸使用者和群組]。 [儲存體遷移服務不支援](faq.md)DC 遷移。 因為 DC 不具有真正的本機使用者和群組，所以儲存體遷移服務會將這些安全性主體視為在兩部成員伺服器之間進行遷移時的處理方式，並嘗試依照指示來調整 Acl，因而導致錯誤和已損壞或複製的帳戶。 

如果您已多次執行轉移，請執行下列動作：

 1. 對 DC 使用下列 AD PowerShell 命令，以找出任何已修改的使用者或群組（變更 SearchBase 以符合您的網域 dinstringuished 名稱）： 

    ```PowerShell
    Get-ADObject -Filter 'Description -like "*storage migration service renamed*"' -SearchBase 'DC=<domain>,DC=<TLD>' | ft name,distinguishedname
    ```
   
 2. 針對以原始名稱傳回的任何使用者，編輯其 [使用者登入名稱（Windows 前2000）] 以移除儲存體遷移服務所新增的隨機字元尾碼，讓此失敗者可以登入。
 3. 針對以原始名稱傳回的任何群組，編輯其「組名（預先 Windows 2000）」，以移除儲存體遷移服務所新增的隨機字元尾碼。
 4. 針對任何已停用的使用者或名稱現在包含儲存體遷移服務所新增之後綴的群組，您可以刪除這些帳戶。 您可以確認稍後新增的是使用者帳戶，因為它們只會包含網域使用者群組，而且會有與儲存體遷移服務傳輸開始時間相符的建立日期/時間。
 
 如果您想要使用儲存體遷移服務搭配網域控制站來進行傳輸，請務必在 Windows 系統管理中心的 [傳輸設定] 頁面上，選取 [不要傳輸使用者和群組]。
 
 ## <a name="error-53-failed-to-inventory-all-specified-devices-when-running-inventory"></a>執行清查時，發生錯誤53「無法清查所有指定的裝置」 

嘗試執行清查時，您會收到：

    Failed to inventory all specified devices 
    
    Log Name:      Microsoft-Windows-StorageMigrationService/Admin
    Source:        Microsoft-Windows-StorageMigrationService
    Date:          1/16/2020 8:31:17 AM
    Event ID:      2516
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      ned.corp.contoso.com
    Description:
    Couldn't inventory files on the specified endpoint.
    Job: ned1
    Computer: ned.corp.contoso.com
    Endpoint: hithere
    State: Failed
    File Count: 0
    File Size in KB: 0
    Error: 53
    Error Message: Endpoint scan failed
    Guidance: Check the detailed error and make sure the inventory requirements are met. This could be because of missing permissions on the source computer.

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Debug
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          1/16/2020 8:31:17 AM
    Event ID:      10004
    Task Category: None
    Level:         Critical
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      ned.corp.contoso.com
    Description:
    01/16/2020-08:31:17.031 [Crit] Consumer Task failed with error:The network path was not found.
    . StackTrace=   at Microsoft.Win32.RegistryKey.Win32ErrorStatic(Int32 errorCode, String str)
       at Microsoft.Win32.RegistryKey.OpenRemoteBaseKey(RegistryHive hKey, String machineName, RegistryView view)
       at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetEnvironmentPathFolders(String ServerName, Boolean IsServerLocal)
       at Microsoft.StorageMigration.Proxy.Service.Discovery.ScanUtils.<ScanSMBEndpoint>d__3.MoveNext()
       at Microsoft.StorageMigration.Proxy.EndpointScanOperation.Run()
       at Microsoft.StorageMigration.Proxy.Service.Discovery.EndpointScanRequestHandler.ProcessRequest(EndpointScanRequest scanRequest, Guid operationId)
       at Microsoft.StorageMigration.Proxy.Service.Discovery.EndpointScanRequestHandler.ProcessRequest(Object request)
       at Microsoft.StorageMigration.Proxy.Common.ProducerConsumerManager`3.Consume(CancellationToken token)    
       
    01/16/2020-08:31:10.015 [Erro] Endpoint Scan failed. Error: (53) The network path was not found.
    Stack trace:
       at Microsoft.Win32.RegistryKey.Win32ErrorStatic(Int32 errorCode, String str)
       at Microsoft.Win32.RegistryKey.OpenRemoteBaseKey(RegistryHive hKey, String machineName, RegistryView view)

在這個階段，儲存體遷移服務協調器正在嘗試進行遠端登入讀取以判斷來源電腦設定，但來源伺服器拒絕，指出登錄路徑不存在。 這可能是由以下原因所造成：

 - 來源電腦上沒有執行遠端登入服務。
 - 防火牆不允許從 Orchestrator 對來源伺服器進行遠端登入連線。
 - 來源遷移帳戶沒有連接到來源電腦的遠端登入權利。
 - 來源遷移帳戶在來源電腦的登錄中，沒有 [HKEY_LOCAL_MACHINE \Software\microsoft\windows server\ NT\CurrentVersion] 或 [HKEY_LOCAL_MACHINE \SYSTEM\CurrentControlSet\Services\] 底下的 [讀取] 許可權LanmanServer
 
 ## <a name="cutover-hangs-on-38-mapping-network-interfaces-on-the-source-computer"></a>在來源電腦上的「38% 對應網路介面已停止回應」 

嘗試在來源電腦上執行剪下時，切換會停滯在來源 comnputer 上的「38% 對應網路介面」階段 ...」而且您會在 SMS 事件記錄檔中收到下列錯誤：

    Log Name:      Microsoft-Windows-StorageMigrationService-Proxy/Admin
    Source:        Microsoft-Windows-StorageMigrationService-Proxy
    Date:          1/11/2020 8:51:14 AM
    Event ID:      20505
    Task Category: None
    Level:         Error
    Keywords:      
    User:          NETWORK SERVICE
    Computer:      nedwardo.contosocom
    Description:
    Couldn't establish a CIM session with the computer.

    Computer: 172.16.10.37
    User Name: nedwardo\MsftSmsStorMigratSvc
    Error: 40970
    Error Message: Unknown error (0xa00a)

    Guidance: Confirm that the Netlogon service on the computer is reachable through RPC and that the credentials provided are correct.

此問題是由在來源電腦上設定下列登錄值的群組原則所造成：

 "HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System LocalAccountTokenFilterPolicy = 0"
 
這項設定不是標準群組原則的一部分，而是使用[Microsoft 安全性合規性工具](https://www.microsoft.com/download/details.aspx?id=55319)組所設定的附加元件：
 
 - Windows Server 2012 R2：「電腦建構管理的 Templates\SCM：將雜湊 Mitigations\Apply UAC 限制傳遞至網路登入上的本機帳戶」
 - 孤行伺服器2016：「電腦建構管理的 Templates\MS 安全性 Guide\Apply 在網路登入上對本機帳戶的 UAC 限制」
 
您也可以使用群組原則喜好設定，搭配自訂的登錄設定。 您可以使用 GPRESULT 工具來判斷哪個原則要將此設定套用至來源電腦。

儲存體遷移服務會暫時啟用[LocalAccountTokenFilterPolicy](https://support.microsoft.com/help/951016/description-of-user-account-control-and-remote-restrictions-in-windows)做為剪下程式的一部分，然後在完成時將其移除。 當群組原則套用衝突的群組原則物件（GPO）時，它會覆寫儲存體遷移服務並防止切換。

若要解決此問題，請使用下列其中一個選項：

1. 暫時將來源電腦從套用此衝突 GPO 的 Active Directory OU 中移出。 
2. 暫時停用套用此衝突原則的 GPO。
3. 暫時建立新的 GPO，將此設定設為停用，並套用至來源伺服器的特定 OU，其優先順序高於任何其他 Gpo。

## <a name="see-also"></a>另請參閱

- [儲存體遷移服務總覽](overview.md)
