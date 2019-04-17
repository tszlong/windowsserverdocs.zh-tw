---
title: "管理 Windows Server Essentials 伺服器資料夾"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 090cf1b8-7b9b-48b9-ae85-b98477b8d7cc
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e1f3640f21b95acafa850b2204cd52f9c0f324e5
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="manage-server-folders-in-windows-server-essentials"></a>管理 Windows Server Essentials 伺服器資料夾

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集
  
 身為伺服器管理員中，您可以管理存取的任何伺服器資料夾 （稱為從 Launchpad、 遠端 Web 存取、 我伺服器應用程式適用於 Windows Phone 或我伺服器適用於 Windows 8 的應用程式存取時的共用資料夾） 伺服器上使用工作上**伺服器資料夾**索引標籤的儀表板可讓使用者的不同層級的存取權各種不同的檔案。  
  
 下列主題會提供資訊可協助您了解、 建立及管理伺服器資料夾：  
  
-   [管理伺服器資料夾使用儀表板](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [管理伺服器資料夾的存取權](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [新增或移動伺服器資料夾](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [新增資料夾遺失伺服器](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [了解共用的資料夾](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [了解陰影複製](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Shadow)  
  
##  <a name="BKMK_2"></a>管理伺服器資料夾使用儀表板  
 Windows Server Essentials 可讓您可以使用儀表板執行一般管理工作。 **資料夾伺服器**頁面上的儀表板可提供動作：  
  
-   伺服器資料夾清單的顯示：  
  
    -   資料夾的名稱  
  
    -   資料夾的描述  
  
    -   資料夾的位置  
  
    -   免費的資料夾位置的可用空間量  
  
    -   簡短的狀態資訊任何資料夾; 正在執行的工作**狀態**如果資料夾健康狀態，而正在執行的工作是空白必填欄位  
  
-   詳細資料窗格，可能會提供其他資訊選取的資料夾  
  
-   包含的一組資料夾相關管理工作工作] 窗格  
  
 下表描述 Windows Server Essentials 儀表板，您可以使用各種伺服器資料夾工作。 大部分的工作資料夾特定且都是只顯示當您在清單中選取的資料夾。  
  
### <a name="server-folder-tasks-on-the-dashboard"></a>伺服器資料夾工作儀表板  
  
|工作的名稱|描述|  
|---------------|-----------------|  
|打開資料夾|在 [檔案總管] （先前的 Windows 版本中稱為 [Windows 檔案總管]），會顯示到選取的資料夾。|  
|Delete 資料夾|可讓您 delete 使用者建立資料夾。 這項工作不適用於預設伺服器安裝建立的資料夾。|  
|移動資料夾|開啟精靈可協助您的 [伺服器] 資料夾移動到新的位置。|  
|停止分享] 資料夾|停止分享所選取的資料夾，但不是 delete。 不再共用資料夾時，它不會出現在儀表板。 這項工作不適用於預設伺服器安裝建立的資料夾。|  
|檢視資料夾屬性|顯示已選取的資料夾，屬性，可讓您：<br /><br /> -[變更使用者建立資料夾的名稱。<br /><br /> 變更描述選取的資料夾。<br /><br /> -檢視資料夾的大小。<br /><br /> -在檔案總管] 左選取的資料夾。<br /><br /> -指定使用者 account 存取權限已選取的資料夾。<br /><br /> -隱藏存取網路和 Web 服務應用程式從選取的資料夾。<br /><br /> -指定資料夾配額。|  
|新增資料夾|可協助您建立新的伺服器資料夾和指派的存取權的每個使用者 account 允許層級。|  
|了解伺服器資料夾|開啟協助主題描述伺服器資料夾中的功能與使用網際網路上。|  
  
##  <a name="BKMK_1"></a>管理伺服器資料夾的存取權  
 Windows Server Essentials 可讓您使用伺服器資料夾中央位置 client 電腦上儲存位於檔案。 將您的檔案儲存在伺服器資料夾可確保您的檔案都是安全的方式存取從每個 client 的地方。  
  
 使用伺服器資料夾中儲存您的檔案可讓您：  
  
-   防範伺服器總失敗使用伺服器備份與還原備份的伺服器資料夾。  
  
-   適用於 Windows Phone 和 Windows 8 中使用網際網路瀏覽器透過遠端網路存取，或透過我伺服器應用程式儲存上從任何位置的伺服器資料夾的存取檔案。  
  
-   從 client 的任何電腦存取伺服器新資料夾。  
  
 您可以管理伺服器上的任何伺服器資料夾的存取權的工作，利用**資料夾伺服器**索引標籤的儀表板。 下表列出伺服器安裝 Windows Server Essentials 或將媒體串流伺服器上時，預設所建立的資料夾。  
  
|伺服器資料夾名稱|描述|  
|------------------------|-----------------|  
|Client 電腦備份|根據預設，Windows Server Essentials 建立 client 電腦這個資料夾中儲存的備份。 網路系統管理員可以修改 Client 電腦備份的設定。|  
|公司|用來儲存和存取您的組織網路使用者的相關文件。|  
|檔案歷史備份|根據預設，Windows Server Essentials 會用來建立檔案的備份儲存在此資料夾的檔案歷史。 網路系統管理員可以修改這些檔案歷史設定。|  
|資料夾重新導向|用來儲存和存取資料夾的資料夾重新導向網路使用者的設定。|  
|使用者|用來儲存和存取網路使用者的檔案。 使用者特定資料夾自動也在**使用者**伺服器的每個網路帳號，您所建立的資料夾。|  
|音樂|用來儲存和存取網路使用者的音樂檔案。 您將媒體共用時，使用此資料夾。|  
|圖片|用來儲存和存取網路使用者的圖片檔案。 您將媒體共用時，使用此資料夾。|  
|錄製的電視節目|用來儲存和存取錄製的網路使用者的電視節目。 您將媒體共用時，使用此資料夾。|  
|影片|用來儲存和存取網路使用者的影片檔案。 您將媒體共用時，使用此資料夾。|  
  
 若要隱藏或設定權限伺服器資料夾，或修改伺服器資料夾屬性，請查看下列程序：  
  
-   [隱藏伺服器資料夾](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Hide)  
  
-   [設定伺服器資料夾的權限](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Perms)  
  
-   [檢視或修改伺服器資料夾屬性](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_10)  
  
###  <a name="BKMK_Hide"></a>隱藏伺服器資料夾  
 為網路系統管理員，您可以選擇要隱藏這些伺服器的資料夾，避免遠端網站存取的網站或 Web 服務 （例如我伺服器） 的應用程式上顯示。  
  
> [!NOTE]
>  您必須網路系統管理員身分執行此程序。  
  
##### <a name="to-hide-server-folders-from-being-displayed-in-remote-web-access"></a>若要隱藏顯示在網路上的存取伺服器資料夾  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  按一下**存放裝置**，然後按一下 [**伺服器資料夾**。  
  
3.  在清單檢視中，選取 [伺服器資料夾您想要檢視或修改的屬性。  
  
4.  在**< ServerFolder\ > 工作**窗格中，按**屬性資料夾的檢視**。  
  
5.  在**< FolderName\ > 屬性**，按一下 [**共用**，請選取**隱藏此資料夾的存取網路和 Web 服務應用程式**，然後按一下**套用**。  
  
###  <a name="BKMK_Perms"></a>設定伺服器資料夾的權限  
 利用儀表板伺服器上新增任何其他伺服器資料夾，您可以選擇它三種不同的存取設定：  
  
-   **讀取/寫入**  
  
     如果您想要允許此人建立、 變更或 delete 伺服器資料夾中的任何檔案，請選擇這個設定。  
  
-   **唯讀模式**  
  
     如果您想要允許此人只讀取伺服器資料夾中的檔案，請選擇這個設定。 唯讀存取的使用者無法建立、 變更或 delete 伺服器資料夾中的任何檔案。  
  
-   **不能存取**  
  
     如果您不希望此人存取伺服器資料夾中的任何檔案，請選擇這個設定。  
  
> [!IMPORTANT]
>  顯示資料夾屬性的權限代表受儀表板的使用者。 它們不包含使用者權限的群組或服務帳號，例如或是包含可能資料夾中設定使用其他原生的工具，或包含不新增至儀表板的使用者權限。  
  
> [!NOTE]
>  您必須網路系統管理員身分執行此程序。  
  
##### <a name="to-set-permissions-to-server-folders-on-the-server"></a>設定伺服器伺服器資料夾權限  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  按一下**存放裝置**，然後按一下 [**伺服器資料夾**。  
  
3.  在清單檢視中，選取 [伺服器資料夾您想要檢視或修改的屬性。  
  
4.  在**< ServerFolder\ > 工作**窗格中，按**屬性資料夾的檢視**。  
  
5.  在**< FolderName\ > 屬性**，按一下 [**共用**，並選取適當的使用者存取層級列出的帳號，然後按一下 [**套用]**。  
  
> [!NOTE]
>  根據預設，當您新增到您的網路的使用者 account 子資料夾建立的使用者，在**使用者**在伺服器上的資料夾。 子資料夾的使用者或系統管理員可以存取的網路的電腦。 權限的設定在每個子資料夾的**使用者**，讓您有任何一般的存取權限的最上層**使用者**資料夾。  
  
> [!NOTE]
>  您無法修改共用的權限的**檔案歷史備份**，**資料夾重新導向**，並**使用者**伺服器資料夾。 因此，不包含下列伺服器資料夾的資料夾屬性**共用**索引標籤。  
  
###  <a name="BKMK_10"></a>檢視或修改伺服器資料夾屬性  
 您可以修改伺服器資料夾名稱、 描述，並定義哪個帳號存取伺服器資料夾透過**檢視資料夾屬性**工作上**資料夾伺服器**索引標籤的儀表板。  
  
> [!NOTE]
>  在 Windows Server Essentials 和 Windows Server 2012 R2 與 Windows Server Essentials 體驗角色安裝，您也可以修改配額資料夾。  
  
##### <a name="to-view-or-modify-folder-properties"></a>若要檢視或修改屬性資料夾  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  按一下**存放裝置**，然後按一下 [**伺服器資料夾**。  
  
3.  在清單檢視中，選取 [伺服器資料夾您想要檢視或修改的屬性。  
  
4.  在**< ServerFolder\ > 工作**窗格中，按**屬性資料夾的檢視**。  
  
5.  在**< Foldername\ > 屬性**，在**一般**索引標籤上，檢視或修改描述伺服器資料夾的名稱。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 和 Windows Server 2012 R2 與 Windows Server Essentials 體驗角色安裝，您也可以修改，以提供警告訊息時伺服器資料夾到達尺寸指定的資料夾配額。  
  
##  <a name="BKMK_5"></a>新增或移動伺服器資料夾  
 您可以**新增更多伺服器資料夾**以儲存您的預設伺服器資料夾會建立在設定期間，除了伺服器上的檔案。 您可以新增伺服器資料夾主要伺服器或執行 Windows Server Essentials 的成員伺服器上。  
  
 您可以**移動伺服器資料夾**，位於執行 Windows Server Essentials 的主要伺服器，並且會顯示在**伺服器資料夾**索引標籤上的另一部磁碟機時所需的使用 [移動資料夾精靈儀表板。 如果，您可以伺服器] 資料夾移動到另一個硬碟位置的地址：  
  
-   資料硬碟已經不再資料儲存空間不足。  
  
-   您想要變更的預設儲存位置。 更快速地移動，請考慮移動伺服器資料夾，而不包含任何資料。  
  
-   您想要移除現有的硬碟機而不會遺失的伺服器資料夾，這位於。  
  
 日前資料夾，請確定下列動作：  
  
-   請確定您已備份您的伺服器。  
  
-   確認所有 client 備份都停止，而不是在進度，如果您在 [移動資料夾 Client 電腦備份計劃。 移動 Client 電腦備份資料夾、 同時伺服器將無法備份任何 client 電腦，直到已完成] 資料夾移動。  
  
-   請確定伺服器無法執行任何重要的系統作業。 建議您完成任何更新或進行中的某個資料夾開始之前備份移動或處理程序可能需要較長的時間來完成。  
  
-   在 [移動] 資料夾的檔案都使用。 您將無法存取伺服器資料夾，而正在移動。  
  
 如果在 [伺服器] 資料夾的檔案實作下列技術不受支援資料夾的 NTFS 前往 ReFS:  
  
-   替代資料流  
  
-   Id 物件  
  
-   簡短的名稱 （8.3 名稱）  
  
-   壓縮  
  
-   EFS 加密  
  
-   交易的 NTFS、 TxF （引進了 Windows Vista）  
  
-   疏鬆檔案  
  
-   硬碟連結  
  
-   延伸屬性  
  
-   配額  
  
###  <a name="BKMK_6"></a>若要新增或移動伺服器資料夾位置  
 一般而言，您應該新增或移動伺服器資料夾拖曳到硬碟擁有最大的可用空間量。 如果可能，請避免新增或共用的資料夾移動到系統磁碟機 （例如 c:），在該地點的氣象可能需要所需的作業系統，其更新所需的磁碟機空間。 此外，請避免新增或移動到外部硬碟機伺服器資料夾，因為它們可以輕鬆地中斷，如此一來，您可能就無法存取您的檔案。 而是，我們建議您內部磁碟機上建立資料夾。  
  
 無法新增或移至下列位置伺服器資料夾和如果新增項目，或移選取這些位置，將導致發生錯誤︰  
  
-   不以的 NTFS 或 ReFS 檔案系統格式化硬碟磁碟機  
  
-   %Windir%資料夾  
  
-   對應的網路磁碟機]  
  
-   包含的共用的資料夾的資料夾  
  
-   硬碟位於**裝置與抽取式存放裝置**  
  
-   根 directory 硬碟磁碟機 （例如 C:\\，D:\\，E:\\)  
  
-   現有的共用資料夾的子資料夾  
  
-   已安裝 Windows Server Essentials 體驗角色執行 Windows Server Essentials 或 Windows Server 2012 R2 成員伺服器  
  
### <a name="steps-to-add-or-move-a-server-folder"></a>步驟來新增或移動伺服器資料夾  
  
> [!NOTE]
>  您必須由伺服器管理員完成這些程序。  
  
##### <a name="to-add-a-server-folder"></a>若要新增的伺服器資料夾  
  
1.  打開儀表板。  
  
2.  按一下**存放裝置**，然後按一下 [**伺服器資料夾**。  
  
3.  在**伺服器資料夾工作**，按一下 [**資料夾新增**。 這時限新增資料夾] 精靈。  
  
4.  請依照下列指示完成精靈。  
  
    > [!NOTE]
    >  -   如果您使用瀏覽按鈕，指定的伺服器資料夾位置的特定資料夾瀏覽，您已經看過的資料夾會新增為伺服器資料夾中。  
    > -   您可以定義透過遠端網路存取可存取伺服器資料夾。 如需詳細資訊，請查看[管理伺服器資料夾的存取權](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1)。  
  
##### <a name="to-move-a-server-folder"></a>若要將伺服器] 資料夾移動  
  
1.  打開儀表板。  
  
2.  按一下**存放裝置**，然後按一下 [**伺服器資料夾**。  
  
3.  從清單中的伺服器資料夾中，選取您想要移動的資料夾。  
  
4.  在 [工作] 窗格中，按一下**[移動資料夾**。  
  
5.  請依照下列指示完成精靈。  
  
##  <a name="BKMK_9"></a>新增資料夾遺失伺服器  
 如果伺服器偵測到的一個預先定義的伺服器資料夾？公司、 使用者、 Client 電腦備份、 檔案歷史備份或資料夾重新導向？ 不再共用 （適用於某些原因或其他），警示引導修正此問題的相關的使用者。 建議您嘗試從伺服器備份還原的資料夾。 不過，如果伺服器未備份之後，選取遺失的資料夾，然後按一下**重新建立的資料夾遺失**以重新設定伺服器資料夾的位置。  
  
> [!NOTE]
>  預先定義的資料夾？公司、 使用者、 Client 電腦備份、 檔案歷史備份或資料夾重新導向？ 可以重新建立。 無法重新建立的使用者建立伺服器資料夾與媒體伺服器資料夾。  
  
 還原或重新建立遺失資料夾之後，它應該不會再列為**遺失]**。  
  
 從伺服器備份還原檔案的相關資訊，會看到區段深入了解更多還原的檔案和資料夾主題中的相關[管理備份與還原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)。  
  
##  <a name="BKMK_11"></a>了解共用的資料夾  
 有幾種不同方式可從裝置連接到伺服器存取您在 Windows Server Essentials 的共用的資料夾。 如需詳細資訊，請查看主題[使用共用資料夾](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)。  
  
##  <a name="BKMK_Shadow"></a>了解陰影複製  
 伺服器陰影複製的使用者都可以檢視共用的檔案和資料夾存在於過去中的時間點。 存取舊版檔案或陰影複製很有用，因為使用者可以：  
  
1.  **復原檔案已不慎的**。 如果您不小心 delete 檔案，您可以開放先前的版本，並將它複製到安全的位置。  
  
2.  **復原意外覆寫檔案**。 如果您不小心要覆寫檔案，您可以復原先前的版本的檔案。 (的版本而定您已經建立多少快照。)  
  
3.  **比較時工作檔案的版本**。 當您想要查看不同版本的檔案已變更的項目，您可以使用先前的版本。  
  
 若要使用陰影複製從 client 的電腦，伺服器共用的資料夾上按一下滑鼠右鍵，然後選取**還原舊版**。  
  
## <a name="see-also"></a>也了  
  
-   [管理伺服器儲存空間](Manage-Server-Storage-in-Windows-Server-Essentials.md)  
  
-   [使用共用資料夾](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
