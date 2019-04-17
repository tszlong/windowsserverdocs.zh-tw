---
title: "管理 Windows Server Essentials 的數位媒體"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9378bffa-487c-43ca-9ec3-7e7864d2dd9a
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6906c1dafc6d4131e07c008b9db47ebebe9b7770
ms.sourcegitcommit: 5c8d8acc59d61756377c9c4e459a47a9ab555b39
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2018
---
# <a name="manage-digital-media-in-windows-server-essentials"></a>管理 Windows Server Essentials 的數位媒體

>適用於：Windows Server 2012 R2 Essentials、Windows Server 2012 程式集

下列主題討論媒體串流處理您的伺服器的功能，以及如何設定及使用您的網路上串流媒體。  
  
> [!NOTE]
>  根據預設，媒體串流功能不是 Windows Server Essentials 和 Windows Server 2012 R2 已安裝 Windows Server Essentials 體驗角色。 若要將媒體串流功能這些版本中，[下載並安裝 Windows Server Essentials 媒體套件](https://www.microsoft.com/download/details.aspx?id=40837)從 Microsoft 下載中心」。  
  
-   [數位媒體概觀](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [管理媒體伺服器使用儀表板](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [串流媒體的運作方式](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [將媒體串流關閉](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [數位媒體檔案新增至伺服器](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [允許或限制的伺服器上的媒體櫃的存取](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [重新命名媒體櫃](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8)  
  
-   [停止分享數位媒體](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [讓使用伺服器訊息區 (SMB) 通訊協定存取伺服器上的檔案共用的媒體裝置](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [常見的處理器和他們支援的影片設定檔](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_CommonProcessors)  
  
-   [已知的問題類型的媒體檔案](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_KnownIssues)  
  
##  <a name="BKMK_1"></a>數位媒體概觀  
 數位媒體指的是音訊、視訊及相片 content 已編碼（數位壓縮）。 編碼 content 包括轉換音訊和影片輸入數位媒體檔案，例如 Windows Media 檔案。 數位媒體編碼之後，它可以輕鬆地操作、散發，和播放的電腦，並輕鬆傳輸到電腦的網路。  
  
 數位媒體類型的範例包括：Windows Media 音訊 (WMA)、Windows Media 視訊 (WMV)、MP3、JPEG 和 AVI。 適用於數位媒體類型的 Windows Media player 支援的相關資訊，請查看[的檔案類型的 Windows Media player 支援的](https://support.microsoft.com/kb/316992)。  
  
### <a name="why-would-i-want-to-stream-my-digital-media"></a>為何要串流處理我的數位媒體？  
 很多人會儲存在 Windows Server Essentials 的共用資料夾中的音樂、影片、和圖片。 有時候可能會想要時執行下列動作：  
  
-   **觀賞影片**。 您的伺服器可用來儲存，並串流大型收藏的影片和錄製的電視節目到您的電腦或其他播放裝置在您的網路。 您可以使用 Windows Media Player 串流至 Xbox 360 或電腦的影片。  
  
-   **播放音樂**。 當您將媒體共用適用於**音樂**共用的資料夾，您可以從裝置支援 Windows Media 連接存取您的音樂。 您不需要讓或設定從串流至任何帳號**音樂**之後分享您電腦的共用的資料夾。  
  
-   **顯示相片的投影片放映**。 您可以將您的數位相片**相片**共用您的伺服器上的資料夾，然後從任何電腦或 Xbox 360 連接至電視，在您的家用或 office 存取它們。 觀看相片的投影片放映，它就像將您的電視轉換成大型相框。  
  
###  <a name="BKMK_1.5"></a>分享複製保護的媒體  
  Windows Server Essentials 不支援分享複製保護的媒體。 這包括透過 online 音樂網上商店購買的音樂。  
  
 Copy-protected 媒體可播放只在的電腦或裝置，您用來購買它。 複製保護會防止您播放媒體一個以上的電腦或裝置上，即使您將媒體複製到您的伺服器，並從該處播放。 不過，您可以在 Windows Server Essentials 市集複製保護的媒體，並繼續進行播放媒體上的電腦或裝置，您用來購買它。  
  
##  <a name="BKMK_2"></a>管理媒體伺服器使用儀表板  
  Windows Server Essentials 可讓您可以使用 Windows Server Essentials 儀表板執行一般管理工作。 **媒體**索引標籤的伺服器**設定**頁面的儀表板可提供動作：  
  
|區段|功能|  
|-------------|-------------------|  
|媒體伺服器|**將 / 關閉**按鈕，可讓您將媒體串流關閉。|  
|串流的視訊品質|下拉式箭號可讓您選擇 [串流播放來自伺服器的視訊品質的影片。|  
|媒體櫃|顯示媒體櫃的名稱。 預設名稱的媒體櫃稱為**數位媒體櫃**，當您將媒體串流上建立的。 若要變更的媒體櫃名稱，請查看[媒體櫃重新命名](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8)。 您可以按一下**自訂]**到媒體櫃的共用資料夾來自訂。|  
  
 如需詳細資訊，請查看[允許限制存取伺服器上的媒體櫃或](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)和[分享複製保護的媒體](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1.5)。  
  
##  <a name="BKMK_3"></a>串流媒體的運作方式  
 功能在 Windows Server Essentials 的串流媒體讓它可能網路中電腦及某些網路數位媒體裝置播放數位媒體檔案儲存在伺服器上的。  
  
 當您在 [將媒體伺服器時，您共用的媒體櫃中的 content 將是可從您的伺服器接收串流媒體網路上的裝置上播放。 您可以串流大部分的數位媒體檔案的類型。 一些常見的檔案，您可以串流類型包括︰  
  
-   Windows Media 格式 (.asf、.wma、.wmv、.wm)  
  
-   聲音的視覺交錯 (.avi)  
  
-   移動相片專家群組 (.mpeg、.mpg、.mp3)  
  
-   適用於 Windows (.wav) 音訊  
  
-   CD 音訊曲目 (.cda)  
  
 播放檔案，只要找出歌曲、影片或共用資料夾中的圖片按兩下檔案，且 content 將會從伺服器串流到您的電腦並播放。 了解如何找出並播放數位媒體檔案儲存在伺服器上的資訊，請查看[播放數位媒體](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)。  
  
 若要串流媒體，您會需要下列硬體：  
  
-   有線或 wireless 私人網路  
  
-   您的網路或稱為（有時稱為網路的數位媒體播放）數位媒體收件者的裝置上的任一另一部電腦。 數位媒體接收器是硬體裝置連接到您可以使用您的電腦來控制您有線或 wireless 網路？即使您的電腦處於另一個俱樂部。  
  
 如需詳細資訊，請查看[將媒體串流關閉](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)。  
  
##  <a name="BKMK_4"></a>將媒體串流關閉  
 您可以由串流至任何支援的數位媒體的接收器 (DMR) 的檔案共用音樂、影片與相片來自 Windows Server Essentials，例如電腦、行動裝置版的手機、電視、數位媒體接收器、延伸的 Windows Media Center（包括 Xbox 360），以及其他的個人電子裝置。  
  
 適用於目前與 Windows Server Essentials 相容的數位媒體裝置的清單，請查看[Windows 相容性中心](https://www.microsoft.com/windows/compatibility/CompatCenter/Home)。  
  
### <a name="enabling-media-sharing"></a>讓共用媒體  
 若要共用的媒體，會儲存在 Windows Server Essentials，您需要將媒體串流。 串流媒體是預設關閉。  
  
####  <a name="BKMK_2.5"></a>若要將媒體串流關閉  
  
1.  打開 Windows Server Essentials 儀表板。  
  
2.  按一下**設定**，按一下 [**媒體**，然後執行下列其中一個動作：  
  
    -   按一下**將**若要分享的所有檔案儲存在伺服器的媒體櫃中的 [開始都]。  
  
    -   按一下**關閉**若要停止分享的所有檔案是儲存在伺服器的媒體櫃。  
  
3.  如果您想要共用的媒體櫃中的其他資料夾，按一下 [**自訂**，然後選取 [**是**的每個您想要在 [媒體櫃中所包含的共用資料夾。  
  
4.  按一下**[確定]**儲存變更。  
  
 適用於數位媒體類型 Windows Media Player 支援的相關資訊，請查看[的檔案類型的 Windows Media player 支援的](https://support.microsoft.com/kb/316992)。  
  
 如需詳細資訊，請查看[允許限制存取伺服器上的媒體櫃或](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)。  
  
##  <a name="BKMK_5"></a>數位媒體檔案新增至伺服器  
 伺服器管理員可以將數位媒體新增到媒體櫃中的共用資料夾存取伺服器直接，或使用遠端網站存取的網站來登入儀表板。 其他使用者可以將媒體檔案新增至伺服器使用**共用資料夾**連接 Launchpad 使用遠端網站存取的網站，或使用我的伺服器應用程式適用於 Windows Phone 上。 如需有關播放媒體資訊，請查看[播放數位媒體](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)。  
  
> [!NOTE]
>  您也可以使用我的伺服器應用程式適用於 Windows Phone 伺服器上載媒體檔案。 您可以下載我伺服器 app，從[Windows Phone 市集中](http://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a)。 如需有關我伺服器適用於 Windows phone app 中，查看部落格文章[適用於 Windows Server Essentials 伺服器我的手機應用程式](http://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx)。  
  
#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>若要新增數位媒體檔案伺服器上的共用資料夾  
  
1.  登入伺服器使用其中一項下列方法：  
  
    1.  用於登入遠端網站存取的相關資訊，請查看[登入遠端 Web 存取](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)。  
  
    2.  用於登入 Launchpad 的相關資訊，請查看[Launchpad 概觀](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md)。  
  
2.  搜尋，然後按一下您要新增的媒體的資料夾。  
  
3.  複製和貼上或拖放您想要媒體檔案新增到適當伺服器上的共用資料夾。  
  
##  <a name="BKMK_6"></a>允許或限制的伺服器上的媒體櫃的存取  
  
-   當您要共用的媒體時，它會建立四預先定義的資料夾：音樂、圖片、影片與錄製電視。 這些資料夾中的任何 preexists 伺服器上，如果現有的資料夾會重複使用為共用資料夾設為共用的媒體。 會保留所有現有的資料夾的媒體 content 和使用者的權限，以及他們所有的網路使用者的共用。  
  
-   您將在共用的媒體櫃的共用資料夾之前，您應該會知道共用的媒體櫃略過任何類型的使用者帳號存取您的共用資料夾中的設定。 讓我們例如，您將媒體的共用的媒體櫃**相片**共用資料夾，並設定**相片**共用的資料夾**無存取**使用者 account 名陳。 陳仍然可以串流任何數位媒體的**影片**任何 player 支援的數位媒體或 DMR 共用的資料夾。 如果您不想在這種方式串流數位媒體，請將檔案儲存在未配備媒體亮起共用媒體櫃的資料夾。  
  
-   如果您要共用的媒體櫃上的共用資料夾，任何 player 支援的數位媒體或 DMR 可以存取您的 Windows Server Essentials 網路，也可以存取您的數位媒體的共用資料夾中。 例如，如果您有 wireless 網路，您有未受保護 wireless 網路的範圍中的任何人可能可以存取數位媒體的資料夾中。 您將在共用的媒體櫃之前，請確定您的安全 wireless 網路。 如需詳細資訊，wireless 存取點的文件。  
  
##  <a name="BKMK_8"></a>重新命名媒體櫃  
 媒體櫃的預設名稱是**數位媒體伺服器**。 當您將媒體串流 Windows Server Essentials 中建立它。 如需有關關閉串流媒體，請[將媒體串流關閉](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.5)。 您可以隨時修改媒體櫃名稱伺服器儀表板。  
  
#### <a name="to-rename-the-media-library"></a>若要重新命名媒體櫃  
  
1.  打開伺服器儀表板。 若要從 Launchpad 打開儀表板，按一下 [**儀表板**，輸入伺服器的密碼，然後按一下 [登入的箭頭。  
  
2.  在伺服器儀表板上，按一下 [**設定**。  
  
3.  在**設定**頁面上，按**媒體**索引標籤。  
  
4.  在**的媒體櫃**區段**媒體設定**頁面上，按一下您的媒體櫃的名稱。  
  
5.  在**媒體櫃名稱變更**對話方塊中，輸入新的媒體櫃、的名稱，然後按一下**[確定]**。  
  
##  <a name="BKMK_9"></a>停止分享數位媒體  
 伺服器管理員，可以停止分享儲存在執行 Windows Server Essentials 的伺服器上的共用資料夾中的數位媒體。  
  
#### <a name="to-stop-sharing-media-in-shared-folders"></a>若要停止分享共用資料夾中的媒體  
  
1.  打開伺服器儀表板。  
  
2.  儀表板上**Home**頁面上，按一下 [**設定**，按一下 [**設定媒體伺服器**，，然後按一下**按一下媒體伺服器設定**。  
  
3.  在**媒體**設定頁面上，您可以選擇其中一項下列選項：  
  
    -   按一下**關閉**若要停止分享的所有檔案是儲存在伺服器上。  
  
    -   按一下**自訂**，然後選取 [**無**您想要停止分享特定資料夾。  
  
4.  按一下**套用]**或**[確定]**以儲存您的變更。  
  
##  <a name="BKMK_10"></a>讓使用伺服器訊息區 (SMB) 通訊協定存取伺服器上的檔案共用的媒體裝置  
 使用網路檔案和分享存取權 DLNA 而（的串流媒體）伺服器訊息區 (SMB) 的裝置需要啟動來賓。 這可讓任何裝置或使用者檢視的共用資料夾驗證不到網路上。  
  
> [!CAUTION]
>  當您讓「來賓時的任何人都可以存取伺服器上的共用的資源預設。  
  
##  <a name="BKMK_CommonProcessors"></a>常見的處理器和他們支援的影片設定檔  
 將媒體串流從您的 Windows Server Essentials 伺服器，您可以使用的電腦執行 Windows 7 或 Windows 8 作業系統或其他網路的裝置（例如數位媒體玩家）或 Media Center 延伸（例如 Xbox 360 等）。 當您離開您的網路，請使用遠端網站存取的 Media Player 播放檔案是儲存在您的伺服器。  
  
 您需要 200 KBps 之間 10 MBps 資料傳輸速率。 您必須使用您的電腦和裝置，可以辨識和播放媒體格式。 並非所有裝置都支援媒體格式相同，因此必須是您的電腦和裝置的方式播放您媒體檔案。  
  
  Windows Server Essentials 包含轉碼支援判斷的電腦或裝置的功能，而且動態將不支援的影片檔案轉換到支援的作業系統。 一般而言，Windows Media Player 12 可以播放 content 執行 Windows 7 或 Windows 8 的電腦上，如果在伺服器上 content 播放連上網路的裝置上。  
  
 選擇轉碼格式和位元速率是非常依賴伺服器處理器的效能。 處理器效能被視為 Windows 體驗指數」的一部分。 若要判斷您的伺服器效能分數，執行下列其中一個動作：  
  
-   網路在電腦上執行的是 Windows 7 或 Windows 8 的相同處理器為您的伺服器，請移至**[控制台]**，按一下 [**效能資訊及工具]**，然後檢視並**速率與改善您的電腦的效能**頁面。  
  
-   請連絡製造商的處理器。  
  
 最佳的使用者體驗，選擇串流解析度品質適用於您的伺服器處理器的影片。 伺服器會自動調整這些設定其中的位元速率：  
  
-   **低**如果小於 3.6 處理器分數。  
  
-   **媒體**如果超過 3.6 與小於 4.2 處理器分數。  
  
-   **高**如果超過 4.2 和小於 6.0 處理器分數。  
  
-   **最佳**如果大於 6.0 處理器分數。  
  
 如果您選擇串流解析度需要更多處理能力的超過一段影片，您可能會遇到緩衝區和串流媒體伺服器時就會停止。  
  
> [!NOTE]
>  串流處理高解析度視訊透過遠端網路存取，您需要至少 6.0 的分數處理器。  
  
##  <a name="BKMK_KnownIssues"></a>已知的問題類型的媒體檔案  
 在遠端 Web 存取的媒體串流功能使用 Windows Media Player 12 網路共用服務。 遠端存取網頁媒體串流支援音訊、視訊和支援的 Windows Media Player 12 及 Silverlight 4 圖片檔案類型。  
  
 下表列出遠端網站存取的媒體串流所支援的檔案類型（格式）。 如果有媒體檔案伺服器上的表格中不包含類型，您無法透過遠端網站存取的媒體串流串流它們。  
  
|檔案類型|副檔名|  
|---------------|-------------------------|  
|3GPP 檔案|.3gp、.3gpp、.3g2 和.3gp2|  
|音訊資料傳輸串流 (ADTS) 音訊檔|.adts 和.adt|  
|AU 音訊檔案|.au 和.snd|  
|音訊交換檔案格式 (AIFF) 音訊檔|.aif、.aifc 和.aiff|  
|AVCHD 檔案|.m2ts、.m2t 和.mts|  
|CD 音訊磁碟|.cda|  
|Dvd 磁碟|.vob|  
|JPEG 圖片檔|.jpg|  
|MP3 音訊檔|.mp3 和.m3u|  
|MPEG 視訊檔|.mpeg、.mpg、.m1v、.mpa、.mpe、.mp4、.mp4v、.m4v、.m4a 和.mov|  
|Windows 音訊和視訊檔案|.avi 和.wav|  
|Windows Media 音訊和視訊檔案|.asf、.asx、.wax、.wm、.wma、.wmd、.wmp、.wmv、.wmx、.wpl 和.wvx|  
  
> [!NOTE]
>  如果您無法使用此表格所列的檔案類型，檔案可能也會使用 Windows Media player 不支援的轉碼器編碼。  
  
 如需支援的檔案格式的詳細資訊，請查看[檔案類型的 Windows Media player 支援的](https://go.microsoft.com/fwlink/p/?LinkID=196118)和[支援媒體格式、通訊協定，以及登入之欄位](https://go.microsoft.com/fwlink/p/?LinkId=203339)適用於 Silverlight。  
  
## <a name="see-also"></a>也了  
  
-   [播放數位媒體](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)
