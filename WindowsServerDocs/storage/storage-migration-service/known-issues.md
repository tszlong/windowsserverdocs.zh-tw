---
title: 儲存體遷移服務的已知問題
description: 儲存體遷移服務的已知問題和疑難排解支援，例如如何收集 Microsoft 支援服務的記錄。
author: nedpyle
ms.author: nedpyle
manager: tiaascs
ms.date: 10/23/2020
ms.topic: article
ms.openlocfilehash: 25d0c6666e0706b1c772957d9328db43ecfc5b18
ms.sourcegitcommit: 1b214ca5030c77900f095d77c73cedc6381eb0e4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/27/2020
ms.locfileid: "92639041"
---
# <a name="storage-migration-service-known-issues"></a>儲存體遷移服務的已知問題

本主題包含使用 [儲存體遷移服務](overview.md) 來遷移伺服器時的已知問題答案。

儲存體遷移服務分為兩個部分： Windows Server 中的服務，以及 Windows Admin Center 中的使用者介面。 服務可在 Windows Server、Long-Term 維護通道以及 Windows Server Semi-Annual 通道中使用;雖然 Windows Admin Center 可作為個別下載。 我們也會定期包含 Windows Server 累計更新中的變更，並透過 Windows Update 發行。

例如，Windows Server 1903 版包含適用于儲存體遷移服務的新功能和修正程式，這些功能也適用于 Windows Server 2019 和 Windows Server 1809 版（藉由安裝 [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)）。

## <a name="how-to-collect-log-files-when-working-with-microsoft-support"></a><a name="collecting-logs"></a> 使用 Microsoft 支援服務時如何收集記錄檔

儲存體遷移服務包含 Orchestrator 服務和 Proxy 服務的事件記錄檔。 Orchestrator 伺服器一律會包含事件記錄檔，而安裝 proxy 服務的目的地伺服器則包含 proxy 記錄。 這些記錄位於：

- 應用程式和服務記錄檔 \ Microsoft \ Windows \ StorageMigrationService
- 應用程式和服務記錄檔 \ Microsoft \ Windows \ StorageMigrationService-Proxy

如果您需要收集這些記錄檔以進行離線的查看或傳送至 Microsoft 支援服務，可在 GitHub 上取得開放原始碼 PowerShell 腳本：

 [儲存體遷移服務 Helper](https://aka.ms/smslogs)

請參閱讀我檔案以取得使用方式。

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>儲存體遷移服務不會顯示在 Windows Admin Center 除非管理 Windows Server 2019

使用1809版的 Windows Admin Center 來管理 Windows Server 2019 協調器時，您看不到儲存體遷移服務的工具選項。

Windows Admin Center 儲存體遷移服務延伸模組的版本系結，只管理 Windows Server 2019 1809 版或更新版本的作業系統。 如果您使用它來管理舊版 Windows Server 作業系統或 insider preview，則不會顯示此工具。 這是設計的行為。

若要解決，請使用或升級至 Windows Server 2019 組建1809或更新版本。

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>儲存體遷移服務轉換驗證失敗，並出現「目的地電腦上的權杖篩選原則拒絕存取」錯誤

執行切換驗證時，您會收到錯誤「失敗：目的地電腦上的權杖篩選原則拒絕存取」。 即使您為來源和目的地電腦提供正確的本機系統管理員認證，也會發生這種情況。

此問題已在 [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) 更新中修正。

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-or-windows-server-2019-essentials-edition"></a>儲存體遷移服務未包含在 Windows Server 2019 評估或 Windows Server 2019 Essentials edition 中

使用 Windows Admin Center 連接到 [Windows server 2019 評估版](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) 或 windows Server 2019 Essentials 版本時，並沒有管理儲存體遷移服務的選項。 儲存體遷移服務也不包含在角色和功能中。

此問題是由 Windows Server 2019 和 Windows Server 2019 Essentials 評估媒體中的服務問題所造成。

若要解決此問題以進行評估，請安裝零售、MSDN、OEM 或大量授權版本的 Windows Server 2019，而不要加以啟動。 如果沒有啟用，所有版本的 Windows Server 都會在評估模式下運作180天。

我們已在 Windows Server 的較新版本中修正此問題。

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>儲存體遷移服務時間下載傳輸錯誤 CSV

使用 Windows Admin Center 或 PowerShell 下載傳送作業詳細的錯誤（僅限錯誤） CSV 記錄檔時，您會收到錯誤訊息：

```
Transfer Log - Please check file sharing is allowed in your firewall. : This request operation sent to net.tcp://localhost:28940/sms/service/1/transfer did not receive a reply within the configured timeout (00:01:00). The time allotted to this operation may have been a portion of a longer timeout. This may be because the service is still processing the operation or because the service was unable to send a reply message. Please consider increasing the operation timeout (by casting the channel/proxy to IContextChannel and setting the OperationTimeout property) and ensure that the service is able to connect to the client.
```

發生此問題的原因是，傳輸檔案數量過多，無法在儲存體遷移服務所允許的預設一分鐘時間內進行篩選。

若要解決這個問題：

1. 在 orchestrator 電腦上，使用 Notepad.exe 來編輯 *% SYSTEMROOT% \SMS\Microsoft.StorageMigration.Service.exe.config* 檔案，將 "sendTimeout" 的預設值從1分鐘變更為10分鐘

    ```
    <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:10:00"
    ```

2. 重新開機 orchestrator 電腦上的「儲存體遷移服務」服務。

3. 在 orchestrator 電腦上，啟動 Regedit.exe

4. 建立下列登錄子機碼（如果尚未存在）：

    `HKEY_LOCAL_MACHINE\Software\Microsoft\SMSPowershell`

5. 在 [編輯] 功能表中指向 [新增]，然後按一下 [DWORD 值]。

6. 輸入 "WcfOperationTimeoutInMinutes" 作為 DWORD 的名稱，然後按 ENTER。

7. 在 [WcfOperationTimeoutInMinutes] 上按一下滑鼠右鍵，然後按一下 [修改]。

8. 在 [基本資料] 方塊中，按一下 [Decimal]

9. 在 [數值資料] 方塊中，輸入 "10"，然後按一下 [確定]。

10. 結束 [登錄編輯器]。

11. 嘗試再次下載錯誤-唯一的 CSV 檔案。

如果您要遷移極大量的檔案，您可能需要將此超時時間增加至10分鐘以上。 

## <a name="validation-warnings-for-destination-proxy-and-credential-administrative-privileges"></a>目的地 proxy 和認證系統管理許可權的驗證警告

驗證傳送工作時，您會看到下列警告：

```
The credential has administrative privileges.
Warning: Action isn't available remotely.
The destination proxy is registered.
Warning: The destination proxy wasn't found.
```

如果您尚未在 Windows Server 2019 目的地電腦上安裝儲存體遷移服務 Proxy 服務，或者目的地電腦是 Windows Server 2016 或 Windows Server 2012 R2，則這是設計的行為。 建議您遷移至已安裝 proxy 的 Windows Server 2019 電腦，以大幅改善傳輸效能。

## <a name="certain-files-dont-inventory-or-transfer-error-5-access-is-denied"></a>某些檔案不會進行清查或傳輸，錯誤5「拒絕存取」

從來源清查或傳輸檔案到目的地電腦時，使用者已移除系統管理員群組許可權的檔案將無法遷移。 檢查儲存體遷移 Service-Proxy 的調試顯示：

```
Log Name: Microsoft-Windows-StorageMigrationService-Proxy/Debug
Source: Microsoft-Windows-StorageMigrationService-Proxy
Date: 2/26/2019 9:00:04 AM
Event ID: 10000
Task Category: None
Level: Error
Keywords:
User: NETWORK SERVICE
Computer: srv1.contoso.com
Description:

02/26/2019-09:00:04.860 [Error] Transfer error for \\srv1.contoso.com\public\indy.png: (5) Access is denied.
Stack Trace:
at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.OpenFile(String fileName, DesiredAccess desiredAccess, ShareMode shareMode, CreationDisposition creationDisposition, FlagsAndAttributes flagsAndAttributes)
at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile(String path)
at Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile(FileInfo file)
at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.InitializeSourceFileInfo()
     at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.Transfer()
at Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.TryTransfer()
```

發生此問題的原因是儲存體遷移服務中的程式碼缺失，但未叫用備份許可權。

若要解決此問題，請安裝 [Windows Update 2019 年4月2日： ](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481) 在 orchestrator 電腦和目的地電腦上，如果已安裝 proxy 服務，則 KB4490481 (OS 組建 17763.404) 。 確定來源遷移使用者帳戶是來源電腦上的本機系統管理員和儲存體遷移服務協調器。 確定目的地遷移使用者帳戶是目的地電腦上的本機系統管理員和儲存體遷移服務協調器。

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>使用儲存體遷移服務預置資料時，DFSR 雜湊不相符

使用儲存體遷移服務將檔案傳送至新的目的地，然後將 DFS 複寫設定為透過 preseeded 複寫或 DFS 複寫資料庫複製將該資料複寫到現有的伺服器，所有檔案都會經歷雜湊不相符的情況，並會重新複寫。 資料流程、安全性資料流程、大小和屬性在使用儲存體遷移服務進行傳輸之後，全都看起來完全相符。 使用 ICACLS 或 DFS 複寫資料庫複製調試記錄檔來檢查檔案，會顯示：

### <a name="source-file"></a>來源檔案
```
  icacls d:\test\Source:

  icacls d:\test\thatcher.png /save out.txt /t thatcher.png
  D:AI(A;;FA;;;BA)(A;;0x1200a9;;;DD)(A;;0x1301bf;;;DU)(A;ID;FA;;;BA)(A;ID;FA;;;SY)(A;ID;0x1200a9;;;BU)
```

### <a name="destination-file"></a>目的地檔案

```
  icacls d:\test\thatcher.png /save out.txt /t thatcher.png
  D:AI(A;;FA;;;BA)(A;;0x1301bf;;;DU)(A;;0x1200a9;;;DD)(A;ID;FA;;;BA)(A;ID;FA;;;SY)(A;ID;0x1200a9;;;BU)**S:PAINO_ACCESS_CONTROL**
```
### <a name="dfsr-debug-log"></a>DFSR Debug 記錄檔

```
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
```

[KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534)更新已修正此問題。

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transferring-from-windows-server-2008-r2"></a>從 Windows Server 2008 R2 傳輸時，發生「無法在任何端點上傳送存放裝置」錯誤

嘗試從 Windows Server 2008 R2 來源電腦傳送資料時，不會傳輸任何資料，而且您會收到錯誤：

```
Couldn't transfer storage on any of the endpoints.
0x9044
```

如果您的 Windows Server 2008 R2 電腦未完全以 Windows Update 的重大和重要更新進行修補，則預期會發生此錯誤。 為了安全起見，請務必為 Windows Server 2008 R2 電腦更新，因為該作業系統不包含較新版本 Windows Server 的安全性改良功能。

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-and-check-if-the-source-device-is-online---we-couldnt-access-it"></a>「無法在任何端點上傳送儲存體」和「檢查來源裝置是否在線上-無法存取」錯誤。

嘗試從來源電腦傳送資料時，部分或所有共用都不會傳送，錯誤如下：

```
Couldn't transfer storage on any of the endpoints.
0x9044
```

檢查 SMB 傳輸詳細資料會顯示錯誤：

```
Check if the source device is online - we couldn't access it.
```

檢查 StorageMigrationService/Admin 事件記錄檔會顯示：

```
Couldn't transfer storage.

Job: Job1
ID:
State: Failed
Error: 36931
Error Message:

Guidance: Check the detailed error and make sure the transfer requirements are met. The transfer job couldn't transfer any source and destination computers. This could be because the orchestrator computer couldn't reach any source or destination computers, possibly due to a firewall rule, or missing permissions.
```

檢查 StorageMigrationService-Proxy/Debug 記錄檔會顯示：

```
07/02/2019-13:35:57.231 [Error] Transfer validation failed. ErrorCode: 40961, Source endpoint is not reachable, or doesn't exist, or source credentials are invalid, or authenticated user doesn't have sufficient permissions to access it.
at Microsoft.StorageMigration.Proxy.Service.Transfer.TransferOperation.Validate()
at Microsoft.StorageMigration.Proxy.Service.Transfer.TransferRequestHandler.ProcessRequest(FileTransferRequest fileTransferRequest, Guid operationId)
```

如果您的遷移帳戶沒有 SMB 共用的讀取權限，就會出現程式碼缺失。 此問題是在累積更新 [4520062](https://support.microsoft.com/help/4520062/windows-10-update-kb4520062)中最先修正。

## <a name="error-0x80005000-when-running-inventory"></a>執行清查時發生錯誤0x80005000

安裝 [KB4512534](https://support.microsoft.com/help/4512534/windows-10-update-kb4512534) 並嘗試執行清查之後，清查會失敗，並出現錯誤：

```
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
```

當您以使用者主體名稱的形式提供遷移認證 (UPN) （例如 ' '）時，儲存體遷移服務中的程式碼缺失就會導致此錯誤 meghan@contoso.com 。 儲存體遷移服務協調器服務無法正確剖析此格式，導致在 KB4512534 和19H1 中針對叢集遷移支援新增的網域查閱失敗。

若要解決此問題，請提供 domain\user 格式的認證，例如 ' Contoso\Meghan '。

## <a name="error-serviceerror0x9006-or-the-proxy-isnt-currently-available-when-migrating-to-a-windows-server-failover-cluster"></a>錯誤「ServiceError0x9006」或「目前無法使用 proxy」。 遷移至 Windows Server 容錯移轉叢集時

嘗試針對叢集檔案伺服器傳送資料時，您會收到如下的錯誤：

```
Make sure the proxy service is installed and running, and then try again. The proxy isn't currently available.
0x9006
ServiceError0x9006,Microsoft.StorageMigration.Commands.UnregisterSmsProxyCommand
```

如果檔案伺服器資源從原始的 Windows Server 2019 叢集擁有者節點移至新的節點，且該節點上未安裝儲存體遷移服務 Proxy 功能，則預期會發生此錯誤。

因應措施是，將目的地檔案伺服器資源移回您第一次設定轉移配對時使用的原始擁有者叢集節點。

替代方法：

1. 在叢集中的所有節點上安裝「儲存體遷移服務 Proxy」功能。
2. 在 orchestrator 電腦上執行下列儲存體遷移服務 PowerShell 命令：

   ```PowerShell
   Register-SMSProxy -ComputerName <destination server> -Force
   ```
## <a name="error-dll-was-not-found-when-running-inventory-from-a-cluster-node"></a>從叢集節點執行清查時，發生「找不到 Dll」錯誤

嘗試以儲存體遷移服務執行清查並以 Windows Server 容錯移轉叢集為目標時，請使用檔案伺服器來源，您會收到下列錯誤：

```
DLL not found
[Error] Failed device discovery stage VolumeInfo with error: (0x80131524) Unable to load DLL 'Microsoft.FailoverClusters.FrameworkSupport.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)
```

若要解決此問題，請在執行儲存體遷移服務協調器的伺服器上安裝「容錯移轉叢集管理工具」 (RSAT-叢集-管理) 。

## <a name="error-there-are-no-more-endpoints-available-from-the-endpoint-mapper-when-running-inventory-against-a-windows-server-2003-source-computer"></a>針對 Windows Server 2003 來源電腦執行清查時，出現「端點對應程式中沒有可用的端點」錯誤

嘗試對 Windows Server 2003 來源電腦執行儲存體遷移服務協調器的清查時，您會收到下列錯誤：

```
There are no more endpoints available from the endpoint mapper
```

[KB4537818](https://support.microsoft.com/help/4537818/windows-10-update-kb4537818)更新會解決此問題。

## <a name="uninstalling-a-cumulative-update-prevents-storage-migration-service-from-starting"></a>卸載累積更新會導致儲存體遷移服務無法啟動

卸載 Windows Server 累計更新可能會導致儲存體遷移服務無法啟動。 若要解決此問題，您可以備份和刪除儲存體遷移服務資料庫：

1. 開啟提升許可權的 cmd 提示字元，其中您是儲存體遷移服務協調器伺服器上的系統管理員成員，並執行：

     ```DOS
     TAKEOWN /d y /a /r /f c:\ProgramData\Microsoft\StorageMigrationService

     MD c:\ProgramData\Microsoft\StorageMigrationService\backup

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService\* /grant Administrators:(GA)

     XCOPY c:\ProgramData\Microsoft\StorageMigrationService\* .\backup\*

     DEL c:\ProgramData\Microsoft\StorageMigrationService\* /q

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService  /GRANT networkservice:F /T /C

     ICACLS c:\ProgramData\Microsoft\StorageMigrationService /GRANT networkservice:(GA) /T /C
     ```

2. 啟動儲存體遷移服務服務，這將會建立新的資料庫。

## <a name="error-clusctl_resource_netname_repair_vco-failed-against-netname-resource-and-windows-server-2008-r2-cluster-cutover-fails"></a>「針對網路服務資源 CLUSCTL_RESOURCE_NETNAME_REPAIR_VCO 失敗」錯誤，且 Windows Server 2008 R2 叢集轉換失敗

當您嘗試執行 Windows Server 2008 R2 叢集來源的剪下時，剪下會停滯于「重新命名來源電腦 ...」階段您會收到下列錯誤：

```
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
```

此問題是舊版 Windows Server 中遺失的 API 所造成。 目前沒有任何方法可以遷移 Windows Server 2008 和 Windows Server 2003 叢集。 您可以在 Windows Server 2008 R2 叢集上執行清查和傳輸，而不會發生問題，然後手動變更叢集的來源檔案伺服器資源網路名稱和 IP 位址，然後將目的地叢集的網路名稱和 IP 位址變更為符合原始來源，以手動方式執行切換。

## <a name="cutover-hangs-on-38-mapping-network-interfaces-on-the-source-computer-when-using-static-ips"></a>切換停止回應來源電腦上的「38% 對應網路介面 ...」使用靜態 Ip 時

當您嘗試在來源電腦上執行剪下時，已將來源電腦設定為使用新的靜態 (而非 DHCP) 一個或多個網路介面上的 IP 位址，則會停滯于來源電腦上的「38% 對應網路介面 ...」階段而且您會在儲存體遷移服務事件記錄檔中收到下列錯誤：

```
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
```

檢查來源電腦會顯示原始 IP 位址無法變更。

只有當您指定新的靜態 IP 位址時，才會在 Windows Admin Center 的「設定轉換」畫面上選取 [使用 DHCP]，就不會發生此問題。

此問題有兩個解決方案：

1. 此問題最初是由 [KB4537818](https://support.microsoft.com/help/4537818/windows-10-update-kb4537818) 更新所解決。 先前的程式碼缺失阻礙了靜態 IP 位址的所有使用。

2. 如果您未在來源電腦的網路介面上指定預設閘道 IP 位址，即使是 KB4537818 更新，也會發生此問題。 若要解決此問題，請使用網路連接小程式 ( # A0) 或 Set-NetRoute Powershell Cmdlet，在網路介面上設定有效的預設 IP 位址。

## <a name="slower-than-expected-re-transfer-performance"></a>比預期的重新傳輸效能慢

完成轉移之後，執行後續的相同資料重新傳送，即使來源伺服器上的資料同時變更，您也可能不會看到傳輸時間的大幅改善。 

[Kb4580390](https://support.microsoft.com/help/4580390/windows-10-update-kb4580390)會解決此問題。 若要進一步調整效能，請參閱將 [清查和傳輸效能優化](./faq.md#optimizing-inventory-and-transfer-performance)。

## <a name="slower-than-expected-inventory-performance"></a>比預期的清查效能慢

清查來源伺服器時，您會發現檔案清查在有許多檔案或嵌套資料夾時，會花費很長的時間。 數百萬個檔案和資料夾可能會導致清查花費數小時的時間，即使是快速的儲存體設定。 

[Kb4580390](https://support.microsoft.com/help/4580390/windows-10-update-kb4580390)會解決此問題。

## <a name="data-does-not-transfer-user-renamed-when-migrating-to-or-from-a-domain-controller"></a>資料不會傳輸，使用者在遷移至網域控制站或從網域控制站遷移時重新命名

從或到網域控制站的傳輸開始之後：

 1. 不會遷移任何資料，也不會在目的地建立任何共用。
 2. Windows Admin Center 中顯示紅色錯誤符號，且沒有錯誤訊息
 3. 一或多個 AD 使用者和網域本機群組已變更其名稱和/或 Windows 2000 之前的登入屬性

 4. 您會在儲存體遷移服務協調器上看到事件3509：

    ```
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
    ```

    如果您嘗試使用儲存體遷移服務從網域控制站遷移或移至網域控制站，並使用 [遷移使用者和群組] 選項來重新命名或重複使用帳戶，則這是預期的行為。 而不是選取 [不要傳送使用者和群組]。 [儲存體遷移服務不支援](faq.md)DC 遷移。 因為 DC 沒有真正的本機使用者和群組，所以儲存體遷移服務會將這些安全性主體視為在兩個成員伺服器之間進行遷移，並嘗試依指示調整 Acl，進而導致錯誤和損壞或複製的帳戶。

如果您已經執行轉移一次或多次：

 1. 針對 DC 使用下列 AD PowerShell 命令，以找出任何修改過的使用者或群組 (變更 SearchBase 以符合網域辨別名稱) ：

    ```PowerShell
    Get-ADObject -Filter 'Description -like "*storage migration service renamed*"' -SearchBase 'DC=<domain>,DC=<TLD>' | ft name,distinguishedname
    ```

 2. 若為任何以其原始名稱傳回的使用者，請編輯其「使用者登入名稱 (Windows 2000 前) 」，以移除儲存體遷移服務新增的隨機字元尾碼，讓此使用者可以登入。

 3. 針對以其原始名稱傳回的任何群組，請編輯其「組名 (Windows 2000 之前的) 」，以移除儲存體遷移服務新增的隨機字元尾碼。

 4. 對於任何已停用的使用者或名稱現在包含儲存體遷移服務新增尾碼的群組，您可以刪除這些帳戶。 您可以確認稍後新增使用者帳戶，因為這些帳戶只會包含網域使用者群組，而且會建立符合儲存體遷移服務傳輸開始時間的日期/時間。

    如果您想要使用儲存體遷移服務搭配網域控制站來進行傳輸，請務必在 Windows Admin Center 的 [傳輸設定] 頁面上，選取 [不要傳送使用者和群組]。

## <a name="error-53-failed-to-inventory-all-specified-devices-when-running-inventory"></a>執行清查時，出現錯誤53：「無法清查所有指定的裝置」

當您嘗試執行清查時，您會收到：

```
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
```

在這個階段，儲存體遷移服務協調器正在嘗試遠端登入讀取以判斷來源機器設定，但來源伺服器正在拒絕，指出登錄路徑不存在。 可能的原因如下：

 - 來源電腦上沒有執行遠端登入服務。
 - 防火牆不允許從 Orchestrator 對來源伺服器進行遠端登入連接。
 - 來源遷移帳戶沒有連接到來源電腦的遠端登入權利。
 - 來源遷移帳戶在來源電腦的登錄中，沒有 [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion] 或 [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer] 下的讀取權限

## <a name="cutover-hangs-on-38-mapping-network-interfaces-on-the-source-computer"></a>切換停止回應來源電腦上的「38% 對應網路介面 ...」

當您嘗試在來源電腦上執行剪下時，剪住會停滯于來源電腦上的「38% 對應網路介面 ...」階段而且您會在儲存體遷移服務事件記錄檔中收到下列錯誤：

```
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
```

發生此問題的原因是在來源電腦上設定下列登錄值的群組原則： "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\LocalAccountTokenFilterPolicy = 0"

這項設定不是標準群組原則的一部分，而是使用 [Microsoft 安全性合規性工具](https://www.microsoft.com/download/details.aspx?id=55319)組設定的附加元件：

- Windows Server 2012 R2：「電腦管理 \ Templates\SCM：將 UAC 限制傳遞至網路登入時的本機帳戶」

- 孤行伺服器2016：「電腦的 Templates\MS 安全性 Guide\Apply 安全性將 UAC 限制用於網路登入上的本機帳戶」

您也可以使用群組原則喜好設定來設定自訂登錄設定。 您可以使用 GPRESULT 工具來判斷要將此設定套用到來源電腦的原則。

儲存體遷移服務會暫時啟用 [LocalAccountTokenFilterPolicy](https://support.microsoft.com/help/951016/description-of-user-account-control-and-remote-restrictions-in-windows) 作為剪下流程的一部分，然後在完成時將其移除。 當群組原則將衝突的群組原則物件套用 (GPO) 時，它會覆寫儲存體遷移服務並防止剪下。

若要解決此問題，請使用下列其中一個選項：

1. 暫時將來源電腦從套用此衝突 GPO 的 Active Directory OU 移出。
2. 暫時停用套用此衝突原則的 GPO。
3. 暫時建立新的 GPO，將此設定設為 [停用]，並套用至來源伺服器的特定 OU，優先順序高於其他任何 Gpo。

## <a name="inventory-or-transfer-fail-when-using-credentials-from-a-different-domain"></a>使用來自不同網域的認證時，清查或傳輸失敗

當您在使用與目標伺服器不同的網域中使用遷移認證時，嘗試執行清查或傳送儲存體遷移服務，並以 Windows Server 為目標時，您會收到下列錯誤
```
Exception from HRESULT:0x80131505

The server was unable to process the request due to an internal error

04/28/2020-11:31:01.169 [Error] Failed device discovery stage SystemInfo with error: (0x490) Could not find computer object 'myserver' in Active Directory    [d:\os\src\base\dms\proxy\discovery\discoveryproxy\DeviceDiscoveryOperation.cs::TryStage::1042]
```

進一步檢查記錄，會顯示從或兩個遷移的遷移帳戶和伺服器位於不同的網域中：

```
06/25/2020-10:11:16.543 [Info] Creating new job=NedJob user=**CONTOSO**\ned
[d:\os\src\base\dms\service\StorageMigrationService.IInventory.cs::CreateJob::133]
```

```
GetOsVersion(fileserver75.**corp**.contoso.com)    [d:\os\src\base\dms\proxy\common\proxycommon\CimSessionHelper.cs::GetOsVersion::66] 06/25/2020-10:20:45.368 [Info] Computer 'fileserver75.corp.contoso.com': OS version
```

此問題是因為儲存體遷移服務中的程式碼缺失所造成。 若要解決此問題，請使用來源和目的地電腦所屬的相同網域中的遷移認證。 比方說，如果來源和目的地電腦屬於 "contoso.com" 樹系中的 "corp.contoso.com" 網域，請使用 ' corp\myaccount ' 來執行遷移，而不是 ' contoso\myaccount ' 認證。

## <a name="inventory-fails-with-element-not-found"></a>清查失敗，並出現「找不到元素」

考慮下列案例：

您有一個來源伺服器的 DNS 主機名稱，且 Active Directory 名稱超過15個 unicode 字元，例如 "iamaverylongcomputername"。 根據設計，Windows 不會讓您設定舊的 NetBIOS 名稱，如此一來，當伺服器的名稱會截斷為15個 unicode 寬字元時，就會收到警告， (範例： "iamaverylongcom" ) 。 當您嘗試清查這部電腦時，您會在 Windows Admin Center 和事件記錄檔中收到：

```DOS
"Element not found"
========================

Log Name:      Microsoft-Windows-StorageMigrationService/Admin
Source:        Microsoft-Windows-StorageMigrationService
Date:          4/10/2020 10:49:19 AM
Event ID:      2509
Task Category: None
Level:         Error
Keywords:
User:          NETWORK SERVICE
Computer:      WIN-6PJAG3DHPLF.corp.contoso.com
Description:
Couldn't inventory a computer.

Job: longnametest
Computer: iamaverylongcomputername.corp.contoso.com
State: Failed
Error: 1168
Error Message:

Guidance: Check the detailed error and make sure the inventory requirements are met. The inventory couldn't determine any aspects of the specified source computer. This could be because of missing permissions or privileges on the source or a blocked firewall port.
```

此問題是因為儲存體遷移服務中的程式碼缺失所造成。 唯一的因應措施是將電腦重新命名為具有與 NetBIOS 名稱相同的名稱，然後使用 [NETDOM COMPUTERNAME/add](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc835082(v=ws.11)) 來新增替代的電腦名稱稱，其中包含在啟動清查之前所使用的較長名稱。 儲存體遷移服務支援遷移替代的電腦名稱稱。

## <a name="storage-migration-service-inventory-fails-with-a-parameter-cannot-be-found-that-matches-parameter-name-includedfsn"></a>儲存體遷移服務清查失敗，並出現「找不到符合參數名稱 ' IncludeDFSN ' 的參數」 

使用2009版的 Windows Admin Center 來管理 Windows Server 2019 協調器時，當您嘗試清查來源電腦時，會收到下列錯誤：

```
Remote exception : a parameter cannot be found that matches parameter name 'IncludeDFSN'" 
```

若要解決此問題，請在 Windows Admin Center 中將儲存體遷移服務延伸模組更新為至少版本1.113.0。 更新應該會自動出現在摘要中，並提示您進行安裝。


## <a name="see-also"></a>請參閱

- [儲存體遷移服務總覽](overview.md)
