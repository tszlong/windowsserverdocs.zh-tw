---
title: 在 Windows Server Essentials 中管理用戶端電腦備份
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b4776e8-9504-4b98-ae80-11da797d9819
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d184c97e47f04b9d7434aaeb0d328761bcfac1c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839269"
---
# <a name="manage-client-computer-backup-in-windows-server-essentials"></a>在 Windows Server Essentials 中管理用戶端電腦備份

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
 
 本主題包含您可以使用 [Windows Server Essentials 儀表板] 完成之一般用戶端電腦備份工作的相關資訊，其中包含下列各節：  
  
-   [修復備份資料庫精靈 的運作方式](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [了解電腦備份設定](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [設定用戶端電腦備份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [變更備份的時間排程執行](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [變更電腦備份保留原則](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [備份重設為預設設定](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [使用修復與復原工具](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [修復備份資料庫](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [停用電腦的備份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [執行備份清理工作](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [在工作列上的用戶端電腦上檢視警示](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [從啟動列檢視備份狀態](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [停止從啟動列的進行中的備份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [從啟動列啟動備份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [電腦備份的運作方式](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [協助防止資料遺失，因為用戶端備份資料庫損毀的祕訣](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [從用戶端電腦備份還原檔案或資料夾](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [了解檔案歷程記錄](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_FileHistory)  
  
##  <a name="BKMK_1"></a> 修復備份資料庫精靈 的運作方式  
 如果 Windows Server Essentials 在您的備份資料庫中偵測到錯誤，它就會傳送健康情況通知給您，警示狀態則會變成紅色，表示有重要狀況。  
  
 請在 [Windows Server Essentials 儀表板] 上，按一下警示狀態圖示來查看備份資料庫錯誤通知。 這個通知會包含一個 [修復]  按鈕，用來啟動 [修復備份資料庫精靈]。 精靈可能需要幾小時的時間才能完成作業。  
  
 精靈會檢查所選電腦的備份資料庫，如果偵測到錯誤，就會嘗試修復資料庫。 在某些情況下，精靈會無法修復整個備份資料庫。 在您啟動精靈之前，請先確認所有在伺服器上使用的外部硬碟皆已連線到伺服器、已開啟且正常運作。 可能藉由將遺失的硬碟重新連線，即可自動修復備份資料庫錯誤。  
  
> [!CAUTION]
>  您應該先備份資料庫，再嘗試修復它。 視在備份資料庫中找到的錯誤類型而定，精靈可能會無法復原某些備份。 部份或全部備份可能會永久遺失。  
  
##  <a name="BKMK_2"></a> 了解電腦備份設定  
 設定妥用戶端電腦的備份之後，您可以指定不同的時間範圍來進行備份。 同樣地，您也可以指定比預設值還要長或短的備份保留時間。  
  
 [還原成預設值]  選項可讓您將備份時間範圍和保留原則重設為在初始備份設定期間提供的預設值。  
  
 預設值是：  
  
|備份設定|預設|描述|  
|--------------------|-------------|-----------------|  
|開始時間|下午 6:00|指定每日備份開始的時間。 建議您將這個時間設定為使用者不使用他們電腦的時間。|  
|結束時間|上午 9:00|指定備份必須完成的時間。 如果備份未要求指定持續時間，就會只使用將電腦成功備份所需的時間。|  
|保留每日備份|5 天|指定保留每日備份的天數。 例如，使用預設設定時，會保留 5 天的每日備份。 在第 6 天及之後的每一天，都會刪除最舊的每日備份檔案。|  
|保留每週備份|4 週|指定保留上次週備份的週數。 例如，使用預設設定時，會保留 4 週的每週備份。 在第 5 週及之後的每一週，都會刪除最舊的每週備份。|  
|保留每月備份|6 個月|指定保留上次月備份的月數。 例如，使用預設設定時，會保留 6 個月的備份。 在第 7 個月及之後的每一個月，都會刪除最舊的每月備份。|  
  
##  <a name="BKMK_3"></a> 設定用戶端電腦備份  
 如果停用備份，您便可以從 [儀表板] 設定電腦的備份。 當您設定電腦的備份時，您可以選擇備份電腦上的所有項目，或是選取您想要備份的磁碟區和資料夾。  
  
> [!NOTE]
>  電腦必須在線上，您才能設定備份。  
  
> [!IMPORTANT]
>   Windows Server Essentials 不支援備份和還原用戶端電腦上的動態磁碟。  
  
#### <a name="to-set-up-backup-for-a-client-computer"></a>設定用戶端電腦的備份  
  
1.  開啟 [儀表板]，然後按一下 [裝置] 索引標籤。  
  
2.  按一下您想要設定備份的用戶端電腦名稱，然後在 [工作] 窗格中，按一下 [設定這部電腦的備份]。  
  
    > [!NOTE]
    >  如果已經為用戶端電腦設定備份，[工作]  窗格中就會列出 [自訂這部電腦的備份]  ，而不是 [設定這部電腦的備份] 。  
  
3.  在 [設定備份精靈] 中，您可以選擇備份所有資料夾，或是選取您想要備份的特定資料夾。 遵循精靈的指示進行。  
  
4.  為電腦設定妥備份之後，按一下 [關閉]  。  
  
### <a name="critical-system-files"></a>重要系統檔案  
 當您安裝 Windows 作業系統時，安裝程式會在它放置系統啟動和執行所需之檔案的系統磁碟機上建立資料夾。 重要系統檔案包含副檔名為 .dll、.exe、.ocx 及 .sys 的檔案。 這當中有些檔案是 True Type 字型。 此外，系統狀態檔案，例如 s 登錄中，所需作業系統正常執行。  
  
### <a name="find-the-file-you-are-looking-for"></a>尋找您要尋找的檔案  
 您可以從現有的備份還原電腦的所有資料夾、多個檔案和資料夾，或是單一檔案或資料夾。  
  
 在您選取要用來做為還原來源的備份之後，系統就會讀取該備份檔案並顯示所有的檔案和資料夾。 您可以向下切入到要還原的特定檔案或資料夾，方法是按兩下最上層資料夾，然後向下切入資料夾階層，直到找出您要尋找的檔案為止。  
  
### <a name="why-am-i-unable-to-select-some-items"></a>為什麼我無法選取某些項目？  
 [選取要備份的項目] 頁面之選項功能表上的核取方塊可以指出每個資料夾的不同狀態。 當核取方塊為：  
  
-   **已選取**時，表示已選取關聯的資料夾及資料夾內容來進行備份。  
  
-   **未選取**時，表示將關聯的資料夾及資料夾內容排除在備份範圍之外。  
  
-   **實心**時，表示已選取關聯的資料夾來進行備份，但將該資料夾中的一或多個項目排除在備份範圍之外。  
  
 如果您無法選取特定的資料夾：  
  
-   該磁碟區可能設定要進行備份，但可能已離線。 這在卸除式 USB 磁碟機上很常見。 離線的磁碟區會以灰色文字顯示。  
  
-   您只能從格式化為 NTFS 檔案系統的本機磁碟機備份資料。 格式化為 FAT (包括 FAT32) 或 ReFS 檔案系統的磁碟機不會出現在要備份的磁碟機清單中。  
  
> [!IMPORTANT]
>  磁碟區陰影複製服務 (VSS) 不支援在同一個快照集中建立虛擬磁碟區和主機磁碟區的陰影複製。 有必要備份虛擬磁碟區時，VSS 支援在虛擬硬碟 (VHD) 上建立磁碟區的快照集。 如需詳細資訊，請參閱 [為虛擬硬碟提供服務和備份](https://go.microsoft.com/fwlink/p/?LinkId=256577)。  
  
##  <a name="BKMK_4"></a> 變更備份的時間排程執行  
 備份程序應該排定在越少人使用其連線到網路的電腦時越好。 這通常是在深夜或清晨的時段。 預設的備份時間是下午 6:00 到上午 9:00。 伺服器只有在排定的時間範圍內才會嘗試備份用戶端電腦。  
  
 不過，如果您的公司在這些傳統上的下班時間處於營運狀態，您可能會想要變更這些預設設定。  
  
#### <a name="to-change-the-time-that-backup-is-scheduled-to-run"></a>變更排定備份執行的時間  
  
1.  開啟伺服器 [儀表板]，然後按一下 [裝置]。  
  
2.  在 [裝置工作] 中，按一下 [自訂電腦備份和檔案歷程記錄設定] 。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 中，這項工作已重新命名**用戶端電腦備份工作**。  
  
3.  在 [用戶端電腦備份設定和工具] 的 [電腦備份]  索引標籤上，您可以變更開始和結束時間以符合您需求。  
  
4.  按一下 [套用]，然後按一下 [確定]。  
  
##  <a name="BKMK_5"></a> 變更電腦備份保留原則  
 您所有電腦的每日備份會在您的伺服器上隨時間累積。 為了協助您管理這些備份，Windows Server Essentials 可協助您管理電腦備份的資料庫。 您可以設定要為您所有的電腦保留多少備份。  
  
 備份保留原則可決定要將備份保留多久之後再於備份清理程序期間刪除。 備份清理執行時間是每星期六的下午 11:59。 它會刪除在備份保留原則範圍外的所有備份。 備份保留原則的預設值是：  
  
-   **保留 5 天的每日備份。** 第一個日備份會被保留為每日備份。 5 天之後，最舊的每日備份就會在清理程序中被刪除。  
  
-   **保留 4 週的每週備份。** 第一個週備份會被保留為每週備份。 4 週天之後，最舊的每週備份就會在清理程序中被刪除。  
  
-   **保留 6 個月的每月備份。** 第一個月備份會被保留為每月備份。 6 個月之後，最舊的每月備份就會在清理程序中被刪除。  
  
#### <a name="to-change-the-computer-backup-retention-policy"></a>變更電腦備份保留原則  
  
1.  開啟 [儀表板] 。  
  
2.  按一下 [裝置]。  
  
3.  在 [裝置工作] 窗格中，按一下 [自訂電腦備份和檔案歷程記錄設定]。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 中，這項工作已重新命名**用戶端電腦備份工作**。  
  
4.  在 [用戶端電腦備份設定和工具] 的 [用戶端電腦備份保留原則]  區段中，對保留原則進行符合您需求的變更。  
  
5.  當您完成時，按一下 [確定] 以套用變更並關閉對話方塊。 更新的保留原則將會在下一次備份清理執行時套用。  
  
    > [!NOTE]
    >  更新的保留原則會套用到您網路上所有設定要進行備份的用戶端電腦上。  
  
##  <a name="BKMK_6"></a> 備份重設為預設設定  
 為用戶端電腦設定備份之後，網路系統管理員可能指定了不同的時間範圍。 同樣地，系統管理員也可能指定了比預設值還要長或短的備份保留時間。 [重設為預設值]  按鈕可讓您將備份時間範圍和保留原則重設為在初始備份設定期間提供的預設值。  
  
 預設值是：  
  
-   備份開始時間：下午 6:00  
  
-   結束時間：上午 9:00  
  
-   保留每日備份的天數：5 天  
  
-   保留上次週備份的週數：4 週  
  
-   保留上次月備份的月數：6 個月  
  
#### <a name="to-reset-computer-backup-to-the-default-settings"></a>將電腦備份重設為預設設定  
  
1.  開啟 [儀表板] ，然後開啟 [裝置]  頁面。  
  
2.  在 [裝置工作] 中，按一下 [自訂電腦備份和檔案歷程記錄設定] 。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 中，按一下**用戶端電腦備份工作**。  
  
3.  在 [用戶端電腦備份設定和工具]  頁面的 [電腦備份]  索引標籤上，按一下 [重設為預設值] 。  
  
    > [!NOTE]
    >  您可能會想要記下自訂的排程和保留原則設定，因為當您重設為預設設定時，這些資料就會消失。  
  
4.  按一下 [套用]，然後按一下 [確定]。  
  
##  <a name="BKMK_7"></a> 使用修復與復原工具  
 **修復備份：** 如果電腦備份的資料庫損毀或因某種原因而無法使用，您可以使用 [修復備份資料庫精靈] 來嘗試修復資料庫。 精靈會分析備份檔案，以判斷是否有任何可修復的問題。 然後精靈會嘗試修正任何找到的問題。  
  
> [!WARNING]
>  如果無法修正問題，精靈可能就會永久刪除電腦備份檔案。  
  
 **電腦復原：** 您可以建立可開機的 USB 快閃磁碟機，用來將電腦從現有的備份還原。 您必須使用 1 GB 以上的 USB 快閃磁碟機。 建立可開機的 USB 快閃磁碟機之後，您需將它插入您想要還原的電腦，然後以 USB 快閃磁碟機開機來啟動完整的系統還原程序。  
  
##  <a name="BKMK_8"></a> 修復備份資料庫  
 如果您收到通知您電腦備份資料庫發生問題的警示，您可以嘗試修復它。  
  
#### <a name="to-repair-the-backup-database"></a>修復備份資料庫  
  
1.  開啟 [儀表板] 。  
  
2.  按一下 [裝置]。  
  
3.  在 [裝置工作] 窗格中，按一下 [自訂電腦備份和檔案歷程記錄設定]。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 中，按一下**用戶端電腦備份工作**。  
  
4.  在 [用戶端電腦備份設定和工具] 中，按一下 [工具] 索引標籤。  
  
5.  在 [修復備份]  區段中，按一下 [立即修復] 。 [修復備份資料庫精靈] 隨即開啟。  
  
6.  依照 [修復備份資料庫精靈] 的指示進行。  
  
7.  視備份資料庫的大小而定，資料庫修復可能需要花費幾小時的時間。 按一下 [關閉]，然後按一下 [確定] 來關閉 [自訂電腦備份和檔案歷程記錄設定] 頁面。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 中，按一下**用戶端電腦備份設定和工具**。  
  
#### <a name="to-review-the-results-of-the-backup-database-repair"></a>檢閱備份資料庫修復結果  
  
1.  開啟 [儀表板] 。  
  
2.  按一下 [裝置]。  
  
3.  在 [裝置工作] 窗格中，按一下 [自訂電腦備份和檔案歷程記錄設定]。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 中，按一下**用戶端電腦備份工作**。  
  
4.  在 [用戶端電腦備份設定和工具] 中，按一下 [工具] 索引標籤。  
  
5.  結果會顯示在 [修復備份] 區段中。  
  
##  <a name="BKMK_9"></a> 停用電腦的備份  
 使用 [儀表板] 可快速停用您網路上電腦的備份。  
  
#### <a name="to-disable-backup-for-a-computer"></a>停用電腦的備份  
  
1.  開啟 [儀表板] 。  
  
2.  按一下 **[裝置]** 標籤。  
  
3.  按一下您想要停用備份的電腦名稱。  
  
4.  在 [工作] 窗格中，按一下 [自訂電腦的備份] 。 [自訂備份精靈] 隨即出現。  
  
5.  按一下 [停用這部電腦的備份]，然後選取您要保留或刪除現有的備份檔案。  
  
6.  按一下 [儲存變更]，然後按一下 [關閉]。  
  
 如需有關如何在停用備份之後啟用電腦備份的資訊，請參閱[設定用戶端電腦的備份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3)。  
  
##  <a name="BKMK_10"></a> 執行備份清理工作  
 用戶端電腦備份清理工作是排定在每星期六的下午 11:59 於所有備份完成之後執行。 清理工作會根據備份保留原則，從用戶端電腦備份資料庫刪除備份。 備份保留原則的預設設定是：  
  
-   保留每日備份的天數：5 天  
  
-   保留上次週備份的週數：4 週  
  
-   保留上次月備份的月數：6 個月  
  
 如需有關變更備份保留原則的資訊，請參閱[變更電腦備份保留原則](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5)。  
  
#### <a name="to-run-the-client-backup-database-cleanup-task"></a>執行用戶端備份資料庫清理工作  
  
1.  使用您的系統管理員使用者帳戶和密碼登入伺服器。  
  
2.  在伺服器上，開啟 [工作排程器] 。 您可以從 [系統管理工具] 主控台存取 [工作排程器] 程式。  
  
3.  在 [工作排程器] 中，依序展開 [工作排程器程式庫] 、[Microsoft] 、[Windows] ，然後按一下 [Windows Server Essentials] 。  
  
4.  按一下 [備份清理]工作，然後按一下 [動作] 窗格中的 [執行]。 [狀態] 會變成 [執行中]，直到工作完成為止。  
  
##  <a name="BKMK_11"></a> 在工作列上的用戶端電腦上檢視警示  
 您可以在您電腦上的工作列收到因下列原因而發出的備份通知：  
  
-   備份開始。  
  
-   備份結束。  
  
-   最近沒有成功的備份。 通知中會包含自上次成功備份後的天數。  
  
-    Windows Server Essentials 只有：有新磁碟區新增到您的電腦中。 管理您網路的人員必須執行 [自訂備份精靈]，才能納入或排除要供未來備份的磁碟區。  
  
##  <a name="BKMK_12"></a> 從啟動列檢視備份狀態  
 使用 [啟動列] 可檢視您電腦的快速備份狀態。  
  
> [!TIP]
>  若要取得快速狀態，請將游標移到桌面通知區域中的 [啟動列] 圖示上。 備份狀態會出現在工具提示中。  
  
#### <a name="to-view-status-of-a-backup-for-your-computer"></a>檢視您電腦的備份狀態  
  
1.  使用您的使用者名稱和密碼登入 [啟動列]。  
  
2.  按一下 [備份] 。  
  
3.  在 [備份內容]  對話方塊的 [備份狀態]  區段中，會顯示備份的狀態。  
  
    -   **沒有先前的備份**：尚未執行備份，或備份歷程記錄已被刪除。  
  
    -   **備份擱置中**：備份正在等候伺服器上的另一個程序完成。  
  
    -   **正在連線**：備份正在連線到伺服器。  
  
    -   **無法連線**：備份無法連線到 Windows Server Client Computer Backup Provider Service。  
  
        > [!NOTE]
        >  在 Windows Server Essentials 中，此服務已重新命名 Windows Server Essentials Client Computer Management Service。  
  
    -   **備份進行中**：會顯示備份已完成的百分比。  
  
    -   **備份已取消**：如果您或網路系統管理員取消備份，會顯示備份開始的日期和時間。  
  
    -   **備份成功**：如果備份成功完成，會顯示備份開始的日期和時間。  
  
    -   **備份未完成**：如果備份未成功完成，會顯示備份開始的日期和時間。  
  
    -   **備份不成功**：如果備份未成功完成，會顯示備份開始的日期和時間。  
  
4.  按一下 [確定] 以關閉 [備份內容] 對話方塊。  
  
##  <a name="BKMK_13"></a> 停止從啟動列的進行中的備份  
 您可以輕鬆地停止進行中的備份。  
  
#### <a name="to-stop-a-backup-in-progress"></a>停止進行中的備份  
  
1.  開啟 [啟動列] 。  
  
    > [!NOTE]
    >  如果系統要求，請用使用者名稱和密碼登入**啟動控制板**。  
  
2.  按一下 [備份] 。  
  
3.  當備份正在進行時，在 [備份內容]  對話方塊的 [備份狀態]  區段中，[開始備份]  按鈕會變成 [停止備份]  。 按一下 [停止備份] ，然後按一下 [是]  來進行確認。 備份將會繼續執行，直到您按一下 [是] 為止。  
  
4.  當您停止進行中的備份時，狀態會變成 [備份已取消]  並顯示備份開始的日期和時間。 如果您看到的不是該狀態，而是 [成功] 、[未完成] 或 [失敗] ，則表示上次的備份已經結束。  
  
5.  按一下 [確定] 以關閉 [備份內容] 對話方塊。  
  
##  <a name="BKMK_14"></a> 從啟動列啟動備份  
 有時，您可能會想要在於伺服器上定期排定的備份時間之前，先備份您的檔案和資料夾。 [啟動列] 可讓您手動起始您的電腦備份。  
  
#### <a name="to-start-a-backup"></a>啟動備份  
  
1.  開啟 [啟動列] 。  
  
    > [!NOTE]
    >  如果系統要求，請用使用者名稱和密碼登入**啟動控制板**。  
  
2.  按一下 [備份] 。  
  
3.  在 [備份內容] 對話方塊的 [備份狀態] 區段中，按一下 [開始備份]，然後按一下 [確定]。  
  
4.  輸入備份的描述，然後按一下 [確定]。 狀態會變成 [開始備份]，然後再變成 [備份進行中] 並顯示完成百分比。  
  
5.  在備份開始之後，按鈕會變成 [停止備份]。 如果備份正在進行中，而您想要將它停止，請按一下 [停止備份] ，然後按一下 [是] 。 當您停止進行中的備份時，狀態會變成 [備份已取消] 並顯示備份開始的日期和時間。  
  
6.  當備份成功完成時，狀態會變成 [備份成功]  並顯示備份開始的日期和時間。  
  
7.  按一下 [確定] 以關閉 [備份內容] 對話方塊。  
  
##  <a name="BKMK_15"></a> 電腦備份的運作方式  
 在每日備份期間會發生下列事件：  
  
-   依序備份網路電腦。  
  
-   即使您關閉備份視窗，進行中的備份也會執行到結束。  
  
-   安裝 Windows 更新，然後重新啟動 Windows Server Essentials (如有必要)。  
  
-   在星期日執行備份清理。  
  
> [!IMPORTANT]
>  磁碟區陰影複製服務 (VSS) 不支援在同一個快照集中建立虛擬磁碟區和主機磁碟區的陰影複製。 有必要備份虛擬磁碟區時，VSS 支援在虛擬硬碟 (VHD) 上建立磁碟區的快照集。 如需詳細資訊，請參閱 [為虛擬硬碟提供服務和備份](https://go.microsoft.com/fwlink/p/?LinkId=256577)。  
  
##  <a name="BKMK_16"></a> 協助防止資料遺失，因為用戶端備份資料庫損毀的祕訣  
 如果用戶端備份資料庫損毀，您可能會遺失重要資料。  
  
 若要協助防止因用戶端備份資料庫損毀而導致資料遺失，請考慮執行下列動作：  
  
-   啟用其他備份用戶端備份資料庫的方法。 例如，根據您正在執行的 Windows Server Essentials 版本，使用伺服器備份、 線上備份或 Microsoft Azure 備份來備份資料庫。  
  
    > [!IMPORTANT]
    >  請確定您選取備份 [用戶端電腦備份] 資料夾中的所有檔案。  
  
-   如果您必須還原資料庫，請務必還原 [用戶端電腦備份]  資料夾中的所有檔案。 如果您沒有還原所有檔案，資料庫可能會變得不一致。  
  
-   在您清理或還原用戶端備份資料庫之前，請確定將目前進行中的所有用戶端備份都停止。  
  
     如果您執行 Windows Server Essentials，您還應該停止 Windows Server Client Computer Backup Service 和 Windows Server Client Computer Backup Provider Service。  
  
     如果您執行 Windows Server Essentials，您還應該停止 Windows Server Computer Backup Service。  
  
     完成還原操作之後，請重新啟動服務。  
  
##  <a name="BKMK_17"></a> 從用戶端電腦備份還原檔案或資料夾  
 您可以從備份瀏覽並還原個別的檔案和資料夾。  
  
> [!NOTE]
>  您無法使用伺服器上的 [儀表板] 將檔案和資料夾還原到用戶端電腦。 您必須開啟用戶端電腦上的 [儀表板] 來完成工作。  
  
> [!IMPORTANT]
>  您無法將檔案和資料夾還原到已設定為動態磁碟的硬碟。  
  
#### <a name="to-restore-files-and-folders"></a>還原檔案和資料夾  
  
1.  開啟用戶端電腦上的 [儀表板]  。  
  
2.  按一下 **[裝置]** 標籤。  
  
3.  按一下您想要還原檔案和資料夾的電腦，然後按一下 [工作] 窗格中的 [還原電腦的檔案或資料夾]。 [還原檔案或資料夾精靈] 便會開啟。  
  
4.  遵循精靈的指示進行。  
  
##  <a name="BKMK_FileHistory"></a> 了解檔案歷程記錄  
 Windows Server Essentials 的「檔案歷程記錄」功能會自動備份有「檔案歷程記錄」能力之網路電腦的 [媒體櫃]、[連絡人]、[桌面] 及 [我的最愛] 資料夾中的檔案。 如果原始資料遺失、損毀或被刪除，您可以將它們還原。 您也可以從特定的時間點找出您檔案的各種不同版本。 經過一段時間後，您就會擁有您檔案的完整歷程記錄。  
  
 在 Windows Server Essentials 中，您可以自訂 從檔案歷程記錄設定**檔案歷程記錄**頁**用戶端電腦備份設定和工具**。  
  
 在 Windows Server Essentials 中，您可以自訂的檔案歷程記錄設定，方法是前往**使用者** 索引標籤，然後選取**變更檔案歷程記錄設定**工作。  
  
 [檔案歷程記錄] 頁面提供下列選項：  
  
|備份設定|預設|描述|  
|--------------------|-------------|-----------------|  
|開啟/關閉|開啟|當您安裝 Windows Server Essentials 時，預設會開啟「檔案歷程記錄」。|  
|備份資料|文件和桌面|有三種預先設定的設定，可讓您指定各種不同的備份解決方案。 您可以選擇下列其中一個選項：<br /><br /> -文件和桌面<br /><br /> -所有媒體櫃、 桌面、 連絡人和我的最愛<br /><br /> -在 [媒體櫃、 桌面、 連絡人和我的最愛不包括音樂、 視訊和圖片] 媒體櫃中的資料所有的資料|  
|備份頻率|每小時|指定「檔案歷程記錄」建立所選資料之備份的頻率。 您可以從數個選項中選擇，範圍從每 10 分鐘到每天。|  
|保留複本期限|1 年|指定的「檔案歷程記錄」保留備份複本的時間長短。|  
  
 如需有關檔案歷程記錄問題的資訊，請參閱[檔案歷程記錄問題疑難排解](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md)。  
  
## <a name="see-also"></a>另請參閱  
  
-   [管理備份和還原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
