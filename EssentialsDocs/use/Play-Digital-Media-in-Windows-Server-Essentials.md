---
title: 在 Windows Server Essentials 中播放數位媒體
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 5f570492-ee21-471b-92c1-3fd9bfb84f55
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: bd14631917a9f0211e39bd44c9d01e11ad2a177f
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/27/2020
ms.locfileid: "87179884"
---
# <a name="play-digital-media-in-windows-server-essentials"></a>在 Windows Server Essentials 中播放數位媒體

>適用于： Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

數位媒體係指已經過數位壓縮的音訊、視訊及相片內容。  Windows Server Essentials 可讓網路電腦和一些網路數位媒體裝置播放儲存在伺服器上的數位媒體檔案。

 下列主題提供有關存取和播放儲存在 Windows Server Essentials 上之數位媒體檔案的資訊：


-   [數位媒體概觀](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)

-   [播放和共用數位媒體](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)

-   [從遠端位置播放共用數位媒體檔案](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)

-   [將數位媒體檔案新增到伺服器](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)

-   [下載格式選項](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)

-   [簡易檔案上傳工具](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)

-   [檢視和瀏覽共用數位媒體](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_7)


##  <a name="digital-media-overview"></a><a name="BKMK_1"></a>數位媒體總覽
 數位媒體係指已編碼 (數位壓縮) 的音訊、視訊及相片內容。 將內容編碼涉及將音訊和視訊輸入轉換成數位媒體檔案 (例如 Windows 媒體檔案)。 數位媒體在經過編碼之後，就可以輕鬆地被操作、散佈及由電腦播放，並且可以輕鬆地透過電腦網路來傳輸。

 數位媒體類型的範例包括：Windows Media 音訊 (WMA)、Windows Media 視訊 (WMV)、MP3、JPEG 及 AVI。 如需 Windows Media Player 所支援之數位媒體類型的相關資訊，請參閱 [Windows Media Player 所支援的檔案類型](https://support.microsoft.com/kb/316992)。

### <a name="why-would-i-want-to-stream-my-digital-media"></a>我為什麼要對數位媒體進行串流處理？
 許多人會在 Windows Server Essentials 的共用資料夾中儲存音樂、影片和圖片。 有時您可能會想要執行下列作業：

-   **觀賞視訊**。 您的伺服器可用來儲存大量的視訊和錄製的電視節目，並透過串流處理將其傳送到您的電腦或您網路上的其他播放裝置。 您可以使用 Windows Media Player，將視訊透過串流處理傳送到 Xbox 360 或電腦。

-   **播放音樂**。 當您開啟 [音樂]**** 共用資料夾的「媒體共用」時，便可以從支援 Windows Media Connect 的裝置存取您的音樂。 在開啟共用之後，您不需要啟用或設定任何使用者帳戶，即可以從 [音樂]**** 共用資料夾進行串流處理。

-   **顯示相片投影片放映**。 您可以將數位相片儲存在您伺服器上的 [相片] **** 共用資料夾中，然後從任何電腦或從連接到您住家或辦公室中電視的 Xbox 360 存取這些相片。 您可以觀賞相片投影片放映，就像將電視變成大型相框一樣。

### <a name="sharing-copy-protected-media"></a>共用禁止複製的媒體
  Windows Server Essentials 不支援共用受禁止複製的媒體。 這包括透過線上音樂商店購買的音樂。

 禁止複製的媒體只能在您用來購買它的電腦或裝置上播放。 禁止複製會防止您在多部電腦或裝置上播放媒體，即使您將媒體複製到您的伺服器並從該處播放它也不行。 不過，您可以將受禁止複製的媒體儲存在 Windows Server Essentials 上，並繼續在您用來購買它的電腦或裝置上播放媒體。

##  <a name="play-and-share-digital-media"></a><a name="BKMK_2"></a>播放和共用數位媒體
 設定妥您的網路並順利將您的電腦和媒體裝置連線到伺服器網路之後，您便可以搜尋您在伺服器上儲存和共用的任何數位媒體檔案。

> [!NOTE]
>  如需目前與 Windows Server Essentials 相容的數位媒體播放機/接收裝置清單，請參閱[Windows 相容性中心](https://www.microsoft.com/windows/compatibility/CompatCenter/Home)。

 您可以使用下列任一方式來搜尋和播放儲存在您的伺服器上的數位媒體檔案：


-   [從網路上的電腦或數位媒體播放機搜尋和播放 Windows Server Essentials 上的媒體檔案](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)

-   [將 Windows Server Essentials 上的媒體檔案傳送到 Windows 媒體播放機、Xbox 360，或網路中的網路數位媒體播放機](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SendToDevice)



###  <a name="search-for-and-play-media-files-on-windows-server-essentials-from-a-computer-or-digital-media-player-on-the-network"></a><a name="BKMK_2.1"></a>從網路上的電腦或數位媒體播放機搜尋和播放 Windows Server Essentials 上的媒體檔案
 當您的裝置加入 Windows Server Essentials 網路時，您可以透過下列任何方式來搜尋和播放數位媒體檔案：


-   [從執行 Windows Media Center 的電腦搜尋和播放媒體檔案](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_WMC)

-   [從執行 Windows 的電腦使用 Windows Media Center 來搜尋和播放媒體檔案](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_MWP)

-   [使用 Xbox 360 來搜尋和播放媒體檔案](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Xbox)

-   [使用與 Windows Server Essentials 相容的其他數位媒體播放機或接收者來搜尋和播放媒體檔案](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_Other)

-   [使用啟動列的共用資料夾功能來搜尋和播放媒體檔案](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_SharedFolders)

-   [使用遠端 Web 存取來搜尋和播放共用媒體](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_RWA2)



####  <a name="search-for-and-play-media-files-from-a-computer-that-is-running-windows-media-center"></a><a name="BKMK_WMC"></a>從執行 Windows Media Center 的電腦搜尋和播放媒體檔案

1.  依序按一下 [開始]****、[所有程式]****，然後按一下 [Windows Media Center]****。

2.  在 [Windows Media Center]**** 頁面上，捲動到您要搜尋的媒體類型，然後按一下媒體櫃。

3.  手動搜尋您感興趣的檔案，或按一下 [搜尋]****，然後輸入您想要尋找的檔案名稱。

4.  按一下媒體檔案影像以檢視或播放檔案。

####  <a name="search-for-and-play-media-files-from-a-computer-that-is-running-windows-by-using-windows-media-player"></a><a name="BKMK_MWP"></a>使用 Windows 媒體播放機從執行 Windows 的電腦搜尋和播放媒體檔案

-   從電腦或媒體裝置，開啟 [Windows Media Player]****，然後搜尋您的媒體櫃。

    > [!NOTE]
    >  搜尋步驟會依您使用的 Windows Media Player 版本而有所不同。 如需詳細資訊，請參閱您版本的說明。

####  <a name="search-for-and-play-media-files-by-using-xbox-360"></a><a name="BKMK_Xbox"></a>使用 Xbox 360 搜尋和播放媒體檔案

1.  使用有線或無線連線將您的 Xbox 360 主機連線到您的家用網路。

2.  在執行 Windows Server Essentials 的電腦上，開啟 [媒體共用]。 如需詳細資訊，請參閱[開啟或關閉媒體串流處理](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)主題。

3.  使用您的 Xbox 360 主機來播放數位媒體檔案：

    1.  前往 [我的 Xbox]****，然後依據你想要檢視或播放的媒體類型，選取 [視訊媒體櫃]****、[音樂媒體櫃]**** 或 [圖片媒體櫃]****。

    2.  選取您伺服器的名稱。

        > [!NOTE]
        >  如果未列出您伺服器的名稱，請選取 [電腦]****，然後按一下 [測試連線]****。

    3.  瀏覽檔案清單並選取您想要播放的項目。

####  <a name="search-for-and-play-media-files-by-using-other-digital-media-players-or-receivers-that-are-compatible-with-windows-server-essentials"></a><a name="BKMK_Other"></a>使用與 Windows Server Essentials 相容的其他數位媒體播放機或接收者來搜尋和播放媒體檔案

1.  前往 [Windows 相容性中心](https://www.microsoft.com/windows/compatibility/CompatCenter/Home) ，確定您的數位媒體播放程式或接收器出現在相容的裝置清單中。

2.  由於搜尋步驟會依您使用的數位媒體播放機而有所不同，因此請參閱您裝置的說明，以取得詳細的指示。

####  <a name="search-for-and-play-media-files-by-using-the-shared-folders-feature-of-the-launchpad"></a><a name="BKMK_SharedFolders"></a>使用 [啟動列] 的 [共用資料夾] 功能來搜尋和播放媒體檔案

1.  登入 [Windows Server Essentials 啟動列]。

2.  從 [啟動列] 中，按一下 [共用資料夾]****。 這會開啟 [Windows 檔案總管] 視窗，並顯示伺服器上的共用資料夾。

3.  在 [搜尋]**** 方塊中，輸入媒體檔案的名稱。 搜尋查詢的結果隨即顯示。

    > [!NOTE]
    >  您也可以選擇按兩下共用資料夾來瀏覽資料夾內容。

####  <a name="search-for-and-play-shared-media-by-using-remote-web-access"></a><a name="BKMK_RWA2"></a>使用遠端 Web 存取搜尋和播放共用媒體

1.  登入 [遠端 Web 存取]。

2.  按一下 [共用資料夾]。 網頁的 [共用資料夾]**** 區段會顯示伺服器上的共用資料夾清單。

3.  按兩下資料夾來檢視該資料夾的內容。

###  <a name="send-media-files-on-windows-server-essentials-to-windows-media-player-xbox-360-or-to-a-networked-digital-media-player-in-the-network"></a><a name="BKMK_SendToDevice"></a>將 Windows Server Essentials 上的媒體檔案傳送到 Windows 媒體播放機、Xbox 360，或網路中的網路數位媒體播放機
 請使用 [Windows Media Player]**** 來搜尋您想要的媒體檔案。 在媒體檔案上按一下滑鼠右鍵，然後按一下 [播放至]**** 將媒體檔案傳送到網路媒體裝置。

##  <a name="play-shared-digital-media-files-from-a-remote-location"></a><a name="BKMK_3"></a>從遠端位置播放共用的數位媒體檔案
 當您離開 Windows Server Essentials 網路時，可以使用遠端 Web 存取來播放媒體檔案。 您可以使用行動電話、遠端電腦或數位媒體播放機，來搜尋和播放您儲存在伺服器上的共用媒體檔案。

#### <a name="to-play-shared-media-files-when-you-are-away-from-the-network"></a>在您不位於網路近端時播放共用媒體檔案

1. 開啟網際網路瀏覽器。

2. 前往「遠端 Web 存取」網站。 在網際網路瀏覽器的網址列中輸入**HTTPs://<您功能變數名稱 \> /remote** ，然後按 enter。

   > [!NOTE]
   >  *<您功能變數名稱 \> *這是一個預留位置。 它會是您的伺服器特有的名稱，因此您輸入的位址看起來會像這樣 **https://contoso.com/remote** 。 如果您不知道您的網域名稱，請詢問在伺服器上設定「遠端存取」功能時選擇網域名稱的系統管理員。 如需詳細資訊，請參閱[開啟遠端 Web 存取](../manage/Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)。

3. 在 [遠端 Web 存取] 登入頁面上，輸入您的使用者帳戶名稱和密碼，然後按一下箭號。

4. 使用任何您想要搜尋要播放之媒體檔案的方法。

   > [!NOTE]
   >
   >  如需各種搜尋方法的相關資訊，請參閱[從網路上的電腦或數位媒體播放機搜尋和播放 Windows Server Essentials 上的媒體](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)檔案。
   >
   >  如需各種搜尋方法的相關資訊，請參閱[從網路上的電腦或數位媒體播放機搜尋和播放 Windows Server Essentials 上的媒體](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.1)檔案。


5. 媒體檔案名稱出現時，按一下該檔案名稱來播放媒體。

##  <a name="add-digital-media-files-to-the-server"></a><a name="BKMK_4"></a>將數位媒體檔案新增至伺服器

 伺服器系統管理員可以直接存取伺服器，或使用遠端 Web 存取網站登入儀表板，將數位媒體新增至媒體櫃中的共用資料夾。 其他使用者可以使用 [啟動列] 上的 [**共用資料夾**] 連線、使用 [遠端 Web 存取] 網站，或使用適用于 Windows Phone 的 My server 應用程式，將媒體檔案新增至伺服器。 如需有關播放媒體資訊，請參閱[播放和共用數位媒體](Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)。

 伺服器系統管理員可以直接存取伺服器，或使用遠端 Web 存取網站登入儀表板，將數位媒體新增至媒體櫃中的共用資料夾。 其他使用者可以使用 [啟動列] 上的 [**共用資料夾**] 連線、使用 [遠端 Web 存取] 網站，或使用適用于 Windows Phone 的 My server 應用程式，將媒體檔案新增至伺服器。 如需有關播放媒體資訊，請參閱[播放和共用數位媒體](../use/Play-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)。


> [!NOTE]
>  您也可以透過使用適用於 Windows Phone 的 My Server 應用程式，將媒體檔案上傳到伺服器。 您可以從 [Windows Phone 市集](http://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a)下載 My Server 應用程式。 如需有關適用于 Windows Phone 的 My Server 應用程式的詳細資訊，請參閱[Windows Server Essentials 的 My server Phone 應用程式](https://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx)的 blog 文章。

#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>將數位媒體檔案新增到伺服器上的共用資料夾

1.  請使用下列其中一種方法來登入伺服器：


    1.  如需登入遠端 Web 存取的詳細資訊，請參閱[使用遠端 Web 存取](Use-Remote-Web-Access-in-Windows-Server-Essentials.md)。

    1.  如需登入遠端 Web 存取的詳細資訊，請參閱[使用遠端 Web 存取](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)。


    2.  如需使用 [啟動列] 登入的相關資訊，請參閱啟動控制[板總覽](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md)。

2.  搜尋並按一下您要新增媒體的資料夾。

3.  將您想要新增的媒體檔案複製並貼到或拖放到伺服器上適當的共用資料夾中。

##  <a name="download-format-options"></a><a name="BKMK_5"></a>下載格式選項
 有兩個下載檔案的選項。 只有當您要將多個檔案或資料夾下載到 Windows 型電腦時，才會有這些選項。

 請從以下選擇符合您下載需求的選項：

- **壓縮的 ZIP 檔 (.zip)**

   壓縮檔案會建立比原始檔案小的檔案壓縮版本。 檔案的壓縮版本具有.zip 副檔名。 壓縮後縮減最多的檔案類型是文字導向的檔案類型 (例如 .txt、.doc 及 .xls) 和使用非壓縮檔案類型的圖形檔案 (例如 .bmp)。 有些圖形檔案 (例如 .jpg 和 .gif 檔案) 已經使用壓縮，因此壓縮後檔案大小縮減很少。 此外，包含大量圖形的 Word 文件縮減程度也不如大部份是文字的文件。

  > [!NOTE]
  >  這個選項對國際檔案名稱僅提供有限的支援。

- **自我解壓縮的執行檔 (.exe)**

   自我解壓縮的執行檔是一種您可下載且結合了壓縮檔之解壓縮 (可執行) 程式的檔案。 當您執行可執行程式時，它會自動將壓縮檔解壓縮。 這是一種散佈壓縮資料的常見方式，可不需擔心接收者是否具有正確的解壓縮公用程式。

  > [!NOTE]
  >  這個選項支援 Unicode 字元。

  在實際下載開始之前，會先建立 exe 或 zip 檔。 視要下載的檔案數目和檔案總大小而定，這可能會花費數分鐘的時間。 建立下載檔案之後，就會在背景進行下載檔案的程序。 這可讓您在下載程序完成的過程中繼續工作。

##  <a name="easy-file-upload-tool"></a><a name="BKMK_6"></a>簡易檔案上傳工具
 「簡易檔案上傳」工具可以簡化在 Windows Server Essentials 伺服器上上傳檔案的程式。 您可以視需要將多個檔案新增至「簡易檔案上傳」工具，然後在單一批次中將它們上傳到 Windows Server Essentials 伺服器上的共用資料夾。 如需詳細資訊，請參閱部落格文章 [了解「遠端 Web 存取」檔案共用](https://blogs.technet.com/b/sbs/archive/2012/04/19/understanding-remote-web-access-file-sharing.aspx)。

##  <a name="view-and-browse-shared-digital-media"></a><a name="BKMK_7"></a>查看和流覽共用數位媒體
 您可以使用「儀表板」、「啟動列」、「遠端 Web 存取」網站或適用於 Windows Phone 的 My Server 應用程式來檢視或瀏覽資源。

#### <a name="to-view-and-browse-shared-media-from-the-dashboard"></a>從儀表板檢視和瀏覽共用媒體

1.  開啟伺服器 [儀表板]。

2.  在主瀏覽列上，按一下 [存放]****。

3.  按一下 [**伺服器資料夾**] 索引標籤。伺服器資料夾的清單隨即出現。

4.  按兩下資料夾來檢視該資料夾的內容。

#### <a name="to-view-and-browse-shared-media-using-the-launchpad-from-a-network-computer"></a>使用啟動列從網路電腦檢視和瀏覽共用媒體

1.  登入 [啟動列]。

2.  按一下 [共用資料夾]。 這會開啟 [Windows 檔案總管]，並顯示伺服器上的共用資料夾清單。

3.  按兩下資料夾來檢視該資料夾的內容。

#### <a name="to-view-and-browse-shared-media-using-remote-web-access"></a>使用遠端 Web 存取來檢視和瀏覽共用媒體

1.  登入 [遠端 Web 存取]。

2.  按一下 [共用資料夾]。 網頁的 [共用資料夾]**** 區段會顯示伺服器上的共用資料夾清單。

3.  按兩下資料夾來檢視該資料夾的內容。

## <a name="additional-references"></a>其他參考

-   [管理數位媒體](../manage/Manage-Digital-Media-in-Windows-Server-Essentials.md)


-   [取得連線](Get-Connected-in-Windows-Server-Essentials.md)

-   [使用共用資料夾](Use-Shared-Folders-in-Windows-Server-Essentials.md)

-   [遠端工作](Work-Remotely-in-Windows-Server-Essentials.md)

< < < < < < < HEAD
-   [取得連線](../use/Get-Connected-in-Windows-Server-Essentials.md)

-   [使用共用資料夾](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)

-   [遠端工作](../use/Work-Remotely-in-Windows-Server-Essentials.md)

=======
>>>>>>> 97724df67237ac603cf9eb996732230bdb7c0b88
