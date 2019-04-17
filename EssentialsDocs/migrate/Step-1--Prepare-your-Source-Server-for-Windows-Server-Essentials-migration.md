---
title: "步驟 1： 準備來源 Windows Server Essentials 伺服器移轉"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 244c8a06-04c6-4863-8b52-974786455373
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2efb1badde6d0ca11bc3b7526fdfb377d9f95d3f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="step-1-prepare-your-source-server-for-windows-server-essentials-migration"></a>步驟 1： 準備來源 Windows Server Essentials 伺服器移轉

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

本主題如何備份來源伺服器評估來源伺服器系統健康、 安裝的最新的 service pack 和修正程式，並確認網路設定。  
  
## <a name="to-prepare-for-migration"></a>若要準備移轉  
 完成下列步驟初步以確保您的設定和資料來源伺服器上的成功移轉到目的伺服器。  
  
1.  [備份您的來源伺服器](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [安裝最新的 service pack](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [Delete 上的登入服務 account 設定為](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_DeleteSvcAcctSetting)  
  
4.  [評估來源伺服器的健康狀態](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_EvaluateHealth)  
  
5.  [建立計劃移轉的業務應用程式](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MigrateLOB)  
  
###  <a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>備份您的來源伺服器  
 開始移轉程序之前，先備份您來源的伺服器。 讓備份可協助保護您的資料意外遺失的如果時發生移轉的錯誤處於無法復原。  
  
##### <a name="to-back-up-the-source-server"></a>若要備份來源伺服器  
  
1.  使用下表資源的其中一個引導您在執行來源 Server 的完整備份。  
  
2.  請確認已順利執行備份。 若要測試備份的完整性，從您的備份選取隨機檔案，將它們還原至另一個位置，並確認還原的檔案會原始檔相同。  
  
   |Product|資源|
   |---|---|
   |Windows 小型企業 Server 2003|[備份及還原 Windows 小型企業 Server 2003](https://msdn.microsoft.com/library/cc875809.aspx) 
   |Windows 小型企業 Server 2008|[備份及還原 Windows 小型企業 Server 2008 上的資料](https://technet.microsoft.com/library/cc527505\(WS.10\).aspx)
   |Windows Server 2008 基本知識|[備份與還原](https://technet.microsoft.com/library/cc754097\(WS.10\).aspx)  
   |Windows 小型企業 Server 2011 程式集|[深入了解備份伺服器設定](https://technet.microsoft.com/library/server-backup-support-1.aspx)
   |Windows 小型企業 Server 2011 標準|[管理伺服器備份](https://technet.microsoft.com/library/cc527488.aspx)  
   |Windows Server Essentials|[管理備份及還原 Windows Server Essentials](https://technet.microsoft.com/library/jj713536.aspx)

###  <a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>安裝最新的 service pack  
 您必須安裝最新的更新及 service pack 之前移轉的來源伺服器上。  
  
###  <a name="BKMK_DeleteSvcAcctSetting"></a>Delete 上的登入服務 account 設定為  
 如果您從 Windows 小型企業 Server 2003 或 Windows Server 2003 移轉、 delete**以服務登入**帳號從群組原則設定。  
  
##### <a name="to-delete-the-log-on-as-a-service-account-setting"></a>若要在 delete 登入，服務 account 設定為  
  
1.  打開**群組原則管理**工具，按一下 [ **[開始]**，按一下 [ **[控制台]**，按一下**系統管理工具**，然後按一下 [**群組原則管理**。  
  
2.  以滑鼠右鍵按一下**預設網域控制站原則**，然後按**編輯**。  
  
3.  瀏覽至**電腦 \windows 安全性設定本機原則的權限指派**。  
  
4.  在詳細資料窗格中，按兩下 [**以服務登入**。  
  
5.  清除**定義這些原則設定**核取方塊。  
  
6.  Delete \\\localhost\SYSVOL\\ < domainname\ > \scripts\SBS_LOGIN_SCRIPT.bat。  
  
###  <a name="BKMK_EvaluateHealth"></a>評估來源伺服器的健康狀態  
 請務必評估來源 Server 的健康狀態，才能開始移轉。 使用下列程序，以確認更新是最新的產生系統健康報告，並執行 Windows Server 方案適用的最佳做法分析 (BPA)。  
  
#### <a name="download-and-install-critical-and-security-updates"></a>下載並安裝重要的安全性更新  
 安裝重要與安全性更新可協助確保您的移轉會成功，並協助保護您的網路移轉程序期間來源伺服器上。  
  
###### <a name="to-check-for-the-latest-updates"></a>若要檢查最新的更新  
  
1.  來源的伺服器，按一下**[開始]**，按一下 [**所有程式**，然後按一下 [ **Windows Update**。  
  
2.  按一下**檢查是否有更新**。  
  
3.  如果找到更新，請按一下**安裝的更新**。  
  
#### <a name="run-the-best-practices-analyzer"></a>執行最佳做法分析  
 您可以執行若要確認不有任何問題，您的伺服器、 網路，或網域開始移轉程序之前最佳做法分析 (BPA)。 BPA 會收集下列來源的設定資訊：  
  
-   Active Directory Windows 管理檢測 (WMI)  
  
-   登錄  
  
-   網際網路資訊服務 (IIS)  
  
###### <a name="to-use-the-bpa-to-analyze-your-source-server"></a>使用 BPA 分析原始檔伺服器  
  
1.  下表會提供的連結到 Microsoft 下載中心下載和安裝適用於來源伺服器的最佳做法分析 (BPA)。  
  
   |如果您的來源伺服器正在執行|您可以取得 BPA 工具|
   |---|---|
   |Windows SBS 2003|[Microsoft Windows 小型企業 Server 2003 最佳做法分析網站](https://www.microsoft.com/download/details.aspx?id=5334)
   |Windows SBS 2008|[Microsoft Windows 小型企業 Server 2008 最佳做法分析網站](https://www.microsoft.com/download/details.aspx?id=6231)  
   |Windows SBS 2011 Essentials 或 Windows SBS 2011 標準|[Windows Server 方案最佳做法分析網站](https://www.microsoft.com/download/details.aspx?id=15556) 
   |Windows Server Essentials 或 Windows Server 2012|伺服器儀表板  
  
2.  下載完成之後，按一下 [ **[開始]**，指向 [**所有程式**，然後按一下 [ **SBS 最佳做法分析工具**。  
  
    > [!NOTE]
    >  檢查您掃描伺服器之前的更新。  
  
3.  在瀏覽窗格中，按一下**開始掃描]**。  
  
     如果您的來源伺服器執行的 Windows Server Essentials，執行下列動作：  
  
    1.  目的地伺服器以系統管理員的身分登入，然後打開儀表板。  
  
    2.  儀表板，請按一下**裝置**索引標籤。  
  
    3.  在 <**伺服器** >**工作**] 窗格中，按一下 [**最佳做法分析**。  
  
4.  在詳細資料窗格中，輸入掃描指定標籤，然後按一下**開始掃描**。 掃描標籤是掃描報告的名稱，例如**SBS BPA 掃描 1 年 7 月 2013年**。  
  
5.  在掃描完成之後，請按一下**檢視報告的這個最佳做法掃描**。  
  
 BPA 工具會收集資訊伺服器設定之後，它會驗證資訊看起來正確，然後系統管理員會提供資訊和依嚴重性問題的清單。 在清單描述每個問題，並提供建議或可能方案。 使用三報告類型︰  
  
|報告類型|描述
|-----------------|----------------- 
|清單報告|顯示報告維清單中。 
|樹報告|顯示報告階層清單中。

若要檢視描述及方案的問題，請按一下的問題報告中。 並非所有 BPA 工具會回報問題的影響移轉，但您應該克服最多問題越好，確保成功移轉。  
  
####  <a name="BKMK_SynchronizeTheSourceServerTimeWithAnExternalTimeSource"></a>使用外部的時間來源同步來源伺服器的時間  
 來源伺服器上的時間必須設定為在 5 分鐘的時間在目的伺服器，並的日期和時間區域必須是相同的兩部伺服器上。 如果來源伺服器執行中一樣，日期、 時間和時區主機伺服器上必須符合的來源伺服器和目的地伺服器。 為了確保 Windows Server Essentials 已成功安裝，您必須同步來源伺服器的時間在網際網路上的網路時間通訊協定 (NTP) 伺服器。  
  
###### <a name="to-synchronize-the-source-server-time-with-the-ntp-server"></a>若要同步的來源伺服器的時間與 NTP 伺服器  
  
1.  來源伺服器管理員核對與密碼登入。  
  
2.  按一下**[開始]**，按一下 [**執行**，輸入**cmd**中文字方塊，然後按下 ENTER。  
  
3.  在命令提示字元中，輸入 w32tm /config /syncfromflags:domhier / 可靠： 不 /update、，然後按下 ENTER。  
  
4.  在命令提示字元中，輸入網路停止 w32time、，然後按 ENTER 鍵。  
  
5.  在命令提示字元中，輸入網路開始 w32time、，然後按 ENTER 鍵。  
  
> [!IMPORTANT]
>  Windows Server Essentials 安裝期間，您可以確認目的伺服器上的時間，變更，若有必要。 確定時間的 5 分鐘的時間來源伺服器] 設定中。 安裝完成時，NTP 同步目的伺服器。 所有加入網域的電腦，包括來源伺服器同步目的地伺服器，控制器 (PDC) 模擬器主機假設主要網域中的角色。  
  
###  <a name="BKMK_MigrateLOB"></a>建立計劃移轉的業務應用程式  
 行營運 (LOB) 應用程式是係指生意重大電腦應用程式。 LOB 應用程式包括會計、 供應鏈管理、 和資源規劃應用程式。  
  
 當您想要移轉您的 LOB 應用程式時，請洽詢 LOB 應用程式提供者來判斷移轉每個應用程式的適當的方法。 您還必須尋找目的伺服器上安裝 LOB 應用程式使用的媒體。  
  
> [!NOTE]
>  如果您使用 Windows 的小型企業 Server 2011 Essentials SDK，開發自訂的系統健康或警示增益集，以及您想要繼續使用 Windows Server Essentials 增益集，您必須增益集更新，並將它部署目的伺服器上。  
  
  
### <a name="create-a-plan-to-migrate-email-hosted-on-windows-sbs-2011-windows-sbs-2008-and-windows-sbs-2003"></a>建立計劃移轉 Windows SBS 2011、 Windows SBS 2008 和 Windows SBS 2003 裝載的電子郵件  
 在 Windows SBS 2011、 Windows SBS 2008 和 Windows SBS 2003，透過 Microsoft Exchange Server 提供的電子郵件。 不過，Windows Server Essentials 並未提供收件匣電子郵件服務。 如果您目前使用執行 Windows SBS 2011、 Windows SBS 2008 或 Windows SBS 2003 伺服器管理您的公司 s 電子郵件，您需要移轉至其他先或裝載方案。  
  
> [!NOTE]
>  您更新並準備移轉的來源伺服器之後，建議您先移轉程序建立備份的已更新的伺服器。  
  
#### <a name="migrate-email-to-microsoft-office-365"></a>與 Microsoft Office 365 移轉的電子郵件  
 如果您選擇使用 Microsoft Office 365 郵件方案您的網域，請依照下列指導方針在[移轉到雲端的系統轉換 Exchange 移轉的所有信箱](http://help.outlook.com/140/ms.exch.ecp.emailmigrationwizardexchangelearnmore.aspx)到 [開始] 與 Office 365 郵件移轉。 我們建議您安裝 Windows Server Essentials 之前，先完成移轉的電子郵件。  
  
> [!NOTE]
>  移除先 Exchange Server 來源伺服器上的步驟是當您想要使用 Office 365 整合 Windows Server Essentials。 有關如何將 Exchange Server 公用資料夾移轉到 Office 365，請查看部落格文章[Microsoft 換貨 2013年公開資料夾移轉指令碼 Office 365 的](http://blogs.technet.com/b/fmustafa/archive/2013/04/11/microsoft-exchange-2013-public-folders-migration-scripts-for-office-365.aspx)。  
>   
>  在您完成安裝之後，您應該會執行關閉 Office 365 整合功能在 Windows Server Essentials **Microsoft Office 365 的整合**工作。  
  
> [!IMPORTANT]
>  若要允許連接 Exchange Server 的執行來源伺服器上的 Office 365 移轉工具，您必須讓 RPC 透過 HTTP 來源伺服器上。 了解如何透過 HTTP 支援 RPC 資訊，請查看[上的小型企業 Server 2003 （標準或 Premium） 中第一次 HTTP 的部署 RPC 如何](https://technet.microsoft.com/library/bb123622%28EXCHG.65%29.aspx)。 如果您無法順利執行 Office 365 移轉工具之後您透過 HTTP 支援 RPC,，檢視**ValidPorts**的在 HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy，並確認列出的來源 Server 的完整的網域名稱 (FQDN)。 如果未列出 FQDN，將它新增手動使用以下的範例：  
>   
>  遠端。 *contoso*.com:6001-6002; 遠端。 *contoso*.com:6004 (取代*以 contoso*與您的網域名稱)。  
  
#### <a name="migrate-email-to-another-on-premises-exchange-server"></a>移轉到另一個先 Exchange Server 的電子郵件  
 資訊，了解如何將電子郵件移轉到另一個先 Exchange Server，請查看[和 Windows Server Essentials 整合 On 先 Exchange Server](https://technet.microsoft.com/library/jj200172.aspx)。 我們建議您安裝 Windows Server Essentials 之後再完成之前降級來源伺服器移轉的電子郵件, 設定新的先 Exchange Server。  
  
> [!NOTE]
>  不包含 Exchange Server Windows 小型企業伺服器 POP3 連接器。 您將電子郵件資料移轉到另一個 Exchange Server 之後，您可以不再使用的連接器 POP3 功能。  
  
> [!NOTE]
>  更新及準備移轉的來源伺服器之後，您應該先移轉程序建立備份已更新的伺服器。  
  
## <a name="next-steps"></a>後續步驟  
 您已經準備您的來源伺服器移轉到 Windows Server Essentials 的。  立即移至[步驟 2： 安裝 Windows Server Essentials 的全新複本網域控制站以](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md)。  

若要檢視所有的步驟，請查看[Windows Server essentials 移轉](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

