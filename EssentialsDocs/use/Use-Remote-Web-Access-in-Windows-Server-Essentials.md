---
title: "Windows Server Essentials 中使用 Web 遠端存取"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47ea21a0-5e05-4b4b-8fa4-338c82601276
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ac384b545fe61b4a832debdc8aedcc81a75c669a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="use-remote-web-access-in-windows-server-essentials"></a>Windows Server Essentials 中使用 Web 遠端存取

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集
  
  遠端網路存取可協助您保持連接到您的 Windows Server Essentials 網路，當您離開。 當您登入遠端網路存取權時，您可以連接到電腦，Windows Server Essentials 網路上、 開放儀表板以管理您的 Windows Server Essentials 網路，並存取所有的伺服器上的共用資料夾和媒體檔案。  
  
 本主題包含下列的區段：  
  

-   [連接到遠端的網路存取權](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [分享檔案和資料夾](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [連接行動裝置版裝置](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a>連接到遠端的網路存取權  
  
-   [登入遠端存取](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [遠端存取您的電腦](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

-   [連接到遠端的網路存取權](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [分享檔案和資料夾](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [連接行動裝置版裝置](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ConnectMobile)  
  
##  <a name="BKMK_Connect"></a>連接到遠端的網路存取權  
  
-   [登入遠端存取](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [遠端存取您的電腦](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1.5)  

  
###  <a name="BKMK_1"></a>登入遠端存取  
 當您從登入遠端 Web 存取本機或遠端電腦時，您可以存取伺服器您網路上執行 Windows Server Essentials 和電腦上的資源。  
  
##### <a name="to-log-on-to-remote-web-access-from-a-network-computer"></a>若要登入遠端網路存取權的網路的電腦  
  
1.  開放網頁瀏覽器中，輸入**https://***< YourServerName\ >***/遠端**中網址列，然後按下 Enter。  
  
    > [!NOTE]
    >  請確定您已 https 中包含 s。  
  
2.  在遠端 Web 存取登入頁面上，您的使用者名稱和密碼方塊中輸入文字，然後箭號。  
  
##### <a name="to-log-on-to-remote-web-access-from-a-remote-computer"></a>登入遠端 Web 存取從遠端電腦  
  
1.  開放網頁瀏覽器中，輸入**https://***< YourDomainName\ >***/遠端**中網址列，然後按下 Enter。  
  
    > [!NOTE]
    >  您可以從您的網路系統管理員取得您的網域名稱資訊。 請確定您已 https 中包含 s。  
  
2.  在遠端 Web 存取登入頁面上，您的使用者名稱和密碼方塊中輸入文字，然後箭號。  
  
###  <a name="BKMK_1.5"></a>遠端存取您的電腦  
 當您離開您的 office 時，您可以使用您的網頁瀏覽器來登入遠端 Web 存取網站遠端存取您的 Windows Server Essentials 儀表板，共用的資料夾和網路上的電腦。  
  
 當您連接至儀表板時，您可以管理 Windows Server Essentials 就如同您是否在辦公室。 您可以執行一般管理工作，例如新增帳號，所有加入共用的資料夾，設定的共用的資料夾的存取，等等。 當您在您的網路連接到電腦時，您可以存取自己的桌面，如同您坐在前面他們在辦公室。  
  
 **狀態**欄顯示您，您可以連接到電腦上您的網路，並可以包含下列值：  
  
-   **可使用**  
  
     電腦已，並可連接遠端。 即使您會看到這個狀態，您仍然無法如果步驟 8 封鎖連接到這部電腦連接。  
  
-   **離線或休眠**  
  
     電腦已關閉或處於睡眠或休眠模式。 如果電腦是離線或休眠狀態即時更新，讓您知道電腦時使用。  
  
-   **不支援的作業系統**  
  
     遠端桌面不支援的作業系統，在電腦上。 可能需要 6 小時的時間來更新伺服器上，如果有一項變更此狀態。  
  
-   **連接已停用**  
  
     電腦連接可能被阻擋防火牆，或在電腦上或群組原則來停用遠端桌面。 可能需要 6 小時的時間來更新伺服器上，如果有一項變更此狀態。  
  
#### <a name="to-connect-to-a-computer-on-your-network"></a>連接您網路上的電腦  
 在**裝置**索引標籤上，按一下 [電腦名稱。 您可以選擇僅電腦與**可用**狀態。  
  
#### <a name="to-connect-to-the-server-dashboard"></a>連接至伺服器儀表板  
 在**裝置**索引標籤上，按一下您的伺服器的名稱。 您可以選擇僅電腦與**可用**狀態。 您必須使用儀表板伺服器上提供系統管理員的使用者及密碼。  
  
##  <a name="BKMK_SharedFolders"></a>分享檔案和資料夾  
  

-   [上傳與下載遠端 Web 存取檔案](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [建立、 重新命名移動、 delete，或在遠端 Web 存取複製的檔案和資料夾](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

-   [上傳與下載遠端 Web 存取檔案](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UploadRWA)  
  
-   [建立、 重新命名移動、 delete，或在遠端 Web 存取複製的檔案和資料夾](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  

  
###  <a name="BKMK_UploadRWA"></a>上傳與下載遠端 Web 存取檔案  
 遠端存取網路上**共用資料夾**索引標籤上，您可以執行下列動作：  
  
-   （傳送） 將檔案上傳您的電腦從 Windows Server essentials。  
  
    > [!NOTE]
    >  您可以到遠端的網路存取上載只檔案並不是資料夾。 如果您想要中有相同的檔案和資料夾階層**共用資料夾**在伺服器上為您在電腦上，您必須遠端網路存取，在伺服器上建立資料夾，然後將檔案上傳到您所建立的資料夾。 建立資料夾伺服器的相關資訊，請查看[新增或移動伺服器資料夾](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)。  
  
-   下載 （收到） 的檔案和資料夾從 Windows Server Essentials 到您的電腦。  
  
-   在 Windows Server Essentials 的共用資料夾中的資料夾。  
  

-   移動、 delete，並重新命名檔案和資料夾的 Windows Server Essentials。 如需詳細資訊，請查看[建立、 重新命名，移動、 delete 或複製檔案和資料夾遠端 Web 存取](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)。  

-   移動、 delete，並重新命名檔案和資料夾的 Windows Server Essentials。 如需詳細資訊，請查看[建立、 重新命名，移動、 delete 或複製檔案和資料夾遠端 Web 存取](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)。  

  
#### <a name="upload-files"></a>將檔案上傳  
  
###### <a name="to-upload-files"></a>若要將檔案上傳  
  
1.  在 [遠端網路存取，按一下 [**的共用資料夾**索引標籤，然後按一下 [共用資料夾的連結。 會顯示的檔案和資料夾的共用資料夾中的清單。  
  
2.  從檔案和資料夾的共用資料夾清單中，按一下您要上傳檔案，然後按一下的資料夾**上傳**。  
  
3.  如果已經無法載入標準上傳工具，按一下 [**使用標準上傳方法**。  
  
4.  按一下**瀏覽]**來尋找您的電腦上的檔案。  
  
5.  瀏覽以尋找您想要上傳，檔案您電腦上的資料夾，然後按一下**開放**。  
  
6.  重複步驟 2 和 3 針對每個您想要上傳檔案。  
  
7.  當您新增了將檔案上傳，請按一下您想要的所有**上傳**。  
  
 簡單的檔案上傳工具來簡化您執行的 Windows Server Essentials 的伺服器上的檔案上傳程的序。 您可以新增多個您要使用的拖放功能，輕鬆檔案上傳工具的檔案，然後將檔案上傳到伺服器上的共用資料夾。  
  
> [!NOTE]
>  在網頁瀏覽器與 HTML5 相容的原生支援上傳多個檔案。 網頁瀏覽器不支援 HTML5 時，只需要這項工具。  
  
###### <a name="to-upload-files-using-the-easy-file-upload-tool"></a>上傳檔案使用簡單的檔案上傳工具  
  
1.  在 [遠端網路存取，按一下 [**的共用資料夾**索引標籤，然後按一下 [共用資料夾的連結。 會顯示的檔案和資料夾的共用資料夾中的清單。  
  
2.  從檔案和資料夾的共用資料夾清單中，按一下您要上傳，檔案，然後按一下的資料夾**上傳**。 如果您想要上傳到不存在，資料夾按一下**新資料夾**，在對話方塊中，輸入新資料夾的名稱，然後按一下 [ **[確定]**。  
  
3.  您可能需要執行 Windows Server 方案附加元件。 若是如此，請按一下螢幕頂端的黃色區域中，按一下 [執行]**附加元件**，然後按**執行**對話方塊中。  
  
4.  如果已經無法載入輕鬆檔案上傳工具，按一下 [**使用簡單的檔案上傳工具**。  
  
5.  您可以將檔案從 [Windows 檔案總管拖放輕鬆檔案上傳工具，或按一下 [**瀏覽]，選取 [檔案]**。  
  
6.  當您完成新增的檔案上傳到選取的資料夾，按一下您想要**上傳**。  
  
7.  時的成功上傳檔案，請按一下**關閉**。  
  
#### <a name="download-files-or-folders"></a>下載檔案或資料夾  
  
###### <a name="to-download-a-single-file"></a>若要下載單一檔案  
  
1.  在 [遠端網路存取，按一下 [**的共用資料夾**索引標籤，然後按一下 [共用資料夾的連結。 會顯示的檔案和資料夾的共用資料夾中的清單。  
  
2.  從共用資料夾的檔案清單中，按一下您想要的家用電腦下載檔案旁的核取方塊。  
  
3.  按一下**下載**以開始下載。  
  
4.  在**下載檔案**對話方塊中，按**儲存**來將檔案儲存到您的電腦。  
  
5.  在**另存新檔**對話方塊中，選取 [儲存的檔案，然後按一下 [位置**儲存**。 下載它之前尚未壓縮單一檔案。  
  
 有兩個選項來下載多個檔案或資料夾。 選擇符合您需求的選項：  
  
> [!NOTE]
>  只有當您正在下載多個檔案或資料夾到您的電腦時，可以使用這些選項。  
  
-   **自動解壓縮可執行檔 (.exe)**  
  
    > [!NOTE]
    >   本節適用於執行 Windows Server Essentials 的伺服器。  
  
     自動解壓縮的可執行檔是結合了特定地區的壓縮檔案的解壓縮 （可執行檔） 的程式則可以下載的檔案。 當您執行的可執行的程式時，它會自動解壓縮 （自動解壓縮） 的壓縮的檔案。 這是常見的方式，而不用擔心收件者是否有正確解壓縮公用程式散發壓縮的資料。  
  
    > [!NOTE]
    >  這個選項支援 Unicode 字元。  
  
-   **Windows 壓縮資料夾 (.zip)**  
  
     壓縮檔案會建立的壓縮的檔案小於原始檔案版本。 壓縮的檔案的版本已經.zip 檔案名稱擴充功能。 減少最來壓縮的檔案類型的文字導向檔案類型，例如.txt，.doc 意見和圖形檔案.bmp 該使用未壓縮的檔案類型。 某些圖形檔案.jpg 與.gif 檔案，例如已使用壓縮，並檔案縮小幾乎來壓縮。 此外，包含許多圖形 Word 文件不一樣，大部分文字文件減少。  
  
    > [!NOTE]
    >  此選項適用於在 Windows Server Essentials 的國際檔名提供有限的支援。  
  
 實際下載在開始之前，建立 exe 或 zip 檔案。 根據您的檔案和下載檔案的大小，這可能需要花費幾分鐘的時間。 建立下載檔案之後，會在背景下載檔案發生。 這可讓您下載的程序完成時繼續工作。  
  
###### <a name="to-download-multiple-files-or-folders"></a>若要下載多個檔案或資料夾  
  
1.  在 [遠端網路存取，按一下 [**的共用資料夾**索引標籤，然後按一下 [共用資料夾的連結。 會顯示的檔案和資料夾的共用資料夾中的清單。  
  
2.  從共用資料夾的檔案清單中，按一下 [的檔案或資料夾您想要下載到您的家用電腦旁邊的核取方塊。  
  
3.  按一下**下載**以開始下載。  
  
4.  在**選擇下載格式**對話方塊中，按一下以選取的下載格式化選項，您想要的話，然後按**[確定]**。 壓縮的檔案已準備好您選取的格式化選項中。  
  
5.  在**下載檔案**對話方塊中，按**儲存**來將檔案儲存到您的電腦。  
  
6.  在**另存新檔**對話方塊中，選取 [儲存的檔案，然後按一下 [位置**儲存**。  
  
#### <a name="retrieve-compressed-files-downloaded-to-your-computer"></a>擷取壓縮的檔案下載到您的電腦  
  
> [!NOTE]
>   本節適用於執行 Windows Server Essentials 的伺服器。  
  
 如果您選取多個檔案或資料夾，下載，您會收到可自動解壓縮壓縮可執行檔 (.exe) 或壓縮的 (.zip) 檔案。  
  
###### <a name="to-retrieve-a-file-from-the-compressed-exe-file"></a>抓取檔案從壓縮的 (.exe) 檔案  
  
1.  在您的電腦上按兩下打開它壓縮的檔案。  
  
2.  依照指示資料夾到您的電腦上將檔案解壓縮。  
  
###### <a name="to-retrieve-a-file-from-the-compressed-zip-file"></a>抓取檔案從壓縮的 (.zip) 檔案  
  
1.  在您的電腦上按兩下打開它壓縮的檔案。  
  
2.  選取您想要擷取的檔案，然後將檔案拖曳到某個資料夾您的電腦上您想要用來儲存它們。  
  
    > [!NOTE]
    >  如果您使用協力廠商檔案壓縮程式，請依照下列從壓縮的檔案解壓縮檔案該程式的程序。  
  
###  <a name="BKMK_2"></a>建立、 重新命名移動、 delete，或在遠端 Web 存取複製的檔案和資料夾  
 您可以使用遠端 Web 存取在現有的共用資料夾，若要重新命名檔案和資料夾，以移動及複製的檔案和資料夾，並 delete 檔案伺服器上的資料夾中建立新資料夾。  
  
> [!NOTE]
>  若要新增執行的 Windows Server Essentials 的伺服器上的共用的資料夾，您必須使用儀表板。 連接到伺服器主機的遠端網路存取權，以**電腦**索引標籤上，按一下 [伺服器名稱，按一下 [**連接**，然後依照指示登入伺服器。 如需如何建立共用的資料夾有關的資訊，請查看[新增或移動伺服器資料夾](../manage/Manage-Server-Folders-in-Windows-Server-Essentials.md#BKMK_5)。  
  
##### <a name="to-create-a-new-folder"></a>若要建立新的資料夾  
  
1.  在 [遠端網路存取，按一下 [**的共用資料夾**索引標籤，然後按一下 [共用] 資料夾的連結。 會顯示的檔案和資料夾的共用資料夾中的清單。  
  
2.  在工作列中，按一下**資料夾新**。  
  
3.  輸入名稱的資料夾，然後按一下**[確定]**。  
  
##### <a name="to-rename-a-file-or-folder"></a>若要重新命名檔案或資料夾  
  
1.  在 [遠端網路存取，按一下 [**的共用資料夾**索引標籤，然後按一下 [共用] 資料夾的連結。 會顯示的檔案和資料夾的共用資料夾中的清單。  
  
2.  以滑鼠右鍵按一下檔案或資料夾，您想要重新命名，然後按一下**重新命名**。  
  
3.  在文字方塊中輸入新名稱，然後按一下**[確定]**。  
  
##### <a name="to-move-files-or-folders"></a>若要移動的檔案或資料夾  
  
1.  在 [遠端網路存取，按一下 [**的共用資料夾**索引標籤，然後按一下 [共用] 資料夾的連結。 會顯示的檔案和資料夾的共用資料夾中的清單。  
  
2.  選取核取方塊旁的檔案或資料夾您想要移動，其中選取的檔案或資料夾上按一下滑鼠右鍵，然後按一下**剪下**。  
  
3.  以滑鼠右鍵按一下您想要移動的檔案或資料夾，然後按一下 [資料夾**貼上**。  
  
##### <a name="to-delete-a-file-or-folder"></a>若要 delete 檔案或資料夾  
  
1.  在 [遠端網路存取，按一下 [**的共用資料夾**索引標籤，然後按一下 [共用] 資料夾的連結。 會顯示的檔案和資料夾的共用資料夾中的清單。  
  
2.  選取核取方塊旁的檔案或資料夾您想要 delete，其中選取的檔案或資料夾上按一下滑鼠右鍵，然後按一下**Delete**。  
  
3.  若要確認您想要 delete 選取的檔案和資料夾，按一下 [ **[是]**。  
  
##### <a name="to-copy-files-or-folders"></a>若要複製的檔案或資料夾  
  
1.  在 [遠端網路存取，按一下 [**的共用資料夾**索引標籤，然後按一下 [共用] 資料夾的連結。 會顯示的檔案和資料夾的共用資料夾中的清單。  
  
2.  選取核取方塊旁的檔案或資料夾，您要複製，其中選取的檔案或資料夾上按一下滑鼠右鍵，然後按一下**複製**。  
  
3.  以滑鼠右鍵按一下您要複製的檔案或資料夾，然後按一下 [資料夾**貼上**。  
  
##  <a name="BKMK_ConnectMobile"></a>連接行動裝置版裝置  
  

-   [使用遠端 Web 存取行動裝置版裝置](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [適用於行動裝置版裝置支援網頁瀏覽器](Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

-   [使用遠端 Web 存取行動裝置版裝置](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [適用於行動裝置版裝置支援網頁瀏覽器](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_9)  

  
###  <a name="BKMK_8"></a>使用遠端 Web 存取行動裝置版裝置  
 您可以從登入遠端 Web 存取伺服器上的共用資料夾的檔案和資料夾檢視智慧型手機。  
  
> [!NOTE]
>  您也可以下載並我伺服器 app 用於 Windows Server Essentials 的[Windows Phone 市集](http://www.windowsphone.com/apps/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a)來存取您的共用的資料夾和媒體檔案會儲存在伺服器。  
  
##### <a name="to-log-on-to-remote-web-access-from-a-mobile-device"></a>登入遠端 Web 存取行動裝置版裝置  
  
1.  打開網頁瀏覽器並輸入**https://***< YourDomainName\ >***/遠端**在網址列。  請確定您已 https 中包含 s。  
  
2.  在遠端 Web 存取登入頁面上，您的使用者名稱和密碼方塊中輸入文字，然後箭號。 登入遠端網站存取的行動裝置版版本。  
  
##### <a name="to-switch-to-the-desktop-version-of-remote-web-access"></a>若要切換到遠端的網路存取的傳統型版本  
  
1.  打開網頁瀏覽器並輸入**https://***< YourDomainName\ >***/遠端**在網址列。  請確定您已 https 中包含 s。  
  
2.  在遠端 Web 存取登入頁面上，在文字方塊中輸入您的使用者名稱和密碼，請按一下**檢視桌面版本**，然後按一下箭頭。 登入遠端 Web 存取的傳統型版本。  
  
##### <a name="to-return-to-the-mobile-version-of-remote-web-access"></a>若要返回遠端網站存取的行動裝置版版本  
  
1.  請先登出。  
  
2.  打開網頁瀏覽器並輸入**https://***< YourDomainName\ >***日遠端日 m**在網址列。 請確定您已 https 中包含 s。  
  
3.  行動裝置版的遠端 Web 存取版本會顯示。 在遠端 Web 存取登入頁面上，您的使用者名稱和密碼方塊中輸入文字，然後箭號。 登入遠端網站存取的行動裝置版版本。  
  
 您可以搜尋伺服器上的共用資料夾中檔案和資料夾。  
  
###  <a name="BKMK_9"></a>適用於行動裝置版裝置支援網頁瀏覽器  
 適用於行動裝置版裝置的支援的網頁瀏覽器包括：  
  
-   Internet Explorer 行動裝置版 6.0 或更新版本  
  
-   Safari  
  
-   要在 blackberry  
  
-   Symbian 6.0 或更新版本  
  
-   Android  
  
-   Google Chrome  
  
-   Firefox  
  
## <a name="see-also"></a>也了  
  
-   [管理網路遠端存取](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  

-   [從遠端使用](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](Use-Windows-Server-Essentials.md)

-   [從遠端使用](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

