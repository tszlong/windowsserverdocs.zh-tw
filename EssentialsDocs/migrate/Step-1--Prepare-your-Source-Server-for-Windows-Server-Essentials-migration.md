---
title: 步驟 1：準備好來源伺服器以進行 Windows Server Essentials 移轉
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 244c8a06-04c6-4863-8b52-974786455373
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 214f25d1743cca3693907b7a7a8380fd564d4114
ms.sourcegitcommit: 32f810c5429804c384d788c680afac427976e351
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83203481"
---
# <a name="step-1-prepare-your-source-server-for-windows-server-essentials-migration"></a>步驟 1：準備好來源伺服器以進行 Windows Server Essentials 移轉

> 適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

本節說明如何備份來源伺服器、評估來源伺服器系統健康情況、安裝最新的 Service Pack 和修正程式，以及確認網路設定。

## <a name="to-prepare-for-migration"></a>準備移轉
 完成下列幾個開始步驟，確保來源伺服器上的設定和資料可順利移轉到目的地伺服器。

1.  [備份來源伺服器](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)

2.  [安裝最新的 Service Pack](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)

3.  [刪除 [以服務方式登入] 帳戶設定](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_DeleteSvcAcctSetting)

4.  [評估來源伺服器的健康情況](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_EvaluateHealth)

5.  [建立移轉企業營運應用程式規劃](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MigrateLOB)

###  <a name="back-up-your-source-server"></a><a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>備份您的來源伺服器
 開始移轉程序之前先備份您的來源伺服器。 進行備份可協助您保護資料，以免因移轉期間發生無法復原的錯誤而導致資料遺失。

##### <a name="to-back-up-the-source-server"></a>若要備份來源伺服器

1. 使用下表的其中一個資源引導您執行來源伺服器的完整備份。

2. 確認備份執行成功。 若要測試備份的完整性，從備份隨機選取檔案、將這些檔案還原到其他位置，然後確認還原的檔案與原始檔案相同。

   |Products|資源|
   |---|---|
   |Windows Small Business Server 2003|[備份及還原 Windows Small Business Server 2003](https://msdn.microsoft.com/library/cc875809.aspx)
   |Windows Small Business Server 2008|[備份及還原 Windows Small Business Server 2008 上的資料](https://technet.microsoft.com/library/cc527505\(WS.10\).aspx)
   |Windows Server 2008 Foundation|[備份和復原](https://technet.microsoft.com/library/cc754097\(WS.10\).aspx)
   |Windows Small Business Server 2011 Essentials|[深入了解設定伺服器備份](https://technet.microsoft.com/library/server-backup-support-1.aspx)
   |Windows Small Business Server 2011 Standard|[管理伺服器備份](https://technet.microsoft.com/library/cc527488.aspx)
   |Windows Server Essentials|[在 Windows Server Essentials 中管理備份與還原](https://technet.microsoft.com/library/jj713536.aspx)

###  <a name="install-the-most-recent-service-packs"></a><a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>安裝最新的 service pack
 移轉之前，您必須在來源伺服器上安裝最新的更新和 Service Pack。

###  <a name="delete-the-log-on-as-a-service-account-setting"></a><a name="BKMK_DeleteSvcAcctSetting"></a>刪除 [以服務方式登入] 帳戶設定
 如果您從 Windows Small Business Server 2003 或 Windows Server 2003 移轉，請從群組原則刪除 [以服務方式登入]**** 帳戶設定。

##### <a name="to-delete-the-log-on-as-a-service-account-setting"></a>刪除 [以服務方式登入] 帳戶設定

1.  若要開啟 [群組原則管理]**** 工具，請依序按一下 [開始]****、[控制台]****、[系統管理工具]****，然後按一下 [群組原則管理]****。

2.  以滑鼠右鍵按一下 [預設網域控制站原則]****，然後按一下 [編輯]****。

3.  瀏覽至 [電腦設定 \Windows 設定\安全性設定\本機原則\使用者權限指派]****。

4.  在詳細資料窗格中，連按兩下 [以服務方式登入]****。

5.  清除 [定義這些原則設定]**** 核取方塊。

6.  刪除 \\ \localhost\SYSVOL \\<domainname \> \scripts\ SBS_LOGIN_SCRIPT .bat。

###  <a name="evaluate-the-health-of-the-source-server"></a><a name="BKMK_EvaluateHealth"></a>評估來源伺服器的健全狀況
 請務必在開始移轉前評估來源伺服器的健康情況。 您可以使用下列程序，確保更新為最新的，以產生系統健康情況報告，並執行 Windows Server 解決方案最佳做法分析程式 (BPA)。

#### <a name="download-and-install-critical-and-security-updates"></a>下載及安裝重大和安全性更新
 在來源伺服器上安裝重大和安全性更新，可協助確保在移轉過程中會成功移轉，並協助保護您的網路。

###### <a name="to-check-for-the-latest-updates"></a>檢查最新的更新

1.  從來源伺服器上，按一下 [開始]****、[所有程式]****，然後按一下 [Windows Update]****。

2.  按一下 [檢查更新]****。

3.  如果找到更新，按一下 [安裝更新]****。

#### <a name="run-the-best-practices-analyzer"></a>執行最佳做法分析程式
 在開始移轉程序之前，您可以執行最佳做法分析程式 (BPA) 確認伺服器、網路或網域沒有任何問題。 BPA 會從下列來源收集設定資訊：

-   Active Directory Windows Management Instrumentation (WMI)

-   登錄

-   網際網路資訊服務 (IIS)

###### <a name="to-use-the-bpa-to-analyze-your-source-server"></a>使用 BPA 分析您的來源伺服器

1. 下表提供您 Microsoft 下載中心的連結，您可以從該處為來源伺服器下載並安裝最佳做法分析程式 (BPA)。


   |             如果您的來源伺服器正在執行             |                                                     您可以從取得 BPA 工具                                                      |
   |----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
   |                     Windows SBS 2003                     | [Microsoft Windows Small Business Server 2003 最佳做法分析程式網站](https://www.microsoft.com/download/details.aspx?id=5334) |
   |                     Windows SBS 2008                     | [Microsoft Windows Small Business Server 2008 最佳做法分析程式網站](https://www.microsoft.com/download/details.aspx?id=6231) |
   | Windows SBS 2011 Essentials 或 Windows SBS 2011 Standard |          [Windows Server 解決方案最佳做法分析程式網站](https://www.microsoft.com/download/details.aspx?id=15556)           |
   |     Windows Server Essentials 或 Windows Server 2012     |                                                          伺服器儀表板                                                           |


2. 下載完成後，按一下 [開始]****，指向 [所有程式]****，然後按一下 [SBS 最佳做法分析程式工具]****。

   > [!NOTE]
   >  掃描伺服器前先檢查更新。

3. 在瀏覽窗格中按一下 [開始掃描]****。

    如果您的來源伺服器執行的是 Windows Server Essentials，請執行下列動作：

   1.  請以系統管理員身分登入目的地伺服器，然後開啟儀表板。

   2.  在儀表板上，按一下 [裝置]**** 索引標籤。

   3.  在 [<**伺服器**工作  > **Tasks** ] 窗格中，按一下 [**最佳做法分析**程式]。

4. 在詳細資料窗格中，輸入掃描標籤，然後按一下 [開始掃描]****。 掃描標籤是掃描報告的名稱，例如 **SBS BPA Scan 1Jul2013**。

5. 掃描結束後，按一下 [檢視此最佳做法掃描的報告]****。

   BPA 工具收集伺服器設定的相關資訊之後，它會驗證資訊是否正確，然後向系統管理員呈現一份資訊和依重要性排序問題的清單。 清單中會描述每個問題，並提供建議或可能的解決方案。 有三種可用的報告類型：

|報表類型|說明
|-----------------|-----------------
|清單報告|以一維清單的方式顯示報告。
|樹狀報告|以階層式清單的方式顯示報告。

若要檢視問題的描述和解決方案，在報告中按一下該問題。 並非 BPA 工具回報的所有問題都會影響移轉，但您應盡可能解決這些問題，以確保移轉能順利進行。

####  <a name="synchronize-the-source-server-time-with-an-external-time-source"></a><a name="BKMK_SynchronizeTheSourceServerTimeWithAnExternalTimeSource"></a>同步處理來源伺服器時間與外部時間來源
 來源伺服器上的時間與目的地伺服器上的時間差距不能超過 5 分鐘，而且兩部伺服器上的日期和時區必須相同。 若來源伺服器是在虛擬機器中執行，則主機伺服器的日期、時間與時區必須符合來源伺服器與目的地伺服器的日期、時間與時區。 為協助確保已成功安裝 Windows Server Essentials，您必須將來源伺服器的時間與網際網路上的網路時間通訊協定（NTP）伺服器同步。

###### <a name="to-synchronize-the-source-server-time-with-the-ntp-server"></a>同步來源伺服器和 NTP 伺服器的時間

1.  使用網域系統管理員帳戶和密碼登入來源伺服器。

2.  按一下 [開始]****、[執行]****，在文字方塊中輸入 **cmd**，然後按 ENTER。

3.  在命令提示字元中輸入 w32tm /config /syncfromflags:domhier /reliable:no /update，然後按 ENTER。

4.  在命令提示字元中，輸入 net stop w32time，然後按 ENTER。

5.  在命令提示字元中，輸入 net start w32time，然後按 ENTER。

> [!IMPORTANT]
>  在 Windows Server Essentials 安裝期間，您有機會確認目的地伺服器上的時間，並視需要加以變更。 確定與來源伺服器上所設時間的誤差不超過 5 分鐘。 安裝完成後，目的地伺服器會與 NTP 進行同步。 所有加入網域的電腦 (包含來源伺服器) 都會與目的地伺服器同步，目的地伺服器將成為網域主控站 (PDC) 模擬器主機的角色。

###  <a name="create-a-plan-to-migrate-line-of-business-applications"></a><a name="BKMK_MigrateLOB"></a>建立可遷移企業營運應用程式的計畫
 企業營運 (LOB) 應用程式是指對企業運作而言不可或缺的重要電腦應用程式。 LOB 應用程式包括會計、供應鏈管理與資源規劃應用程式。

 當您計劃移轉 LOB 應用程式時，請務必諮詢 LOB 應用程式提供者，以決定適用於移轉每個應用程式的方式。 您也必須尋找用於在目的地伺服器上安裝 LOB 應用程式的媒體。

> [!NOTE]
>  如果您使用 Windows Small Business Server 2011 Essentials SDK 來開發自訂的系統健康情況或警示增益集，而且您想要繼續使用增益集與 Windows Server Essentials，您也必須更新增益集，並將它部署在目的地伺服器上。


### <a name="create-a-plan-to-migrate-email-hosted-on-windows-sbs-2011-windows-sbs-2008-and-windows-sbs-2003"></a>建立計劃以移轉在 Windows SBS 2011、Windows SBS 2008 以及 Windows SBS 2003 上託管的電子郵件
 在 Windows SBS 2011、Windows SBS 2008 以及 Windows SBS 2003 中，電子郵件是透過 Microsoft Exchange Server 提供。 不過，Windows Server Essentials 並不提供收件匣電子郵件服務。 如果您目前使用執行 Windows SBS 2011、Windows SBS 2008 或 Windows SBS 2003 的伺服器來裝載您公司的電子郵件，則必須遷移至替代的內部部署或託管解決方案。

> [!NOTE]
>  在您更新並備妥來源伺服器以進行移轉之後，建議您在繼續移轉程序之前先建立已更新伺服器的備份。

#### <a name="migrate-email-to-microsoft-office-365"></a>將電子郵件移轉至 Microsoft Office 365
 如果您選擇要使用 Microsoft Office 365 做為您網域的電子郵件解決方案，請依照[使用 Exchange 完全遷移將所有的信箱移轉至雲端](https://help.outlook.com/140/ms.exch.ecp.emailmigrationwizardexchangelearnmore.aspx)中的指導方針，開始將電子郵件移轉至 Office 365。 我們建議您先完成電子郵件遷移，再安裝 Windows Server Essentials。

> [!NOTE]
>  如果您想要將 Windows Server Essentials 與 Office 365 整合，則在來源伺服器上移除內部部署 Exchange Server 的步驟是必要的。 如需如何將 Exchange Server 公用資料夾移轉至 Office 365 的相關資訊，請參閱部落格文章 [Office 365 的 Microsoft Exchange 2013 公用資料夾移轉指令碼](https://blogs.technet.com/b/fmustafa/archive/2013/04/11/microsoft-exchange-2013-public-folders-migration-scripts-for-office-365.aspx)。
>
>  完成安裝之後，您應該執行 [**與 Microsoft Office 365 整合**] 工作，在 Windows Server Essentials 中開啟 Office 365 整合功能。

> [!IMPORTANT]
>  若要允許 Office 365 移轉工具連線到來源伺服器上執行的 Exchange Server，您必須啟用來源伺服器的 RPC over HTTP。 如需如何啟用 RPC over HTTP 的相關資訊，請參閱 [如何在 Small Business Server 2003 (Standard 或 Premium) 中第一次部署 RPC over HTTP](https://technet.microsoft.com/library/bb123622%28EXCHG.65%29.aspx)。 如果您無法在啟用 RPC over HTTP 後順利執行 Office 365 移轉工具，請檢視登錄 HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy 中的 [ValidPorts] **** 設定，確定有列出來源伺服器的完整網域名稱 (FQDN)。 如果沒有列出 FQDN，請使用下列範例手動將它加入：
>
>  remote. *contoso*.com:6001-6002;remote. *contoso*.com:6004 (以您的網域名稱取代 *contoso*)。

#### <a name="migrate-email-to-another-on-premises-exchange-server"></a>將電子郵件移轉到其他內部部署 Exchange Server
 如需如何將電子郵件遷移至另一個內部部署 Exchange Server 的相關資訊，請參閱[整合內部部署 Exchange server 與 Windows Server Essentials](https://technet.microsoft.com/library/jj200172.aspx)。 建議您在安裝 Windows Server Essentials 之後設定新的內部部署 Exchange Server，然後在降級來源伺服器之前完成電子郵件遷移。

> [!NOTE]
>  Exchange Server 不含 Windows Small Business Server POP3 連接器。 當您將電子郵件資料移轉到其他 Exchange Server 後，將無法再使用 POP3 連接器功能。

> [!NOTE]
>  在您更新並備妥來源伺服器以進行移轉之後，您應該在繼續移轉程序之前先建立已更新伺服器的備份。

## <a name="next-steps"></a>後續步驟
 您已準備好要遷移到 Windows Server Essentials 的來源伺服器。  現在請移至[步驟2：將 Windows Server Essentials 安裝為新的複本網域控制站](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md)。

若要查看所有步驟，請參閱[遷移至 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

