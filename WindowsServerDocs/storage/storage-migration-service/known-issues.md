---
title: 儲存體遷移服務的已知問題
description: 儲存體遷移服務的已知問題和疑難排解支援，例如如何收集 Microsoft 支援服務的記錄。
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 10/09/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.openlocfilehash: 0f549310d568142f819e22422d41a72d38b306e2
ms.sourcegitcommit: 8771a9f5b37b685e49e2dd03c107a975bf174683
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2020
ms.locfileid: "76145934"
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
10. 結束 [登錄編輯器]。
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

  記錄名稱： StorageMigrationService-Proxy/Debug 來源： Microsoft-Windows-StorageMigrationService-Proxy 日期： 2/26/2019 9:00:04 AM 事件識別碼：10000工作類別：無層級：錯誤關鍵字：      
  使用者：網路服務電腦： srv1.contoso.com 描述：

  02/26/2019-09：00： 04.860 [Error] \\srv1 的傳輸錯誤。 com\public\indy.png：（5）存取被拒。
堆疊追蹤：在 StorageMigration. FileDirUtils. OpenFile （String fileName，DesiredAccess desiredAccess，ShareMode shareMode，CreationDisposition creationDisposition，FlagsAndAttributes flagsAndAttributes），位於StorageMigration. FileDirUtils. GetTargetFile （String path），網址為： FileDirUtils （GetTargetFile 檔案），網址為. FileInfo. StorageMigration。FileTransfer. InitializeSourceFileInfo （），位於 Microsoft. StorageMigration. Proxy. FileTransfer. transfer. StorageMigration （），位於StorageMigration. FileTransfer. TryTransfer （） [d:\os\src\base\dms\proxy\transfer\transferproxy\FileTransfer.cs：： TryTransfer：： 55]


此問題是由儲存體遷移服務中的程式碼缺失所造成，其中不會叫用備份許可權。 

若要解決此問題，請在[KB4490481 （OS 組建17763.404）](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481)在 orchestrator 電腦和目的地電腦（如果已安裝 proxy 服務）上安裝 Windows Update。 請確定來源遷移使用者帳戶是來源電腦上的本機系統管理員，以及儲存體遷移服務協調器。 請確定目的地遷移使用者帳戶是目的地電腦上的本機系統管理員，以及儲存體遷移服務協調器。 

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>使用儲存體遷移服務預置資料時，DFSR 雜湊不相符

當使用儲存體遷移服務將檔案傳輸到新的目的地，然後設定 DFS 複寫（DFSR）透過 preseeded 複寫或 DFSR 資料庫複製來複寫該資料與現有的 DFSR 伺服器時，所有檔案都會 experiemce 雜湊不相符，而且會重新複寫。 在使用 SMS 傳送資料流程、安全性資料流程、大小和屬性之後，這些資料流程會完全相符。 使用 ICACLS 或 DFSR 資料庫複製 debug 記錄來檢查檔案會顯示：

來源檔案：

  icacls d:\test\Source：

  icacls d:\test\thatcher.png/save out .txt/t thatcher .png D:AI （A;;FA;;;BA）（A;; 0x1200a9;;;DD）（A;; 0x1301bf;;;DU）（A; ID; FA;;;BA）（A; ID; FA;;;SY）（A; ID; 0x1200a9;;;BU

目的地檔案：

  icacls d:\test\thatcher.png/save out .txt/t thatcher .png D:AI （A;;FA;;;BA）（A;; 0x1301bf;;;DU）（A;; 0x1200a9;;;DD）（A; ID; FA;;;BA）（A; ID; FA;;;SY）（A; ID; 0x1200a9;;;BU）**S： PAINO_ACCESS_CONTROL**

DFSR Debug 記錄檔：

  20190308 10：18： 53.116 3948 DBCL 4045 [警告] DBClone：： IDTableImportUpdate 不相符的記錄。 

  本機 ACL 雜湊： 1BCDFE03-A18BCE01-D1AE9859-23A0A5F6 LastWriteTime： 20190308 18：09： 44.876 FileSizeLow： 1131654 FileSizeHigh：0屬性：32 

  複製 ACL 雜湊：**DDC4FCE4-DDF329C4-977CED6D-F4D72A5B** LastWriteTime： 20190308 18：09： 44.876 FileSizeLow： 1131654 FileSizeHigh：0屬性：32 

[KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)更新已修正此問題

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transferring-from-windows-server-2008-r2"></a>從 Windows Server 2008 R2 傳輸時，發生錯誤「無法在任何端點上轉移存放裝置」

嘗試從 Windows Server 2008 R2 來源電腦傳輸資料時，不會傳輸任何資料，而且您會收到錯誤訊息：  

  無法在任何端點上傳輸儲存體。
0x9044

如果您的 Windows Server 2008 R2 電腦未使用 Windows Update 的所有重大和重要更新進行完整修補，則預期會發生此錯誤。 無論儲存體遷移服務為何，基於安全考慮，我們一律建議修補 Windows Server 2008 R2 電腦，因為該作業系統並未包含較新版本 Windows Server 的安全性改進。

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-and-check-if-the-source-device-is-online---we-couldnt-access-it"></a>「無法在任何端點上轉移儲存體」和「檢查來源裝置是否在線上-我們無法存取它」錯誤。

嘗試從來源電腦傳送資料時，部分或所有共用不會傳輸，摘要錯誤如下：

   無法在任何端點上傳輸儲存體。
0x9044

檢查 SMB 傳輸詳細資料會顯示錯誤：

   檢查來源裝置是否在線上-我們無法存取它。

檢查 StorageMigrationService/Admin 事件記錄檔會顯示：

   無法傳輸儲存體。

   作業： Job1 識別碼：  
   狀態：失敗錯誤：36931錯誤訊息： 

   指引：檢查詳細的錯誤，並確定已符合傳輸需求。 傳送作業無法傳輸任何來源和目的地電腦。 這可能是因為協調器電腦無法連線到任何來源或目的地電腦，可能是因為防火牆規則或遺失許可權所致。

檢查 StorageMigrationService-Proxy/Debug 記錄檔會顯示：

   07/02/2019-13：35： 57.231 [Error] 傳輸驗證失敗。 錯誤碼：40961、來源端點無法連線或不存在，或來源認證無效，或驗證的使用者沒有足夠的許可權可以存取它。
在 TransferOperation. StorageMigration （ProcessRequest fileTransferRequest，Guid operationId）上進行 StorageMigration （）的驗證（）（& g.）。   [d:\os\src\base\dms\proxy\transfer\transferproxy\TransferRequestHandler.cs::

這是一個程式碼缺失，如果您的遷移帳戶至少沒有 SMB 共用的讀取權限，就會產生資訊清單。 此問題先在累計更新[4520062](https://support.microsoft.com/help/4520062/windows-10-update-kb4520062)中修正。 

## <a name="error-0x80005000-when-running-inventory"></a>執行清查時發生錯誤0x80005000

安裝[KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)並嘗試執行清查之後，清查會失敗，並出現錯誤：

  來自 HRESULT 的例外狀況：0x80005000
  
  記錄名稱： StorageMigrationService/Admin 來源： Microsoft-Windows-StorageMigrationService Date： 9/9/2019 5:21:42 PM 事件識別碼：2503工作類別：無層級：錯誤關鍵字：      
  使用者：網路服務電腦： FS02。TailwindTraders.net 描述：無法清查電腦。
作業： foo2 識別碼：20ac3f75-4945-41d1-9a79-d11dbb57798b 狀態：失敗錯誤：36934錯誤訊息：所有裝置的清查失敗指引：請檢查詳細錯誤，並確定已符合清查需求。 作業無法清查任何指定的來源電腦。 這可能是因為協調器電腦無法透過網路連線，可能是因為防火牆規則或遺失許可權所致。
  
  記錄名稱： StorageMigrationService/Admin 來源： Microsoft-Windows-StorageMigrationService Date： 9/9/2019 5:21:42 PM 事件識別碼：2509工作類別：無層級：錯誤關鍵字：      
  使用者：網路服務電腦： FS02。TailwindTraders.net 描述：無法清查電腦。
作業： foo2 電腦： FS01。TailwindTraders.net 狀態：失敗錯誤：-2147463168 錯誤訊息：指引：檢查詳細錯誤，並確定符合清查需求。 清查無法判斷指定來源電腦的任何層面。 這可能是因為來源或封鎖的防火牆埠缺少許可權或許可權。
  
當您以使用者主體名稱（UPN）的形式提供遷移認證（例如 'meghan@contoso.com'）時，儲存體遷移服務中的程式碼缺失會導致此錯誤。 儲存體遷移服務協調器服務無法正確剖析此格式，這會導致在 KB4512534 和19H1 中針對叢集遷移支援新增的網域查閱發生失敗。

若要解決此問題，請以 domain\user 格式提供認證，例如 ' Contoso\Meghan '。

## <a name="error-serviceerror0x9006-or-the-proxy-isnt-currently-available-when-migrating-to-a-windows-server-failover-cluster"></a>錯誤「ServiceError0x9006」或「目前無法使用 proxy」。 遷移至 Windows Server 容錯移轉叢集時

嘗試傳輸叢集檔案伺服器的資料時，您會收到如下的錯誤： 

   請確定 proxy 服務已安裝且正在執行，然後再試一次。 目前無法使用 proxy。
0x9006 ServiceError0x9006，StorageMigration. 命令. UnregisterSmsProxyCommand

如果檔案伺服器資源從其原始的 Windows Server 2019 叢集擁有者節點移至新節點，而該節點上未安裝儲存體遷移服務 Proxy 功能，就會發生此錯誤。

因應措施是將目的地檔案伺服器資源移回您第一次設定傳輸配對時所使用的原始擁有者叢集節點。

另一個因應措施：

1. 在叢集中的所有節點上安裝儲存體遷移服務 Proxy 功能。
2. 在 orchestrator 電腦上執行下列儲存體遷移服務 PowerShell 命令： 

   ```PowerShell
   Register-SMSProxy -ComputerName *destination server* -Force
   ```
## <a name="error-dll-was-not-found-when-running-inventory-from-a-cluster-node"></a>從叢集節點執行清查時發生「找不到 Dll」錯誤

嘗試使用安裝在 Windows Server 2019 容錯移轉叢集節點上的儲存體遷移服務協調器執行清查，並將目標設為 Windows Server 容錯移轉叢集一般使用檔案伺服器來源時，您會收到下列錯誤：

    DLL not found
    [Error] Failed device discovery stage VolumeInfo with error: (0x80131524) Unable to load DLL 'Microsoft.FailoverClusters.FrameworkSupport.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)   

若要解決此問題，請在執行儲存體遷移服務協調器的伺服器上安裝「容錯移轉叢集管理工具」（RSAT-叢集-管理）。 

## <a name="error-there-are-no-more-endpoints-available-from-the-endpoint-mapper-when-running-inventory-against-a-windows-server-2003-source-computer"></a>針對 Windows Server 2003 來源電腦執行清查時，發生「端點對應程式中沒有其他可用的端點」錯誤

當嘗試使用[KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)累計更新或更新版本修補的儲存體遷移服務 orchestrator 伺服器來執行清查時，您會收到下列錯誤：

    There are no more endpoints available from the endpoint mapper  

若要解決此問題，請從儲存體遷移服務協調器電腦暫時卸載 KB4512534 累計更新（以及取代它的任何）。 當遷移完成時，請重新安裝最新的累計更新。  

請注意，在某些情況下，卸載 KB4512534 或其取代更新可能會導致儲存體遷移服務無法再啟動。 若要解決此問題，您可以備份和刪除儲存體遷移服務資料庫：

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

## <a name="cutover-hangs-on-38-mapping-network-interfaces-on-the-source-computer"></a>在來源電腦上的「38% 對應網路介面已停止回應」 

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

此問題是因為[KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)更新中的回歸所造成。 此問題目前有兩個解決方法：

  - 切換之前：在切換前不要設定新的靜態 IP 位址，請選取 [使用 DHCP]，並確定 DHCP 領域涵蓋該子網。 SMS 會將來源電腦設定為在來源電腦介面上使用 DHCP，而將其切換為正常進行。 
  
  - 如果 [剪下] 已停滯，請在確定 DHCP 領域涵蓋該子網後，登入來源電腦，並在其網路介面上啟用 DHCP。 當來源電腦取得 DHCP 提供的 IP 位址時，SMS 會在正常情況下繼續進行切換。
  
在這兩種因應措施中，在完成之後，您就可以視需要在舊的來源電腦上設定靜態 IP 位址，並使用 DHCP 來停止。   

## <a name="slower-than-expected-re-transfer-performance"></a>比預期的重新傳輸效能慢

完成傳輸之後，如果來源伺服器上的資料同時發生變更，則在傳輸時間執行後續的重新傳輸時，您可能不會看到更大的改善。

傳輸非常大量的檔案和嵌套資料夾時，這是預期的行為。 資料的大小不相關。 我們先對[KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)中的這項行為進行改良，並繼續優化傳輸效能。 若要進一步調整效能，請參閱[優化清查和傳輸效能](https://docs.microsoft.com/windows-server/storage/storage-migration-service/faq#optimizing-inventory-and-transfer-performance)。

## <a name="data-does-not-transfer-user-renamed-when-migrating-to-or-from-a-domain-controller"></a>在網域控制站上進行遷移時，資料不會傳輸、使用者重新命名

開始從或到網域控制站的傳輸之後：

 1. 不會遷移任何資料，也不會在目的地上建立任何共用。
 2. Windows 系統管理中心顯示紅色錯誤符號，沒有錯誤訊息
 3. 一或多個 AD 使用者和網域本機群組已變更其名稱和/或 Windows 前2000的登入屬性
 4. 您會在 SMS orchestrator 上看到事件3509：
 
 記錄名稱： StorageMigrationService/Admin 來源： Microsoft-Windows-StorageMigrationService Date： 1/10/2020 2:53:48 PM 事件識別碼：3509工作類別：無層級：錯誤關鍵字：      
 使用者：網路服務電腦： orc2019-rtm.corp.contoso.com 描述：無法傳輸電腦的存放裝置。

 作業： dctest3 電腦： dc02-2019.corp.contoso.com 目的地電腦： dc03-2019.corp.contoso.com 狀態：失敗的錯誤：53251錯誤訊息：本機帳戶遷移失敗，發生錯誤系統。例外狀況：-2147467259 于StorageMigration. DeviceHelper. MigrateSecurity （IDeviceRecord sourceDeviceRecord，IDeviceRecord destinationDeviceRecord，TransferConfiguration config，Guid proxyId，CancellationToken cancelToken）

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

## <a name="see-also"></a>請參閱

- [儲存體遷移服務總覽](overview.md)
