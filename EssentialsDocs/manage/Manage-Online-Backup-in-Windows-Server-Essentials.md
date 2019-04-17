---
title: "管理 Windows Server Essentials Online 備份"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="manage-online-backup-in-windows-server-essentials"></a>管理 Windows Server Essentials Online 備份

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集
  
 您整合 Microsoft Azure 備份之後, **Online Backup** Windows Server Essentials 儀表板中的管理頁面隨即顯示。 **Online Backup**頁面，允許執行一般管理工作。 在 [Online Backup 頁面上的功能包括：  
  
-   下列子區段頁面：  
  
    -   **Online Backup**您登記伺服器 online 備份之後，這個區段會顯示目前的備份狀態、存放裝置狀態，以及 account 資訊。  
  
    -   **保護資料夾**本節列出所有共用的資料夾和伺服器上的所有檔案歷史資料夾與任何其他您選擇備份 Azure 中的資料夾。 此清單會包括資料夾名稱、路徑資料夾，並分享的每個資料夾的狀態。  
  
    -   **備份歷史**這個區段會顯示一份 online 的最近備份。 此清單會包含作業的時間和每個備份狀態的類型。  
  
-   選取的備份或還原工作的其他資訊的詳細資料窗格。  
  
-   工作窗格包含設定的系統管理員可以執行的工作。  
  
 做為最佳做法，您應該備份最重要的企業和使用者資料。 例如，您應該備份伺服器包含重要的資料檔案的資料夾。 您也應該備份檔案歷史網路的電腦含有重要資訊。  
  
 當決定 online 備份資料，請考慮將您想要實作備份頻率及保留原則。 檢視您預算然後判斷您的儲存空間量。 平衡成本與您的需求，對儲存空間的磁碟區，然後設定 online 的備份，以盡量盡可能重要的資料保護。 價格資訊，請查看[價格詳細資料 Azure 備份的](https://azure.microsoft.com/pricing/details/backup/)。  
  
 下列章節描述 Windows Server Essentials 儀表板中可能會出現的各種 online 備份工作。  
  
## <a name="online-backup-tasks-in-the-dashboard"></a>儀表板 online 備份工作  
  
-   [若要備份 Azure 保存庫上傳憑證](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [設定 online 備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [開始 online 備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [從 online 備份還原的檔案和資料夾](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [登記此伺服器的備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [移除伺服器](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
###  <a name="BKMK_1"></a>若要備份 Azure 保存庫上傳憑證  
 您可以使用 Azure 備份 online 備份在 Windows Server Essentials 的之前，您必須上傳公用為隊伍登記備份保存庫的憑證。 憑證用於驗證 Azure 備份部署（代理人）代表 Microsoft Online Services 來管理裝機費相關聯的資源裝機費擁有者做。  
  
> [!NOTE]
>  您上傳憑證之前，您必須先完成中的程序[適用於 Azure 備份服務登入](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)。  
  
##### <a name="to-upload-a-certificate-to-use-with-the-azure-backup-service"></a>上傳 Azure 備份服務搭配使用的憑證  
  
1.  Windows Server Essentials 儀表板的系統管理員 account 登入。  
  
2.  儀表板**Home**頁面上，按**ONLINE BACKUP**。  
  
3.  在**ONLINE BACKUP**區域中，按**上傳憑證，以 Azure 備份保存庫**。  
  
     開啟**修復服務**在 [管理 Azure 入口網站。 如果您不 t 已登入 Azure，您將需要使用您的 Microsoft account 登入。  
  
4.  按一下您將會使用適用於 online 備份打開備份保存庫名稱**快速 [開始]**頁面備份保存庫。  
  
5.  按一下**管理憑證**。  
  
6.  在**管理憑證**對話方塊中，貼上的憑證公用路徑由 Windows Server Essentials。 若要取得公開憑證的路徑，執行下列動作：  
  
    1.  Windows Server Essentials 儀表板，請按一下**Online Backup**索引標籤。  
  
    2.  在**Online Backup**頁面上，複製產生憑證的路徑。  
  
    3.  切換至 Azure 管理入口網站，然後在**管理憑證**對話方塊中，貼上傳產生公用憑證的路徑。  
  
    > [!NOTE]
    >  您也可以使用您自己的公用憑證。 了解哪些憑證是必要的在**快速 [開始]**頁面上，按一下 [**取得憑證**連結。  
  
    > [!NOTE]
    >   Azure 需要 v2 x.509 公開的金鑰。 如需詳細資訊，請查看[保存庫管理的憑證](https://msdn.microsoft.com/library/azure/dn169036.aspx)。  
  
7.  您選擇憑證之後，請按一下**[確定]**（核取記號）。  
  
8.  您會回到**快速 [開始]**頁面。 按一下**儀表板**，並確認憑證已成功上載。 成功上傳憑證之後，儀表板會顯示憑證指紋和到期日期。  
  
 完成之後此程序，執行下列動作：  
  
1.  使用 Azure 備份服務登記伺服器。 如需詳細資訊，請查看[登記此伺服器備份的](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)。  
  
2.  設定伺服器的 online 備份。 如需詳細資訊，請查看[設定 online 備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
###  <a name="BKMK_2"></a>設定 online 備份  
 您對於備份 Azure 登記伺服器之後，您可以在 Windows Server Essentials 設定 online 備份設定。  
  
##### <a name="to-configure-online-backup"></a>若要設定備份 online  
  
1.  從登記您伺服器精靈，或從**Online 備份**頁面上的 Windows Server Essentials 儀表板，按一下 [**設定 online 備份**。 設定 Online 備份精靈會出現。  
  
2.  在**設定 Online Backup**頁面上的精靈中，選取您想要備份到 Azure 備份的伺服器每個資料夾的核取方塊。 清除不想要備份包含每個伺服器資料夾的核取方塊。 若要新增資料夾，不會顯示在清單中，按一下**[新增資料夾**。 當您完成您的選取項目時，請按一下**下一步**。  
  
    > [!NOTE]
    >  您無法選擇不在本機伺服器或為 ReFS 格式化的磁碟機上的資料夾。  
  
3.  在**新增檔案歷史備份**頁面上的精靈中，您可以選取網路電腦支援的檔案歷史功能，包含歷史檔案的備份。 然後按一下**下一步**以繼續。  
  
4.  在**指定備份排程**頁面上的精靈中，進行下列設定，然後按一下 [**下一步**繼續：  
  
    -   選取或接受您想要執行的 online 備份天的時間。 預設值是 10:00。 您也可以指定一次執行第二個備份。  
  
    -   選取或接受您想要執行的 online 備份的天。 根據預設，星期五每上執行 online 備份。  
  
5.  在**指定備份保留原則**頁面上的精靈中，選取您想要保留 online 備份，然後按一下 [天數**下**。 預設為 7 天。 您也可以選擇要保留 online 備份 15 或 30 天。  
  
    > [!NOTE]
    >   Azure 備份一律會保留您的最近備份。 如果不備份目的地不會有可用的備份儲存空間不足，備份程序就會失敗。 若要避免發生這種情形，購買額外的儲存空間，或是縮短資料保留期間。 適用於定價資訊，請查看[價格詳細資料](https://azure.microsoft.com/pricing/details/backup/)Microsoft Azure 備份。  
  
6.  在**選擇您的頻寬**頁面上的精靈中，如果您想要限制的網際網路頻寬配置給 online 備份，選取**支援的網際網路頻寬使用**。 用以指定允許小時與非工作小時的時間在工作時使用 online 備份量網際網路頻寬頁面上的選項。 定義您企業時間和工作天。  
  
    > [!NOTE]
    >   Azure 備份可讓您自訂整合軟體如何利用頻寬時備份或還原資訊。 使用程序稱為節流的技術，您可以控制的頻寬，可供使用的備份與還原處理程序期間特定的日期和時間間隔。 選取**讓的網際網路頻寬**核取方塊設定 Online Backup 精靈中，您可以指定工作天和工作小時期間所套用的頻寬工作小時的時間限制。 非工作時間限制。使用其他時間。 有效的頻寬範圍是從 256 Kbps 到無限制的這兩個限制。  
  
7.  Online 備份設定完成後，按一下 [**關閉**。  
  
    > [!TIP]
    >  成功備份設定之後，**Online 備份**頁面上顯示的最近備份 online 和備份保存庫所使用的儲存空間量。 若要查看先前的備份的狀態，請按一下**備份歷史**。  
  
###  <a name="BKMK_3"></a>開始 online 備份  
  
> [!NOTE]
>  可以開始 online 備份之前，您必須先[登記此伺服器備份的](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)，然後[設定 online 備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
##### <a name="to-start-an-online-backup-immediately"></a>若要立即開始 online 備份  
  
1.  以系統管理員身分登入儀表板。  
  
2.  在瀏覽列中，按一下 [ **ONLINE BACKUP**。  
  
3.  在**Online 備份工作**窗格中，按**開始備份**。  
  
###  <a name="BKMK_4"></a>從 online 備份還原的檔案和資料夾  
 還原的檔案和資料夾精靈會引導您尋找、選取，及還原的檔案和資料夾的 online 備份。 您將會為您播放精靈中，請執行下列工作：  
  
1.  **選擇要還原的檔案和資料夾 online 備份**  
  
     當您執行的還原的檔案和資料夾精靈時，第一件事時，您必須是指定您要從 online 備份您目前登入的伺服器或其他伺服器的備份還原檔案。  
  
2.  **選擇要還原的檔案和資料夾伺服器**  
  
     如果您選擇還原的檔案和資料夾從不同的伺服器，請選取您想要還原的在清單中可用的伺服器，然後按一下 [的伺服器**下一步**。  
  
3.  **選擇檔案和資料夾還原包含磁碟區**  
  
     Online 的備份包含對應至備份來源伺服器上的磁碟區的磁碟區。 選取 [磁碟區之後，選取的日期和時間的備份，用來還原、，然後按一下 [**下一步**。  
  
4.  **選取要還原檔案和資料夾**  
  
     在**還原來選取項目**頁面上的精靈中，您可以開放的各種不同的資料夾位置，然後選取每個檔案或資料夾還原到您想要的核取方塊。 當您完成後選取檔案，請按**下一步**。  
  
5.  **指定目的位置還原的檔案和資料夾**  
  
     **指定還原選項**頁面上的精靈可讓您指定要還原的檔案和資料夾的位置。 有兩個選項：  
  
    -   **還原至原始位置**。 選取要還原的檔案和資料夾起始備份的確切位置此選項。  
  
    -   **還原到不同的位置**。  選擇此選項，指定要還原的檔案伺服器上的不同的位置。  
  
         預設安裝，在是否具有相同名稱為檔案還原的檔案已存在的目的位置，精靈會建立原始檔案一份。 此外，已更新存取控制清單 (ACL)，以反映目前的檔案權限。 您可以按一下**進階**以自訂的預設設定。  
  
6.  **確認還原資訊**  
  
     **確認您還原資訊**頁面上提供還原指示您所指定的摘要。 若要繼續進行還原檔案，您必須 online 備份洽詢您的輸入正確的複雜密碼。  
  
###  <a name="BKMK_5"></a>登記此伺服器的備份  
 若要備份或還原的檔案、資料夾及 Windows Server Essentials 伺服器上的檔案歷史 Azure 備份，您必須使用 Microsoft Azure 備份服務第一次登記您的伺服器。  
  
> [!NOTE]
>  您登記伺服器之前，您必須上傳 online 的備份將會儲存的備份保存庫搭配使用的憑證。 如需詳細資訊，請查看[上傳憑證 Azure 備份保存庫以](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)。  
  
##### <a name="to-register-your-server-with-azure-backup"></a>到您的伺服器登記 Azure 備份  
  
1.  伺服器以系統管理員的身分登入並打開儀表板。  
  
2.  在瀏覽列中，按一下 [ **ONLINE BACKUP**。  
  
3.  在**Online Backup**子區段移動，按**登記**。  
  
4.  選擇您想要備份的保存庫 online 備份時要使用的憑證。 預設; 選取預設的憑證如果您想要選擇不同的憑證，使用**瀏覽]**。 然後按一下**下一步**。  
  
5.  請依照下列建立複雜密碼，然後完成登記精靈中的指示操作。  
  
###  <a name="BKMK_6"></a>移除伺服器  
  
> [!CAUTION]
>  如果您移除您的 Windows Server Essentials 伺服器從 Microsoft Azure 備份服務，Azure 備份可以不再備份伺服器。 此外，將會清除之前上傳 server 的資料。 若要繼續 online 備份，您必須再次登記伺服器。  
  
##### <a name="to-unregister-your-server-with-azure-backup"></a>若要移除您的伺服器 Azure 備份  
  
1.  若要登入[Azure 管理入口網站](https://manage.windowsazure.com)。  
  
2.  按一下**修復服務**。  
  
3.  在**修復服務**，按一下備份保存庫的名稱。  
  
4.  從**快速 [開始]**針對保存庫頁面上，按一下 [**伺服器]**。  
  
5.  選取的伺服器，請按一下**Delete**，然後按一下 [**是**確認的提示。  
  
## <a name="related-tasks"></a>相關的工作  
  
-   [變更 online 備份原則](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [檢視 online 備份存放裝置使用方式](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [Online 的備份包含新的資料夾](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [移除或檔案歷史備份排除 online 備份原則](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [停用或重新讓 online 伺服器備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [停止在進行中的 online 伺服器備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [檢視 online 備份狀態](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [檢視及管理 online 備份警示](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [重設為預設設定的備份 online](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_15)  
  
-   [登入 Azure 備份服務](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)  
  
-   [與 Windows Server Essentials 整合 Azure 備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_17)  
  
-   [保護 online 備份在 Windows Server Essentials 的資料夾](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_18)  
  
-   [在 Windows Server Essentials online 備份歷程](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_19)  
  
###  <a name="BKMK_7"></a>變更 online 備份原則  
 很容易使用 Windows Server Essentials 儀表板 online 備份原則做出的變更。  
  
##### <a name="to-change-the-online-backup-policy"></a>若要變更 online 備份原則  
  
1.  以系統管理員身分登入儀表板。  
  
2.  在瀏覽列中，按一下 [ **Online Backup**。  
  
3.  在**Online 備份工作**窗格中，按**設定 online 備份**。  
  
4.  依照精靈中的指示，來自訂 online 備份原則。  
  
 如需詳細資訊，您可以自訂的設定，請查看[設定 online 備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
###  <a name="BKMK_8"></a>檢視 online 備份存放裝置使用方式  
  
##### <a name="to-view-the-amount-of-storage-space-that-online-backup-uses"></a>若要檢視的 Online Backup 使用的儲存空間  
  
1.  以系統管理員身分登入儀表板。  
  
2.  在瀏覽列中，按一下 [ **ONLINE BACKUP**。  
  
3.  在**Online Backup**索引標籤上，**儲存狀態**出現在 [資訊] 窗格中。  
  
###  <a name="BKMK_9"></a>Online 的備份包含新的資料夾  
  
##### <a name="to-include-a-new-folder-in-the-online-backup-policy"></a>在 online 原則備份包含新的資料夾  
  
1.  以系統管理員身分登入儀表板。  
  
2.  在瀏覽列中，按一下 [ **Online Backup**。  
  
3.  在**Online 備份工作**窗格中，按**設定 online 備份**。  
  
4.  在**變更或移除備份原則**頁面上，按**自訂 Online 備份原則**。  
  
5.  在**設定 Online Backup**頁面上，如果您想要包含的資料夾會不會出現在清單中，按**將資料夾新增**。  
  
6.  在**[新增資料夾**視窗中，瀏覽並選取您想要包含在 online 備份，然後按一下的資料夾**[確定]**。  
  
7.  在**設定 Online Backup**頁面上，選取 [其他資料夾，包含，然後按**下**。  
  
8.  依照精靈中的指示，完成自訂 online 備份原則。  
  
 如需詳細資訊，您可以自訂其他設定，請查看[設定 online 備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
###  <a name="BKMK_10"></a>移除或檔案歷史備份排除 online 備份原則  
  
##### <a name="to-remove-or-exclude-a-folder-from-the-online-backup-policy"></a>移除或資料夾排除 online 備份原則  
  
1.  以系統管理員身分登入儀表板。  
  
2.  在瀏覽列中，按一下 [ **Online Backup**。  
  
3.  按一下**保護資料夾**索引標籤。詳細資料窗格中，會顯示備份他們主意與資料夾的清單。  
  
4.  選取的資料夾，您想要排除 online 備份原則，然後在 [工作] 窗格中，按一下 [**資料夾移除 online 備份**。  
  
###  <a name="BKMK_11"></a>停用或重新讓 online 伺服器備份  
 如需如何備份或還原伺服器資料使用 Azure 備份，請查看[登記此伺服器備份的](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)。  
  
 如需如何停止使用 Azure 備份備份或還原 server 的資料，請查看[取消註冊伺服器](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_6)。  
  
###  <a name="BKMK_12"></a>停止在進行中的 online 伺服器備份  
  
##### <a name="to-stop-an-online-server-backup-in-progress"></a>若要停止在進行中的 online 伺服器備份  
  
1.  以系統管理員身分登入儀表板。  
  
2.  在瀏覽列中，按一下 [ **ONLINE BACKUP**。  
  
3.  在**Online 備份工作**窗格中，按**停止備份**。  
  
     停止狀態的備份之後,**已取消**就會出現在 [備份**備份歷史**清單。  
  
###  <a name="BKMK_13"></a>檢視 online 備份狀態  
  
##### <a name="to-view-the-backup-status"></a>若要檢視備份狀態  
  
1.  以系統管理員身分登入儀表板。  
  
2.  在瀏覽列中，按一下 [ **ONLINE BACKUP**。  
  
3.  按一下**備份歷史**索引標籤。清單檢視顯示每個備份的狀態。 選擇備份檢視相關工作的其他詳細資料的工作。  
  
###  <a name="BKMK_14"></a>檢視及管理 online 備份警示  
 許多其他的提醒，例如 Azure 備份警示會顯示提醒檢視器中。  
  
##### <a name="to-view-online-backup-alerts-in-the-alert-viewer"></a>若要檢視 online 備份警示警示檢視器  
  
1.  打開儀表板。  
  
2.  執行下列其中一個動作：  
  
      Windows Server Essentials：在瀏覽窗格中，按一下 [提醒] 圖示 \（可能重大、警告或 Informational\）。 這樣警示檢視器。  
  
      Windows Server Essentials：在**Home**頁面上，按一下 [**健康監視**索引標籤。  
  
3.  檢查清單的警示 Azure 備份相關的問題。  
  
 如需有關使用管理通知的警示檢視器或健康監視索引標籤，請查看[管理系統健康](Manage-System-Health-in-Windows-Server-Essentials.md)。  
  
###  <a name="BKMK_15"></a>重設為預設設定的備份 online  
 Windows Server Essentials 提供精靈可協助您設定的設定 online 備份。 如果您想要還原預設設定，請執行**設定 online 備份**工作，然後選擇 [**移除 online 備份原則**選項。 然後執行**設定 online 備份**工作再試一次。 您上傳之前的資料會保持不變。  
  
###  <a name="BKMK_16"></a>登入 Azure 備份服務  
 若要準備與 Windows Server Essentials 整合，Microsoft Azure 備份，您將會使用您的 Microsoft Online Services 帳號，登入 Azure 管理入口網站，然後再建立備份保管庫中，將 online 的備份儲存在 Azure。 您將會再下載 Azure 備份整合模組，並使用下載的檔案 Azure 備份 Add-In 安裝 Windows Server Essentials 伺服器上。 如果您不 Microsoft account，您可以申請免費試用版。  
  
 若要執行此設定，請完成下列工作：  
  
1.  適用於 Online Services 的 Microsoft account 和備份預覽登入。  
  
2.  建立備份保管庫中，市集 online 備份。  
  
3.  下載 Azure 備份代理程式。  
  
4.  Azure 備份 Add-In 安裝在伺服器上。  
  
####  <a name="BKMK_SignupforaMicrosoftOnlineServiceAccount"></a>適用於 Online Services 的 Microsoft account 和備份預覽登入  
  
1.  登入 Windows Server Essentials 儀表板。  
  
2.  儀表板上**Home**頁面上，按一下 [**增益集**明細，按一下 [**整合 Azure 備份**，，然後按一下 [**按一下登入 Azure 備份**。  
  
3.  Azure 上**修復服務**頁面上，在**備份（預覽版）**區段中，檢視詳細資料。  
  
4.  如果您不需要 Azure 裝機費，請按一下**免費試用版**，然後依照指示，以取得 Azure 裝機費。  
  
     如果您已經有 Azure 裝機費，按**入口網站**右上角的網頁，以移至管理 Azure 入口網站。  
  
5.  在 Azure 管理入口網站頁面上，您會看到**修復服務**在左窗格中。 讓您市管理備份 s vaults 從 Windows Server Essentials 的市集 online 備份。  
  
####  <a name="BKMK_Createabackupvaulttostoreonlinebackups"></a>建立備份保管庫中，市集 online 備份  
  
1.  若要登入[Azure 管理入口網站](https://manage.windowsazure.com)Windows Server Essentials 伺服器上的網頁瀏覽器。  
  
2.  在 Azure 管理 Portal 中，按一下 [**新**，按一下 [**的資料服務**，按一下 [**修復服務**，按一下**備份保存庫**，，然後按一下 [**快速建立**。  
  
3.  輸入您的備份保存庫的名稱，選擇您想要用來儲存您的備份，然後按一下**快速建立**。  
  
     **修復服務**區域中會顯示您新備份保存庫開啟。  
  
####  <a name="BKMK_DownloadtheWindowsAzureBackupAgent"></a>下載 Azure 備份代理程式  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  儀表板上**Home**頁面上，按一下 [**開始**索引標籤上，按一下 [**增益集**分類，按一下**整合 Azure 備份**，，然後按一下 [**按一下以下載 Azure 備份整合模組**。  
  
####  <a name="BKMK_InstalltheWindowsAzureBackupAddIn"></a>在伺服器上安裝 [Azure 備份 Add-In  
  
1.  使用系統管理員帳號，您的伺服器來登入，然後執行**OnlineBackupAddin.wssx**您在前一個步驟中下載的檔案。  
  
2.  **軟體授權條款**會顯示。 如果您同意條款及條件，請按一下**接受]**以繼續安裝。  
  
3.  在**安裝增益集**頁面上，按**安裝增益集**。  
  
    > [!NOTE]
    >  您可能需要重新開機才能安裝任何必要的軟體的伺服器。  
  
     **安裝**頁面會顯示。 開始安裝，並會顯示進度安裝時，會顯示進度列指示器。 安裝完成時，您會收到一則訊息，Azure 備份 Add-in 已成功安裝。  
  
4.  按一下**完成**。  
  
5.  關閉和重新開啟儀表板。  
  
     新的索引標籤中，**Online Backup**，儀表板中新增了。 從這個索引標籤，登記伺服器、設定備份設定和開放**修復服務**在 Azure 管理入口網站管理您的伺服器備份保存庫。  
  
 完成這些步驟之後，執行下列動作：  
  
1.  Windows Server Essentials，在上傳憑證以用於 online 備份。 如需詳細資訊，請查看[上傳憑證 Azure 備份保存庫以](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)。  
  
2.  使用 Azure 備份保存庫登記伺服器。 如需詳細資訊，請查看[登記此伺服器備份的](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)。  
  
3.  設定伺服器的 online 備份。 如需詳細資訊，請查看[設定 online 備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)。  
  
###  <a name="BKMK_17"></a>與 Windows Server Essentials 整合 Azure 備份  
 Microsoft Azure 備份整合模組是，可讓您將加密的檔案和資料夾從備份您的伺服器來提供 Microsoft Azure 裝載儲存系統的 Windows Server Essentials 的新功能。 使用 Azure 備份加密及備份伺服器上的資料，您可以協助防止嚴重因為火焰、大量、遭竊或其他嚴重損壞，而重要的業務資料遺失。 當您使用 Azure 備份資料備份伺服器時，資訊加密使用您複雜密碼，才能在網際網路上的資料安全中心以上傳。 存取資料的 online 備份，您必須伺服器驗證憑證的及必須提供複雜密碼。  
  
 整合登記伺服器的 Azure 備份之後, 您可以設定 online 備份執行定期排定的備份。 您也可以起始隨時 online 備份，按一下**開始備份**Online 備份儀表板中的工作。  
  
 Online 備份設定包括以下步驟：  
  
1.  [登入 Azure 備份服務的](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_16)-之後您登入以檢視備份服務，會建立備份的保存庫、下載和安裝 Azure 備份整合模組。  
  
2.  [若要備份 Azure 保存庫上傳憑證](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
3.  [登記此伺服器的備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
4.  [設定 online 備份](Manage-Online-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
> [!NOTE]
>   Azure 備份使用複雜密碼加密的檔案和資料夾 online 備份。 變更加密複雜密碼，將會取代您登記伺服器時所指定複雜密碼。 複雜密碼，可接受只 ASCII 編碼的字元。  
  
###  <a name="BKMK_18"></a>保護 online 備份在 Windows Server Essentials 的資料夾  
 **保護資料夾**小節儀表板 Online Backup 一節中的顯示的所有的共用資料夾清單伺服器上。 下表描述清單中所包含的資訊。  
  
|欄|描述|  
|------------|-----------------|  
|**資料夾名稱：**|隨附於 online 備份資料夾的名稱。<br /><br /> 若要排除某個資料夾，請執行**設定 online 備份**工作。|  
|**資料夾：**|資料夾的位置。|  
|**狀態：**|有三種類型的狀態**保護**，**未受保護**，並**未知**。|  
  
###  <a name="BKMK_19"></a>在 Windows Server Essentials online 備份歷程  
 **備份歷史**小節儀表板 Online Backup 一節中的顯示最近 online 備份的清單。 您可以使用成功的備份還原檔案和資料夾。 下表描述清單中所包含的資訊。  
  
|欄|描述|  
|------------|-----------------|  
|**操作：**|有兩種類型的作業-**備份**和**還原**。|  
|**時間：**|這是登入的最新狀態的時間。|  
|**狀態：**|有五種類型的狀態**成功**，**在進行中**、**已取消**、**警告**，並**Unsuccessful**。|  
  
## <a name="see-also"></a>也了  
  
-   [管理備份與還原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [管理 Microsoft Online Services](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
