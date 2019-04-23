---
title: 儲存體移轉服務的已知問題
description: 已知的問題與疑難排解支援儲存體移轉服務，例如如何收集記錄檔的 Microsoft 支援服務。
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 02/27/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: f5fefab2c1b7ba8b62ffd6734217eab9a13ae95e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888869"
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

我們想要的 Windows Server 的未來版本中修正此問題。 請開啟支援案例，透過[Microsoft 支援服務](https://support.microsoft.com)要求建立向下的移植此修正程式。

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

移轉至目的地 VM 時的來源，例如 Azure IaaS 執行個體，與不同的網路中執行完全移轉將會失敗，來源使用靜態 IP 位址時。 

此行為是根據設計，以防止使用者、 應用程式，以及透過 IP 位址連線的指令碼自移轉後的連線問題。 當 IP 位址從舊的來源電腦移動到新的目的地目標時，它將不會符合新的網路子網路資訊和可能是 DNS 和 WINS。

若要解決這個問題，請在相同網路上執行的電腦移轉。 然後將該電腦移至新的網路，並重新指派其 IP 資訊。 比方說，如果移轉至 Azure IaaS，第一次移轉到本機的 VM，然後使用 Azure Migrate 轉移至 Azure 的 VM。  

Windows Server 2019 的未來版本中，我們已修正此問題。 我們現在將可讓您指定並不會變更目的地伺服器的網路設定的移轉。 我們可能會發行更新至現有版本的 Windows Server 2019 一般每月的更新週期的一部分。 


## <a name="see-also"></a>另請參閱

- [儲存體移轉服務概觀](overview.md)