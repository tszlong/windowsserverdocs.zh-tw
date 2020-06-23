---
title: 使用 Windows Server Essentials 中的遠端 Web 存取
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 47ea21a0-5e05-4b4b-8fa4-338c82601276
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 17ed30b708ce76396d49b4dc1c3c4a4785b67fe8
ms.sourcegitcommit: 56ac7cf3f4bbcc5175f140d2df5f37cc42ba76d1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85217578"
---
# <a name="use-remote-web-access-in-windows-server-essentials"></a>使用 Windows Server Essentials 中的遠端 Web 存取

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials
  
  遠端 Web 存取是 Windows Server Essentials 的一項功能，可讓您透過網頁瀏覽器，透過網際網路連線能力存取網路上的檔案/資料夾和電腦。 
  
  「遠端 Web 存取」可協助您在置身他處時與 Windows Server Essentials 網路保持連線。 當您登入遠端 Web 存取時，您可以連線到 Windows Server Essentials 網路上的電腦、開啟 [儀表板] 來管理 Windows Server Essentials 網路，以及存取伺服器上的所有共用資料夾和媒體檔案。  
  
 這個主題包括下列各節：  
  

-   [連線到遠端 Web 存取](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [共用檔案與資料夾](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [從行動裝置連線](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="connect-to-remote-web-access"></a><a name="BKMK_Connect"></a>連接至遠端 Web 存取  
  
-   [登入遠端 Web 存取](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [從遠端存取您的電腦](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

-   [連線到遠端 Web 存取](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [共用檔案與資料夾](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [從行動裝置連線](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="connect-to-remote-web-access"></a><a name="BKMK_Connect"></a>連接至遠端 Web 存取  
  
-   [登入遠端 Web 存取](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [從遠端存取您的電腦](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

  
###  <a name="log-on-to-remote-web-access"></a><a name="BKMK_1"></a>登入遠端 Web 存取  
 當您從本機或遠端電腦登入遠端 Web 存取時，您可以存取執行 Windows Server Essentials 之伺服器上的資源，以及網路上的電腦。  
  
##### <a name="to-log-on-to-remote-web-access-from-a-network-computer"></a>從網路電腦登入遠端 Web 存取  
  
1.  開啟網頁瀏覽器，在網址列中輸入**HTTPs://** _<您伺服器名稱 \> _ **/remote** ，然後按 enter。  
  
    > [!NOTE]
    >  請確定您在 HTTPs 中包含 s。  
  
2.  在 [遠端 Web 存取登入] 頁面的文字方塊中輸入您的使用者名稱和密碼，然後按一下箭號。  
  
##### <a name="to-log-on-to-remote-web-access-from-a-remote-computer"></a>從遠端電腦登入遠端 Web 存取  
  
1.  開啟網頁瀏覽器，在網址列中輸入**HTTPs://** _<您功能變數名稱 \> _ **/remote** ，然後按 enter。  
  
    > [!NOTE]
    >  您可以向您的網路系統管理員取得您的網域名稱資訊。 請確定您在 HTTPs 中包含 s。  
  
2.  在 [遠端 Web 存取登入] 頁面的文字方塊中輸入您的使用者名稱和密碼，然後按一下箭號。  
  
###  <a name="remotely-access-your-computer"></a><a name="BKMK_1.5"></a>從遠端存取您的電腦  
 當您不在辦公室時，您可以使用網頁瀏覽器登入遠端 Web 存取網站，從遠端存取您的 Windows Server Essentials 儀表板、共用資料夾和網路上的電腦。  
  
 連線到 [儀表板] 之後，您就可以像在辦公室裡一樣管理 Windows Server Essentials。 您可以執行所有一般的系統管理工作，例如新增使用者帳戶、新增共用資料夾、設定共用資料夾存取等等。 當您連線到網路上的電腦時，您可以存取它們的桌面，就好像您置身於辦公室中坐在那些電腦前一樣。  
  
 [狀態]**** 欄會顯示您是否可以連線到網路上的某部電腦，並且可包含下列值：  
  
-   **可用**  
  
     電腦已開啟並且可供遠端連線使用。 當有協力廠商防火牆封鎖連線時，即使您看見這個狀態，仍然有可能無法連線到這部電腦。  
  
-   **離線或睡眠**  
  
     電腦已關閉，或是處於 [睡眠] 或 [休眠] 模式。 如果電腦已離線或正在睡眠，系統會即時更新狀態，讓您知道電腦何時可供使用。  
  
-   **不支援的作業系統**  
  
     電腦上的作業系統不支援「遠端桌面」。 如果這個狀態有變更，可能需要多達 6 小時的時間才會在伺服器上更新。  
  
-   **連線已停用**  
  
     電腦連線被防火牆封鎖，或遠端桌面在電腦上被停用或被「群組原則」停用。 如果這個狀態有變更，可能需要多達 6 小時的時間才會在伺服器上更新。  
  
#### <a name="to-connect-to-a-computer-on-your-network"></a>連線到您網路上的電腦  
 在 [裝置]**** 索引標籤上，按一下電腦的名稱。 您只能選取狀態為 [可用]**** 的電腦。  
  
#### <a name="to-connect-to-the-server-dashboard"></a>連線到伺服器儀表板  
 在 [裝置]**** 索引標籤上，按一下您伺服器的名稱。 您只能選取狀態為 [可用]**** 的電腦。 您必須要能夠提供您伺服器上系統管理員使用者帳號和密碼，才能使用 [儀表板]。  
  
##  <a name="share-files-and-folders"></a><a name="BKMK_SharedFolders"></a>共用檔案和資料夾  
  

-   [在遠端 Web 存取中上傳和下載檔案](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [在遠端 Web 存取中建立、重新命名、移動、刪除或複製檔案和資料夾](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

  
###  <a name="upload-and-download-files-in-remote-web-access"></a><a name="BKMK_UploadRWA"></a>在遠端 Web 存取中上傳和下載檔案  
 在 [遠端 Web 存取] 的 [共用資料夾]**** 索引標籤上，您可以執行下列動作：  
  
-   將檔案從您的電腦上傳 (傳送) 到 Windows Server Essentials。  
  
    > [!NOTE]
    >  您只能將檔案而不能將資料夾上傳到 [遠端 Web 存取]。 如果您想要讓伺服器上的 [共用資料夾]**** 擁有與您電腦上相同的檔案和資料夾階層，您必須在 [遠端 Web 存取] 中建立伺服器上的資料夾，然後將檔案上傳到您所建立的資料夾。 如須有關建立伺服器資料夾的資訊，請參閱[新增或移動伺服器資料夾](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)。  
  
-   將檔案和資料夾從 Windows Server Essentials 下載 (接收) 到您的電腦。  
  
-   在 Windows Server Essentials 上的共用資料夾內建立資料夾。  
  

-   移動、刪除及重新命名 Windows Server Essentials 上的檔案和資料夾。 如需詳細資訊，請參閱[建立、重新命名、移動、刪除或複製遠端 Web 存取中的檔案和資料夾](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)。  

-   移動、刪除及重新命名 Windows Server Essentials 上的檔案和資料夾。 如需詳細資訊，請參閱[建立、重新命名、移動、刪除或複製遠端 Web 存取中的檔案和資料夾](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)。  

  
#### <a name="upload-files"></a>上傳檔案  
  
###### <a name="to-upload-files"></a>上傳檔案  
  
1. 在 [遠端 Web 存取] 中，按一下 [共用資料夾]**** 索引標籤，然後按一下某個共用資料夾連結。 這會顯示該共用資料夾中檔案和資料夾的清單。  
  
2. 從共用資料夾的檔案和資料夾清單中，按一下您想要上傳檔案的目的地資料夾，然後按一下 [上傳]****。  
  
3. 如果尚未載入標準上傳工具，請按一下 [使用標準上傳方法]****。  
  
4. 按一下 [瀏覽] ****  來尋找您電腦上的檔案。  
  
5. 瀏覽您電腦上的資料夾來尋找您想要上傳的檔案，然後按一下 [開啟]****。  
  
6. 針對您想要上傳的每個檔案，重複步驟 2 和 3。  
  
7. 在您已新增所有想要上傳的檔案之後，按一下 [上傳]****。  
  
   「簡易檔案上傳」工具可讓執行 Windows Server Essentials 之伺服器上的上傳檔案程序更有效率。 您可以使用拖放功能將所需數量的檔案新增到「簡易檔案上傳」工具，然後將它們上傳到伺服器上的共用資料夾。  
  
> [!NOTE]
>  與 HTML5 相容的網頁瀏覽器中原本就支援上傳多個檔案。 只有當網頁瀏覽器不支援 HTML5 時，才需要這個工具。  
  
###### <a name="to-upload-files-using-the-easy-file-upload-tool"></a>使用簡易檔案上傳工具來上傳檔案  
  
1.  在 [遠端 Web 存取] 中，按一下 [共用資料夾]**** 索引標籤，然後按一下某個共用資料夾連結。 這會顯示該共用資料夾中檔案和資料夾的清單。  
  
2.  從共用資料夾的檔案和資料夾清單中，按一下您想要上傳檔案的目的地資料夾，然後按一下 [上傳]****。 如果要做為上傳目的地的資料夾不存在，請按一下 [新增資料夾]****，在對話方塊中輸入新資料夾的名稱，然後按一下 [確定]****。  
  
3.  您可能需要執行「Windows Server 解決方案」附加元件。 如果是的話，請按一下畫面頂端的黃色長條，按一下 [執行附加元件]****，然後按一下對話方塊中的 [執行]****。  
  
4.  如果尚未載入「簡易檔案上傳」工具，請按一下 [使用簡易檔案上傳工具]****。  
  
5.  您可以將檔案從 [Windows 檔案總管] 拖放到「簡易檔案上傳」工具中，或按一下 [瀏覽以選取檔案]****。  
  
6.  當您新增完您想要上傳到所選資料夾的檔案時，按一下 [上傳]****。  
  
7.  當檔案上傳成功時，按一下 [關閉]****。  
  
#### <a name="download-files-or-folders"></a>下載檔案或資料夾  
  
###### <a name="to-download-a-single-file"></a>下載單一檔案  
  
1. 在 [遠端 Web 存取] 中，按一下 [共用資料夾]**** 索引標籤，然後按一下某個共用資料夾連結。 這會顯示該共用資料夾中檔案和資料夾的清單。  
  
2. 從共用資料夾檔案清單中，按一下您想要下載到您家用電腦之檔案旁邊的核取方塊。  
  
3. 按一下 [下載]**** 來開始下載。  
  
4. 在 [檔案下載]**** 對話方塊中，按一下 [儲存]**** 以將檔案儲存到您的電腦。  
  
5. 在 [另存新檔]**** 對話方塊中，選取要儲存檔案的位置，然後按一下 [儲存]****。 單一檔案在下載前不會先經過壓縮。  
  
   下載多個檔案或資料夾的選項有兩個。 請選擇符合您需求的選項：  
  
> [!NOTE]
>  只有當您要將多個檔案或資料夾下載到您的電腦時，才會有這些選項。  
  
- **自我解壓縮的執行檔 (.exe)**  
  
  > [!NOTE]
  >   本節適用于執行 Windows Server Essentials 的伺服器。  
  
   自我解壓縮的執行檔是一種您可下載且結合了壓縮檔之解壓縮 (可執行) 程式的檔案。 當您執行可執行程式時，它會自動將壓縮檔解壓縮 (自我解壓縮)。 這是一種散佈壓縮資料的常見方式，可不需擔心接收者是否具有正確的解壓縮公用程式。  
  
  > [!NOTE]
  >  這個選項支援 Unicode 字元。  
  
- **Windows 壓縮的資料夾 (.zip)**  
  
   壓縮檔案會建立比原始檔案小的檔案壓縮版本。 檔案的壓縮版本具有.zip 副檔名。 壓縮後縮減最多的檔案類型是文字導向的檔案類型 (例如 .txt、.doc、.xls) 和使用非壓縮檔案類型的圖形檔案 (例如 .bmp)。 有些圖形檔案 (例如 .jpg 和 .gif 檔案) 已經使用壓縮，因此壓縮後檔案大小縮減很少。 此外，包含大量圖形的 Word 文件縮減程度也不如大部份是文字的文件。  
  
  > [!NOTE]
  >  此選項針對 Windows Server Essentials 中的國際檔案名提供有限的支援。  
  
  在實際的下載開始之前，會先建立 exe 或 zip 檔。 視要下載的檔案數目和檔案總大小而定，這可能會花費數分鐘的時間。 建立下載檔案之後，就會在背景進行下載檔案的程序。 這可讓您在下載程序完成的過程中繼續工作。  
  
###### <a name="to-download-multiple-files-or-folders"></a>下載多個檔案或資料夾  
  
1.  在 [遠端 Web 存取] 中，按一下 [共用資料夾]**** 索引標籤，然後按一下某個共用資料夾連結。 這會顯示該共用資料夾中檔案和資料夾的清單。  
  
2.  從共用資料夾檔案清單中，按一下您想要下載到您家用電腦之檔案或資料夾旁邊的核取方塊。  
  
3.  按一下 [下載]**** 來開始下載。  
  
4.  在 [選擇下載格式]**** 對話方塊中，按一下以選取您慣用的下載格式選項，然後按一下 [確定]****。 這樣就會以您選取的格式選項準備壓縮檔。  
  
5.  在 [檔案下載]**** 對話方塊中，按一下 [儲存]**** 以將檔案儲存到您的電腦。  
  
6.  在 [另存新檔]**** 對話方塊中，選取要儲存檔案的位置，然後按一下 [儲存]****。  
  
#### <a name="retrieve-compressed-files-downloaded-to-your-computer"></a>擷取下載到您電腦的壓縮檔  
  
> [!NOTE]
>   本節適用于執行 Windows Server Essentials 的伺服器。  
  
 如果您選取多個檔案或資料夾來下載，您可能會收到可自我解壓縮的壓縮執行檔 (.exe) 或壓縮檔 (.zip)。  
  
###### <a name="to-retrieve-a-file-from-the-compressed-exe-file"></a>從壓縮檔 (.exe) 中擷取檔案  
  
1.  在您的電腦上，按兩下壓縮檔來開啟它。  
  
2.  依照指示將檔案解壓縮到您電腦上的資料夾。  
  
###### <a name="to-retrieve-a-file-from-the-compressed-zip-file"></a>從壓縮檔 (.zip) 中擷取檔案  
  
1.  在您的電腦上，按兩下壓縮檔來開啟它。  
  
2.  選取您想要擷取的檔案，然後將檔案拖曳到電腦上您想要用來儲存它們的資料夾。  
  
    > [!NOTE]
    >  如果您使用協力廠商檔案壓縮程式，請依照該程式的程序，將您的檔案從壓縮檔中解壓縮。  
  
###  <a name="create-rename-move-delete-or-copy-files-and-folders-in-remote-web-access"></a><a name="BKMK_2"></a>建立、重新命名、移動、刪除或複製遠端 Web 存取中的檔案和資料夾  
 您可以使用「遠端 Web 存取」在您的伺服器上於現有的共用資料夾中建立新資料夾、重新命名檔案和資料夾、移動及複製檔案和資料夾，以及刪除檔案和資料夾。  
  
> [!NOTE]
>  若要在執行 Windows Server Essentials 的伺服器上新增共用資料夾，您必須使用 [儀表板]。 若要從 [遠端的 Web 存取] 連線到伺服器主控台，請在 [電腦]**** 索引標籤上，按一下伺服器名稱，按一下 [連線]****，然後依照指示來登入伺服器。 如須有關如何建立共用資料夾的資訊，請參閱[新增或移動伺服器資料夾](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)。  
  
##### <a name="to-create-a-new-folder"></a>建立新資料夾  
  
1.  在 [遠端 Web 存取] 中，按一下 [共用資料夾]**** 索引標籤，然後按一下某個共用資料夾連結。 這會顯示該共用資料夾中檔案和資料夾的清單。  
  
2.  在工作列中，按一下 [新增資料夾]****。  
  
3.  輸入資料夾的名稱，然後按一下 [確定]****。  
  
##### <a name="to-rename-a-file-or-folder"></a>重新命名檔案或資料夾  
  
1.  在 [遠端 Web 存取] 中，按一下 [共用資料夾]**** 索引標籤，然後按一下某個共用資料夾連結。 這會顯示該共用資料夾中檔案和資料夾的清單。  
  
2.  在您想要重新命名的檔案或資料夾上按一下滑鼠右鍵，然後按一下 [重新命名]****。  
  
3.  在文字方塊中輸入新名稱，然後按一下 [確定]****。  
  
##### <a name="to-move-files-or-folders"></a>移動檔案或資料夾  
  
1.  在 [遠端 Web 存取] 中，按一下 [共用資料夾]**** 索引標籤，然後按一下某個共用資料夾連結。 這會顯示該共用資料夾中檔案和資料夾的清單。  
  
2.  選取您想要移動之檔案或資料夾旁邊的核取方塊，在其中一個選取的檔案或資料夾上按一下滑鼠右鍵，然後按一下 [剪下]****。  
  
3.  在您想要將檔案或資料夾移到該處的資料夾上按一下滑鼠右鍵，然後按一下 [貼上]****。  
  
##### <a name="to-delete-a-file-or-folder"></a>刪除檔案或資料夾  
  
1.  在 [遠端 Web 存取] 中，按一下 [共用資料夾]**** 索引標籤，然後按一下某個共用資料夾連結。 這會顯示該共用資料夾中檔案和資料夾的清單。  
  
2.  選取您想要刪除之檔案或資料夾旁邊的核取方塊，在其中一個選取的檔案或資料夾上按一下滑鼠右鍵，然後按一下 [刪除]****。  
  
3.  若要確認您想要刪除選取的檔案和資料夾，請按一下 [是]****。  
  
##### <a name="to-copy-files-or-folders"></a>複製檔案或資料夾  
  
1.  在 [遠端 Web 存取] 中，按一下 [共用資料夾]**** 索引標籤，然後按一下某個共用資料夾連結。 這會顯示該共用資料夾中檔案和資料夾的清單。  
  
2.  選取您想要複製之檔案或資料夾旁邊的核取方塊，在其中一個選取的檔案或資料夾上按一下滑鼠右鍵，然後按一下 [複製]****。  
  
3.  在您想要將檔案或資料夾複製到該處的資料夾上按一下滑鼠右鍵，然後按一下 [貼上]****。  
  
##  <a name="connect-from-a-mobile-device"></a><a name="BKMK_ConnectMobile"></a>從行動裝置連接  
  

-   [從行動裝置使用遠端 Web 存取](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [支援的行動裝置網頁瀏覽器](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)   

  
###  <a name="use-remote-web-access-from-a-mobile-device"></a><a name="BKMK_8"></a>從行動裝置使用遠端 Web 存取  
 您可以從智慧型手機登入 [遠端 Web 存取] 來檢視伺服器上共用資料夾中的檔案和資料夾。  
  
> [!NOTE]
>  您也可以從 [Windows Phone Marketplace](http://www.windowsphone.com/apps/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a) 下載並使用適用於 Windows Server Essentials 的 My Server 應用程式，來存取儲存在伺服器上的共用資料夾和媒體檔案。  
  
##### <a name="to-log-on-to-remote-web-access-from-a-mobile-device"></a>從行動裝置登入遠端 Web 存取  
  
1.  開啟網頁瀏覽器，然後在網址列中輸入**HTTPs://** _<您功能變數名稱 \> _ **/remote** 。  請確定您在 HTTPs 中包含 s。  
  
2.  在 [遠端 Web 存取登入] 頁面的文字方塊中輸入您的使用者名稱和密碼，然後按一下箭號。 這會將您登入行動裝置版本的「遠端 Web 存取」。  
  
##### <a name="to-switch-to-the-desktop-version-of-remote-web-access"></a>切換到桌面版本的遠端 Web 存取  
  
1.  開啟網頁瀏覽器，然後在網址列中輸入**HTTPs://** _<您功能變數名稱 \> _ **/remote** 。  請確定您在 HTTPs 中包含 s。  
  
2.  在 [遠端 Web 存取登入] 頁面上，于文字方塊中輸入您的使用者名稱和密碼，按一下 [**觀看桌上出版本**]，然後按一下箭號。 這會將您登入桌面版本的「遠端 Web 存取」。  
  
##### <a name="to-return-to-the-mobile-version-of-remote-web-access"></a>回到行動裝置版本的遠端 Web 存取  
  
1. 登出。  
  
2. 開啟網頁瀏覽器，然後在網址列中輸入**HTTPs://** _<您功能變數名稱 \> _**網址列/remote/m** 。 請確定您在 HTTPs 中包含 s。  
  
3. 隨即顯示 [遠端 Web 存取的行動版。 在 [遠端 Web 存取登入] 頁面的文字方塊中輸入您的使用者名稱和密碼，然後按一下箭號。 您登入的是行動版的遠端 Web 存取。  
  
   您可以搜尋伺服器上共用資料夾中的檔案和資料夾。  
  
###  <a name="supported-web-browsers-for-mobile-devices"></a><a name="BKMK_9"></a>行動裝置支援的網頁瀏覽器  
 支援的行動裝置網頁瀏覽器包括：  
  
-   Internet Explorer Mobile 6.0 或更新版本  
  
-   Safari  
  
-   Blackberry  
  
-   Symbian 6.0 或更新版本  
  
-   Android  
  
-   Google Chrome  
  
-   Firefox  
  
## <a name="see-also"></a>另請參閱  
  
-   [管理遠端 Web 存取](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)

-   [遠端工作](Work-Remotely-in-Windows-Server-Essentials.md)
  
-   [使用 Windows Server Essentials](Use-Windows-Server-Essentials.md)

