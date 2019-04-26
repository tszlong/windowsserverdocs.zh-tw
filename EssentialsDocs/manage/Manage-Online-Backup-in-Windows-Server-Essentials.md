---
title: 在 Windows Server Essentials 中管理線上備份
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95a9f593-fad7-4335-bd4d-c7bb8c033efb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 37d59d500a2de1e2b98c848e7484ae13639d09b7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890719"
---
# <a name="manage-online-backup-in-windows-server-essentials"></a>在 Windows Server Essentials 中管理線上備份

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
  
 使用 Microsoft Azure 備份，整合之後**線上備份**管理頁面會出現在 Windows Server Essentials 儀表板。 [線上備份]  頁面可讓您執行一般系統管理工作。 [線上備份] 頁面上的功能包括：  
  
-   下列子區段頁面：  
  
    -   **線上備份** 在您登錄要進行線上備份的伺服器之後，這個區段就會顯示目前的備份狀態、存放裝置狀態，以及帳戶資訊。  
  
    -   **受保護的資料夾**此區段會列出所有共用的資料夾和所有檔案歷程記錄資料夾的伺服器上，以及任何其他您已選擇要在 Azure 中備份的資料夾。 這份清單包括每個共用資料夾的資料夾名稱、資料夾路徑及狀態。  
  
    -   **備份歷程記錄** 這個區段會顯示新近的線上備份清單。 這份清單包括每個備份的作業類型及時間和狀態。  
  
-   一個含有所選備份工作或還原工作的其他相關資訊的詳細資料窗格。  
  
-   一個包含一組您可執行之系統管理工作的工作窗格。  
  
 最佳做法是備份最重要的商務和使用者資料。 例如，您應該備份包含重要資料檔案的伺服器資料夾。 您還應該備份包含重要資訊之網路電腦的「檔案歷程記錄」。  
  
 在決定要線上備份哪些資料時，請考量您想要實作的備份頻率和保留原則。 然後請審查您的預算，判斷您能夠負擔的儲存空間大小。 根據您的需求來權衡成本和儲存容量，然後設定線上備份來儘可能保護最多的重要資料。 如需定價資訊，請參閱 [Azure 備份定價詳細資料](https://azure.microsoft.com/pricing/details/backup/)。  
  
 下列各節說明可能出現在 [Windows Server Essentials 儀表板] 中的各種線上備份工作。  
  
## <a name="online-backup-tasks-in-the-dashboard"></a>儀表板中的線上備份工作  
  
-   [將憑證上傳至 Azure 備份保存庫](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [設定線上備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [開始線上備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [從線上備份還原檔案和資料夾](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [註冊此伺服器以進行備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [取消註冊伺服器](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
###  <a name="BKMK_1"></a> 將憑證上傳至 Azure 備份保存庫  
 您可以使用 Azure 備份進行 Windows Server Essentials 中的線上備份之前，您必須上傳公開憑證以向備份保存庫註冊。 憑證用來驗證 Azure 備份部署 （代理程式），代表 Microsoft Online Services 訂閱擁有者，來管理訂用帳戶相關聯的資源。  
  
> [!NOTE]
>  在上傳憑證之前，您必須完成 [Sign up for Azure Backup Service](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)中的程序。  
  
##### <a name="to-upload-a-certificate-to-use-with-the-azure-backup-service"></a>上載要在 Azure 備份服務使用的憑證  
  
1.  使用系統管理員帳戶來登入 [Windows Server Essentials 儀表板]。  
  
2.  在 [儀表板] 的 [首頁]  上，按一下 [線上備份] 。  
  
3.  在 [線上備份] 區域中，按一下 [將憑證上傳到 Azure 備份保存庫]。  
  
     這會開啟**復原服務**在 Azure 管理入口網站。 如果您不 t 已登入 Azure，您必須使用您的 Microsoft 帳戶登入。  
  
4.  按一下將用來進行線上備份的備份保存庫名稱，以開啟該備份保存庫的 [快速入門]  頁面。  
  
5.  按一下 [管理憑證]。  
  
6.  在 [管理憑證]  對話方塊中，貼上 Windows Server Essentials 所產生之公開憑證的路徑。 若要取得公開憑證的路徑，請執行下列動作：  
  
    1.  在 [Windows Server Essentials 儀表板] 上，按一下 [線上備份]  索引標籤。  
  
    2.  在 [線上備份]  頁面上，複製所產生之憑證的路徑。  
  
    3.  切換至 Azure 管理入口網站，然後在**管理憑證**對話方塊方塊中，貼上路徑來上傳所產生的公開憑證。  
  
    > [!NOTE]
    >  您也可以使用自己的公開憑證。 若要知道需要什麼憑證，請在 [快速入門] 頁面上，按一下 [取得憑證] 連結。  
  
    > [!NOTE]
    >   Azure 會要求具有公開金鑰的 x.509 v2 憑證。 如需詳細資訊，請參閱 [管理保存庫憑證](https://msdn.microsoft.com/library/azure/dn169036.aspx)。  
  
7.  選取憑證之後，請按一下 [確定]  (核取記號)。  
  
8.  您會返回 [快速入門]  頁面。 請按一下 [儀表板] ，並確認已成功上傳憑證。 成功上傳憑證之後，儀表板會顯示憑證指紋和到期日。  
  
 完成這個程序之後，請執行下列動作：  
  
1.  使用 Azure 備份服務註冊伺服器。 如需詳細資訊，請參閱[登錄這部伺服器來進行備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)。  
  
2.  設定伺服器的線上備份。 如需詳細資訊，請參閱 [Configure online backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
###  <a name="BKMK_2"></a> 設定線上備份  
 使用 Azure 備份註冊伺服器之後，您可以在 Windows Server Essentials 中設定線上備份設定。  
  
##### <a name="to-configure-online-backup"></a>設定線上備份  
  
1.  從 [登錄您的伺服器] 精靈或從 [Windows Server Essentials 儀表板] 的 [線上備份] 頁面，按一下 [設定線上備份]。 [設定線上備份精靈] 隨即出現。  
  
2.  在 **設定線上備份**頁面的精靈中，選取您想要備份至 Azure 備份每個伺服器資料夾的核取方塊。 將您不想要包含在備份中的每個伺服器資料夾核取方塊取消選取。 若要新增未顯示在清單中的資料夾，請按一下 [新增資料夾] 。 完成選取之後，按 [下一步]。  
  
    > [!NOTE]
    >  您無法選取不是位於本機伺服器上的資料夾，或是格式化為 ReFS 之磁碟機上的資料夾。  
  
3.  在精靈的 [新增檔案歷程記錄備份] 頁面上，您可以選取要將支援「檔案歷程記錄」功能之網路電腦的「檔案歷程記錄備份」包含在內。 然後按 [下一步] 以繼續作業。  
  
4.  在精靈的 [指定備份排程] 頁面上，進行下列設定，然後按 [下一步] 以繼續作業：  
  
    -   選取或接受您要執行線上備份的每日時間。 預設時間是下午 10:00。 您也可以指定執行第二個備份的時間。  
  
    -   選取或接受您要執行線上備份的日子。 線上備份預設會在星期一到星期五執行。  
  
5.  在精靈的 [指定備份保留原則]  頁面上，選取您想要保留線上備份的天數，然後按 [下一步] 。 預設值是 7 天。 您也可以選擇保留 15 或 30 天的線上備份。  
  
    > [!NOTE]
    >   Azure 備份一律會保留最新備份。 如果備份目的地沒有足夠的空間可供儲存備份，備份程序將不會成功。 為了避免這種情況，請購買額外的儲存空間，或縮短資料保留期間。 如需定價資訊，請參閱[定價詳細資料](https://azure.microsoft.com/pricing/details/backup/)適用於 Microsoft Azure 備份。  
  
6.  在精靈的 [選擇您的頻寬使用量]  頁面上，如果您想要限制配置給線上備份的網際網路頻寬多寡，請選取 [啟用網際網路頻寬使用量] 。 請使用這個頁面上的選項，指定在工作時間和非工作時間，要允許線上備份使用多少網際網路頻寬。 然後定義您的上班時間和工作日。  
  
    > [!NOTE]
    >   Azure 備份可讓您自訂整合軟體如何利用備份或還原資訊時的網路頻寬。 使用技術通常稱為節流，您就可以控制可供備份所使用的網路頻寬的數量，並還原特定的日期和時間間隔期間的程序。 在 [設定線上備份精靈] 中選取 [啟用網際網路頻寬使用量] 核取方塊之後，您便可以指定要套用工作時間頻寬限制的工作日和工作小時。 在所有其他時間則會使用非工作時間限制。 這兩個限制的有效頻寬範圍都是從 256 Kbps 到無限制。  
  
7.  完成線上備份設定時，按一下 [關閉]。  
  
    > [!TIP]
    >  在成功設定備份之後，[線上備份]  頁面會顯示最新的線上備份狀態，以及備份保存庫所使用的儲存空間大小。 若要查看先前的備份狀態，請按一下 [備份歷程記錄] 。  
  
###  <a name="BKMK_3"></a> 開始線上備份  
  
> [!NOTE]
>  在可以開始線上備份之前，您必須先進行 [Register this server for backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)，然後是 [Configure online backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
##### <a name="to-start-an-online-backup-immediately"></a>立即開始線上備份  
  
1.  以系統管理員身分登入 [儀表板]。  
  
2.  在瀏覽列上，按一下 [線上備份] 。  
  
3.  在 [線上備份工作] 窗格中，按一下 [立即開始備份]。  
  
###  <a name="BKMK_4"></a> 從線上備份還原檔案和資料夾  
 [還原檔案和資料夾精靈] 會引導您完成尋找、選取及還原線上備份的檔案和資料夾的程序。 在逐步執行精靈步驟時，您將執行下列工作：  
  
1.  **選擇要還原檔案和資料夾從線上備份**  
  
     執行 [還原檔案和資料夾精靈] 時，您必須執行的第一件事，就是指定您是要從目前登入之伺服器的線上備份還原檔案，還是要從不同伺服器的備份還原檔案。  
  
2.  **選擇要從中還原檔案和資料夾的伺服器**  
  
     如果您選擇要從不同的伺服器還原檔案和資料夾，請從可用伺服器清單中選取要做為還原來源的伺服器，然後按 [下一步]。  
  
3.  **選擇包含的檔案和資料夾還原的磁碟區**  
  
     線上備份包含與備份來源伺服器上的磁碟區對應的磁碟區。 選取磁碟區之後，請選取做為還原來源之備份的日期和時間，然後按 [下一步] 。  
  
4.  **選取要還原檔案和資料夾**  
  
     在精靈的 [選取要還原的項目] 頁面上，您可以開啟各種不同的資料夾位置，然後選取您要還原的每個檔案或資料夾的核取方塊。 完成檔案選取之後，請按 [下一步]。  
  
5.  **指定還原的檔案與資料夾的目的地位置**  
  
     精靈的 [指定還原選項]  頁面可讓您指定要還原檔案和資料夾的位置。 有兩個選項：  
  
    -   **還原至原始位置**。 若要將檔案和資料夾還原至備份來源處的完全相同位置，請選取這個選項。  
  
    -   **還原至不同位置**。  若要指定伺服器上的不同位置來還原檔案，請選擇這個選項。  
  
         在預設安裝中，如果目的地位置已經有與還原檔案同名的檔案，精靈就會建立原始檔案的複本。 此外，存取控制清單 (ACL) 也會更新以反映目前的檔案權限。 您可以按一下 [進階] 來自訂預設設定。  
  
6.  **確認還原資訊**  
  
     [確認您的還原資訊]  頁面提供您所指定之還原指示的摘要。 若要繼續進行檔案還原，您必須輸入您線上備份帳戶的正確複雜密碼。  
  
###  <a name="BKMK_5"></a> 註冊此伺服器以進行備份  
 若要備份或還原至 Azure 備份的檔案、 資料夾和檔案歷程記錄，您的 Windows Server Essentials 伺服器上，您必須先使用 Microsoft Azure 備份服務註冊您的伺服器。  
  
> [!NOTE]
>  在登錄您的伺服器之前，您必須先上傳要在將存放線上備份之備份保存庫使用的憑證。 如需詳細資訊，請參閱[將憑證上傳到 Azure 備份保存庫](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)。  
  
##### <a name="to-register-your-server-with-azure-backup"></a>向 Azure 備份登錄您的伺服器  
  
1.  以系統管理員身分登入伺服器，然後開啟 [儀表板]。  
  
2.  在瀏覽列上，按一下 [線上備份] 。  
  
3.  在 [線上備份] 子區段中，按一下 [登錄]。  
  
4.  選擇要在您線上備份的備份保存庫使用的憑證。 預設會選取預設憑證；如果您想要選擇不同的憑證，請使用 [瀏覽] 。 然後按一下 [下一步] 。  
  
5.  依照精靈中的指示來建立複雜密碼，然後完成登錄。  
  
###  <a name="BKMK_6"></a> 取消註冊伺服器  
  
> [!CAUTION]
>  如果您取消註冊您的 Windows Server Essentials 伺服器從 Microsoft Azure 備份服務，Azure 備份可以不再會備份伺服器。 此外，先前上傳的伺服器資料也將被清除。 若要繼續線上備份，您必須重新登錄伺服器。  
  
##### <a name="to-unregister-your-server-with-azure-backup"></a>將您的伺服器從 Azure 備份解除登錄  
  
1.  登入 [Azure 管理入口網站](https://manage.windowsazure.com)。  
  
2.  按一下 [復原服務] 。  
  
3.  在 [復原服務] 中，按一下備份保存庫的名稱。  
  
4.  從保存庫的 [快速入門]  頁面，按一下 [伺服器] 。  
  
5.  選取伺服器，按一下 [刪除] ，然後在出現確認提示時按一下 [是]  。  
  
## <a name="related-tasks"></a>相關的工作  
  
-   [變更線上備份原則](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [檢視線上備份儲存體使用量](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [在 線上備份中包含新的資料夾](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [移除，或從線上備份原則中排除檔案歷程記錄備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [停用或重新啟用線上伺服器備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [停止進行中的線上伺服器備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [檢視線上備份狀態](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [檢視及管理線上備份警示](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [線上備份重設為預設設定](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [註冊 Azure 備份服務](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [整合 Windows Server Essentials 中的 Azure 備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [保護 Windows Server Essentials 中的線上備份的資料夾](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_18)  
  
-   [Windows Server Essentials 中的線上備份歷程記錄](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_19)  
  
###  <a name="BKMK_7"></a> 變更線上備份原則  
 使用 [Windows Server Essentials 儀表板] 即可輕鬆對線上備份原則進行變更。  
  
##### <a name="to-change-the-online-backup-policy"></a>變更線上備份原則  
  
1.  以系統管理員身分登入 [儀表板]。  
  
2.  在瀏覽列上，按一下 [線上備份] 。  
  
3.  在 [線上備份工作]  窗格中，按一下 [設定線上備份] 。  
  
4.  依照精靈中的指示來自訂線上備份原則。  
  
 如需可自訂之設定的詳細資訊，請參閱 [Configure online backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
###  <a name="BKMK_8"></a> 檢視線上備份儲存體使用量  
  
##### <a name="to-view-the-amount-of-storage-space-that-online-backup-uses"></a>檢視線上備份所使用的儲存空間大小  
  
1.  以系統管理員身分登入 [儀表板]。  
  
2.  在瀏覽列上，按一下 [線上備份] 。  
  
3.  在 [線上備份]  索引標籤上，[存放裝置狀態]  會出現在資訊窗格中。  
  
###  <a name="BKMK_9"></a> 在 線上備份中包含新的資料夾  
  
##### <a name="to-include-a-new-folder-in-the-online-backup-policy"></a>在線上備份原則中納入新資料夾  
  
1.  以系統管理員身分登入 [儀表板]。  
  
2.  在瀏覽列上，按一下 [線上備份] 。  
  
3.  在 [線上備份工作]  窗格中，按一下 [設定線上備份] 。  
  
4.  在 [變更或移除備份原則] 頁面上，按一下 [自訂線上備份原則]。  
  
5.  在 [設定線上備份]  頁面中，如果清單中沒有您想要納入的資料夾，請按一下 [新增資料夾] 。  
  
6.  在 [新增資料夾]  視窗中，瀏覽並選取您想要納入線上備份的資料夾，然後按一下 [確定] 。  
  
7.  在 [設定線上備份]  頁面上，視需要選取其他要納入的資料夾，然後按 [下一步] 。  
  
8.  依照精靈中的指示來完成線上備份原則自訂。  
  
 如需可自訂之其他設定的詳細資訊，請參閱 [Configure online backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
###  <a name="BKMK_10"></a> 移除，或從線上備份原則中排除檔案歷程記錄備份  
  
##### <a name="to-remove-or-exclude-a-folder-from-the-online-backup-policy"></a>將資料夾從線上備份原則中移除或排除  
  
1.  以系統管理員身分登入 [儀表板]。  
  
2.  在瀏覽列上，按一下 [線上備份] 。  
  
3.  按一下 [受保護資料夾]  索引標籤。詳細資料窗格會顯示一份資料夾及其線上備份狀態的清單。  
  
4.  選取您想要從線上備份原則中排除的資料夾，然後在工作窗格中，按一下 [將資料夾從線上備份中移除] 。  
  
###  <a name="BKMK_11"></a> 停用或重新啟用線上伺服器備份  
 如需有關如何使用 Azure Backup 備份或還原伺服器資料的相關指示，請參閱[登錄這部伺服器來備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)。  
  
 如需有關如何停止使用 Azure 備份來備份或還原伺服器資料的指示，請參閱[解除伺服器登錄](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6)。  
  
###  <a name="BKMK_12"></a> 停止進行中的線上伺服器備份  
  
##### <a name="to-stop-an-online-server-backup-in-progress"></a>停止進行中的線上伺服器備份  
  
1.  以系統管理員身分登入 [儀表板]。  
  
2.  在瀏覽列上，按一下 [線上備份] 。  
  
3.  在 [線上備份工作]  窗格中，按一下 [停止線上備份] 。  
  
     停止備份之後，[備份歷程記錄]  清單中備份的狀態就會顯示為 [已取消]  。  
  
###  <a name="BKMK_13"></a> 檢視線上備份狀態  
  
##### <a name="to-view-the-backup-status"></a>檢視備份狀態  
  
1.  以系統管理員身分登入 [儀表板]。  
  
2.  在瀏覽列上，按一下 [線上備份] 。  
  
3.  按一下 [備份歷程記錄]  索引標籤。清單檢視會顯示每個備份工作的狀態。 選取備份工作以檢視有關該工作的其他詳細資料。  
  
###  <a name="BKMK_14"></a> 檢視及管理線上備份警示  
 如同許多其他警示，Azure 備份的警示會顯示在 警示檢視器。  
  
##### <a name="to-view-online-backup-alerts-in-the-alert-viewer"></a>檢視警示檢視器中的線上備份警示  
  
1.  開啟 [儀表板]。  
  
2.  執行下列其中一項：  
  
      Windows Server Essentials:在 [導覽] 窗格中，按一下警示圖示\(可能是重大、 警告還是資訊\)。 這會開啟 [警示檢視器]。  
  
      Windows Server Essentials:在 [首頁] 上，按一下 [健康情況監視] 索引標籤。  
  
3.  檢閱之 Azure 備份相關的問題的警示清單。  
  
 如需使用 [警示檢視器] 或 [健康情況監視] 索引標籤來管理警示的詳細資訊，請參閱[Manage System Health](Manage-System-Health-in-Windows-Server-Essentials.md)。  
  
###  <a name="BKMK_15"></a> 線上備份重設為預設設定  
 Windows Server Essentials 提供一個可協助您設定線上備份設定的精靈。 如果您想要還原預設設定，請執行 [設定線上備份] 工作，並選擇 [移除線上備份原則] 選項。 然後，重新執行 [設定線上備份] 工作。 您先前上傳的資料會保持不變。  
  
###  <a name="BKMK_16"></a> 註冊 Azure 備份服務  
 若要準備 Windows Server Essentials 整合 Microsoft Azure 備份，您會使用您的 Microsoft Online Services 帳戶，登入 Azure 管理入口網站，然後建立 在 Azure 中儲存您的線上備份的備份保存庫。 然後，您會下載 Azure 備份整合模組，並使用下載的檔案在 Windows Server Essentials 伺服器上安裝 Azure 備份增益集。 如果您沒有 Microsoft 帳戶，您可以註冊申請免費試用。  
  
 若要執行這項設定，請完成下列工作：  
  
1.  註冊 Microsoft Online Services 帳戶和備份預覽。  
  
2.  建立備份保存庫來儲存線上備份。  
  
3.  下載 Azure Backup Agent。  
  
4.  在伺服器上安裝 Azure 備份增益集。  
  
####  <a name="BKMK_SignupforaMicrosoftOnlineServiceAccount"></a> 註冊 Microsoft Online Services 帳戶和備份預覽  
  
1.  登入 [Windows Server Essentials 儀表板]。  
  
2.  在 [儀表板] 的 [首頁] 上，按一下 [增益集] 類別，按一下 [與 Azure 備份整合]，然後按一下 [按一下以註冊 Azure 備份]。  
  
3.  在 Azure 上**復原服務**頁面上，於**備份 （預覽）** 區段中，檢閱詳細資料。  
  
4.  如果您沒有 Azure 訂用帳戶，請按一下**免費試用**，然後依照指示來取得 Azure 訂用帳戶。  
  
     如果您已經有 Azure 訂用帳戶，請按一下**入口網站**来移至 Azure 管理入口網站的 web 網頁右上角。  
  
5.  在 Azure 管理入口網站頁面上，您會看到**復原服務**的左窗格中。 S 您將在其中管理備份保存庫該存放區線上備份從 Windows Server Essentials。  
  
####  <a name="BKMK_Createabackupvaulttostoreonlinebackups"></a> 建立備份保存庫來儲存線上備份  
  
1.  從您 Windows Server Essentials 伺服器上的網頁瀏覽器登入 [Azure 管理入口網站](https://manage.windowsazure.com)。  
  
2.  在 Azure 管理入口網站中，按一下**新增**，按一下**Data Services**，按一下 **復原服務**，按一下 **備份保存庫**，然後按一下 **快速建立**。  
  
3.  輸入您備份保存庫的名稱，選擇您想要儲存備份的地區，然後按一下 [快速建立] 。  
  
     這會開啟 [復原服務] 區域，當中顯示您的新備份保存庫。  
  
####  <a name="BKMK_DownloadtheWindowsAzureBackupAgent"></a> 下載 Azure Backup Agent  
  
1.  開啟 [Windows Server Essentials 儀表板]。  
  
2.  在 [儀表板] 的 [首頁]  上，依序按一下 [開始使用]  索引標籤、[增益集]  類別、[與 Azure 備份整合] ，然後按一下 [按一下以下載 Azure 備份整合模組] 。  
  
####  <a name="BKMK_InstalltheWindowsAzureBackupAddIn"></a> 在伺服器上安裝 Azure 備份增益集  
  
1.  使用系統管理員帳戶來登入您的伺服器，然後執行您在前一個步驟中下載的 **OnlineBackupAddin.wssx** 檔案。  
  
2.  這會顯示 [軟體授權條款]  。 如果您同意這些條款及條件，請按一下 [接受] 來繼續安裝。  
  
3.  在 [安裝增益集]  頁面上，按一下 [安裝增益集] 。  
  
    > [!NOTE]
    >  您可能必須重新啟動伺服器，才能安裝任何先決條件軟體。  
  
     [安裝] 頁面隨即顯示。 安裝開始時，會顯示進度列指示器，並會顯示安裝的進度。 安裝完成時，您會收到一則訊息，Azure 備份增益集已成功安裝。  
  
4.  按一下 **[完成]**。  
  
5.  關閉並重新開啟 [儀表板]。  
  
     [儀表板] 中會新增一個 [線上備份] 新索引標籤。 從這個索引標籤中，您可以註冊您的伺服器、 設定備份設定，並開啟**復原服務**在 Azure 管理入口網站來管理您伺服器的備份保存庫。  
  
 完成這些步驟之後，請執行下列動作：  
  
1.  在 Windows Server Essentials 中上, 傳要用於線上備份的憑證。 如需詳細資訊，請參閱[將憑證上傳到 Azure 備份保存庫](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)。  
  
2.  使用 Azure 備份保存庫註冊伺服器。 如需詳細資訊，請參閱[登錄這部伺服器來進行備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)。  
  
3.  設定伺服器的線上備份。 如需詳細資訊，請參閱 [Configure online backup](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
###  <a name="BKMK_17"></a> 整合 Windows Server Essentials 中的 Azure 備份  
 Microsoft Azure 備份整合模組是可讓您加密並備份檔案和資料夾從伺服器到 Microsoft 所提供的 Azure 裝載的儲存體系統的 Windows Server Essentials 的新功能。 您可以藉由使用 Azure 備份加密並備份伺服器上的資料，協助防止因火災、 水災、 遭竊或其他災害的重要商務資料發生災難性的損失。 當您使用 Azure 備份來備份伺服器資料時，它會上傳到安全的資料中心，在網際網路上之前，請使用您的複雜密碼加密的資訊。 若要從線上備份存取資料，您必須有一部經過憑證驗證的伺服器，並且必須提供複雜密碼。  
  
 整合並向 Azure Backup 註冊伺服器之後, 您可以設定線上備份設定來執行定期排定的備份。 您也可以在 [線上備份儀表板] 中按一下 [立即開始備份]  工作，來隨時起始線上備份。  
  
 線上備份設定包含下列步驟：  
  
1.  [註冊 Azure 備份服務](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)-登入之後註冊備份服務時，您將建立備份保存庫、 下載並安裝 Azure 備份整合模組。  
  
2.  [將憑證上傳至 Azure 備份保存庫](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
3.  [註冊此伺服器以進行備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
4.  [設定線上備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
> [!NOTE]
>   Azure 備份會使用複雜密碼來加密線上備份的檔案和資料夾。 變更加密複雜密碼會取代您在登錄伺服器時所指定的複雜密碼。 複雜密碼只接受 ASCII 編碼的字元。  
  
###  <a name="BKMK_18"></a> 保護 Windows Server Essentials 中的線上備份的資料夾  
 [儀表板] 之 [線上備份] 區段的 [受保護資料夾]  子區段中會顯示伺服器上所有共用資料夾的清單。 下表說明清單中所包含的資訊。  
  
|Column|描述|  
|------------|-----------------|  
|**資料夾名稱：**|包含在線上備份中的資料夾名稱。<br /><br /> 若要新增或排除某個資料夾，請執行 [設定線上備份] 工作。|  
|**資料夾路徑：**|資料夾的位置。|  
|**狀態:**|有三種狀態**Protected**，**未受保護**，並**未知**。|  
  
###  <a name="BKMK_19"></a> Windows Server Essentials 中的線上備份歷程記錄  
 [儀表板] 之 [線上備份] 區段中的 [備份歷程記錄] 子區段會顯示最近的線上備份清單。 您可以使用成功的備份來還原檔案和資料夾。 下表說明清單中所包含的資訊。  
  
|Column|描述|  
|------------|-----------------|  
|**作業：**|有兩種操作 - [備份]  和 [還原] 。|  
|**時間：**|這是最新狀態的記錄時間。|  
|**狀態:**|有五種狀態**成功**，**正在**， **Canceled**，**警告**，和**不成功**.|  
  
## <a name="see-also"></a>另請參閱  
  
-   [管理備份和還原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [Manage Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
