---
title: "Windows Server Essentials 準備您的來源伺服器 migration1"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5861ae9-77cb-4d37-b4c5-8f0757213385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 929c7506c78667646e429c4f28df7e5642c575ab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="prepare-your-source-server-for-windows-server-essentials-migration1"></a>Windows Server Essentials 準備您的來源伺服器 migration1

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

完成下列步驟初步以確保您的設定和資料來源伺服器上的成功移轉到目的伺服器。  
  
#### <a name="to-prepare-for-migration"></a>若要準備移轉  
  

1.  [備份您的來源伺服器](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [安裝最新的 service pack](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [評估來源伺服器的健康狀態](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [執行來源伺服器移轉準備工具](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [建立計劃移轉的業務應用程式](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

1.  [備份您的來源伺服器](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [安裝最新的 service pack](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [評估來源伺服器的健康狀態](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [執行來源伺服器移轉準備工具](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [建立計劃移轉的業務應用程式](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

  
###  <a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>備份您的來源伺服器  
 開始移轉程序之前，先備份您來源的伺服器。 讓備份可協助保護您的資料意外遺失的如果時發生移轉的錯誤處於無法復原。  
  
##### <a name="to-back-up-the-source-server"></a>若要備份來源伺服器  
  
1.  執行來源 Server 的完整備份。 如需 Windows 小型企業 Server 2011 Essentials 備份，請查看[深入了解設定伺服器備份](https://technet.microsoft.com/library/server-backup-support-1.aspx)。  
  
2.  請確認已順利執行備份。 若要測試備份的完整性，從您的備份選取隨機檔案，將它們還原至另一個位置，並確認還原的檔案會原始檔相同。  
  
###  <a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>安裝最新的 service pack  
 您必須安裝最新的更新及 service pack 之前移轉的來源伺服器上。  
  
###  <a name="BKMK_UseWindowsBestPracticeAnalyzer"></a>評估來源伺服器的健康狀態  
 請務必評估來源 Server 的健康狀態，才能開始移轉。 使用下列程序，以確認更新是最新的產生系統健康報告，並執行 Windows Server 方案適用的最佳做法分析 (BPA)。  
  
#### <a name="download-and-install-critical-and-security-updates"></a>下載並安裝重要的安全性更新  
 安裝重要與安全性更新可協助確保您的移轉會成功，並協助保護您的網路移轉程序期間來源伺服器上。  
  
###### <a name="to-check-for-the-latest-updates"></a>若要檢查最新的更新  
  
1.  來源的伺服器，按一下**[開始]**，按一下 [**所有程式**，然後按一下 [ **Windows Update**。  
  
2.  按一下**檢查是否有更新**。  
  
3.  如果找到更新，請按一下**安裝的更新**。  
  
#### <a name="check-the-alert-viewer-for-critical-errors"></a>檢查有重要錯誤警示檢視器  
 您可以檢查有任何重大錯誤儀表板上的警示檢視器。  
  
#### <a name="run-the-windows-server-solutions-best-practices-analyzer"></a>執行 Windows Server 方案最佳做法分析  
 您可以執行 Windows 伺服器方案最佳做法分析 (BPA) 若要確認不有任何問題，您的伺服器、網路，或網域之前開始移轉程序。 BPA 會收集下列來源的設定資訊：  
  
-   使用 Directory® Windows 管理檢測 (WMI)  
  
-   登錄  
  
-   網際網路服務 (IIS) 中繼庫  
  
###### <a name="to-use-the-windows-server-solutions-bpa-to-analyze-your-source-server"></a>使用 Windows Server 方案 BPA 分析原始檔伺服器  
  
1.  下載並安裝[Windows Server 方案最佳做法分析](https://www.microsoft.com/en-us/download/details.aspx?id=15556)在 [Microsoft 下載中心」。  
  
2.  下載完成之後，按一下 [ **[開始]**，指向 [**所有程式**，然後按一下 [ **SBS 最佳做法分析工具**。  
  
    > [!NOTE]
    >  檢查您掃描伺服器之前的更新。  
  
3.  在瀏覽窗格中，按一下**開始掃描]**。  
  
4.  在詳細資料窗格中，輸入掃描指定標籤，然後按一下**開始掃描**。 掃描標籤是掃描報告的名稱，例如**SBS BPA 掃描 1 年 7 月 2012 年**。  
  
5.  在掃描完成之後，請按一下**檢視報告的這個最佳做法掃描**。  
  
 之後收集伺服器設定的相關資訊、Windows Server 方案 BPA 驗證資訊是正確的然後系統管理員會提供資訊和依嚴重性問題的清單。 在清單描述每個問題，並提供建議或可能方案。 使用三報告類型︰  
  
|報告類型|描述|  
|-----------------|-----------------|  
|清單報告|顯示報告維清單中。|  
|樹報告|顯示報告階層清單中。|  
|其他報告|顯示報告，例如 [執行階段登入。|  
  
 若要檢視描述及方案的問題，請按一下的問題報告中。 並非所有 Windows SBS 2011 Essentials BPA 所回報的問題的影響移轉，但您應該盡可能確保成功移轉克服最多的問題。  
  
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
  
###  <a name="BKMK_MPT"></a>執行來源伺服器移轉準備工具  
 您無法執行移轉模式安裝，而不需要第一次執行移轉準備工具來源伺服器上。 這項工具旨在準備您的來源 Server 和網域移轉到 Windows Server Essentials。  
  
> [!IMPORTANT]
>  執行移轉準備工具之前先備份您來源的伺服器。 準備移轉工具對架構所做的所有變更都是無法還原。 如果您在移轉時遇到問題，是從系統備份還原伺服器的唯一方式來源伺服器回到狀態之前您執行的工具。  
  
 若要執行準備移轉工具，您必須企業系統管理員群組、架構系統管理員群組和的成員，網域系統管理員」群組。  
  
##### <a name="to-verify-that-you-have-the-appropriate-permissions-to-run-the-tool-on-the-source-server"></a>若要確認您擁有來源伺服器上執行此工具的適當權限  
  
1.  在來源伺服器上，按一下 [ **[開始]**，按一下 [**系統管理工具]**，，然後按一下 [ **Active Directory 使用者和電腦**。  
  
2.  在主控台按一下以展開您的網域，然後按一下 [**使用者**。  
  
3.  以滑鼠右鍵按一下管理員」，您的移轉，使用，然後按一下**屬性**。  
  
4.  按一下**成員的**索引標籤，然後確認 [企業系統管理員，架構系統管理員和網域系統管理員會列在**的成員**文字方塊。  
  
5.  如果未列出群組，請按一下**新增**，然後新增未列出的每個群組。  
  
    > [!NOTE]
    >  -   如果 Netlogon 服務不會開始，您可能收到的使用權限錯誤。  
    > -   您必須先登出並重新登入的伺服器，變更才會生效。  
  
     若要確保伺服器更新程序可正常運作，您可以使用最新版本的 Windows Update 代理程式。  
  
 若要確保伺服器更新程序可正常運作，您可以使用最新版本的 Windows Update 代理程式。  
  
 您可以來源伺服器上安裝 Windows 更新代理程式之前，您必須先安裝 Windows PowerShell 2.0 和 Microsoft 基準設定分析器 2.0。  
  
-   下載並安裝 Windows PowerShell 2.0，以查看[文章 968929](https://go.microsoft.com/fwlink/p/?LinkId=241483) Microsoft 知識庫中。  
  
-   下載並安裝 Microsoft 基準設定分析器 2.0，以查看[Microsoft 基準設定分析器 2.0](https://go.microsoft.com/fwlink/p/?LinkId=241484)在 [Microsoft 下載中心」。  
  
-   下載並安裝最新版本的 Windows 更新代理程式，請查看[文章 949104](https://go.microsoft.com/fwlink/p/?LinkId=237493) Microsoft 知識庫中。  
  
##### <a name="to-install-and-run-the-migration-preparation-tool-on-the-source-server"></a>安裝及執行來源伺服器上的移轉工具準備  
  
1.  Windows Server Essentials DVD1 插入 DVD 光碟機來源伺服器上。  
  
2.  打開 Windows 檔案總管]，瀏覽至**\support\tools**資料夾的 DVD，然後按兩下**sourcetool.msi**檔案。  
  
    > [!NOTE]
    >  -   如果已安裝在伺服器上準備移轉工具，執行此工具的**[開始]**功能表。  
    > -   若要確定您已經準備好可能移轉的最佳使用體驗，建議您隨時選擇安裝最新的更新。  
  
     精靈會來源伺服器上安裝準備移轉工具。 安裝完成時，移轉準備工具會自動執行，安裝最新的更新。  
  
3.  在移轉準備工具中，選取 [**我有備份，且已準備好進行**，然後按一下 [**下一步**。  
  
    > [!WARNING]
    >  如果您收到安裝 hotfix 裝置相關的許多錯誤訊息，請查看方法 2：重新命名 Catroot2 資料夾中，[文章 822798](https://go.microsoft.com/FWLink/p/?LinkID=118672) Microsoft 知識庫中。  
  
     準備移轉工具準備移轉的延伸 Active Directory 架構來源網域。 在任務完成後，按一下**下一步**以繼續。  
  
4.  準備來源網域後, 移轉準備工具掃描來源伺服器找出兩種類型的問題。  
  
    -   **錯誤**在可以封鎖移轉或造成失敗移轉的來源伺服器上找到的問題。 遵循錯誤訊息，以修正問題，然後按一下**掃描一次**。  
  
    -   **警告**找到可能會造成正常運作的問題移轉的來源伺服器上的問題。 建議您依照修正問題，才能繼續移轉的錯誤訊息中的指示進行。  
  
     修正或活動的所有數個問題之後，請按一下**下一步**。  
  
5.  在 [準備移轉工具，按一下 [**完成**。  
  
6.  準備移轉工具完成時，可能會提示您重新開機來源伺服器，才能開始移轉到 Windows Server Essentials。  
  
> [!NOTE]
>  您必須先完成成功執行的移轉工具準備來源伺服器上的目的伺服器上安裝 Windows Server Essentials 的兩個星期中。 否則，Windows Server Essentials 目的伺服器上安裝將會被封鎖。 發生這種情形，如果您必須再次執行來源伺服器移轉準備工具。  
  
###  <a name="BKMK_PlanToMigrateLineOfBusinessApplications"></a>建立計劃移轉的業務應用程式  
 行營運 (LOB) 應用程式是係指生意重大電腦應用程式。 LOB 應用程式包括會計、 供應鏈管理、 和資源規劃應用程式。  
  
 當您想要移轉您的 LOB 應用程式時，請洽詢 LOB 應用程式提供者來判斷移轉每個應用程式的適當的方法。 您還必須尋找目的伺服器上安裝 LOB 應用程式使用的媒體。  
  
> [!NOTE]
>  如果您使用 Windows 的小型企業 Server 2011 Essentials SDK，開發自訂的系統健康或警示增益集，以及您想要繼續使用 Windows Server Essentials 增益集，您必須增益集更新，並將它部署目的伺服器上。  
  
