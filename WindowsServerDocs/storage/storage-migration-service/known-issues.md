---
title: 儲存體移轉服務的已知問題
description: 已知的問題與疑難排解支援儲存體移轉服務，例如如何收集記錄檔的 Microsoft 支援服務。
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 07/09/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 08156a09491d66016b5fcfe6056ed318d682b987
ms.sourcegitcommit: 514d659c3bcbdd60d1e66d3964ede87b85d79ca9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2019
ms.locfileid: "67735165"
---
# <a name="storage-migration-service-known-issues"></a>儲存體移轉服務的已知問題

本主題使用時，包含已知問題的解答[儲存體移轉服務](overview.md)移轉伺服器。

## <a name="collecting-logs"></a> 如何使用 Microsoft 支援服務時，收集記錄檔

儲存體移轉服務包含在 Orchestrator 服務和 Proxy 服務的事件記錄檔。 Urchestrator 伺服器一定會包含這兩個事件記錄檔中，且目的地伺服器已安裝的 proxy 服務包含 proxy 記錄檔。 這些記錄檔位於：

- Application and Services Logs \ Microsoft \ Windows \ StorageMigrationService
- Application and Services Logs \ Microsoft \ Windows \ StorageMigrationService Proxy

如果您需要收集這些記錄檔進行離線檢視，或傳送給 Microsoft 支援服務，還有一個開放原始碼可在 GitHub 上的 PowerShell 指令碼：

 [儲存體移轉服務的協助程式](https://aka.ms/smslogs) 

檢閱使用方式的讀我檔案。

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>儲存體移轉服務不會顯示在 Windows Admin Center 除非管理 Windows Server 2019

當使用 1809年版本的 Windows Admin Center 來管理 Windows Server 2019 orchestrator，您看存放裝置移轉服務的 [工具] 的選項。 

Windows Admin Center 存放裝置移轉服務延伸模組是版本繫結至只能管理 Windows Server 2019 版本 1809年或更新版本的作業系統。 如果您可以使用它來管理舊版 Windows Server 作業系統或測試人員預覽，此工具不會出現。 此行為是設計使然。 

若要解決，請使用，或升級至 Windows Server 2019 1809年或更新版本的組建。

## <a name="storage-migration-service-doesnt-let-you-choose-static-ip-on-cutover"></a>儲存體移轉服務不會讓您選擇上轉換的靜態 IP

當使用 0.57 版本的 Windows Admin Center 和您的延伸模組連線到 「 轉換 」 階段的移轉儲存體服務，您無法選取靜態 IP 位址。 您會強制使用 DHCP。

若要解決此問題，Windows Admin Center，在查看下**設定** > **延伸模組**警示指出有新版的儲存體移轉服務 0.57.2 是可供安裝。 您可能需要重新啟動您的瀏覽器索引標籤，Windows 系統管理中心。

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>存放裝置移轉服務完全移轉驗證失敗，發生錯誤 「 目的地電腦上的權杖篩選器原則拒絕存取 」

當執行完全移轉的驗證，您會收到錯誤 「 失敗：拒絕存取目的地電腦上的權杖篩選器原則。 」 會發生這種情況是即使您在來源和目的地電腦提供正確的本機系統管理員認證。

這個問題被因為在 Windows Server 2019 的程式碼缺失。 當您使用在目的地電腦作為存放裝置移轉服務協調器，會發生此問題。

若要解決此問題，不是預期的移轉目的地，在 Windows Server 2019 電腦上安裝儲存體移轉服務，然後連接到該伺服器與 Windows Admin Center，並執行移轉。

我們已修正此 Windows Server 的未來版本中。 請開啟支援案例，透過[Microsoft 支援服務](https://support.microsoft.com)要求建立向下的移植此修正程式。

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-edition"></a>儲存體移轉服務不會包含於 Windows Server 2019 評估版

當連接到使用 Windows Admin Center [2019年評估 Windows Server 版本](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019)沒有管理儲存體移轉服務的選項。 儲存體移轉服務也不包含在角色和功能。

這個問題是在 Windows Server 2019 的評估媒體服務問題所導致。 

若要解決此問題，請安裝零售版、 MSDN、 OEM 或大量授權版本的 Windows Server 2019 並沒有啟用。 沒有啟動，所有版本的 Windows Server 在評估模式中都操作 180 天。 

Windows Server 2019 的未來版本中，我們已修正此問題。  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>逾時下載傳輸錯誤 CSV 的存放裝置移轉服務

當使用 Windows Admin Center 或 PowerShell 來下載傳送 operations detailed 僅限錯誤 CSV 記錄檔，您會收到錯誤：

 >   傳送記錄檔-請查看 檔案共用允許您在防火牆中。 :這項要求作業傳送至 net.tcp: //localhost: 28940/簡訊/service/1/傳輸未收到回覆，以在設定的逾時內 (00: 01:00)。 分配給此作業的時間可能已是較長的逾時的一部分。 這可能是因為服務仍然在處理作業，或是服務無法傳送回覆訊息。 請考慮增加作業逾時 （透過轉型將通道 /proxy 傳送至 IContextChannel 並設定 OperationTimeout 內容），並確認服務可連線至用戶端。

此問題起因於極大量的傳輸無法在允許的儲存體移轉服務的預設值一分鐘逾時中篩選的檔案。 

若要解決此問題：

1. Orchestrator 電腦上，編輯 *%SYSTEMROOT%\SMS\Microsoft.StorageMigration.Service.exe.config*檔案中，使用 Notepad.exe，若要從其預設的 1 分鐘的 「 sendTimeout"變更為 10 分鐘

   ```
     <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:01:00"
   ```

2. 重新啟動協調器電腦的 「 儲存體移轉服務 」 服務。 
3. Orchestrator 電腦上，啟動 Regedit.exe
4. 找到並按一下以下的登錄子機碼： 

   `HKEY_LOCAL_MACHINE\\Software\\Microsoft\\SMSPowershell`

5. 在 [編輯] 功能表中指向 [新增]，然後按一下 [DWORD 值]。 
6. 輸入"WcfOperationTimeoutInMinutes"的 DWORD，名稱，然後按 ENTER。
7. 「 WcfOperationTimeoutInMinutes"，以滑鼠右鍵按一下，然後按一下 [修改]。 
8. 在基底的資料] 方塊中，按一下 ["Decimal"
9. 在 [值] 資料方塊中，輸入"10"，然後再按一下 [確定]。
10. 結束登錄編輯程式。
11. 嘗試重新下載僅限錯誤 CSV 檔案。 

我們想要變更此行為在未來版本的 Windows Server 2019。  

## <a name="cutover-fails-when-migrating-between-networks"></a>當網路之間進行移轉時，發生完全移轉會失敗的狀況

移轉至目的地電腦在與來源，例如 Azure IaaS 執行個體不同的網路中執行時完全移轉將會失敗，當來源使用靜態 IP 位址。 

此行為是根據設計，以防止使用者、 應用程式，以及透過 IP 位址連線的指令碼自移轉後的連線問題。 當 IP 位址從舊的來源電腦移動到新的目的地目標時，它將不會符合新的網路子網路資訊和可能是 DNS 和 WINS。

若要解決這個問題，請在相同網路上執行的電腦移轉。 然後將該電腦移至新的網路，並重新指派其 IP 資訊。 比方說，如果移轉至 Azure IaaS，第一次移轉到本機的 VM，然後使用 Azure Migrate 轉移至 Azure 的 VM。  

我們已修正此問題，未來版本的 Windows Admin Center 中。 我們現在將可讓您指定並不會變更目的地伺服器的網路設定的移轉。 發行時，將在此列出的更新延伸模組。 

## <a name="validation-warnings-for-destination-proxy-and-credential-administrative-privileges"></a>目的端 proxy 和認證系統管理權限的驗證警告

當驗證的傳輸作業，您會看到下列警告：

 > **認證具有系統管理權限。**
 > 警告：從遠端動作不適用的。
 > **註冊目的地的 proxy。**
 > 警告：找不到目的端 proxy。

如果您尚未安裝儲存體移轉服務 Proxy 服務的 Windows Server 2019 目的地電腦上，或 destinaton 電腦是 Windows Server 2016 或 Windows Server 2012 R2，此行為是預設行為。 建議您移轉到 Windows Server 2019 電腦安裝的大幅改善的傳輸效能的 proxy。  

## <a name="certain-files-do-not-inventory-or-transfer-error-5-access-is-denied"></a>某些檔案不會清查或傳輸、 錯誤 5 「 拒絕存取 」

當清查或將檔案從來源傳輸至目的地電腦時，使用者已移除系統管理員群組的權限的檔案無法移轉。 檢查儲存體移轉服務 Proxy 的偵錯顯示：

  記錄檔名稱：    Microsoft-Windows-StorageMigrationService-Proxy/偵錯來源：      Microsoft-Windows-StorageMigrationService-Proxy Date:        2/26/2019 上午 9:00:04 事件識別碼：    10000 的工作類別：沒有任何層級：       錯誤的關鍵字：      
  使用者：        網路服務的電腦： srv1.contoso.com 描述：

  [錯誤] 02/26/2019-09:00:04.860 傳輸錯誤\\srv1.contoso.com\public\indy.png:（5） 存取遭拒。
堆疊追蹤： 在 Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.OpenFile （字串的檔名、 DesiredAccess desiredAccess、 ShareMode shareMode、 CreationDisposition creationDisposition、 FlagsAndAttributes flagsAndAttributes）在 Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile （FileInfo 檔案） 在 Microsoft.StorageMigration.Proxy.Service.Transfer.FileDirUtils.GetTargetFile （字串路徑）在 [在 Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.Transfer() Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.InitializeSourceFileInfo()Microsoft.StorageMigration.Proxy.Service.Transfer.FileTransfer.TryTransfer() [d:\os\src\base\dms\proxy\transfer\transferproxy\FileTransfer.cs::TryTransfer::55]


這個問題被因為其中備份特殊權限不正在叫用的移轉儲存體服務中的程式碼缺失。 

若要解決此問題，請安裝[Windows Update 2019 年 4 月 2 日 — KB4490481 (OS 組建 17763.404)](https://support.microsoft.com/help/4490481/windows-10-update-kb4490481) orchestrator 電腦和目的地電腦上如果那里安裝 proxy 服務。 請確定來源移轉使用者帳戶是來源電腦和儲存體移轉服務協調器的本機系統管理員。 請確定目的地移轉使用者帳戶是在目的地電腦和儲存體移轉服務協調器上的本機系統管理員。 

## <a name="dfsr-hashes-mismatch-when-using-storage-migration-service-to-preseed-data"></a>使用儲存體移轉服務預置資料時，DFSR 雜湊不符

使用儲存體移轉服務將檔案傳輸到新的目的地，然後設定 DFS 複寫 (DFSR) 複寫該資料與現有的 DFSR 伺服器透過 presseded 複寫或 DFSR 資料庫複製，所有檔案 experiemce 雜湊不相符，會重新複寫。 資料流、 安全性串流、 大小和完全比對之後使用 SMS 來傳送所有出現的屬性。 Examing ICACLS 檔案或 DFSR 資料庫複製偵錯記錄檔會顯示：

原始程式檔：

  icacls d:\test\Source:

  icacls d:\test\thatcher.png /save out.txt /t thatcher.png D:AI(A;;FA;;;BA)(A;;0x1200a9;;;DD)(A;;0x1301bf;;;DU)(A;ID;FA;;;BA)(A;ID;FA;;;SY)(A;ID;0x1200a9;;;BU)

目的地檔案：

  icacls d:\test\thatcher.png /save out.txt /t thatcher.png D:AI(A;;FA;;;BA)(A;;0x1301bf;;;DU)(A;;0x1200a9;;;DD)(A;ID;FA;;;BA)(A;ID;FA;;;SY)(A;ID;0x1200a9;;;BU)**S:PAINO_ACCESS_CONTROL**

DFSR 偵錯記錄檔：

  找不到 20190308 10:18:53.116 3948 DBCL 4045 [警告] DBClone::IDTableImportUpdate 不相符記錄。 

  本機 ACL 雜湊： 1BCDFE03-A18BCE01-D1AE9859-23A0A5F6 LastWriteTime:20190308 18:09:44.876 FileSizeLow:1131654 FileSizeHigh:0屬性： 32 

  複製 ACL 雜湊：**DDC4FCE4 DDF329C4-977CED6D F4D72A5B** LastWriteTime:20190308 18:09:44.876 FileSizeLow:1131654 FileSizeHigh:0屬性： 32 

這個問題被因為儲存體移轉服務用來設定安全性稽核的 Acl (SACL) 文件庫中的程式碼缺失。 SACL 是空白，導致 DFSR 正確識別的雜湊不相符時，會不小心設定非 null 的 SACL。 

若要解決這個問題，請繼續使用如 Robocopy[預先植入的 DFSR 和 DFSR 資料庫複製作業](../dfs-replication/preseed-dfsr-with-robocopy.md)而非儲存體移轉服務。 我們正在調查此問題，並想要解決此問題在 Windows Server 和可能的向前移植 Windows Update 的更新的版本。 

## <a name="error-404-when-downloading-csv-logs"></a>下載 CSV 時的 404 錯誤記錄檔

嘗試將下載在傳送作業結尾的傳輸或錯誤記錄檔時，您會收到錯誤訊息：

  $jobname:傳送記錄檔： ajax 錯誤 404

如果您未啟用 [檔案及印表機共用 (Smb-in)] 上的防火牆規則的 orchestrator 伺服器，預期此錯誤。 下載 Windows Admin Center 檔案需要連線的電腦上的連接埠 TCP/445 (SMB)。  

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-when-transfering-from-windows-server-2008-r2"></a>錯誤 「 無法移轉儲存體上的任何端點 」 時正在傳輸，從 Windows Server 2008 R2

嘗試將從 Windows Server 2008 R2 的來源電腦傳送資料時，沒有資料傳輸，而且您會收到錯誤訊息：  

  無法傳送儲存體的任何端點。
0x9044

如果您的 Windows Server 2008 R2 電腦不已套用所有重大和重要更新，從 Windows Update，預期此錯誤。 無論儲存體移轉服務，仍一律建議修補 Windows Server 2008 R2 的電腦，基於安全考量，因為該作業系統並不包含較新版本的 Windows Server 的安全性改進功能。

## <a name="error-couldnt-transfer-storage-on-any-of-the-endpoints-and-check-if-the-source-device-is-online---we-couldnt-access-it"></a>錯誤 「 無法移轉儲存體上的任何端點 」 和 「 檢查來源裝置已上線-是否無法存取它。 」

嘗試將資料從來源電腦傳送時，部分或所有共用不會傳送，並摘要的錯誤：

   無法傳送儲存體的任何端點。
0x9044

檢查 SMB 傳輸詳細資料會顯示錯誤：

   請檢查來源裝置已上線-是否無法存取它。

檢查 StorageMigrationService/系統管理事件記錄檔會顯示：

   無法移轉儲存體。

   作業：Job1 識別碼：  
   狀態：失敗的錯誤：36931 出現錯誤訊息： 

   指引：請查看詳細的錯誤，並確定符合傳輸需求。 傳送工作無法傳送的任何來源和目的地電腦。 這可能是因為協調器電腦無法連線到任何來源或目的地的電腦，可能是因為防火牆規則，或缺少權限。

檢查 StorageMigrationService Proxy/偵錯記錄檔會顯示：

   [錯誤] 07/02/2019-13:35:57.231 傳輸驗證失敗。 錯誤碼：40961，來源端點不可以連線，或不存在，或來源的認證不正確，或已驗證的使用者沒有足夠的存取權限。
在 Microsoft.StorageMigration.Proxy.Service.Transfer.TransferOperation.Validate() 在 Microsoft.StorageMigration.Proxy.Service.Transfer.TransferRequestHandler.ProcessRequest （FileTransferRequest fileTransferRequest、 Guid operationId）   [d:\os\src\base\dms\proxy\transfer\transferproxy\TransferRequestHandler.cs::

如果您移轉的帳戶並沒有至少預期此錯誤的 SMB 共用的讀取存取權限。 若要解決這個錯誤，請新增安全性群組，其中包含來源電腦上的 SMB 共用來源移轉帳戶，並授與讀取、 變更或完全控制。 在移轉完成之後，您可以移除此群組。 Windows Server 的未來版本可能會改變此行為，所以不再需要來源共用的明確權限。

## <a name="see-also"></a>另請參閱

- [儲存體移轉服務概觀](overview.md)
