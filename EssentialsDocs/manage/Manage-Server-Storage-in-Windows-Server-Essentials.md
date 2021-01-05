---
title: 管理 Windows Server Essentials 中的伺服器存放裝置
description: 瞭解如何從儀表板的 [儲存體] 索引標籤上的 [硬碟] 頁面，管理所有伺服器存放裝置 (包括硬碟和儲存空間) 。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 1836682e-c7bb-4dd5-a2b5-6ff032693574
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 4f9f1c42996eb0582d9c2eb14bc88012e121e862
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97811255"
---
# <a name="manage-server-storage-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的伺服器存放裝置

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

 Windows Server Essentials 可讓您在儀表板上 [儲存] 索引標籤的 **[硬碟]** 頁面，管理所有伺服器存放裝置 (包括硬碟和儲存空間)。

 下列章節提供的資訊，可協助您增加伺服器存放裝置、了解並使用儲存空間以及管理硬碟：

-   [使用儀表板管理硬碟](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_1)

-   [增加伺服器上的存放裝置](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_2)

-   [在硬碟上執行檢查及修復](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_Check)

-   [格式化硬碟](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_3)

-   [加入新的硬碟](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4)

-   [儲存空間總覽](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_5)

-   [建立儲存空間](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_6)

##  <a name="manage-hard-drives-using-the-dashboard"></a><a name="BKMK_1"></a> 使用儀表板管理硬碟
 Windows Server Essentials 可讓您管理透過儀表板連線到伺服器的所有硬碟。 在儀表板 [儲存體] 索引標籤上，**[硬體]** 會顯示伺服器上可用於儲存資料和伺服器備份的硬碟。 伺服器會監視每個硬碟上的可用空間，並在硬碟空間過低時顯示警示。 [硬碟] 索引標籤會顯示下列資訊：

- 每部硬碟的名稱

- 每部硬碟的容量

- 每部硬碟的已使用空間量

- 每部硬碟的可用空間量

- 每部硬碟的狀態；空白狀態表示硬碟運作正常

- 詳細資料窗格，如果所選的硬碟位在儲存空間 (而非實體磁碟)，窗格即會顯示所有儲存堆疊資訊 (包括儲存集區、儲存空間和硬碟)。

  下表列出儀表板中可用的硬碟管理工作及其描述。 某些工作只有在已選取硬碟時才會顯示。

### <a name="available-hard-drive-management-tasks"></a>可用的硬碟管理工作

|任務名稱|描述|
|---------------|-----------------|
|**檢視硬碟屬性**|開啟 [ _硬碟名稱_**屬性** ] 頁面。 如果選取硬碟，便會顯示此工作。 **硬碟名稱** 屬性頁面的 [一般] 索引標籤包含下列其他工作：<br /><br /> -   **磁片磁碟機清除**：可讓您清除硬碟上未使用的檔案 (此工作僅適用于 Windows Server Essentials) 。<br />-   **檢查和修復**：檢查硬碟是否有檔案系統錯誤，並嘗試自動修復偵測到的錯誤。<br /><br /> [_硬碟名稱_**屬性**] 頁面的 [**陰影複製**] 索引標籤可讓您啟用陰影複製。 此索引標籤也會顯示已排定的陰影複製下次執行時間。|
|**管理儲存空間**|**注意：** 若為 Windows Server Essentials，只有在有現有的儲存空間時，才會顯示此工作。<br /><br /> 開啟 **[儲存空間]** 控制台，您可以從中建立並管理儲存集區與儲存空間。|
|**建立儲存空間**|開啟 [建立儲存空間精靈]，這可讓您使用一或多個硬碟以增加儲存集區容量。|
|**增加儲存集區容量**|**注意：** 只有當選取的硬碟位於儲存空間時，才會顯示此工作。<br /><br /> 開啟 [增加儲存集區容量精靈]，這可讓您使用一或多個硬碟以增加儲存集區容量。|

##  <a name="increase-storage-on-the-server"></a><a name="BKMK_2"></a> 增加伺服器上的存放裝置
 若要增加伺服器上的存放裝置，您可以將額外的內部硬碟加入伺服器。 若要加入額外的內部硬碟，必須關閉伺服器、加入內部硬碟，然後重新啟動伺服器。 如果硬碟連接到 SCSI 控制器，您就不需要關閉伺服器。 在此情況下，可以在伺服器執行時插入硬碟。

 根據要加入的硬碟是否已格式化，執行下列其中一個動作：

-   **格式化** 如果內部硬碟以 NTFS 或 ReFS 格式化，伺服器會為其指派磁碟機號，而硬碟會顯示在 [ **硬碟** ] 索引標籤上。您現在可以建立伺服器資料夾，或將其移至新的硬碟。

-   **未格式化** 如果內部硬碟未格式化，會顯示下列警示：有一或多個未格式化的硬碟連線到伺服器。 使用下列程序格式化硬碟。

#### <a name="to-format-the-hard-disk"></a>格式化硬碟

1.  開啟 [儀表板]。

2.  在流覽窗格中，按一下警示圖示，在 Windows Server Essentials 或 Windows Server Essentials 中的 [**健康情況監視**] 索引標籤中啟動 **警示檢視器**。

3.  在 [警示檢視器] 或 [健康情況監視] 索引標籤中按一下警示，然後在工作窗格中按一下 [疑難排解此問題]。

4.  請遵循指示以完成 [加入新硬碟精靈]。

###  <a name="to-clean-up-the-hard-drive"></a><a name="BKMK_Clean"></a> 清除硬碟

1.  開啟 [儀表板]。

2.  在瀏覽窗格中，按一下 [存放裝置]，然後按一下 [硬碟]。

3.  在 **[硬碟]** 區段中選取已指派給新加入硬碟的磁碟機代號，並在工作窗格中按一下 [檢視硬碟屬性]。

4.  在 **<r \> 屬性**] 的 [ **一般** ] 索引標籤上，按一下 [ **磁片磁碟機清理**]。

##  <a name="perform-checks-and-repairs-on-hard-drives"></a><a name="BKMK_Check"></a> 在硬碟上執行檢查和修復
 硬碟檢查與修復處理會檢查儲存在硬碟上之檔案系統的健康情況。 它會在儲存備份檔案的磁碟機上執行 **chkdsk** 程序。 藉由在硬碟上執行檢查與修復，可解決下列警示問題：

-   必須檢查伺服器備份中的一或多個硬碟。

#### <a name="to-check-and-repair-hard-drives"></a>檢查並修復硬碟

1.  開啟 [儀表板]。

2.  按一下 [伺服器資料夾和硬碟]，然後按一下 [硬碟]。

3.  選取顯示錯誤的硬碟，然後選取 [檢視硬碟屬性]。

4.  在 [檢查並修復] 索引標籤上按一下 [檢查並修復]。

##  <a name="format-hard-drives"></a><a name="BKMK_3"></a> 格式化硬碟
 在伺服器上偵測到未格式化的內部硬碟時，健康情況警示會引導使用者進行將其格式化的程序。 [加入新硬碟精靈] 會逐步引導您格式化硬碟，並且可讓您以下列其中一種方式設定硬碟：

1.  格式化硬碟並在硬碟上自動建立磁碟機。 如果選擇此選項，當精靈完成時，便會建立一個以 NTFS 檔案系統格式化的邏輯硬碟。

2.  格式化硬碟並為伺服器備份加以設定。 如果選擇此選項，便會啟動 [設定伺服器備份精靈]，逐步引導您設定伺服器備份。

3.  如果儲存空間不存在，請使用新的硬碟來建立儲存空間。 您必須要有至少兩個硬碟才可建立儲存空間。

4.  如果儲存空間已經存在，請使用新的硬碟增加儲存集區容量。 只有在現有儲存空間建立於伺服器上時，才會顯示此選項。 如果選擇此選項，精靈會將此硬碟加入儲存集區。

##  <a name="add-a-new-hard-drive"></a><a name="BKMK_4"></a> 新增硬碟
 當您將新的硬碟插入執行 Windows Server Essentials 的伺服器時，您可以：

-   [使用新的硬碟儲存伺服器資料夾](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4a)

-   [使用新的硬碟來儲存伺服器備份](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4b)

-   [使用新硬碟增加儲存集區容量](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c)

###  <a name="use-the-new-hard-drive-to-store-server-folders"></a><a name="BKMK_4a"></a> 使用新硬碟來儲存伺服器資料夾
 若要使用新的硬碟儲存伺服器資料夾，您可以將新的伺服器資料夾加入硬碟，或將現有的伺服器資料夾移至硬碟。

##### <a name="to-store-server-folders"></a>儲存伺服器資料夾

1. 開啟 [儀表板]。

2. 按一下 [存放裝置] 索引標籤，然後按一下 [伺服器資料夾]。

3. 在 **[伺服器資料夾工作]** 窗格中，執行下列其中一個動作：

   1.  若要加入伺服器資料夾，請按一下 [加入資料夾]。

   2.  若要移動伺服器資料夾，請選取您要移動到新硬碟的資料夾，然後按一下 [移動資料夾]。

   > [!NOTE]
   >  如果您流覽至硬碟，並在不建立資料夾的情況下將其選取為伺服器資料夾位置，則會出現下列錯誤訊息： **根目錄 (例如 C： \\ 、D： \\) 無法新增為伺服器資料夾。建立新的資料夾，或在根目錄下選取現有的資料夾，然後再試一次**。 若要解決這個錯誤，請在新加入的硬碟中建立新資料夾，然後選取新資料夾做為儲存伺服器資料夾的位置。

4. 遵循指示以完成精靈。

   如需移動伺服器資料夾的詳細資訊，請參閱 [Add or move a server folder](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)。

###  <a name="use-the-new-hard-drive-to-store-server-backups"></a><a name="BKMK_4b"></a> 使用新硬碟來儲存伺服器備份
 您可以使用新加入的硬碟來儲存伺服器備份。

##### <a name="to-store-server-backups"></a>儲存伺服器備份

1. 開啟 [儀表板]。

2. 按一下 [裝置] 索引標籤，從清單窗格中選取伺服器，然後在工作窗格執行下列其中一個動作：

   1. 如果在伺服器上未設定伺服器備份，請按一下 [設定伺服器備份]。

   2. 如果伺服器上已設定伺服器備份，請按一下 [自訂伺服器備份]。

      [設定伺服器備份精靈] 隨即出現。

3. 在 **[選取備份目的地]** 頁面上，選取新硬碟做為備份目的地。

4. 遵循指示以完成精靈。

###  <a name="use-the-new-hard-drive-to-increase-storage-pool-capacity"></a><a name="BKMK_4c"></a> 使用新硬碟來增加儲存集區容量
 當儲存集區容量過低時，您會收到警示，告知您可以使用 [增加儲存集區容量精靈] 將新的硬碟加入儲存集區，藉此增加儲存集區容量。

> [!NOTE]
>  只有當您已在伺服器上建立儲存集區時，才可執行此程序。

##### <a name="to-increase-storage-pool-capacity"></a>增加儲存集區容量

1.  開啟 [儀表板]。

2.  按一下 [存放裝置] 索引標籤，然後按一下 [硬碟]。

3.  選取顯示低容量的磁碟機。

4.  從工作窗格中選取 [增加儲存集區容量]。 [增加儲存集區容量精靈] 隨即出現。

5.  遵循指示以完成精靈。

##  <a name="storage-spaces-overview"></a><a name="BKMK_5"></a> 儲存空間總覽
 儲存空間可讓您將磁碟區聚集在儲存集區中。 接著，您便可以使用集區容量建立儲存空間。 儲存空間是會顯示在儀表板 [硬碟] 索引標籤上的虛擬硬碟。 您可以像任何其他磁片磁碟機一樣使用儲存空間，因此很容易就能使用檔案。 當集區容量不足時，您可以建立大型的儲存空間，以及在儲存集區中加入更多磁碟機。 如果您在存放集區中有兩個或多個磁片，您可以建立具有雙向鏡像的儲存空間，而不會受磁片磁碟機故障影響？或甚至是兩個磁片磁碟機的失敗？如果您建立三向鏡像儲存空間。

 若要建立儲存空間，除了已安裝 Windows 的磁碟機以外，您只需要一或多部額外的磁碟機。 這些磁碟機可以是內部或外部硬碟，或固態硬碟。 您可以搭配儲存空間使用各種類型的磁碟機，包括 USB、SATA 和 SAS 磁碟機。

> [!NOTE]
>  如果您在執行 Windows Server Essentials 的伺服器上設定儲存空間，您就無法使用 [ **清除資料** ] 選項來執行原廠重設。 此問題的因應措施是先移除儲存空間，然後使用 [清除資料] 選項恢復出廠預設值。

 如需儲存空間的詳細資訊，請參閱 [儲存空間常見問題集 (FAQ)](/windows-server/storage/storage-spaces/storage-spaces-direct-faq)。

##  <a name="create-a-storage-space"></a><a name="BKMK_6"></a> 建立儲存空間
 若要開始使用伺服器上的儲存空間，必須符合下列最低需求：

-   執行 Windows Server Essentials 的伺服器必須連接到其他實體磁碟機 (而不只有開機磁碟機)、磁碟機必須未裝載任何磁碟區，且必須具備 10 GB 的最小容量。 建立儲存集區需要一部實體磁碟機；建立可復原的鏡像儲存空間至少需要兩部實體磁碟機。

-   建立透過同位或雙向鏡像復原的儲存空間，至少需要兩部實體磁碟機。

> [!NOTE]
>  如果您在執行 Windows Server Essentials 的伺服器上設定儲存空間，您就無法使用 [ **清除資料** ] 選項來執行原廠重設。 此問題的因應措施是先移除儲存空間，然後使用 [清除資料] 選項恢復出廠預設值。

#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>若要在 Windows Server Essentials 中建立儲存空間

1. 將您要與儲存空間聚集的所有磁碟機，加入或連線到執行 Windows Server Essentials 的伺服器。

2. 在儀表板上，按一下 [進階：] **[管理儲存空間]**。

3. 按一下 [建立新的集區和儲存空間]。

4. 選取您要加入新儲存空間的磁碟機，然後按一下 [建立集區]。

5. 提供磁碟機名稱和代號，然後選擇配置。 **[雙向鏡像]**、**[三向鏡像]** 和 **[同位]** 有助於保護儲存空間中的檔案不受磁碟機故障影響。

6. 輸入儲存空間可達到的大小上限，然後按一下 [建立儲存空間]。

   在 Windows Server Essentials 中，您可以使用 [儀表板] 中的 [建立儲存空間] 嚮導來建立雙向鏡像儲存空間。

#### <a name="to-create-a-storage-space-in-windows-server-essentials"></a>若要在 Windows Server Essentials 中建立儲存空間

1. 將您要與儲存空間聚集的所有磁碟機，加入或連線到執行 Windows Server Essentials 的伺服器。

2. 在儀表板上按一下 [管理儲存空間]。 [建立儲存空間精靈] 隨即出現。

3. 遵循指示以完成精靈。

   如需增加儲存集區容量的相關資訊，請參閱 [Use the new hard drive to increase storage pool capacity](Manage-Server-Storage-in-Windows-Server-Essentials.md#BKMK_4c)。

## <a name="additional-references"></a>其他參考資料

-   [管理伺服器資料夾](Manage-Server-Folders-in-Windows-Server-Essentials.md)

-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)