---
title: "管理 Windows Server Essentials Client 電腦備份"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="manage-client-computer-backup-in-windows-server-essentials"></a>管理 Windows Server Essentials Client 電腦備份

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集
 
 本主題包含一般備份的工作，您可以使用 Windows Server Essentials 儀表板，完成，包含下列章節 client 電腦的相關資訊：  
  
-   [維修備份資料庫精靈的運作方式](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [了解電腦備份設定](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [備份 client 電腦的設定](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [變更的備份的時間來執行排定](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [變更電腦備份保留原則](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [重設為預設設定的備份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [使用修復與復原工具](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [修復備份資料庫](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [停用備份的電腦](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [執行備份清除工作](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [在工作列 client 的電腦上檢視警示](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [從 Launchpad 檢視備份狀態](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [停止 Launchpad 進行備份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [從 Launchpad 開始建立的備份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [備份電腦的運作方式](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [若要防止損壞 client 備份資料庫因為資料遺失的秘訣](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [從電腦 client 的備份還原檔案或資料夾](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [了解檔案歷史](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_FileHistory)  
  
##  <a name="BKMK_1"></a>維修備份資料庫精靈的運作方式  
 Windows Server Essentials 偵測到您的備份資料庫中錯誤，如果它傳送給您的健康通知，並警示狀態變更紅色，表示重要的條件。  
  
 Windows Server Essentials 儀表板，按一下 [查看備份資料庫錯誤通知的警示狀態圖示。 通知包含**修復**按鈕、開始修復備份資料庫精靈。 精靈可能需要幾個小時的時間來完成。  
  
 精靈會檢查您的備份資料庫適用於所選取的電腦，並嘗試修復資料庫如果偵測到的錯誤。 有時候，精靈無法修復整個備份。 精靈開始之前，確認所有外部硬碟機您使用您的伺服器上的連接至電腦，而且正常運作的伺服器。 重新連接遺失的硬碟，可能會自動修正備份資料庫項錯誤。  
  
> [!CAUTION]
>  您應該先嘗試修復該備份資料庫。 根據備份資料庫中找到的錯誤的類型，精靈可能無法復原一些備份。 部分或所有備份可能會永久遺失。  
  
##  <a name="BKMK_2"></a>了解電腦備份設定  
 備份設定 client 電腦之後，您可以指定時間的備份發生的不同的視窗。 同樣地，您可以指定預設較長的時間或短備份的保留時間。  
  
 **還原成預設值**選項可讓您重設備份視窗及保留原則期間起始備份設定提供的預設值。  
  
 預設值︰  
  
|備份設定|預設值|描述|  
|--------------------|-------------|-----------------|  
|開始時間|6:00|指定時間的開始每天備份。 建議您將此設定為使用者在無法使用他們的電腦使用時間。|  
|結束時間|9:00|指定的時間備份必須完成。 備份不需要指定的持續時間，它會使用只成功備份的電腦所需的時間。|  
|保留每日備份|5 天|指定保留每日備份的天數。 例如，使用預設設定，請 5 每天備份會保留。 第 6 和每日之後，在刪除每日最古老備份檔案。|  
|保留週備份|4 個星期|指定數個星期會保留最後一個星期的備份。 使用預設設定，例如保留 4 週備份。 在第 5 個星期，因此每個星期會刪除週的舊備份。|  
|保留每月備份|6 個月|指定幾個月保留最後一個月的備份。 使用預設設定，例如保留 6 個月的備份。 在月 7 日，因此每個月被刪除每月的舊備份。|  
  
##  <a name="BKMK_3"></a>備份 client 電腦的設定  
 如果已停用備份，您可以從儀表板設定備份的電腦。 當您設定備份的電腦時，您可以選擇您備份的電腦上的所有項目，或選取磁碟區」和「您想要備份的資料夾。  
  
> [!NOTE]
>  必須 online 設定備份您的電腦。  
  
> [!IMPORTANT]
>   Windows Server Essentials 不支援備份及還原 client 電腦上的動態磁碟。  
  
#### <a name="to-set-up-backup-for-a-client-computer"></a>若要備份 client 電腦的設定  
  
1.  開放**儀表板**，然後按**裝置**索引標籤。  
  
2.  按一下您想要設定備份，然後在 client 電腦的名稱**工作**窗格中，按**設定的備份，這台電腦的**。  
  
    > [!NOTE]
    >  如果備份已設定為 client 的電腦，**自訂備份此電腦的**中列出的**工作**窗格中，而不是**設定的備份，這台電腦**。  
  
3.  在 [設定備份精靈，您可以選擇要備份的所有資料夾，或選取您想要備份特定資料夾。 請依照精靈中的指示進行。  
  
4.  按一下**關閉**當電腦設定的備份。  
  
### <a name="critical-system-files"></a>重要的系統檔案  
 當您安裝的 Windows 作業系統時，安裝程式會在您它放置檔案系統開始，執行所需的系統磁碟機上建立資料夾。 重要的系統檔案包含.dll、.exe、ocx 和.sys 副檔名檔案。 部分這些檔案的真類型字型。 此外，系統狀態的檔案，例如系統的登錄，所需的作業系統的正確執行。  
  
### <a name="find-the-file-you-are-looking-for"></a>尋找您要尋找的檔案  
 您可以從現有的備份還原所有適用於電腦、多個檔案和資料夾，或單一檔案或資料夾的資料夾。  
  
 選取您想要用來還原備份之後，在朗讀備份檔案，並顯示所有檔案和資料夾。 您可以查看下的特定檔案或資料夾還原按兩下的最上層資料夾，然後縮放透過資料夾階層直到找到您要尋找的檔案。  
  
### <a name="why-am-i-unable-to-select-some-items"></a>為何我無法選取的項目？  
 ] 核取方塊選取項目] 功能表上的**選擇備份頁面上的項目**表示不同的每個資料夾的狀態。 ] 核取方塊的時：  
  
-   **選取**、的備份選取相關聯的資料夾和到資料夾。  
  
-   **選取**、相關聯的資料夾和資料夾到排除備份。  
  
-   **實心**、相關聯的資料夾已選取的備份，但一或多個資料夾中的項目也會排除備份。  
  
 如果您無法選擇的特定資料夾：  
  
-   音量設定的備份，但它可能離線。 這是常見的抽取式 USB 磁碟機。 磁碟區離線顯示灰色文字。  
  
-   您可以僅限資料備份從本機磁碟機格式化為 NTFS 檔案系統。 磁碟機格式化為 FAT（包括 FAT32）或 ReFS 檔案系統不會出現在清單中備份磁碟機。  
  
> [!IMPORTANT]
>  磁碟區陰影複製服務 (VSS) 不支援相同的快照設定中建立陰影複製 virtual 磁碟區及主機磁碟區。 備份 virtual 磁碟區是否需要 VSS 並 virtual 硬碟上 (VHD)，支援建立的磁碟區的快照。 如需詳細資訊，請查看[的服務和備份上 Virtual 硬碟](https://go.microsoft.com/fwlink/p/?LinkId=256577)。  
  
##  <a name="BKMK_4"></a>變更的備份的時間來執行排定  
 應該排程備份程序期間時最少人盡可能使用他們網路的電腦。 這通常是在最夜晚或清晨。 預設的備份是 6:00 9:00 直到。 嘗試備份 client 電腦只有在視窗已排程的時間伺服器。  
  
 不過，如果您的業務使用這些傳統下班時，您可能想要變更這些預設設定。  
  
#### <a name="to-change-the-time-that-backup-is-scheduled-to-run"></a>若要變更的時間來執行排定備份  
  
1.  打開伺服器**儀表板**，然後按**裝置**。  
  
2.  在**裝置工作**，按一下 [**設定自訂電腦備份與檔案歷史**。  
  
    > [!NOTE]
    >  在 Windows Server Essentials，這項工作已經重新命名**Client 電腦備份工作**。  
  
3.  在**Client 電腦備份設定和工具**，在**電腦備份**索引標籤上，您可以變更開始和結束時間，以符合您需求。  
  
4.  按一下**套用**，然後按**[確定]**。  
  
##  <a name="BKMK_5"></a>變更電腦備份保留原則  
 每天備份的所有電腦累計伺服器上。 為了協助您管理這些備份，Windows Server Essentials 可協助您管理的資料庫設定備份的電腦。 您可以設定多少讓所有電腦的備份。  
  
 備份保留原則判斷多久備份會保留多久才刪除備份清除程序期間。 備份清理會在每個星期六 PM 11:59 執行。 它會刪除以外的備份保留原則的所有備份。 備份保留原則的預設值︰  
  
-   **5 天保留每日備份。** 為每日備份保留一天中的第一個備份。 5 天之後，清除程序中刪除每日最舊的備份。  
  
-   **為 4 個星期會保留週備份。** 做為週備份保留星期的第一個備份。 之後 4 個星期，清除程序中刪除週的舊備份。  
  
-   **6 個月會保留每月的備份。** 與每月備份保留在每個月的第一個備份。 每月的舊備份刪除中清除程序後 6 個月中，  
  
#### <a name="to-change-the-computer-backup-retention-policy"></a>若要變更電腦備份保留原則  
  
1.  開放**儀表板**。  
  
2.  按一下**裝置**。  
  
3.  在**裝置工作**窗格中，按**設定自訂電腦備份與檔案歷史**。  
  
    > [!NOTE]
    >  在 Windows Server Essentials，這項工作已經重新命名**電腦 Client 備份工作**。  
  
4.  在**Client 電腦備份設定和工具**，請在**Client 電腦備份保留原則**區段，讓符合您需求的保留原則所做的變更。  
  
5.  當您完成後時，按一下**[確定]**適用於所做的變更並關閉對話方塊。 將會在下一次備份清理執行套用更新的保留原則。  
  
    > [!NOTE]
    >  更新的保留原則套用到所有 client 網路上的電腦您所設定的備份。  
  
##  <a name="BKMK_6"></a>重設為預設設定的備份  
 備份設定 client 電腦之後，網路系統管理員可能會有指定的時間不同的視窗。 同樣地，系統管理員可以指定預設較長的時間或短備份的保留時間。 **重設為預設值**按鈕，可讓您重設備份視窗及保留原則期間起始備份設定提供的預設值。  
  
 預設值︰  
  
-   備份開始時間：6:00  
  
-   結束時間：9:00  
  
-   保留每日備份的天數：5 天  
  
-   數個星期會保留最後一個星期的備份，：4 個星期  
  
-   幾個月會保留最後一個月的備份，：6 個月  
  
#### <a name="to-reset-computer-backup-to-the-default-settings"></a>若要重設為預設設定的備份電腦  
  
1.  打開**儀表板**，然後打開和**裝置**頁面。  
  
2.  在**裝置工作**，按一下 [**設定自訂電腦備份與檔案歷史**。  
  
    > [!NOTE]
    >  在 Windows Server Essentials，按一下**Client 電腦備份工作**。  
  
3.  在**電腦備份**索引標籤的**Client 的電腦和備份設定和工具**頁面上，按一下 [**重設為預設值**。  
  
    > [!NOTE]
    >  您可能想要記下的自訂的排程和保持原則設定，因為他們會離開您重設為預設設定。  
  
4.  按一下**套用**，然後按**[確定]**。  
  
##  <a name="BKMK_7"></a>使用修復與復原工具  
 **修復備份：**如果的電腦備份資料庫損壞或無法使用某些原因，您可以嘗試使用修復備份資料庫精靈修復資料庫。 精靈會分析判斷是否有任何問題，可以修復備份檔案。 精靈會再嘗試修正找到的任何問題。  
  
> [!WARNING]
>  如果無法修正問題，精靈會永久 delete 電腦備份檔案。  
  
 **電腦的復原：**您可以建立可開機 USB 快閃磁碟機從現有的備份還原電腦使用。 您必須使用 USB 快閃磁碟機的 1 GB 或更大。 建立可開機的 USB 快閃磁碟機之後，您將它插入您想要還原，再開機的 USB 快閃磁碟機以開始程序完整的系統還原至電腦。  
  
##  <a name="BKMK_8"></a>修復備份資料庫  
 如果您收到通知告知您的電腦備份資料庫有問題，您可以嘗試修復該。  
  
#### <a name="to-repair-the-backup-database"></a>若要備份資料庫修復  
  
1.  開放**儀表板**。  
  
2.  按一下**裝置**。  
  
3.  在**裝置工作**窗格中，按**自訂電腦備份與檔案歷史設定。**  
  
    > [!NOTE]
    >  在 Windows Server Essentials，按一下**Client 電腦備份工作**。  
  
4.  在**Client 電腦備份設定和工具**，按一下 [**工具**索引標籤。  
  
5.  在**修復備份**區段中，按**現在修復**。 維修備份資料庫精靈開啟。  
  
6.  請依照下列修復備份資料庫精靈中的指示進行。  
  
7.  根據多少備份資料庫是，資料庫修復可能需要數小時。 按一下**關閉**，然後按一下 [ **[確定]**關閉**自訂電腦備份與檔案歷史設定**頁面。  
  
    > [!NOTE]
    >  在 Windows Server Essentials，按一下**Client 電腦備份設定和工具**。  
  
#### <a name="to-review-the-results-of-the-backup-database-repair"></a>若要檢視的備份資料庫修復  
  
1.  開放**儀表板**。  
  
2.  按一下**裝置**。  
  
3.  在**裝置工作**窗格中，按**自訂電腦備份與檔案歷史設定。**  
  
    > [!NOTE]
    >  在 Windows Server Essentials，按一下**Client 電腦備份工作**。  
  
4.  在**Client 電腦備份設定和工具**，按一下 [**工具**索引標籤。  
  
5.  結果顯示在**修復備份**一節。  
  
##  <a name="BKMK_9"></a>停用備份的電腦  
 若要快速停用備份，您網路上的電腦使用儀表板。  
  
#### <a name="to-disable-backup-for-a-computer"></a>若要停用備份的電腦  
  
1.  開放**儀表板**。  
  
2.  按一下**裝置**索引標籤。  
  
3.  按一下您要停用備份的電腦的名稱。  
  
4.  在 [工作] 窗格中，按一下**適用於電腦的自訂備份**。 [自訂備份精靈會出現。  
  
5.  按一下**停用備份此電腦的**，然後選取您想要保留或 delete 現有備份檔案是否。  
  
6.  按一下**儲存變更**，然後按**關閉**。  
  
 如需有關如何讓電腦的備份之後停用備份, 的資訊，請查看[設定備份 client 電腦的](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_3)。  
  
##  <a name="BKMK_10"></a>執行備份清除工作  
 Client 電腦備份清理任務的 11:59 PM 每個星期六所有備份完成之後，執行。 清除工作移除 client 電腦備份資料庫依據備份保留原則的備份。 預設設定的備份保留原則中︰  
  
-   保留每日備份的天數：5 天  
  
-   數個星期會保留最後一個星期的備份，：4 個星期  
  
-   幾個月會保留最後一個月的備份，：6 個月  
  
 如需變更備份保留原則，[變更電腦備份保留原則](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md#BKMK_5)。  
  
#### <a name="to-run-the-client-backup-database-cleanup-task"></a>若要執行 client 備份資料庫清理  
  
1.  伺服器管理員使用者與密碼登入。  
  
2.  在伺服器上，請打開**工作排程器**。 您可以存取工作排程器程式從**系統管理工具]**主機。  
  
3.  展開 [工作排程器，**工作排程器程式庫**，展開**Microsoft**，展開 [ **Windows**，然後按一下**Windows Server Essentials**。  
  
4.  按一下**備份清理**工作，並再按**執行**中**控制項**窗格。 若要變更的狀態**執行**直到完成工作。  
  
##  <a name="BKMK_11"></a>在工作列 client 的電腦上檢視警示  
 您可以在您的電腦上收到備份通知工作列，原因如下：  
  
-   開始進行備份。  
  
-   備份結束。  
  
-   不成功的最近備份。 在通知中包含上次成功備份的天數。  
  
-    僅限 Windows Server Essentials：新的磁碟區中新增了您的電腦。 管理您的網路的人需要執行自訂備份精靈包含或排除未來備份的音量。  
  
##  <a name="BKMK_12"></a>從 Launchpad 檢視備份狀態  
 使用 Launchpad 檢視快速狀態備份您的電腦。  
  
> [!TIP]
>  快速狀態，請將游標移到桌面的通知區域中的 Launchpad 圖示。 備份狀態會在工具提示中顯示。  
  
#### <a name="to-view-status-of-a-backup-for-your-computer"></a>若要檢視備份您的電腦狀態  
  
1.  若要登入**Launchpad**您的使用者名稱與密碼。  
  
2.  按一下**備份**。  
  
3.  在**備份屬性**對話方塊中，在**備份狀態**備份的狀態，區段會顯示。  
  
    -   **不先前的備份**:「備份尚未執行，或已備份歷史。  
  
    -   **備份擱置中的**：備份正在等待輸入其他處理程序完成伺服器上。  
  
    -   **連接**：備份連接到伺服器。  
  
    -   **無法連接**：備份無法連接到 Windows Server Client 電腦備份提供者服務。  
  
        > [!NOTE]
        >  在 Windows Server Essentials，這項服務已重新命名 Windows Server Essentials Client 電腦管理服務。  
  
    -   **進行中備份**：顯示百分比完成備份。  
  
    -   **已取消備份**：顯示日期和時間，如果您或網路系統管理員取消備份，開始進行備份。  
  
    -   **已成功備份**：顯示日期和時間備份開始是否已成功完成備份。  
  
    -   **備份不完整**：顯示日期和時間，如果備份未成功完成，開始進行備份。  
  
    -   **備份未成功**：顯示日期和時間，如果備份未成功完成，開始進行備份。  
  
4.  按一下**[確定]**以關閉 [**屬性的備份**對話方塊。  
  
##  <a name="BKMK_13"></a>停止 Launchpad 進行備份  
 您可以輕鬆地停止正在進行備份。  
  
#### <a name="to-stop-a-backup-in-progress"></a>若要停止在進行中備份  
  
1.  開放**Launchpad**。  
  
    > [!NOTE]
    >  如果系統要求，登入**Launchpad**您的使用者名稱與密碼。  
  
2.  按一下**備份**。  
  
3.  在**備份屬性**對話方塊中，在**備份狀態**一節，**開始備份**按鈕會變成**停止備份**備份時進行中。 按一下**停止備份**，然後按一下 [**是**來確認。 備份將會繼續執行，直到您按一下**[是]**。  
  
4.  當您要停止在進行備份時，狀態變更為**取消備份**的日期和時間的開始進行備份。 如果您看到的狀態，**成功**，**完整**，或**失敗**，最後一次備份已經完成。  
  
5.  按一下**[確定]**以關閉 [**屬性的備份**對話方塊。  
  
##  <a name="BKMK_14"></a>從 Launchpad 開始建立的備份  
 有時候可能會當您想要備份您的檔案和資料夾前定期排定的備份時間設定您的伺服器上。 Launchpad 可讓您以手動起始備份您的電腦。  
  
#### <a name="to-start-a-backup"></a>若要開始備份  
  
1.  開放**Launchpad**。  
  
    > [!NOTE]
    >  如果系統要求，登入**Launchpad**您的使用者名稱與密碼。  
  
2.  按一下**備份**。  
  
3.  在**備份屬性**對話方塊中，在**備份狀態**區段中，按一下 [**備份 [開始]**，，然後按一下**[確定]**。  
  
4.  輸入備份、描述，然後按一下**[確定]**。 狀態變更**開始備份**，並再**中處理程序備份**百分比完成。  
  
5.  按鈕變更之後備份開始，**停止備份**。 如果正在進行備份，您想要停止分享，請按一下**停止備份**，然後按一下 [**是**。 當您要停止在進行備份時，狀態變更為**取消備份**的日期和時間的備份開始使用。  
  
6.  當備份成功完成時，狀態變更為**備份成功**的日期和時間的開始完成的備份。  
  
7.  按一下**[確定]**以關閉 [**屬性的備份**對話方塊。  
  
##  <a name="BKMK_15"></a>備份電腦的運作方式  
 在 [備份期間每天可以下列項目：  
  
-   備份電腦向上逐一。  
  
-   正在進行備份完成時，即使您關閉備份的視窗。  
  
-   安裝 Windows 更新，以及 Windows Server Essentials 重新開機（如果需要的話）。  
  
-   在星期日備份清理執行。  
  
> [!IMPORTANT]
>  磁碟區陰影複製服務 (VSS) 不支援相同的快照設定中建立陰影複製 virtual 磁碟區及主機磁碟區。 備份 virtual 磁碟區是否需要 VSS 並 virtual 硬碟上 (VHD)，支援建立的磁碟區的快照。 如需詳細資訊，請查看[的服務和備份上 Virtual 硬碟](https://go.microsoft.com/fwlink/p/?LinkId=256577)。  
  
##  <a name="BKMK_16"></a>若要防止損壞 client 備份資料庫因為資料遺失的秘訣  
 如果 client 備份資料庫損壞，您可能會遺失重要的資料。  
  
 若要防止損壞 client 備份資料庫因為遺失的資料，請考慮將：  
  
-   讓備份 client 備份資料庫的其他方法。 例如，根據您正在執行 Windows Server Essentials 的版本，使用伺服器備份、Online Backup 或 Microsoft Azure 備份備份資料庫。  
  
    > [!IMPORTANT]
    >  請確定您備份的所有檔案，選取**Client 電腦備份**資料夾。  
  
-   您必須還原資料庫，請務必還原中的所有檔案**Client 電腦備份**資料夾。 如果您無法還原的所有檔案，可能不一致資料庫。  
  
-   在您清除或還原備份 client 資料庫之前，請確定要停止目前正在進行中的所有 client 備份。  
  
     如果您執行 Windows Server Essentials，您也應該停止 Windows Server Client 電腦備份服務及 Windows Server Client 電腦備份提供者服務。  
  
     如果您執行 Windows Server Essentials，您也應該停止 Windows Server 電腦備份服務。  
  
     完成後還原操作，重新開機服務。  
  
##  <a name="BKMK_17"></a>從電腦 client 的備份還原檔案或資料夾  
 您可以瀏覽，並從備份還原個人檔案和資料夾。  
  
> [!NOTE]
>  您無法還原的檔案和資料夾 client 電腦使用儀表板伺服器上。 您必須開放儀表板 client 的電腦上以完成任務。  
  
> [!IMPORTANT]
>  您無法還原的檔案和資料夾硬碟已設定為動態磁碟。  
  
#### <a name="to-restore-files-and-folders"></a>若要還原的檔案和資料夾  
  
1.  開放**儀表板**client 的電腦上。  
  
2.  按一下**裝置**索引標籤。  
  
3.  按一下您要還原的檔案和資料夾，然後按一下 [的電腦]**還原的檔案或資料夾的電腦**在**工作**窗格。 還原的檔案或資料夾精靈會開啟。  
  
4.  請依照精靈中的指示進行。  
  
##  <a name="BKMK_FileHistory"></a>了解檔案歷史  
 Windows Server Essentials 的檔案歷史功能會自動備份的電腦已檔案歷史功能的媒體櫃、連絡人、桌面和 [我的最愛] 資料夾中的檔案。 如果遺失，損壞或刪除原始檔案發生問題，您可以將它們還原。 您也可以找到您的檔案不同版本的特定點的時間。 隨著時間，您將必須歷史完成您的檔案。  
  
 Windows Server Essentials，您可以自訂檔案歷史設定從**檔案歷史**頁面上的**Client 電腦備份設定和工具**。  
  
 在 Windows Server Essentials，您可以自訂檔案歷史設定，請移至**使用者**索引標籤，然後選取 [**變更設定檔歷史**工作。  
  
 檔案歷史頁面提供下列選項：  
  
|備份設定|預設值|描述|  
|--------------------|-------------|-----------------|  
|打開/關閉|在|檔案歷史已在 [預設當您安裝 Windows Server Essentials。|  
|備份資料|文件和桌面|有三種預先設定的設定，讓您指定備份方案的不同。 您可以選擇其中一項下列選項：<br /><br /> -文件和桌面<br /><br /> -全部媒體櫃、桌面、連絡人和 [我的最愛]<br /><br /> 的媒體櫃、桌面、連絡人及排除資料音樂、影片和圖片媒體櫃中的 [我的最愛] 中所有的資料|  
|備份頻率|每小時|指定頻率檔案歷史建立的備份選取的資料。 您可以從中數個選項每天該範圍的每一個 10 分鐘。|  
|保留針對|1 年|指定的時間，使檔案歷史一份備份。|  
  
 檔案歷史問題的相關資訊，請查看[疑難排解檔案歷史](../support/Troubleshoot-File-History-in-Windows-Server-Essentials.md)。  
  
## <a name="see-also"></a>也了  
  
-   [管理備份與還原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
