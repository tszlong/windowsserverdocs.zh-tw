---
title: "管理 Windows Server Essentials 伺服器備份"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.openlocfilehash: 2b0cd926b15d65e5cd4c784681c40df29b18a48f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="manage-server-backup-in-windows-server-essentials"></a>管理 Windows Server Essentials 伺服器備份

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集
  
 下列主題包含一般備份工作，您可以使用 Windows Server Essentials 儀表板完成的相關資訊：  
  
-   [我應該選擇哪一個備份？](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_WhichBackup)  
  
-   [設定或自訂伺服器備份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [停止在進行中的伺服器備份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [遠端管理您的備份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [停用伺服器備份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [深入了解備份伺服器設定](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [分割伺服器上的硬碟](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [從伺服器備份還原的檔案和資料夾](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_7)  
  
##  <a name="BKMK_WhichBackup"></a>我應該選擇哪一個備份？  
 選擇備份可直接如果您有最新的成功備份，您知道的備份包含所有重要資料。 如果您想要從舊的備份還原伺服器或電腦，選擇良好備份還原到可能需要一些調查，可能是，某些危害。  
  
#### <a name="to-choose-a-backup"></a>若要選擇備份  
  
1.  檢查的擁有者的檔案或資料夾，並記下的日期和時間時新增或編輯他們。 開始使用這些的日期和時間。  
  
2.  在**選擇還原**頁面還原的檔案和資料夾精靈中，按一下 [**還原備份，選取 [我 （進階），從**。  
  
3.  視您是否要還原的檔案或資料夾的舊版或較新版本，選取最適合的日期和時間步驟 1 中所述的備份。  
  
4.  做為最佳做法，您可以還原的檔案和資料夾到另一個位置，然後通知擁有者的檔案和資料夾移動到原始位置所需的相片。 完成時，可以刪除的檔案和資料夾的另一個位置中出現。  
  
##  <a name="BKMK_1"></a>設定或自訂伺服器備份  
 在安裝期間不會自動設定伺服器備份。 您應該保護您的伺服器，並其資料自動排程每天備份。 建議您維護每天備份計劃，因為最組織不承擔遺失已建立幾天的資料。 如需詳細資訊，請查看[設定或自訂伺服器備份](Set-up-or-customize-server-backup.md)。  
  
##  <a name="BKMK_2"></a>停止在進行中的伺服器備份  
 無論定期排定的時間開始伺服器備份您手動開始伺服器備份，您可以停止進行備份。  
  
#### <a name="to-stop-a-backup-in-progress"></a>若要停止在進行中備份  
  
1.  打開儀表板。  
  
2.  在瀏覽列中，按一下 [**裝置**。  
  
3.  在的電腦清單中，按一下 [伺服器]，然後按一下 [**停止備份伺服器的**在**工作**窗格。  
  
4.  按一下**[是]**以確認您的動作。  
  
##  <a name="BKMK_3"></a>遠端管理您的備份  
 當您離開 office 時，您可以使用 Windows Server Essentials 遠端 Web 存取存取 Windows Server Essentials 儀表板以管理您的伺服器。  
  
#### <a name="to-use-remote-web-access-to-manage-your-server"></a>使用遠端 Web 存取管理您的伺服器  
  
1.  打開網頁瀏覽器。  
  
2.  在位址] 方塊中輸入 Windows Server Essentials 網域的名稱。  
  
3.  出現提示時，輸入您的使用者名稱和密碼。  
  
4.  當您按一下遠端 Web 存取伺服器的名稱時，則會顯示儀表板的登入頁面。  
  
5.  儀表板為系統管理員的身分登入，然後按一下**裝置**。  
  
 如遠端網站存取的相關詳細資訊，請查看[遠端 Web 存取概觀](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview)。  
  
##  <a name="BKMK_4"></a>停用伺服器備份  
 您應該保護您的伺服器，並其資料自動排程每天備份。 建議您維護每天備份計劃，因為最組織不承擔遺失已建立幾天的資料。  
  
 如果您已經伺服器備份設定，並稍後想要備份伺服器使用第三方應用程式，您可以停用 Windows Server Essentials 伺服器備份。  
  
#### <a name="to-disable-server-backup"></a>若要停用伺服器備份  
  
1.  Windows Server Essentials 儀表板使用系統管理員和密碼登入。  
  
2.  按一下**裝置**索引標籤，然後按一下 [伺服器名稱。  
  
3.  在 [工作] 窗格中，按一下**自訂備份伺服器的**。  
  
    > [!NOTE]
    >  **自訂備份伺服器的**設定伺服器備份使用設定伺服器備份精靈之後，系統會顯示工作。 如需有關伺服器備份設定的詳細資訊，請[設定或自訂伺服器備份](Manage-Server-Backup-in-Windows-Server-Essentials.md#BKMK_1)。  
  
4.  [自訂伺服器備份] 精靈會出現。  
  
5.  在**設定選項**頁面上，按**停用伺服器備份**。 請依照精靈中的指示進行。  
  
##  <a name="BKMK_5"></a>深入了解備份伺服器設定  
 伺服器備份不被支援期間伺服器設定。  
  
> [!NOTE]
>  當您設定伺服器的備份時，您應該要使用做為備份目的地硬碟伺服器連接至少外部硬碟機。  
  
###  <a name="BKMK_Target"></a>備份目的地磁碟機  
 備份，可以使用多個儲存外部磁碟機，您可以旋轉現場與離站存放位置的磁碟機。 這可以改善計畫，協助您復原您的資料實體損壞站上硬體發生嚴重損壞的準備。  
  
 在選擇儲存空間的磁碟機伺服器備份，請考慮下列動作：  
  
-   選擇您的資料儲存空間不足所在的磁碟機。 儲存空間的磁碟機應至少 2.5 時間的儲存空間容量您想要備份的資料。 磁碟機也會足以容納未來成長您 server 的資料。  
  
-   備份目的磁碟機包含離線磁碟機，如果備份設定不會成功。 時選擇目的地備份，請完成設定，，清除 [排除離線磁碟機] 核取方塊。  
  
-   如果您選擇做為備份的目的地包含先前的備份將磁碟機，精靈可讓您選擇要保留的上一個備份。 如果您的備份，精靈無法格式化磁碟機。  
  
-   時重複使用外部儲存空間的磁碟機，請確定磁碟機空白，或包含不需要您的資料。  
  
-   您也應該瀏覽以確保您備份的磁碟機，會支援執行 Windows Server Essentials 的電腦上您外部儲存空間的磁碟機製造商的網站。  
  
> [!CAUTION]
>  它備份設定時，設定伺服器備份精靈格式儲存空間的磁碟機。  
  
### <a name="server-backup-schedule"></a>伺服器備份排程  
 您應該保護您的伺服器，並其資料自動排程每天備份。 建議您維護每天備份計劃，因為最組織不承擔遺失已建立幾天的資料。  
  
 當您使用 Windows Server Essentials 設定伺服器備份精靈時，您可以選擇備份 server 的資料，在白天多個時間。 精靈排程差異為基礎的備份，因為備份執行速度快，並不會大幅少數伺服器的效能。 根據預設，**設定好伺服器備份**排程每日執行 12:00，11:00，建立的備份。 不過，您可以調整備份排程根據您的組織的需求。 您偶爾會應該評估您備份計劃的效率和變更的計劃視。  
  
> [!NOTE]
>  在 Windows Server Essentials 的預設安裝，自動執行一次每個星期磁碟重組設定伺服器。 這可能會導致大於標準備份如果您使用非 Microsoft 影像的軟體。 如果您不需要重組定期伺服器，您可以依照下列步驟進行以關閉 [磁碟重組排程：  
>   
>  1.  長按 Windows 鍵 + W 打開**搜尋**。  
> 2.  在 [搜尋] 方塊中，輸入**重組**。  
> 3.  在 [結果] 區段中，按一下 [**重組並最佳化磁碟機**。  
> 4.  在**最佳化磁碟機**頁面，選取磁碟機，然後按一下 [**變更設定**。  
> 5.  在**最佳化排程**視窗中，清除**執行排程 （建議選項）**核取方塊，並再按**[確定]**儲存變更。  
  
### <a name="items-to-be-backed-up"></a>若要備份的項目  
 根據預設，所有的作業系統檔案和資料夾的備份選取。 您可以選擇備份所有硬碟、 檔案和資料夾的伺服器上，或選取個人硬碟、 檔案或資料夾的備份。 若要新增或移除備份的項目，請執行下列其中一個動作：  
  
-   若要伺服器的備份包含資料磁碟機，選取旁邊的核取方塊。  
  
-   若要排除伺服器備份資料磁碟機，清除旁邊的核取方塊。  
  
> [!NOTE]
>  如果您想要排除**作業系統**的項目從備份，您必須先清除**系統備份 （建議選項）**核取方塊。  
  
 最小化伺服器備份時要使用的備份硬碟空間量，您可能想要排除任何資料夾，其中包含您認為未寶貴或重要的檔案。  
  
 例如，您可能使用的磁碟機空間包含錄製的電視節目的資料夾。 您可以選擇不備份這些檔案，因為您通常會之後仍檢視這些 delete。 或者，您可能需要您不想要保留的暫存檔案所在的資料夾。  
  
##  <a name="BKMK_6"></a>分割伺服器上的硬碟  
 Windows Server Essentials 伺服器上偵測到格式化內部硬碟時，健康警示包含新增新的硬碟機精靈的連結。 新增新的硬碟機精靈會引導您將硬碟格式化在各種不同的選項。 當您完成精靈之後時，將會在硬碟上建立和格式化為 NTFS 一或多個，根據的磁碟機大小邏輯硬碟格式化的磁碟機。  
  
 如果您需要重新硬碟磁碟分割，請依照這些指示執行：  
  
#### <a name="to-repartition-a-hard-disk-drive"></a>若要重新硬碟磁碟分割  
  
1.  在**[開始]**畫面中，按一下 [**系統管理工具]**，，然後按兩下 [**電腦管理]**。  
  
2.  在 [電腦管理] 中，按一下 [**存放裝置**，然後按兩下 [ **[磁碟管理**。  
  
3.  以滑鼠右鍵按一下您想要分割時，請按一下 [磁碟機**Delete 磁碟區**，然後按一下 [**是**。  
  
    > [!NOTE]
    >  每個分割在硬碟上的重複此步驟。  
  
4.  以滑鼠右鍵按一下**未配置**硬碟，然後再按一下**新增簡單磁碟區**。  
  
5.  在 [新增簡單磁碟區精靈中，建立和格式化磁碟區 16 tb (16,000,000 MB) 或更少。  
  
    > [!NOTE]
    >  重複此步驟，直到使用所有在硬碟上尚未配置的空間。  
  
##  <a name="BKMK_7"></a>從伺服器備份還原的檔案和資料夾  
 您可以瀏覽，並從伺服器備份還原個人檔案和資料夾。  
  
#### <a name="to-restore-files-and-folders-from-a-server-backup"></a>若要從伺服器備份還原的檔案和資料夾  
  
1.  打開，儀表板，然後按一下**裝置**索引標籤。  
  
2.  按一下伺服器的名稱，然後按一下**還原的檔案或資料夾伺服器的**在**工作**窗格。  
  
3.  還原的檔案或資料夾精靈會開啟。 依照精靈中的指示來還原的檔案或資料夾。  
  
## <a name="see-also"></a>也了  
  
-   [管理備份與還原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
