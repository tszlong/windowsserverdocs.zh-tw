---
title: "管理伺服器在 Windows Server Essentials 的儲存空間"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1836682e-c7bb-4dd5-a2b5-6ff032693574
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e0f65dfd25afbd584764d33904ba82e4da4c5443
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="manage-server-storage-in-windows-server-essentials"></a>管理伺服器在 Windows Server Essentials 的儲存空間

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集
   
 Windows Server Essentials 可讓您管理所有伺服器存放裝置 （包括硬碟與儲存空間） 從**硬碟**頁面上**儲存**索引標籤的儀表板。  
  
 下列章節提供的資訊可協助您提高您的伺服器儲存空間、 了解使用儲存空間，及管理您的硬碟：  
  
-   [管理硬碟使用儀表板](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [增加伺服器上的儲存空間](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [在硬碟上執行檢查與修復](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_Check)  
  
-   [格式化硬碟磁碟機](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [新增新的硬碟](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [儲存空間概觀](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [建立儲存空間](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_6)  
  
##  <a name="BKMK_1"></a>管理硬碟使用儀表板  
 Windows Server Essentials 允許您管理所有的硬碟磁碟機連接到透過儀表板。 儀表板**存放裝置**索引標籤上，**硬碟**會顯示可用來儲存的資料與伺服器備份伺服器上的所有硬碟機。 伺服器會每個硬碟上的可用空間，顯示提醒，如果磁碟機空間不足。 **硬碟**索引標籤顯示下列資訊：  
  
-   每個困難的磁碟機名稱  
  
-   每個困難的磁碟機的容量  
  
-   每個硬碟上的已使用空間量  
  
-   每個硬碟上的可用空間量  
  
-   每個困難的磁碟機; 的狀態空白的狀態，表示硬碟正常運作  
  
-   詳細資料窗格中，會顯示所有的儲存空間堆疊資訊 （適用於儲存集區、 儲存空間，以及硬碟） 如果硬碟機選取位於 （而不是實體磁碟） 儲存空間  
  
 下表列出及其描述儀表板中的可用硬碟管理工作。 硬碟機選取時才顯示一些的工作。  
  
### <a name="available-hard-drive-management-tasks"></a>可用硬碟管理工作  
  
|工作的名稱|描述|  
|---------------|-----------------|  
|**檢視硬碟屬性**|開啟*HardDriveName***屬性**頁面。 這項工作的硬碟機選取時顯示。 **一般**索引標籤的*HardDriveName*頁面屬性包含下列其他工作：<br /><br /> -   **清理磁碟機]**： 可讓您清除磁碟機 （這項工作且只適用於 Windows Server Essentials） 硬碟上未使用的檔案。<br />-   **檢查並修復**： 檢查檔案系統錯誤的硬碟，並嘗試自動修復偵測到的錯誤。<br /><br /> **陰影複製**索引標籤的*HardDriveName***屬性**頁面上可讓您陰影複製。 此標籤也會顯示陰影複製到執行排定的下一次。|  
|**管理儲存空間**|**注意：**適用於 Windows Server Essentials，這項工作時才會顯示現有的儲存空間。<br /><br /> 開啟**儲存空間**[控制台]，您可以從中建立及管理儲存集區與儲存空間。|  
|**建立儲存空間**|開啟建立儲存空間精靈，可讓您用於一或多個硬碟增加儲存集區的容量。|  
|**增加儲存集區容量**|**注意：**這項工作才看得到如果選取的硬碟機上儲存空間。<br /><br /> 開啟增加儲存集區精靈，可讓您用於一或多個硬碟增加儲存集區的容量的容量。|  
  
##  <a name="BKMK_2"></a>增加伺服器上的儲存空間  
 若要增加儲存在伺服器上的，您可以新增額外的內部硬碟伺服器。 若要新增其他內部硬碟，必須關閉伺服器、 新增內部硬碟，然後再重新開機伺服器。 您不需要關閉硬碟附加到 SCSI 控制站伺服器。 若是如此，硬碟可以連接時伺服器正在執行。  
  
 根據要新增的硬碟格式化是否，執行下列其中一個動作：  
  
-   **格式化**如果內部硬碟格式化 NTFS 或 ReFS、伺服器將磁碟機代號，並硬碟會出現在**硬碟**] 索引標籤。您現在可以建立或伺服器資料夾移動到新的硬碟機。  
  
-   **格式不**如果內部硬碟格式化，下列提醒會出現： 一或多個格式化硬碟磁碟機連接到伺服器。 若要將硬碟格式化使用下列程序。  
  
#### <a name="to-format-the-hard-disk"></a>若要將硬碟格式化  
  
1.  打開儀表板。  
  
2.  瀏覽窗格中，按一下 [提醒] 圖示上市**警示檢視器**Windows Server Essentials、 中或**健康監視**索引標籤，在 Windows Server Essentials。  
  
3.  在**警示檢視器**或**健康監視**索引標籤上，按一下通知，然後在 [工作] 窗格中，按一下 [**此問題的疑難排解**。  
  
4.  請依照下列指示完成新增新的硬碟磁碟機精靈。  
  
###  <a name="BKMK_Clean"></a>若要清除的硬碟  
  
1.  打開儀表板。  
  
2.  在瀏覽窗格中，按一下 [**存放裝置**，然後按一下 [**磁碟機**。  
  
3.  在**硬碟**區段，選取磁碟機代號指派到新加入的硬碟及窗格中，按**檢視硬碟屬性**。  
  
4.  在**< Driveletter\ > 屬性**，在**一般**索引標籤上，按一下 [**磁碟清理]**。  
  
##  <a name="BKMK_Check"></a>在硬碟上執行檢查與修復  
 硬碟檢查並修復程序驗證健康儲存在硬碟上的檔案系統。 執行**chkdsk**上的磁碟區，備份的檔案會儲存在 [處理程序。 下列警示問題可以解析執行檢查並修復在硬碟：  
  
-   您必須選取一或多個硬碟伺服器的備份。  
  
#### <a name="to-check-and-repair-hard-drives"></a>檢查並修復磁碟機硬碟  
  
1.  打開儀表板。  
  
2.  按一下**伺服器資料夾及磁碟機**，然後按一下 [**磁碟機**。  
  
3.  選取 [顯示錯誤的硬碟，然後選取**檢視硬碟屬性**。  
  
4.  在**檢查並修復**索引標籤上，按**檢查並修復**。  
  
##  <a name="BKMK_3"></a>格式化硬碟磁碟機  
 在伺服器上偵測到內部格式化硬碟機時，健康警示引導使用者的格式將其設定程序完成。 新增新的硬碟機精靈會引導您完成格式化硬碟磁碟機，可讓您的硬碟一下列方式設定：  
  
1.  格式化硬碟磁碟機，並自動上建立磁碟機。 如果您選擇此選項，當您完成時，會建立一個邏輯磁碟機格式化為 NTFS 檔案系統。  
  
2.  格式化硬碟磁碟機，並將它設為伺服器備份。 如果您選擇此選項，而且啟動設定伺服器備份精靈，則它會引導您伺服器備份設定。  
  
3.  儲存空間並不存在，如果建立儲存空間使用新的硬碟機。 您必須建立儲存空間至少兩個硬碟機。  
  
4.  如果已經儲存空間，使用新的硬碟機增加儲存集區的容量。 如果現有建立伺服器上的儲存空間，才會顯示此選項。 如果您選擇此選項，精靈將會新增此硬碟機到儲存集區。  
  
##  <a name="BKMK_4"></a>新增新的硬碟  
 當您將新的硬碟機插入執行 Windows Server Essentials 的伺服器時，您可以：  
  
-   [使用新的硬碟機儲存伺服器資料夾](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4a)  
  
-   [使用新的硬碟機儲存伺服器備份](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4b)  
  
-   [使用新的硬碟機增加儲存集區容量](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c)  
  
###  <a name="BKMK_4a"></a>使用新的硬碟機儲存伺服器資料夾  
 若要使用新的硬碟機儲存伺服器資料夾，您可以新增到硬碟伺服器資料夾或現有伺服器資料夾移動到硬碟。  
  
##### <a name="to-store-server-folders"></a>若要儲存伺服器資料夾  
  
1.  打開儀表板。  
  
2.  按一下**存放裝置**索引標籤，然後按一下 [**資料夾伺服器**。  
  
3.  在**資料夾 Server 工作，**窗格中，執行下列其中一個動作：  
  
    1.  若要新增的伺服器資料夾，按一下**資料夾新增**。  
  
    2.  若要移動伺服器資料夾、 選取您想要移動到新的硬碟，然後按一下 [資料夾**[移動資料夾**。  
  
    > [!NOTE]
    >  如果您瀏覽到硬碟，而不需要建立資料夾選取與伺服器資料夾位置，會顯示下列錯誤訊息： **（例如 C:\\ D:\\) 根 directory 無法新增為伺服器資料夾。建立新的資料夾或選取現有在根 directory，並再試一次**。 若要解析這個錯誤中新加入的硬碟，, 建立新的資料夾，然後選取新的資料夾做為儲存伺服器資料夾的位置。  
  
4.  請依照下列指示完成精靈。  
  
 如需移動伺服器資料夾，請查看[新增或移動伺服器資料夾](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)。  
  
###  <a name="BKMK_4b"></a>使用新的硬碟機儲存伺服器備份  
 您可以使用新加入的硬碟機儲存伺服器備份。  
  
##### <a name="to-store-server-backups"></a>若要儲存伺服器備份  
  
1.  打開儀表板。  
  
2.  按一下**裝置**索引標籤上，從清單窗格，選取 [伺服器，然後從 [工作] 窗格中，執行下列其中一個動作：  
  
    1.  如果未伺服器備份設定伺服器上，按一下 [**設定伺服器的備份**。  
  
    2.  如果伺服器備份設定在伺服器上，按一下**自訂伺服器備份**。  
  
     設定伺服器備份精靈會出現。  
  
3.  在**選擇備份目的地**頁面上，選取 [新增磁碟機做為備份的目的地。  
  
4.  請依照下列指示完成精靈。  
  
###  <a name="BKMK_4c"></a>使用新的硬碟機增加儲存集區容量  
 當您儲存集區容量不足，您會收到，您可以提升儲存集區容量新增新的硬碟機到儲存集區，使用增加精靈儲存集區的容量警告。  
  
> [!NOTE]
>  只有當您建立伺服器上的儲存集區，您可以執行此程序。  
  
##### <a name="to-increase-storage-pool-capacity"></a>增加儲存集區容量  
  
1.  打開儀表板。  
  
2.  按一下**存放裝置**索引標籤，然後按一下 [**磁碟機**。  
  
3.  選取 [顯示較少的容量的磁碟機。  
  
4.  從 [工作] 窗格中，選取 [**增加儲存集區容量**。 顯示增加精靈儲存集區的容量。  
  
5.  請依照下列指示完成精靈。  
  
##  <a name="BKMK_5"></a>儲存空間概觀  
 儲存空間可讓您群組一起磁碟儲存集區中。 集區容量然後可用來建立儲存空間。 儲存空間是 virtual 磁碟機會出現**硬碟**索引標籤的儀表板。 您可以使用儲存空間，例如任何其他磁碟機，讓它 s 易於使用這些檔案。 當您執行集區容量不足時，您可以建立大量儲存空間及新增更多磁碟機到儲存集區。 如果您有兩個或更多磁碟儲存集區中，您可以建立儲存空間的磁碟機故障，將不會受到影響雙向鏡像？ 或甚至的兩部磁碟機故障？ 如果您要建立三向鏡像空間。  
  
 若要建立儲存空間，您只需要是一或多個額外的磁碟機另外 Windows 安裝所在的磁碟機。 這些磁碟機可以內部或外部磁碟機或固態磁碟機。 儲存空間，包括 USB、 SATA 以及 SAS 磁碟機，您可以使用多種類型的磁碟機。  
  
> [!NOTE]
>  如果您在執行 Windows Server Essentials 的伺服器設定儲存空間，您無法執行原廠重設使用**清除資料**選項。 這個問題的因應措施是第一次移除儲存空間，然後執行 [原廠重設以**清除資料**選項。  
  
 有關更多儲存空間，請查看[儲存空間常見問題集 (FAQ)](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)。  
  
##  <a name="BKMK_6"></a>建立儲存空間  
 若要開始使用儲存空間的伺服器上，必須符合下列最低需求：  
  
-   必須執行 Windows Server Essentials 的伺服器連接到其他實體磁碟機 （而不只是將開機磁碟機），磁碟機必須裝載任何磁碟區，必須要有最少 10 GB 的容量。 一個實體磁碟機才能建立儲存集區。需要建立儲存空間復原鏡像空間至少兩個實體磁碟機。  
  
-   建立儲存空間的恢復透過同位或雙向鏡像需要至少兩個實體磁碟機。  
  
> [!NOTE]
>  如果您在執行 Windows Server Essentials 的伺服器設定儲存空間，您無法執行原廠重設使用**清除資料**選項。 這個問題的因應措施是第一次移除儲存空間，然後執行 [原廠重設以**清除資料**選項。  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>若要在 Windows Server Essentials 建立儲存空間  
  
1.  新增或連接想要儲存空間一起群組到執行 Windows Server Essentials 的伺服器的所有磁碟機。  
  
2.  從儀表板，請按一下**進階： 管理儲存空間**。  
  
3.  按一下**建立新集區與儲存空間**。  
  
4.  選取您想要新增到新儲存空間，然後按一下 [磁碟的機**建立集區**。  
  
5.  指定磁碟機名稱及代號，然後選擇 [版面配置。 **雙向鏡像**，**三向鏡像**，與**同位**可協助您的磁碟機故障時保護儲存空間中的檔案。  
  
6.  輸入最大大小的儲存空間，然後按一下**建立儲存空間**。  
  
 在 Windows Server Essentials，您可以建立雙向鏡像的空間來使用 [建立儲存空間精靈的儀表板。  
  
#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>若要在 Windows Server Essentials 建立儲存空間  
  
1.  新增或連接想要儲存空間一起群組到執行 Windows Server Essentials 的伺服器的所有磁碟機。  
  
2.  從儀表板，請按一下**管理儲存空間**。 [建立儲存空間精靈會出現。  
  
3.  請依照下列指示完成精靈。  
  
 有關增加儲存集區容量，請查看[使用新的硬碟機增加儲存集區容量](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c)。  
  
## <a name="see-also"></a>也了  
  
-   [管理伺服器資料夾](Manage-Server-Folders-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
