---
title: 準備您的來源伺服器以進行 Windows Server Essentials migration1
description: 瞭解完成的初步步驟，以確保來源伺服器上的設定和資料順利遷移至目的地伺服器。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: f5861ae9-77cb-4d37-b4c5-8f0757213385
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: dc2866525785506d4e6f3238d22ad8444ed0448f
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97810635"
---
# <a name="prepare-your-source-server-for-windows-server-essentials-migration1"></a>準備您的來源伺服器以進行 Windows Server Essentials migration1

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

完成下列幾個開始步驟，確保來源伺服器上的設定和資料可順利移轉到目的地伺服器。

#### <a name="to-prepare-for-migration"></a>準備移轉


1.  [備份來源伺服器](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)

2.  [安裝最新的 Service Pack](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)

3.  [評估來源伺服器的健康情況](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)

4.  [在來源伺服器上執行「移轉準備工具」](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)

5.  [建立移轉企業營運應用程式規劃](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)

###  <a name="back-up-your-source-server"></a><a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a> 備份來源伺服器
 開始移轉程序之前先備份您的來源伺服器。 進行備份可協助您保護資料，以免因移轉期間發生無法復原的錯誤而導致資料遺失。


##### <a name="to-back-up-the-source-server"></a>若要備份來源伺服器

1.  將來源伺服器進行完整備份。 如需備份 Windows Small Business Server 2011 Essentials 的詳細資訊，請參閱[深入了解設定伺服器備份](/previous-versions/windows/it-pro/windows-server-essentials-sbs/ff402413(v=ws.11))。

2.  確認備份執行成功。 若要測試備份的完整性，從備份隨機選取檔案、將這些檔案還原到其他位置，然後確認還原的檔案與原始檔案相同。

###  <a name="install-the-most-recent-service-packs"></a><a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a> 安裝最新的 service pack
 移轉之前，您必須在來源伺服器上安裝最新的更新和 Service Pack。

###  <a name="evaluate-the-health-of-the-source-server"></a><a name="BKMK_UseWindowsBestPracticeAnalyzer"></a> 評估來源伺服器的健康情況
 請務必在開始移轉前評估來源伺服器的健康情況。 您可以使用下列程序，確保更新為最新的，以產生系統健康情況報告，並執行 Windows Server 解決方案最佳做法分析程式 (BPA)。

#### <a name="download-and-install-critical-and-security-updates"></a>下載及安裝重大和安全性更新
 在來源伺服器上安裝重大和安全性更新，可協助確保在移轉過程中會成功移轉，並協助保護您的網路。

###### <a name="to-check-for-the-latest-updates"></a>檢查最新的更新

1.  從來源伺服器上，按一下 [開始]、[所有程式]，然後按一下 [Windows Update]。

2.  按一下 [檢查更新]。

3.  如果找到更新，按一下 [安裝更新]。

#### <a name="check-the-alert-viewer-for-critical-errors"></a>檢查警示檢視器的嚴重錯誤
 您可以在 [儀表板] 上檢查警示檢視器是否有任何重大錯誤。

#### <a name="run-the-windows-server-solutions-best-practices-analyzer"></a>執行 Windows Server 解決方案最佳做法分析程式
 您可以執行 Windows Server 解決方案最佳做法分析程式 (BPA) 來確定您的伺服器、網路或網域沒有問題，然後再開始移轉程序。 BPA 會從下列來源收集設定資訊：

-   Active Directory &reg; Windows Management Instrumentation (WMI) 

-   登錄

-   網際網路資訊服務 (IIS) Metabase

###### <a name="to-use-the-windows-server-solutions-bpa-to-analyze-your-source-server"></a>使用 Windows Server 解決方案 BPA 來分析您的來源伺服器

1. 於 Microsoft 下載中心下載並安裝 [Windows Server 解決方案最佳做法分析程式](https://www.microsoft.com/download/details.aspx?id=15556)。

2. 下載完成後，按一下 [開始]，指向 [所有程式]，然後按一下 [SBS 最佳做法分析程式工具]。

   > [!NOTE]
   >  掃描伺服器前先檢查更新。

3. 在瀏覽窗格中按一下 [開始掃描]。

4. 在詳細資料窗格中，輸入掃描標籤，然後按一下 [開始掃描]。 掃描標籤是掃描報告的名稱，例如 **SBS BPA Scan 1Jul2012**。

5. 掃描結束後，按一下 [檢視此最佳做法掃描的報告]。

   收集有關伺服器設定的資訊之後，Windows Server 解決方案 BPA 會確認資訊正確，然後提供系統管理員一份依重要性排序的資訊與問題清單。 清單中會描述每個問題，並提供建議或可能的解決方案。 有三種可用的報告類型：

|報表類型|描述|
|-----------------|-----------------|
|清單報告|以一維清單的方式顯示報告。|
|樹狀報告|以階層式清單的方式顯示報告。|
|其他報告|顯示執行階段記錄這類報告。|

 若要檢視問題的描述和解決方案，在報告中按一下該問題。 並非 Windows SBS 2011 Essentials BPA 回報的所有問題都會影響移轉，但您應儘可能解決這些問題，以確保移轉能順利進行。

####  <a name="synchronize-the-source-server-time-with-an-external-time-source"></a><a name="BKMK_SynchronizeTheSourceServerTimeWithAnExternalTimeSource"></a> 將來源伺服器時間與外部時間來源同步處理
 來源伺服器上的時間與目的地伺服器上的時間差距不能超過 5 分鐘，而且兩部伺服器上的日期和時區必須相同。 若來源伺服器是在虛擬機器中執行，則主機伺服器的日期、時間與時區必須符合來源伺服器與目的地伺服器的日期、時間與時區。 若要協助確保已成功安裝 Windows Server Essentials，您必須將來源伺服器的時間與網際網路上 (NTP) Server 的網路時間通訊協定進行同步處理。

###### <a name="to-synchronize-the-source-server-time-with-the-ntp-server"></a>同步來源伺服器和 NTP 伺服器的時間

1.  使用網域系統管理員帳戶和密碼登入來源伺服器。

2.  按一下 [開始]、[執行]，在文字方塊中輸入 **cmd**，然後按 ENTER。

3.  在命令提示字元中輸入 w32tm /config /syncfromflags:domhier /reliable:no /update，然後按 ENTER。

4.  在命令提示字元中，輸入 net stop w32time，然後按 ENTER。

5.  在命令提示字元中，輸入 net start w32time，然後按 ENTER。

> [!IMPORTANT]
>  在 Windows Server Essentials 安裝期間，您有機會可以確認目的地伺服器上的時間，並視需要加以變更。 確定與來源伺服器上所設時間的誤差不超過 5 分鐘。 安裝完成後，目的地伺服器會與 NTP 進行同步。 所有加入網域的電腦 (包含來源伺服器) 都會與目的地伺服器同步，目的地伺服器將成為網域主控站 (PDC) 模擬器主機的角色。

###  <a name="run-the-migration-preparation-tool-on-the-source-server"></a><a name="BKMK_MPT"></a> 在來源伺服器上執行遷移準備工具
 若未先在來源伺服器上執行「移轉準備工具」，即無法執行移轉模式的安裝。 此工具的設計是為了準備您的來源伺服器和網域，以遷移至 Windows Server Essentials。

> [!IMPORTANT]
>  在執行「移轉準備工具」之前，先備份您的「來源伺服器」。 「移轉準備工具」對架構所做的所有變更都是無法還原的。 如果在移轉期間發生任何問題，還原系統備份是將來源伺服器還原至執行工具前狀態的唯一方法。

 若要執行「移轉準備工具」，您必須是 Enterprise Admins 群組、Schema Admins 群組與 Domain Admins 群組的成員。

##### <a name="to-verify-that-you-have-the-appropriate-permissions-to-run-the-tool-on-the-source-server"></a>若要確認您有適當的權限可以在來源伺服器上執行此工具

1. 在來源伺服器上，依序按一下 **[開始]** 及 **[系統管理工具]**，然後按一下 **[Active Directory 使用者和電腦]**。

2. 在主控台樹狀目錄中，按一下以展開您的網域，然後按一下 [使用者]。

3. 在您要用來進行移轉的系統管理員帳戶上按一下滑鼠右鍵，然後按一下 **[內容]**。

4. 按一下 **[成員隸屬]** 索引標籤，並確認 Enterprise Admins、Schema Admins 與 Domain Admins 已列於 **[成員隸屬]** 文字方塊中。

5. 若未列出任何群組，請按一下 **[新增]**，然後新增未列出的每個群組。

   > [!NOTE]
   > - 如果 Netlogon 服務未啟動，您可能會收到權限錯誤。
   >   -   您必須登出再登入伺服器，您所做的變更才會生效。

    您可以使用最新版本的 Windows Update 代理程式，以確保伺服器更新程序運作正常。

   您可以使用最新版本的 Windows Update 代理程式，以確保伺服器更新程序運作正常。

   在來源伺服器上安裝 Windows Update 代理程式之前，您必須先安裝 Windows PowerShell 2.0 和 Microsoft Baseline Configuration Analyzer 2.0。

-   若要下載並安裝 PowerShell 2.0，請參閱 Microsoft 知識庫中的[文章 968929](https://go.microsoft.com/fwlink/p/?LinkId=241483)。

-   若要下載並安裝 Microsoft Baseline Configuration Analyzer 2.0，請參閱 Microsoft 下載中心的 [Microsoft Baseline Configuration Analyzer 2.0](https://go.microsoft.com/fwlink/p/?LinkId=241484)。

-   若要下載並安裝最新版的 Windows Update 代理程式，請參閱 Microsoft 知識庫中的[文章 949104](https://go.microsoft.com/fwlink/p/?LinkId=237493)。

##### <a name="to-install-and-run-the-migration-preparation-tool-on-the-source-server"></a>在來源伺服器上安裝並執行「移轉準備工具」

1. 將 Windows Server Essentials DVD1 插入來源伺服器上的 DVD 光碟機。

2. 開啟 [Windows 檔案總管]，瀏覽至 DVD 的 **\support\tools** 資料夾，然後按兩下 **sourcetool.msi** 檔案。

   > [!NOTE]
   > - 如果「移轉準備工具」已安裝在伺服器上，請從 [開始] 功能表執行該工具。
   >   -   若要確保備妥最佳的可能移轉體驗，建議您永遠選擇安裝最新的更新。

    精靈將在來源伺服器上安裝「移轉準備工具」。 安裝完成後，「移轉準備工具」會自動執行並安裝最新的更新。

3. 在 [移轉準備工具] 中，選取 [我擁有一個備份，而且已準備好繼續進行]，然後按一下 [下一步]。

   > [!WARNING]
   >  如果您收到與修正程式安裝相關的錯誤訊息，請參閱 Microsoft 知識庫 [文章 822798](https://go.microsoft.com/FWLink/p/?LinkID=118672) 中的方法2：重新命名 Catroot2 資料夾。

    「移轉準備工具」會透過擴充 Active Directory 架構來準備移轉用的來源網域。 工作完成後，請按 **[下一步]** 繼續進行。

4. 備妥來源網域之後，「移轉準備工具」會掃描來源伺服器，以辨識出兩種類型的可能問題。

   - **錯誤** 在來源伺服器上發現可封鎖遷移或導致遷移失敗的問題。 遵循錯誤訊息中的指示修正問題，然後按一下 **[再掃描一次]**。

   - **警告** 在來源伺服器上發現的問題，可能會在遷移期間造成功能性問題。 強烈建議您先遵循錯誤訊息中的指示修正問題，再繼續進行移轉。

     在修正或瞭解所有問題之後，按 **[下一步]**。

5. 在 [移轉準備工具] 中，按一下 **[下一步完成]**。

6. 當「遷移準備工具」完成時，系統可能會提示您重新開機來源伺服器，然後才能開始遷移到 Windows Server Essentials。

> [!NOTE]
>  您必須在目的地伺服器上安裝 Windows Server Essentials 的兩周內，于來源伺服器上成功完成執行遷移準備工具。 否則，將會封鎖目的地伺服器上的 Windows Server Essentials 安裝。 若發生此狀況，您必須在來源伺服器上再次執行「移轉準備工具」。

###  <a name="create-a-plan-to-migrate-line-of-business-applications"></a><a name="BKMK_PlanToMigrateLineOfBusinessApplications"></a> 建立計畫以遷移企業營運應用程式
 企業營運 (LOB) 應用程式是指對企業運作而言不可或缺的重要電腦應用程式。 LOB 應用程式包括會計、供應鏈管理與資源規劃應用程式。

 當您計劃移轉 LOB 應用程式時，請務必諮詢 LOB 應用程式提供者，以決定適用於移轉每個應用程式的方式。 您也必須尋找用於在目的地伺服器上安裝 LOB 應用程式的媒體。

> [!NOTE]
>  如果您使用 Windows Small Business Server 2011 Essentials SDK 來開發自訂的系統健康情況或警示增益集，而您想要繼續使用增益集與 Windows Server Essentials，您也必須更新增益集，並將它部署在目的地伺服器上。