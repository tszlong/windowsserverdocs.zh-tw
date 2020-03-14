---
title: 管理 Windows Server Essentials 中的伺服器備份
description: 說明如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0302d070-c58a-40f2-b56d-7e7842813d02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7e40a4675cf77d55a3047b41e0ab852fd7cd9de9
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322240"
---
# <a name="manage-server-backup-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的伺服器備份

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials
  
 下列主題包含您可以使用 [Windows Server Essentials 儀表板] 完成之一般備份工作的相關資訊：  
  
-   [我應該選擇哪個備份？](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_WhichBackup)  
  
-   [設定或自訂伺服器備份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [停止伺服器備份進行中](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [從遠端系統管理您的備份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [停用伺服器備份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [深入瞭解設定伺服器備份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [重新分割伺服器上的硬碟](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [從伺服器備份還原檔案和資料夾](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
##  <a name="BKMK_WhichBackup"></a>我應該選擇哪個備份？  
 如果您有最新的成功備份，也知道該備份包含您的所有重要資料，則選擇備份可以是很直接的。 如果您正嘗試從舊的備份還原到伺服器或電腦，選擇良好的備份來還原可能需要一些調查，也可能需要一些妥協。  
  
#### <a name="to-choose-a-backup"></a>選擇備份  
  
1.  向檔案或資料夾的擁有者確認，並記下他們新增或編輯這些檔案或資料夾的日期和時間。 使用這些日期和時間做為起始點。  
  
2.  在 [還原檔案和資料夾精靈] 中的 [選擇還原選項] 頁面上，按一下 [從我選取的備份還原(進階)]。  
  
3.  視您要還原的是較舊版或較新版的檔案或資料夾而定，選取最符合在步驟 1 中記下之日期和時間的備份。  
  
4.  最佳做法是將檔案和資料夾還原到替代位置，然後讓檔案和資料夾的擁有者將他們所需的檔案和資料夾移到原始位置。 當他們完成時，就可以刪除留在替代位置中的檔案和資料夾。  
  
##  <a name="BKMK_1"></a>設定或自訂伺服器備份  
 在安裝期間不會自動設定伺服器備份。 您應該排定每日備份來自動保護您的伺服器及其資料。 建議您維持一個每日備份計劃，因為大多數組織皆無法承受失去數天所建立之資料的損失。 如需詳細資訊，請參閱[設定或自訂伺服器備份](Set-up-or-customize-server-backup.md)。  
  
##  <a name="BKMK_2"></a>停止伺服器備份進行中  
 不論伺服器備份是在定期排定的時間開始，或是由您手動啟動伺服器備份，您都可以停止進行中的備份。  
  
#### <a name="to-stop-a-backup-in-progress"></a>停止進行中的備份  
  
1.  開啟 [儀表板]。  
  
2.  在瀏覽列中，按一下 [裝置]。  
  
3.  在電腦清單中，按一下 [伺服器]，然後按一下 [工作] 窗格中的 [停止伺服器的備份]。  
  
4.  按一下 [是] 以確認您的動作。  
  
##  <a name="BKMK_3"></a>從遠端系統管理您的備份  
 當您不在辦公室時，您可以使用 Windows Server Essentials 的「遠端 Web 存取」來存取 [Windows Server Essentials 儀表板] 以管理您的伺服器。  
  
#### <a name="to-use-remote-web-access-to-manage-your-server"></a>使用遠端 Web 存取來管理您的伺服器  
  
1. 開啟網頁瀏覽器。  
  
2. 在網址方塊中，輸入 Windows Server Essentials 網域的名稱。  
  
3. 出現提示時，輸入您的使用者名稱和密碼。  
  
4. 當您按一下 遠端 Web 存取中的伺服器名稱時，會顯示儀表板的登入頁面。  
  
5. 以系統管理員身分登入 [儀表板]，然後按一下 [裝置]。  
  
   如需遠端 Web 存取的詳細資訊，請參閱[遠端 Web 存取總覽](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview)。  
  
##  <a name="BKMK_4"></a>停用伺服器備份  
 您應該排定每日備份來自動保護您的伺服器及其資料。 建議您維持一個每日備份計劃，因為大多數組織皆無法承受失去數天所建立之資料的損失。  
  
 如果您已經設定伺服器備份，而稍後想要使用協力廠商應用程式來備份伺服器，則可以停用 Windows Server Essentials 伺服器備份。  
  
#### <a name="to-disable-server-backup"></a>停用伺服器備份  
  
1.  使用系統管理員帳戶和密碼來登入 [Windows Server Essentials 儀表板]。  
  
2.  按一下 [裝置] 索引標籤，然後按一下伺服器的名稱。  
  
3.  在 [工作] 窗格中，按一下 [自訂伺服器的備份]。  
  
    > [!NOTE]
    >  在您使用 [設定伺服器備份精靈] 來設定伺服器備份之後，就會顯示 [自訂伺服器的備份] 工作。 如需有關設定伺服器備份的詳細資訊，請參閱[設定或自訂伺服器備份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)。  
  
4.  [自訂伺服器備份精靈] 隨即出現。  
  
5.  在 [設定選項] 頁面上，按一下 [停用伺服器備份]。 遵循精靈的指示進行。  
  
##  <a name="BKMK_5"></a>深入瞭解設定伺服器備份  
 在伺服器設定期間並不會啟用伺服器備份。  
  
> [!NOTE]
>  當您設定伺服器備份時，應該至少將一個外接式硬碟連接到伺服器，以做為備份目的地硬碟。  
  
###  <a name="BKMK_Target"></a>備份目的地磁片磁碟機  
 您可以使用多個外部存放磁碟機進行備份，然後讓這些磁碟機在站上與離站位置之間輪替。 這可以協助您在站上硬體發生實體損壞時復原資料，改善嚴重損壞的準備計畫。  
  
 為您的伺服器備份選擇存放磁碟機時，請考慮下列各項：  
  
-   選擇有足夠空間來儲存資料的磁碟機。 存放磁碟機的儲存容量應該至少是您想要備份之資料的 2.5 倍。 磁碟機也應該大到足以應付您伺服器資料未來的成長。  
  
-   如果備份目的地磁碟機包含離線磁碟機，備份設定將不會成功。 若要完成設定，請在選取備份目的地時，取消選取核取方塊來排除離線的磁碟機。  
  
-   如果您選擇包含先前備份的磁碟機做為備份目的地，精靈會讓您選擇是否要保留先前的備份。 如果您要保留那些備份，精靈就不會將磁碟機格式化。  
  
-   重複使用了外部存放磁碟機時，請確定磁碟機是空的或只包含您不需要的資料。  
  
-   您應該瀏覽您外部存放磁碟機製造商的網站，以確保在執行 Windows Server Essentials 的電腦上支援您的備份磁碟機。  
  
> [!CAUTION]
>  [設定伺服器備份精靈] 會在設定存放磁碟機來進行備份時，將這些磁碟機格式化。  
  
### <a name="server-backup-schedule"></a>伺服器備份排程  
 您應該排定每日備份來自動保護您的伺服器及其資料。 建議您維持一個每日備份計劃，因為大多數組織皆無法承受失去數天所建立之資料的損失。  
  
 使用 Windows Server Essentials 的 [設定伺服器備份精靈] 時，您可以選擇在一天的多個時間備份伺服器資料。 由於精靈會排定差異式備份，因此備份執行相當快速，不會大幅影響伺服器效能。 [設定伺服器備份] 預設會排定在每天的下午 12:00 和下午 11:00 執行備份。 不過，您可以根據您組織的需求調整備份排程。 您應該偶爾評估您備份計劃的有效性，然後視需要變更計劃。  
  
> [!NOTE]
>  在 Windows Server Essentials 的預設安裝中，是將伺服器設定為每週自動執行一次磁碟重組。 如果您使用非 Microsoft 映像處理軟體，這可能會導致比一般備份還要大。 如果沒有必要定期進行伺服器磁碟重組，您可以依照下列步驟關閉磁碟重組排程：  
> 
> 1. 按 Windows 鍵 + W 鍵來開啟 [搜尋]。  
>    2. 在 [搜尋] 文字方塊中，輸入 **Defragment**。  
>    3. 在結果區段中，按一下 [重組並最佳化磁碟機]。  
>    4. 在 [最佳化磁碟機] 頁面中，選取一個磁碟機，然後按一下 [變更設定]。  
>    5. 在 [最佳化排程] 視窗中，取消選取 [依排程執行 (建議)] 核取方塊，然後按一下 [確定] 來儲存變更。  
  
### <a name="items-to-be-backed-up"></a>要備份的項目  
 預設會選取所有作業系統檔案和資料夾來進行備份。 您可以選擇備份伺服器上所有的硬碟、檔案及資料夾，或只選取個別的硬碟、檔案或資料夾來進行備份。 若要新增或移除要備份的項目，請執行下列其中一個動作：  
  
-   若要將某個資料磁碟機納入伺服器備份，請選取相鄰的核取方塊。  
  
-   若要將某個資料磁碟機從伺服器備份中排除，請取消選取相鄰的核取方塊。  
  
> [!NOTE]
>  如果您想要將 [作業系統] 項目從備份中排除，就必須先取消選取 [系統備份 (建議選項)] 核取方塊。  
  
 為了將您伺服器備份所使用的備份硬碟空間量縮減到最少，您可能會想要將任何包含您不認為有用或特別重要之資料的資料夾排除。  
  
 例如，您可能有資料夾包含使用大量硬碟空間的錄製電視節目。 您可以選擇不要備份這些檔案，因為您通常在看過它們之後就會將它們刪除。 或者，您可能有資料夾包含您不想要保留的暫存檔案。  
  
##  <a name="BKMK_6"></a>重新分割伺服器上的硬碟  
 在 Windows Server Essentials 伺服器上偵測到未格式化的內部硬碟時，系統會發出含有 [新增硬碟精靈] 連結的健康狀態警示。 [新增硬碟精靈] 會引導您完成將硬碟格式化的各種選項。 當精靈完成時，視磁碟機的大小，將會在硬碟上建立一或多個格式化邏輯硬碟，並格式化成 NTFS。  
  
 如果有必要重新分割硬碟，請依照下列指示進行：  
  
#### <a name="to-repartition-a-hard-disk-drive"></a>重新分割硬碟  
  
1.  在 [開始] 畫面上，按一下 [系統管理工具]，然後按兩下 [電腦管理]。  
  
2.  在 [電腦管理] 中，按一下 [存放裝置]，然後按兩下 [磁碟管理]。  
  
3.  在您想要重新分割的磁碟機上按一下滑鼠右鍵，按一下 [刪除磁碟區]，然後按一下 [是]。  
  
    > [!NOTE]
    >  針對硬碟上的每個磁碟分割重複這個步驟。  
  
4.  在 [未配置] 硬碟上按一下滑鼠右鍵，然後按一下 [新增簡單磁碟區]。  
  
5.  在 [新增簡單磁碟區精靈] 中，建立並格式化一個等於或小於 16 TB (16,000,000 MB) 的磁碟區。  
  
    > [!NOTE]
    >  重複這個步驟，直到硬碟上的所有未配置空間都已使用為止。  
  
##  <a name="BKMK_7"></a>從伺服器備份還原檔案和資料夾  
 您可以從伺服器備份瀏覽並還原個別的檔案和資料夾。  
  
#### <a name="to-restore-files-and-folders-from-a-server-backup"></a>從伺服器備份還原檔案和資料夾  
  
1.  開啟 [儀表板]，然後按一下 [裝置] 索引標籤。  
  
2.  按一下伺服器的名稱，然後按一下 [工作] 窗格中的 [還原伺服器的檔案或資料夾]。  
  
3.  [還原檔案或資料夾精靈] 便會開啟。 請依照精靈中的指示來還原檔案或資料夾。  
  
## <a name="see-also"></a>另請參閱  
  
-   [管理備份與還原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
