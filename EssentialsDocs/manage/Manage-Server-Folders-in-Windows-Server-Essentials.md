---
title: 管理 Windows Server Essentials 中的伺服器資料夾
description: 說明如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 090cf1b8-7b9b-48b9-ae85-b98477b8d7cc
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6ba5dd5e5978687c9d80a6d34e5e622aaa37c4b9
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311127"
---
# <a name="manage-server-folders-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的伺服器資料夾

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials
  
 身為伺服器系統管理員，您可以使用 [儀表板] 之 [**伺服器資料夾**] 索引標籤上的工作，在伺服器上管理任何伺服器資料夾（稱為共用資料夾，從 [啟動列]、[遠端 Web 存取]、[我的伺服器應用 Windows Phone 程式] 或 [Windows 8] 的 [我的伺服器] 應用程式）存取。  
  
 下列主題提供可協助您了解、建立及管理伺服器資料夾的資訊：  
  
-   [使用儀表板管理伺服器資料夾](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [管理伺服器資料夾的存取權](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [新增或移動伺服器資料夾](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [新增遺失的伺服器資料夾](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [瞭解共用資料夾](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [瞭解陰影複製](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Shadow)  
  
##  <a name="manage-server-folders-using-the-dashboard"></a><a name="BKMK_2"></a>使用儀表板管理伺服器資料夾  
 Windows Server Essentials 可讓您使用 [儀表板] 來執行一般系統管理工作。 [儀表板] 的 [伺服器資料夾] 頁面提供下列各項：  
  
- 一份伺服器資料夾清單，當中顯示：  
  
  -   資料夾的名稱  
  
  -   資料夾的描述  
  
  -   資料夾的位置  
  
  -   資料夾位置的可用空間大小  
  
  -   與正在資料夾上執行之任何工作相關的簡要狀態資訊；如果資料夾的狀況良好且沒有工作正在執行，[狀態] 欄位就會空白  
  
- 一個可能提供與所選資料夾相關之其他資訊的詳細資料窗格  
  
- 一個包含一組資料夾相關系統管理工作的工作窗格  
  
  下表說明 [Windows Server Essentials 儀表板] 上可用的各種伺服器資料夾工作。 大多數工作都是資料夾特定的工作，只有當您選取清單中的某個資料夾時才能看見。  
  
### <a name="server-folder-tasks-on-the-dashboard"></a>儀表板上的伺服器資料夾工作  
  
|工作名稱|描述|  
|---------------|-----------------|  
|開啟資料夾|會在 [檔案總管] (在舊版的 Windows 中稱為 [Windows 檔案總管]) 中顯示所選資料夾的內容。|  
|刪除資料夾|可讓您刪除使用者建立的資料夾。 這個工作不適用於伺服器安裝所建立的預設資料夾。|  
|移動資料夾|會開啟可協助您將伺服器資料夾移到新位置的精靈。|  
|停止共用資料夾|會將選取的資料夾停止共用，但不會刪除它。 當資料夾已不再共用時，它就不會出現在 [儀表板] 中。 這個工作不適用於伺服器安裝所建立的預設資料夾。|  
|檢視資料夾內容|會顯示所選資料夾的內容，並可讓您：<br /><br /> -變更使用者建立之資料夾的名稱。<br /><br /> -變更所選資料夾的描述。<br /><br /> -查看資料夾的大小。<br /><br /> -在檔案瀏覽器中開啟選取的資料夾。<br /><br /> -指定所選資料夾的使用者帳戶存取權限。<br /><br /> -從遠端 Web 存取和 Web 服務應用程式中隱藏選取的資料夾。<br /><br /> -指定資料夾配額。|  
|新增資料夾|會協助您建立新的伺服器資料夾，以及指派針對每個使用者帳戶允許的存取層級。|  
|了解伺服器資料夾|會開啟網際網路上的說明主題，當中說明伺服器資料夾的使用方式和功能。|  
  
##  <a name="manage-access-to-server-folders"></a><a name="BKMK_1"></a>管理伺服器資料夾的存取權  
 Windows Server Essentials 可讓您藉由使用伺服器資料夾，將用戶端電腦上的檔案儲存在一個集中位置。 將您的檔案儲存在伺服器資料夾中，可確保那些檔案是位於一個永遠都能從每個用戶端安全存取的位置。  
  
 使用伺服器資料夾來儲存您的檔案可讓您：  
  
- 使用「伺服器備份與還原」來備份伺服器資料夾，以協助防範伺服器全面故障。  
  
- 使用網際網路瀏覽器透過「遠端 Web 存取」，或透過適用於 Windows Phone 和 Windows 8 的 My Server 應用程式，從任何位置存取儲存在伺服器資料夾上的檔案。  
  
- 從任何用戶端電腦存取新的伺服器資料夾。  
  
  您可以使用 [儀表板] 之 [伺服器資料夾] 索引標籤上的工作，來管理伺服器上任何伺服器資料夾的存取權。 下表列出當您在伺服器上安裝 Windows Server Essentials 或開啟媒體串流處理時，預設會建立的伺服器資料夾。  
  
|伺服器資料夾名稱|描述|  
|------------------------|-----------------|  
|用戶端電腦備份|Windows Server Essentials 預設會建立儲存在這個資料夾中的用戶端電腦備份。 網路系統管理員可以修改「用戶端電腦備份」的設定。|  
|公司|用來供網路使用者儲存及存取與您組織相關的文件。|  
|檔案歷程記錄備份|Windows Server Essentials 預設會使用「檔案歷程記錄」來建立儲存在這個資料夾中的檔案備份。 網路系統管理員可以修改這些「檔案歷程記錄」的設定。|  
|資料夾重新導向|用來供網路使用者儲存及存取為資料夾重新導向設定的資料夾。|  
|使用者|用來供網路使用者儲存及存取檔案。 在 [使用者] 伺服器資料夾中，會自動為您建立的每個網路使用者帳戶產生一個使用者特定資料夾。|  
|音樂|用來供網路使用者儲存及存取音樂檔案。 當您開啟媒體共用時，就會有這個資料夾。|  
|圖片|用來供網路使用者儲存及存取圖片檔案。 當您開啟媒體共用時，就會有這個資料夾。|  
|錄製節目|用來供網路使用者儲存及存取錄製的電視節目。 當您開啟媒體共用時，就會有這個資料夾。|  
|Videos|用來供網路使用者儲存及存取視訊檔案。 當您開啟媒體共用時，就會有這個資料夾。|  
  
 若要隱藏或設定伺服器資料夾的權限，或修改伺服器資料夾內容，請參閱下列程序：  
  
-   [隱藏伺服器資料夾](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Hide)  
  
-   [設定伺服器資料夾的許可權](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_Perms)  
  
-   [View 或 modify server 資料夾屬性](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_10)  
  
###  <a name="hide-server-folders"></a><a name="BKMK_Hide"></a>隱藏伺服器資料夾  
 如果您是網路系統管理員，您可以選擇隱藏任何這些伺服器資料夾，防止它們在「遠端 Web 存取」網站或「Web 服務」應用程式 (例如 My Server) 上顯示。  
  
> [!NOTE]
>  您必須是網路系統管理員，才能執行這個程序。  
  
##### <a name="to-hide-server-folders-from-being-displayed-in-remote-web-access"></a>在遠端 Web 存取中隱藏隱藏伺服器資料夾  
  
1.  開啟 [Windows Server Essentials 儀表板]。  
  
2.  按一下 [存放]，然後按一下 [伺服器資料夾]。  
  
3.  在清單檢視中，選取您想要檢視或修改其內容的伺服器資料夾。  
  
4.  在 [ **< r\>** 工作] 窗格中，按一下 [**查看資料夾屬性**]。  
  
5.  在 **< 資料夾名\> 屬性** 中，按一下 **共用**，選取 **從遠端 Web 存取和 Web 服務應用程式隱藏此資料夾**，**然後按一下** 套用。  
  
###  <a name="set-permissions-to-server-folders"></a><a name="BKMK_Perms"></a>設定伺服器資料夾的許可權  
 對於您使用 [儀表板] 在伺服器上新增的任何其他伺服器資料夾，您可以為其選擇三種不同的存取設定：  
  
-   **讀取/寫入**  
  
     如果您想要讓這位使用者建立、變更及刪除伺服器資料夾中的任何檔案，請選擇這個設定。  
  
-   **唯讀**  
  
     如果您想要讓這位使用者只能讀取伺服器資料夾中的檔案，請選擇這個設定。 具有唯讀存取權的使用者無法建立、變更或刪除伺服器資料夾中的任何檔案。  
  
-   **無存取權**  
  
     如果您不想要讓這位使用者存取伺服器資料夾中的任何檔案，請選擇這個設定。  
  
> [!IMPORTANT]
>  資料夾內容中顯示的權限只代表 [儀表板] 所管理的使用者。 它們不包含群組或服務帳戶之類的使用者權限、可能是使用其他原生工具在資料夾上設定的任何權限，也不包含不是透過 [儀表板] 新增的使用者。  
  
> [!NOTE]
>  您必須是網路系統管理員，才能執行這個程序。  
  
##### <a name="to-set-permissions-to-server-folders-on-the-server"></a>設定對伺服器上伺服器資料夾的權限  
  
1.  開啟 [Windows Server Essentials 儀表板]。  
  
2.  按一下 [存放]，然後按一下 [伺服器資料夾]。  
  
3.  在清單檢視中，選取您想要檢視或修改其內容的伺服器資料夾。  
  
4.  在 [ **< r\>** 工作] 窗格中，按一下 [**查看資料夾屬性**]。  
  
5.  在  **< 資料夾名\> 屬性** 中，按一下 **共用**，然後為列出的使用者帳戶選取適當的使用者存取層級，**然後按一下** 套用。  
  
> [!NOTE]
>  根據預設，當您將使用者帳戶新增到網路時，會在伺服器上的 [使用者] 資料夾底下為該使用者建立一個子資料夾。 只有該使用者或系統管理員可以從網路電腦存取該子資料夾。 系統會為 [使用者] 底下的每個子資料夾設定權限，因此最上層的 [使用者] 資料夾並沒有一般存取權限。  
  
> [!NOTE]
>  您無法修改 [檔案歷程記錄備份]、[資料夾重新導向] 及 [使用者] 伺服器資料夾的共用權限。 因此，這些伺服器資料夾的資料夾內容也就沒有包含 [共用] 索引標籤。  
  
###  <a name="view-or-modify-server-folder-properties"></a><a name="BKMK_10"></a>View 或 modify server 資料夾屬性  
 您可以修改伺服器資料夾名稱、其描述，以及定義哪些使用者帳戶可以透過 [儀表板] 之 [伺服器資料夾] 索引標籤上的 [檢視資料夾內容] 工作存取伺服器資料夾。  
  
> [!NOTE]
>  在 Windows Server Essentials 和已安裝「Windows Server Essentials 體驗」角色的 Windows Server 2012 R2 中，您也可以修改資料夾配額。  
  
##### <a name="to-view-or-modify-folder-properties"></a>檢視或修改資料夾內容  
  
1.  開啟 [Windows Server Essentials 儀表板]。  
  
2.  按一下 [存放]，然後按一下 [伺服器資料夾]。  
  
3.  在清單檢視中，選取您想要檢視或修改其內容的伺服器資料夾。  
  
4.  在 [ **< r\>** 工作] 窗格中，按一下 [**查看資料夾屬性**]。  
  
5.  在  **< 資料夾名稱\> 屬性** 的 **一般** 索引標籤上，查看或修改伺服器資料夾的名稱和描述。  
  
    > [!NOTE]
    >  在 Windows Server Essentials 和已安裝「Windows Server Essentials 體驗」角色的 Windows Server 2012 R2 中，您也可以修改資料夾配額，以在伺服器資料夾達到指定的大小時提供警告訊息。  
  
##  <a name="add-or-move-a-server-folder"></a><a name="BKMK_5"></a>新增或移動伺服器資料夾  
 除了在安裝期間建立的預設伺服器資料夾之外，您也可以在伺服器上**新增更多伺服器資料夾**來儲存您的檔案。 您可以在執行 Windows Server Essentials 的主要伺服器或成員伺服器上新增伺服器資料夾。  
  
 您可以視需要使用**移動資料夾精靈**，將位於執行 Windows Server Essentials 之主要伺服器上並顯示在 [儀表板] 之 [伺服器資料夾] 索引標籤上的伺服器資料夾移動到另一個硬碟。 在下列情況下，您可以將伺服器資料夾移到另一個硬碟位置位址：  
  
- 資料硬碟已經沒有足夠的空間來儲存資料。  
  
- 您想要變更預設的存放位置。 若想要更快速移動，請考慮在伺服器資料夾未包含任何資料時移動資料夾。  
  
- 您想要移除現有的硬碟，但不想要失去該硬碟上的伺服器資料夾。  
  
  在移動資料夾之前，請先確定下列情況：  
  
- 確定您已經備份您的伺服器。  
  
- 如果您打算移動 [用戶端電腦備份] 資料夾，請確定所有用戶端備份皆已停止而不在進行中。 移動 [用戶端電腦備份] 資料夾時，在完成資料夾移動之前，伺服器將無法備份任何用戶端電腦。  
  
- 確定伺服器沒有在執行任何重要的系統操作。 建議您先完成任何進行中的更新或備份，再開始移動資料夾，否則可能會花費較長的時間才能完成程序。  
  
- 要移動之資料夾中沒有任何檔案正在使用中。 當伺服器資料夾正在移動時，您將無法存取該伺服器資料夾。  
  
  當伺服器資料夾中的檔案實作下列技術時，不支援將資料夾從 NTFS 移到 ReFS：  
  
- 替代資料流  
  
- 物件識別碼  
  
- 簡短名稱 (8.3 名稱)  
  
- 壓縮  
  
- EFS 加密  
  
- 交易式 NTFS (TxF) (在 Windows Vista 中導入)  
  
- 疏鬆檔案  
  
- 永久連結  
  
- 擴充屬性  
  
- 配額  
  
###  <a name="where-to-add-or-move-a-server-folder"></a><a name="BKMK_6"></a>要在何處新增或移動伺服器資料夾  
 一般而言，您應該將伺服器資料夾新增或移到擁有最大可用空間的硬碟上。 儘可能避免將共用資料夾新增或移到系統磁碟機 (例如 C:) 上，因為這可能會佔用作業系統及其更新所需的必要磁碟機空間。 此外，也請避免將伺服器資料夾新增或移到外接式硬碟上，因為這些硬碟可被輕易地中斷連線，而導致您可能無法存取您的檔案。 取而代之的是，我們建議您在內部磁碟機上建立資料夾。  
  
 伺服器資料夾無法被新增或移到下列位置，如果選取任何這些位置來進行新增或移動，將會導致發生錯誤：  
  
-   不是以 NTFS 或 ReFS 檔案系統格式化的硬碟  
  
-   %windir% 資料夾  
  
-   連線的網路磁碟機  
  
-   包含共用資料夾的資料夾  
  
-   位於 [含有抽取式儲存媒體的裝置] 底下的硬碟  
  
-   硬碟的根目錄（例如 C：\\、D：\\、E：\\）  
  
-   現有共用資料夾的子資料夾  
  
-   執行 Windows Server Essentials 或 Windows Server 2012 R2 並已安裝 Windows Server Essentials 體驗角色的成員伺服器  
  
### <a name="steps-to-add-or-move-a-server-folder"></a>新增或移動伺服器資料夾的步驟  
  
> [!NOTE]
>  您必須是伺服器系統管理員，才能完成這些程序。  
  
##### <a name="to-add-a-server-folder"></a>新增伺服器資料夾  
  
1. 開啟 [儀表板]。  
  
2. 按一下 [存放]，然後按一下 [伺服器資料夾]。  
  
3. 在 [伺服器資料夾工作] 中，按一下 [新增資料夾]。 這會啟動 [新增資料夾精靈]。  
  
4. 遵循指示以完成精靈。  
  
   > [!NOTE]
   > - 如果您使用 [瀏覽] 按鈕來瀏覽特定的資料夾以指定伺服器資料夾位置，就會將您瀏覽的目的地資料夾新增為伺服器資料夾。  
   >   -   您可以定義可透過「遠端 Web 存取」存取的伺服器資料夾。 如需詳細資訊，請參閱[管理伺服器資料夾的存取權](Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_1)。  
  
##### <a name="to-move-a-server-folder"></a>移動伺服器資料夾  
  
1.  開啟 [儀表板]。  
  
2.  按一下 [存放]，然後按一下 [伺服器資料夾]。  
  
3.  從伺服器資料夾清單中，選取您想要移動的資料夾。  
  
4.  在 [工作] 窗格中，按一下 [移動資料夾]。  
  
5.  遵循指示以完成精靈。  
  
##  <a name="add-a-missing-server-folder"></a><a name="BKMK_9"></a>新增遺失的伺服器資料夾  
 當伺服器偵測到預先定義的伺服器資料夾時？公司、使用者、用戶端電腦備份、「檔案歷程記錄備份」或「資料夾重新導向」已不再共用（基於某些原因或另一個），會產生警示以引導使用者解決此問題。 建議您從伺服器備份嘗試還原資料夾。 不過，如果尚未備份伺服器，則請選取遺失的資料夾，然後按一下 [重建遺失的資料夾] 來重新設定伺服器資料夾的位置。  
  
> [!NOTE]
>  只有預先定義的資料夾？公司、使用者、用戶端電腦備份、檔案歷程記錄備份或資料夾重新導向等，都可以重新建立。 使用者建立的伺服器資料夾和媒體伺服器資料夾無法重建。  
  
 在您還原或重建遺失的資料夾之後，它應該就不會再被列為 [遺失]。  
  
 如需有關從伺服器備份還原檔案的詳細資訊，請參閱[管理備份和還原](Manage-Backup-and-Restore-in-Windows-Server-Essentials.md)主題中的深入瞭解還原檔案和資料夾一節。  
  
##  <a name="understand-shared-folders"></a><a name="BKMK_11"></a>瞭解共用資料夾  
 有數種不同的方式可供您從連線到伺服器的裝置存取您在 Windows Server Essentials 上的共用資料夾。 如需詳細資訊，請參閱[使用共用資料夾](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)主題。  
  
##  <a name="understand-shadow-copies"></a><a name="BKMK_Shadow"></a>瞭解陰影複製  
 使用伺服器「陰影複製」，使用者便可以檢視存在於過去時間點的共用檔案和資料夾。 存取先前版本的檔案或陰影複製是非常有用的，因為使用者可以：  
  
1. **復原意外刪除的檔案**。 如果您意外刪除了檔案，可以開啟先前版本，然後將它複製到安全的位置。  
  
2. **復原意外覆寫的檔案**。 如果您意外覆寫了檔案，可以復原檔案的先前版本。 (版本數目取決於您所建立的快照集數目。)  
  
3. **在工作時比較檔案的版本**。 當您想要檢查檔案的各個版本之間有什麼變更時，可以使用舊的版本。  
  
   若要使用「陰影複製」，請從用戶端電腦，在伺服器共用資料夾上按一下滑鼠右鍵，然後選取 [還原舊版]。  
  
## <a name="see-also"></a>另請參閱  
  
-   [管理伺服器儲存體](Manage-Server-Storage-in-Windows-Server-Essentials.md)  
  
-   [使用共用資料夾](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
