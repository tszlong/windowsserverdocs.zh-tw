---
title: "Windows Server Essentials 播放數位媒體"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5f570492-ee21-471b-92c1-3fd9bfb84f55
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8228d0b17a58858ed893181ddceb465715ffdeb5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="play-digital-media-in-windows-server-essentials"></a>Windows Server Essentials 播放數位媒體

>適用於：Windows Server 2012 R2 Essentials、Windows Server 2012 程式集

數位媒體指的是音訊、視訊及相片 content 數位壓縮。  Windows Server Essentials 可讓網路電腦及某些網路數位媒體播放數位媒體檔案儲存在伺服器上的裝置。  
  
 下列主題會提供資訊存取和播放數位媒體檔案是儲存在 Windows Server Essentials:  
  

-   [數位媒體概觀](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [播放，並分享數位媒體](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [播放共用從遠端位置的數位媒體檔案](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [數位媒體檔案新增至伺服器](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [下載格式化選項](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [簡單的檔案上傳工具](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [檢視及瀏覽共用數位媒體](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_7)  

-   [數位媒體概觀](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [播放，並分享數位媒體](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [播放共用從遠端位置的數位媒體檔案](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [數位媒體檔案新增至伺服器](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [下載格式化選項](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [簡單的檔案上傳工具](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [檢視及瀏覽共用數位媒體](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_7)  

  
##  <a name="BKMK_1"></a>數位媒體概觀  
 數位媒體指的是音訊、視訊及相片 content 已編碼（數位壓縮）。 編碼 content 包括轉換音訊和影片輸入數位媒體檔案，例如 Windows Media 檔案。 數位媒體編碼之後，它可以輕鬆地操作、散發，和播放的電腦，並輕鬆傳輸到電腦的網路。  
  
 數位媒體類型的範例包括：Windows Media 音訊 (WMA)、Windows Media 視訊 (WMV)、MP3、JPEG 和 AVI。 適用於數位媒體類型的 Windows Media player 支援的相關資訊，請查看[的檔案類型的 Windows Media player 支援的](https://support.microsoft.com/kb/316992)。  
  
### <a name="why-would-i-want-to-stream-my-digital-media"></a>為何要串流處理我的數位媒體？  
 很多人會儲存在 Windows Server Essentials 的共用資料夾中的音樂、影片、和圖片。 當您想要有時候可能會下列：  
  
-   **觀賞影片**。 您的伺服器可用來儲存，並串流大型收藏的影片和錄製的電視節目到您的電腦或其他播放裝置在您的網路。 您可以使用 Windows Media Player 串流至 Xbox 360 或電腦的影片。  
  
-   **播放音樂**。 當您將媒體共用適用於**音樂**共用的資料夾，您可以從裝置支援 Windows Media 連接存取您的音樂。 您不需要讓或設定從串流至任何帳號**音樂**之後分享您電腦的共用的資料夾。  
  
-   **顯示相片的投影片放映**。 您可以將您的數位相片**相片**共用您的伺服器上的資料夾，然後從任何電腦或 Xbox 360 連接至電視，在您的家用或 office 存取它們。 觀看相片的投影片放映，它就像將您的電視轉換成大型相框。  
  
### <a name="sharing-copy-protected-media"></a>分享複製保護的媒體  
  Windows Server Essentials 不支援分享複製保護的媒體。 這包括透過 online 音樂網上商店購買的音樂。  
  
 Copy-protected 媒體可播放只在的電腦或裝置，您用來購買它。 複製保護會防止您播放媒體一個以上的電腦或裝置上，即使您將媒體複製到您的伺服器，並從該處播放。 不過，您可以在 Windows Server Essentials 市集複製保護的媒體，並繼續進行播放媒體上的電腦或裝置，您用來購買它。  
  
##  <a name="BKMK_2"></a>播放，並分享數位媒體  
 在您的網路設定並成功連接您的電腦和裝置媒體伺服器網路之後，您可以搜尋您儲存及分享伺服器上的任何數位媒體檔案。  
  
> [!NOTE]
>  適用於數位媒體 player/接收的裝置相容於 Windows Server Essentials 的目前清單，請查看[Windows 相容性中心](https://www.microsoft.com/windows/compatibility/CompatCenter/Home)。  
  
 您可以使用下列方式來搜尋，播放數位媒體檔案儲存在您的伺服器上的其中一項：  
  

-   [搜尋，並在網路上播放在 Windows Server Essentials 的媒體檔案從電腦或數位媒體播放程式](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)  
  
-   [Windows Media Player，在 Xbox 360 或網路數位媒體播放網路傳送 Windows Server Essentials 媒體檔案](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SendToDevice)  

-   [搜尋，並在網路上播放在 Windows Server Essentials 的媒體檔案從電腦或數位媒體播放程式](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)  
  
-   [Windows Media Player，在 Xbox 360 或網路數位媒體播放網路傳送 Windows Server Essentials 媒體檔案](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SendToDevice)  

  
###  <a name="BKMK_2.1"></a>搜尋，並在網路上播放在 Windows Server Essentials 的媒體檔案從電腦或數位媒體播放程式  
 當您的裝置已加入到 Windows Server Essentials 的網路時，您可以搜尋並播放數位媒體檔案下列方式：  
  

-   [搜尋及播放從電腦所執行的 Windows Media Center 的媒體檔案](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_WMC)  
  
-   [搜尋及播放從電腦使用 Windows Media Player 執行 Windows media 檔案](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_MWP)  
  
-   [搜尋，並使用 Xbox 360 播放 media 檔案](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Xbox)  
  
-   [搜尋及其他數位媒體玩家或接收器相容於 Windows Server Essentials 使用播放媒體檔案](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Other)  
  
-   [搜尋和播放媒體檔案所使用的共用資料夾 Launchpad 功能](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [搜尋，並使用遠端 Web 存取播放共用的媒體](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_RWA2)  

-   [搜尋及播放從電腦所執行的 Windows Media Center 的媒體檔案](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_WMC)  
  
-   [搜尋及播放從電腦使用 Windows Media Player 執行 Windows media 檔案](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_MWP)  
  
-   [搜尋，並使用 Xbox 360 播放 media 檔案](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Xbox)  
  
-   [搜尋及其他數位媒體玩家或接收器相容於 Windows Server Essentials 使用播放媒體檔案](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Other)  
  
-   [搜尋和播放媒體檔案所使用的共用資料夾 Launchpad 功能](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SharedFolders)  
  
-   [搜尋，並使用遠端 Web 存取播放共用的媒體](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_RWA2)  

  
####  <a name="BKMK_WMC"></a>搜尋及播放從電腦所執行的 Windows Media Center 的媒體檔案  
  
1.  按一下**[開始]**，按一下 [**所有程式**，然後按一下 [ **Windows Media Center**。  
  
2.  在**Windows Media Center**頁面上，捲動到 [您要搜尋的媒體類型，按一下媒體櫃。  
  
3.  手動搜尋的檔案您感興趣，或按一下 [**搜尋**，然後輸入您要尋找的檔案名稱。  
  
4.  按一下媒體檔案影像來檢視或播放檔案。  
  
####  <a name="BKMK_MWP"></a>搜尋及播放從電腦使用 Windows Media Player 執行 Windows media 檔案  
  
-   從電腦或裝置媒體，請打開**Windows Media Player**和 [媒體櫃中的搜尋。  
  
    > [!NOTE]
    >  搜尋步驟會根據您所使用的 Windows Media player 版本而有所不同。 詳細資訊，請洽詢您的版本的協助。  
  
####  <a name="BKMK_Xbox"></a>搜尋，並使用 Xbox 360 播放 media 檔案  
  
1.  使用有線或 wireless 連接到您家用網路連接您的 Xbox 360 主機。  
  
2.  在電腦上所執行的 Windows Server Essentials，關閉共用的媒體。 如需詳細資訊，請查看主題[將媒體串流關閉](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)。  
  
3.  若要播放的數位媒體檔案，使用您的 Xbox 360 主機：  
  
    1.  移至**我的 Xbox**，，然後選取 [**視訊媒體櫃**、**的音樂媒體櫃**，或**的圖片媒體櫃**，視想要檢視，或播放的媒體類型而定。  
  
    2.  選取您的伺服器的名稱。  
  
        > [!NOTE]
        >  如果未列出您的伺服器的名稱，選取 [**電腦**，然後按一下 [**測試連接]**。  
  
    3.  瀏覽 [檔案] 清單，然後選取您想要播放的項目。  
  
####  <a name="BKMK_Other"></a>搜尋及其他數位媒體玩家或接收器相容於 Windows Server Essentials 使用播放媒體檔案  
  
1.  移至[Windows 相容性中心](https://www.microsoft.com/windows/compatibility/CompatCenter/Home)，並確認您的數位媒體 player 或接收器會出現在清單中的相容裝置。  
  
2.  搜尋步驟會根據使用您的數位媒體播放程式而有所不同，因為參考協助為您的裝置的詳細指示操作。  
  
####  <a name="BKMK_SharedFolders"></a>搜尋和播放媒體檔案所使用的共用資料夾 Launchpad 功能  
  
1.  Windows Server Essentials Launchpad 登入。  
  
2.  從 Launchpad，按一下 [**共用資料夾**。 Windows 檔案總管] 視窗開啟並顯示伺服器上的共用的資料夾。  
  
3.  在**搜尋**方塊中，輸入媒體檔案的名稱。 顯示您的搜尋查詢的結果。  
  
    > [!NOTE]
    >  選項，您也可以按兩下共用的資料夾瀏覽到資料夾。  
  
####  <a name="BKMK_RWA2"></a>搜尋，並使用遠端 Web 存取播放共用的媒體  
  
1.  登入遠端網路存取權。  
  
2.  按一下**的共用資料夾**。 **的共用資料夾**的網頁] 區段會顯示在伺服器上的共用資料夾清單。  
  
3.  按兩下某個資料夾來技術的資料夾。  
  
###  <a name="BKMK_SendToDevice"></a>Windows Media Player，在 Xbox 360 或網路數位媒體播放網路傳送 Windows Server Essentials 媒體檔案  
 使用**Windows Media Player**若要搜尋您想要媒體檔案。 媒體檔案上按一下滑鼠右鍵，然後按一下**播放以**將媒體檔案給媒體網路的裝置。  
  
##  <a name="BKMK_3"></a>播放共用從遠端位置的數位媒體檔案  
 您可以在您何時離開您的 Windows Server Essentials 網路使用遠端 Web 存取播放媒體檔案。 您可以使用手機、遠端電腦時或數位媒體播放中搜尋和播放您儲存在您的伺服器的共用的媒體檔案。  
  
#### <a name="to-play-shared-media-files-when-you-are-away-from-the-network"></a>您何時離開網路播放共用的媒體檔案  
  
1.  打開網際網路瀏覽器。  
  
2.  請移至您的網路存取網站。 輸入**https://<YourDomainName\>/remote**在網址列的網際網路瀏覽器，然後按下 Enter。  
  
    > [!NOTE]
    >  *< YourDomainName\ >*是預留位置。 將會名稱是唯一您的伺服器，讓您輸入的位址看起來就像**https://contoso.com/remote**。 如果您不知道您的網域名稱，請管理員人員選擇當遠端存取的功能設定在伺服器上的網域名稱。 如需詳細資訊，請查看[上遠端 Web 存取關閉](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)。  
  
3.  在遠端 Web 存取登入頁面上，輸入您的 account 的使用者名稱和密碼，然後按一下箭頭。  
  
4.  使用所選任何您想要搜尋您要播放的媒體檔案的方法。  
  
    > [!NOTE]

    >  如各種資訊搜尋方法，請查看[中的搜尋和播放媒體檔案，在 Windows Server Essentials 的電腦或網路上的數位媒體 player](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)。  

    >  如各種資訊搜尋方法，請查看[中的搜尋和播放媒體檔案，在 Windows Server Essentials 的電腦或網路上的數位媒體 player](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)。  

  
5.  媒體檔案名稱出現時，按一下播放媒體檔案名稱。  
  
##  <a name="BKMK_4"></a>數位媒體檔案新增至伺服器  

 伺服器管理員可以將數位媒體新增到媒體櫃中的共用資料夾存取伺服器直接，或使用遠端網站存取的網站來登入儀表板。 其他使用者可以將媒體檔案新增至伺服器使用**共用資料夾**連接 Launchpad 使用遠端網站存取的網站，或使用我的伺服器應用程式適用於 Windows Phone 上。 如需有關播放媒體資訊，請查看[播放分享數位媒體](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)。  

 伺服器管理員可以將數位媒體新增到媒體櫃中的共用資料夾存取伺服器直接，或使用遠端網站存取的網站來登入儀表板。 其他使用者可以將媒體檔案新增至伺服器使用**共用資料夾**連接 Launchpad 使用遠端網站存取的網站，或使用我的伺服器應用程式適用於 Windows Phone 上。 如需有關播放媒體資訊，請查看[播放分享數位媒體](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)。  

  
> [!NOTE]
>  您也可以使用我的伺服器應用程式適用於 Windows Phone 伺服器上載媒體檔案。 您可以下載我伺服器 app，從[Windows Phone 市集中](http://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a)。 適用於 Windows Phone 我伺服器應用程式的相關詳細資訊，會看到部落格文章[適用於 Windows Server Essentials 伺服器我的手機應用程式](http://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx)。  
  
#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>若要新增數位媒體檔案伺服器上的共用資料夾  
  
1.  登入伺服器使用其中一項下列方法：  
  

    1.  用於登入遠端網站存取的相關資訊，請查看[使用遠端 Web 存取](Use-Remote-Web-Access-in-Windows-Server-Essentials.md)。  

    1.  用於登入遠端網站存取的相關資訊，請查看[使用遠端 Web 存取](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)。  

  
    2.  用於登入 Launchpad 的相關資訊，請查看[Launchpad 概觀](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md)。  
  
2.  搜尋，然後按一下您要新增的媒體的資料夾。  
  
3.  複製和貼上或拖放您想要新增至適當的媒體檔案共用的伺服器上的資料夾。  
  
##  <a name="BKMK_5"></a>下載格式化選項  
 有兩個選項來下載檔案。 只有當您正在下載多個檔案或資料夾的 windows 電腦時，可以使用這些選項。  
  
 選擇下列符合您需求的下載項目：  
  
-   **壓縮的 ZIP 檔案 (.zip)**  
  
     壓縮檔案會建立的壓縮的檔案小於原始檔案版本。 壓縮的檔案的版本已經.zip 檔案名稱擴充功能。 減少最來壓縮的檔案類型的文字導向檔案類型（例如.txt、.doc，以及.xls），以及圖形檔案該使用未壓縮的檔案類型（例如.bmp)。 某些圖形檔案.jpg 與.gif 檔案，例如已使用壓縮，並檔案縮小幾乎來壓縮。 此外，包含許多圖形 Word 文件不一樣，大部分文字文件減少。  
  
    > [!NOTE]
    >  這個選項會提供有限的支援國際檔案的名稱。  
  
-   **自動解壓縮可執行檔 (.exe)**  
  
     自動解壓縮的可執行檔是結合了特定地區的壓縮檔案的解壓縮 （可執行檔） 的程式則可以下載的檔案。 當您執行的可執行的程式時，它會自動將壓縮的檔案解壓縮。 這是常見的方式，而不用擔心收件者是否有正確解壓縮公用程式散發壓縮的資料。  
  
    > [!NOTE]
    >  這個選項支援 Unicode 字元。  
  
 實際下載在開始之前，建立.exe 或.zip 檔案。 根據您的檔案和下載檔案的大小，這可能需要花費幾分鐘的時間。 建立下載檔案之後，會在背景下載檔案發生。 這可讓您下載的程序完成時繼續工作。  
  
##  <a name="BKMK_6"></a>簡單的檔案上傳工具  
 簡單的檔案上傳工具來簡化您 Windows Server Essentials 的伺服器上的檔案上傳程的序。 您可以新增多個檔案，以及您想要的簡單檔案上傳的工具，然後將檔案上傳到單一批次在 Windows Server Essentials 伺服器上的共用資料夾。 如需詳細資訊，請查看部落格文章[了解遠端 Web 存取檔案共用](http://blogs.technet.com/b/sbs/archive/2012/04/19/understanding-remote-web-access-file-sharing.aspx)。  
  
##  <a name="BKMK_7"></a>檢視及瀏覽共用數位媒體  
 您可以檢視或瀏覽資源適用於 Windows Phone 中使用儀表板、Launchpad、遠端網站存取的網站或我伺服器應用程式。  
  
#### <a name="to-view-and-browse-shared-media-from-the-dashboard"></a>若要檢視和瀏覽共用的媒體從儀表板  
  
1.  打開伺服器儀表板。  
  
2.  在主要瀏覽列中，按一下 [**存放裝置**。  
  
3.  按一下**資料夾伺服器**索引標籤。出現的伺服器資料夾清單。  
  
4.  按兩下某個資料夾來技術的資料夾。  
  
#### <a name="to-view-and-browse-shared-media-using-the-launchpad-from-a-network-computer"></a>若要檢視和瀏覽使用的網路電腦 Launchpad 共用的媒體  
  
1.  登入 Launchpad。  
  
2.  按一下**的共用資料夾**。 Windows 檔案總管開啟，並顯示在伺服器上的共用資料夾清單。  
  
3.  按兩下某個資料夾來技術的資料夾。  
  
#### <a name="to-view-and-browse-shared-media-using-remote-web-access"></a>若要檢視和瀏覽使用存取網路共用的媒體  
  
1.  登入遠端網路存取權。  
  
2.  按一下**的共用資料夾**。 **的共用資料夾**的網頁] 區段會顯示在伺服器上的共用資料夾清單。  
  
3.  按兩下某個資料夾來技術的資料夾。  
  
## <a name="see-also"></a>也了  
  
-   [管理數位媒體](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md)  
  

-   [連接](Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [使用共用資料夾](Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [從遠端使用](Work-Remotely-in-Windows-Server-Essentials.md)

-   [連接](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [使用共用資料夾](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [從遠端使用](../use/Work-Remotely-in-Windows-Server-Essentials.md)

