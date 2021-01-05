---
title: 管理 Windows Server Essentials 中的數位媒體
description: 瞭解您伺服器的媒體串流處理功能，並瞭解如何在網路上設定和使用媒體串流處理。
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 9378bffa-487c-43ca-9ec3-7e7864d2dd9a
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 59149ddfdde440d4b5d5ce722e054a858c5e5a6b
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97811385"
---
# <a name="manage-digital-media-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的數位媒體

>適用于： Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

下列主題討論您伺服器的媒體串流處理功能，並說明如何在您的網路上設定及使用媒體串流處理。

> [!NOTE]
>  依預設，媒體串流功能無法在已安裝 Windows Server Essentials 體驗角色的 Windows Server Essentials 和 Windows Server 2012 R2 中使用。 若要在這些版本中加入媒體串流處理功能，請從 Microsoft 下載中心 [下載並安裝 Windows Server Essentials 媒體套件](https://www.microsoft.com/download/details.aspx?id=40837) 。

-   [數位媒體概觀](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1)

-   [使用儀表板來管理媒體伺服器](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2)

-   [媒體串流處理如何運作](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_3)

-   [開啟或關閉媒體串流處理](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)

-   [將數位媒體檔案新增到伺服器](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_5)

-   [允許或限制存取伺服器上的媒體櫃](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)

-   [重新命名媒體櫃](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8)

-   [停止共用數位媒體](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_9)

-   [讓使用伺服器訊息區 (SMB) 通訊協定的媒體裝置能夠存取伺服器上的共用檔案](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_10)

-   [常見的處理器以及它們所支援的視訊設定檔](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_CommonProcessors)

-   [媒體檔案類型的已知問題](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_KnownIssues)

##  <a name="digital-media-overview"></a><a name="BKMK_1"></a> 數位媒體總覽
 數位媒體係指已編碼 (數位壓縮) 的音訊、視訊及相片內容。 將內容編碼涉及將音訊和視訊輸入轉換成數位媒體檔案 (例如 Windows 媒體檔案)。 數位媒體在經過編碼之後，就可以輕鬆地被操作、散佈及由電腦播放，並且可以輕鬆地透過電腦網路來傳輸。

 數位媒體類型的範例包括：Windows Media 音訊 (WMA)、Windows Media 視訊 (WMV)、MP3、JPEG 及 AVI。 如需 Windows Media Player 所支援之數位媒體類型的相關資訊，請參閱 [Windows Media Player 所支援的檔案類型](https://support.microsoft.com/kb/316992)。

### <a name="why-would-i-want-to-stream-my-digital-media"></a>我為什麼要對數位媒體進行串流處理？
 許多人都將音樂、影片和圖片儲存在 Windows Server Essentials 的共用資料夾中。 有時您可能會想要執行下列作業：

-   **觀賞視訊**。 您的伺服器可用來儲存大量的視訊和錄製的電視節目，並透過串流處理將其傳送到您的電腦或您網路上的其他播放裝置。 您可以使用 Windows Media Player，將視訊透過串流處理傳送到 Xbox 360 或電腦。

-   **播放音樂**。 當您開啟 [音樂] 共用資料夾的「媒體共用」時，便可以從支援 Windows Media Connect 的裝置存取您的音樂。 在開啟共用之後，您不需要啟用或設定任何使用者帳戶，即可以從 [音樂] 共用資料夾進行串流處理。

-   **顯示相片投影片放映**。 您可以將數位相片儲存在您伺服器上的 [相片]  共用資料夾中，然後從任何電腦或從連接到您住家或辦公室中電視的 Xbox 360 存取這些相片。 您可以觀賞相片投影片放映，就像將電視變成大型相框一樣。

###  <a name="sharing-copy-protected-media"></a><a name="BKMK_1.5"></a> 共用禁止複製的媒體
  Windows Server Essentials 不支援共用禁止複製的媒體。 這包括透過線上音樂商店購買的音樂。

 禁止複製的媒體只能在您用來購買它的電腦或裝置上播放。 禁止複製會防止您在多部電腦或裝置上播放媒體，即使您將媒體複製到您的伺服器並從該處播放它也不行。 不過，您可以將禁止複製的媒體儲存在 Windows Server Essentials 上，然後繼續在您用來購買它的電腦或裝置上播放該媒體。

##  <a name="manage-media-server-using-the-dashboard"></a><a name="BKMK_2"></a> 使用儀表板管理媒體伺服器
  Windows Server Essentials 可讓您使用 Windows Server Essentials 儀表板執行一般系統管理工作。 儀表板之伺服器 [設定] 頁面的 [媒體] 索引標籤會提供下列資訊：

|區段|功能|
|-------------|-------------------|
|媒體伺服器|[開啟/關閉] 按鈕可讓您開啟或關閉媒體串流處理。|
|視訊串流處理品質|下拉式箭頭可讓您選擇從伺服器播放之視訊的視訊串流處理品質。|
|媒體櫃|顯示媒體櫃的名稱。 媒體櫃的預設名稱叫做 [數位媒體櫃]，這是在您開啟媒體串流處理時所建立。 若要變更媒體櫃名稱，請參閱[重新命名媒體櫃](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_8)。 您可以按一下 [自訂] 來自訂在媒體櫃中共用的資料夾。|

 如需詳細資訊，請參閱[允許或限制存取伺服器上的媒體櫃](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)和[共用禁止複製的媒體](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_1.5)。

##  <a name="how-media-streaming-works"></a><a name="BKMK_3"></a> 媒體串流的運作方式
 Windows Server Essentials 中的媒體串流處理功能可讓網路電腦和一些網路數位媒體裝置播放儲存在伺服器上的數位媒體檔案。

 當您開啟媒體伺服器時，您網路上的裝置只要能夠從您的伺服器接收串流處理媒體，便能夠播放您在媒體櫃中共用的內容。 您可以對大多數類型的數位媒體檔案進行串流處理。 您可以進行串流處理的較常見檔案類型包括：

- Windows Media 格式 (.asf、.wma、.wmv、.wm)

- 影音交錯 (.avi)

- 動畫專家群組 (mpeg、.mpg、.mp3)

- Windows 音訊 (.wav)

- CD 曲目 (.cda)

  若要播放檔案，只要在共用資料夾中找出一首歌曲、視訊或圖片，按兩下該檔案，就會透過串流處理將其內容從伺服器傳送到您的電腦並進行播放。 如需如何尋找及播放儲存在伺服器上之數位媒體檔案的相關資訊，請參閱 [播放數位媒體](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)。

  若要對您的媒體進行串流處理，您需要下列硬體：

- 有線或無線的私人網路

- 您網路上的另一部電腦，或是一個稱為數位媒體接收器 (有時稱為網路數位媒體播放機) 的裝置。 數位媒體接收器是連接到您有線或無線網路的硬體裝置，您可以使用電腦來控制這些裝置，即使您的電腦位於其他房間也一樣。

  如需詳細資訊，請參閱 [開啟或關閉媒體串流處理](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_4)。

##  <a name="turn-media-streaming-on-or-off"></a><a name="BKMK_4"></a> 開啟或關閉媒體串流
 您可以透過將檔案串流處理到任何支援的數位媒體接收器， (DMR) 例如電腦、行動電話、電視、數位媒體接收器、Windows Media Center 的擴充項 (包括 Xbox 360) 和其他個人電子裝置，從 Windows Server Essentials 共用音樂、影片和圖片。

 如需目前與 Windows Server Essentials 相容的數位媒體裝置清單，請參閱 [Windows 相容性中心](https://www.microsoft.com/windows/compatibility/CompatCenter/Home)。

### <a name="enabling-media-sharing"></a>啟用媒體共用
 若要共用儲存在 Windows Server Essentials 中的媒體，您必須開啟媒體串流處理。 媒體串流處理預設為關閉。

####  <a name="to-turn-media-streaming-on-or-off"></a><a name="BKMK_2.5"></a> 開啟或關閉媒體串流處理

1. 開啟 [Windows Server Essentials 儀表板]。

2. 按一下 [設定]，按一下 [媒體]，然後執行下列其中一個動作：

   -   按一下 [開啟] 以開始共用儲存在伺服器之 [媒體櫃] 中的所有檔案。

   -   按一下 [關閉] 以停止共用儲存在伺服器之 [媒體櫃] 中的所有檔案。

3. 如果您想要在 [媒體櫃] 中共用其他的資料夾，請按一下 [自訂]，然後針對您想要包含在 [媒體櫃] 中的每個共用資料夾選取 [是]。

4. 按一下 [確定]  以儲存變更。

   如需 Windows Media Player 所支援之數位媒體類型的相關資訊，請參閱 [Windows Media Player 所支援的檔案類型](https://support.microsoft.com/kb/316992)。

   如需詳細資訊，請參閱[允許或限制存取伺服器上的媒體櫃](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_6)。

##  <a name="add-digital-media-files-to-the-server"></a><a name="BKMK_5"></a> 將數位媒體檔案新增至伺服器
 伺服器管理員可以直接存取伺服器，或使用遠端 Web 存取網站來登入儀表板，藉以將數位媒體新增至媒體櫃中的共用資料夾。 其他使用者可以使用 [啟動列] 上的 [ **共用資料夾** ] 連線、使用遠端 Web 存取網站，或使用適用于 Windows Phone 的 My server 應用程式，將媒體檔案新增至伺服器。 如需播放媒體的相關資訊，請參閱 [播放數位媒體](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)。

> [!NOTE]
>  您也可以透過使用適用於 Windows Phone 的 My Server 應用程式，將媒體檔案上傳到伺服器。 您可以從 [Windows Phone 市集](https://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a)下載 My Server 應用程式。 如需適用于 Windows phone 的 My Server 應用程式的詳細資訊，請參閱 [Windows Server Essentials 的 my server phone 應用程式](/archive/blogs/sbs/my-server-phone-app-for-windows-server-2012-essentials)的 blog 文章。

#### <a name="to-add-digital-media-files-to-shared-folders-on-the-server"></a>將數位媒體檔案新增到伺服器上的共用資料夾

1.  請使用下列其中一種方法來登入伺服器：

    1.  如需有關登入「遠端 Web 存取」的資訊，請參閱[登入遠端 Web 存取](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)。

    2.  如需使用 [啟動列] 登入的相關資訊，請參閱啟動控制 [板總覽](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md)。

2.  搜尋並按一下您要新增媒體的資料夾。

3.  將您想要新增的媒體檔案複製並貼到或拖放到伺服器上適當的共用資料夾中。

##  <a name="allow-or-restrict-access-to-a-media-library-on-the-server"></a><a name="BKMK_6"></a> 允許或限制存取伺服器上的媒體櫃

-   當您開啟媒體共用時，它會建立四個預先定義的資料夾：[音樂]、[圖片]、[視訊] 及 [錄製的節目]。 如果這些資料夾中有任何資料夾已預先存在於伺服器上，就會重複使用現有的資料夾做為媒體共用的共用資料夾。 所有現有資料夾的媒體內容和使用者權限都會保留下來，並與所有網路使用者共用。

-   在您開啟共用資料夾的「媒體櫃共用」之前，您應該先了解「媒體櫃共用」會略過您為共用資料夾設定的任何一種使用者帳戶存取。 例如，假設您開啟 [相片] 共用資料夾的「媒體櫃共用」，而您已針對名為 Bobby 的使用者帳戶將 [相片] 共用資料夾設定為 [不允許存取]。 Bobby 便仍然可以透過串流處理，將任何數位媒體從 [視訊] 共用資料夾傳送到任何支援的數位媒體播放機或 DMR。 如果您有不想要以這種方式進行串流處理的數位媒體，請將那些檔案儲存在沒有開啟「媒體櫃共用」的資料夾中。

-   如果您開啟共用資料夾的「媒體櫃共用」，任何可存取您 Windows Server Essentials 網路的支援數位媒體播放機或 DMR 也都可以存取該共用資料夾中的數位媒體。 例如，如果您有無線網路並且未加以防護，則在您無線網路範圍內的任何人都可能得以存取您在該資料夾中的數位媒體。 在您開啟「媒體櫃共用」之前，請先確定為您的無線網路加上防護。 如需詳細資訊，請參閱您無線存取點的文件。

##  <a name="rename-the-media-library"></a><a name="BKMK_8"></a> 重新命名媒體櫃
 媒體櫃的預設名稱是 [數位媒體伺服器]。 當您在 Windows Server Essentials 中開啟媒體串流處理時，就會建立此檔案。 如需開啟媒體串流處理的詳細資訊，請參閱 [開啟或關閉媒體串流處理](Manage-Digital-Media-in-Windows-Server-Essentials.md#BKMK_2.5)。 您可以使用伺服器 [儀表板] 來隨時修改媒體櫃名稱。

#### <a name="to-rename-the-media-library"></a>重新命名媒體櫃

1.  開啟伺服器 [儀表板]。 若要從 [啟動列] 開啟 [儀表板]，請按一下 [儀表板]，輸入伺服器密碼，然後按一下箭號來進行登入。

2.  在伺服器 [儀表板] 上，按一下 [設定]。

3.  在 [設定] 頁面上，按一下 [媒體] 索引標籤。

4.  在 [媒體設定] 頁面的 [媒體櫃] 區段中，按一下您媒體櫃的名稱。

5.  在 [變更媒體櫃名稱] 對話方塊中，輸入媒體櫃的新名稱，然後按一下 [確定]。

##  <a name="stop-sharing-digital-media"></a><a name="BKMK_9"></a> 停止共用數位媒體
 伺服器系統管理員可以停止共用儲存在執行 Windows Server Essentials 之伺服器上共用資料夾中的數位媒體。

#### <a name="to-stop-sharing-media-in-shared-folders"></a>將共用資料夾中的媒體停止共用

1.  開啟伺服器 [儀表板]。

2.  在 [儀表板] 的 [首頁] 上，按一下 [設定]，按一下 [設定媒體伺服器]，然後按一下 [按一下以設定媒體伺服器]。

3.  在 [媒體] 設定頁面上，您可以選擇下列其中一個選項：

    -   按一下 [關閉] 以停止共用儲存在伺服器上的所有檔案。

    -   按一下 [自訂]，然後針對您想要停止共用的特定資料夾選取 [否] 。

4.  按一下 [套用] 或 [確定] 以儲存您的變更。

##  <a name="enable-media-devices-that-use-the-server-message-block-smb-protocol-to-access-shared-files-on-the-server"></a><a name="BKMK_10"></a> 啟用使用伺服器訊息區 (SMB) 通訊協定的媒體裝置，以存取伺服器上的共用檔案
 裝置如果是使用「伺服器訊息區」(SMB) 而不是 DLNA (適用於媒體串流處理) 來存取網路檔案和共用存取，就需要啟用來賓帳戶。 這可讓網路上的任何裝置或使用者不需驗證，即可檢視共用資料夾的內容。

> [!CAUTION]
>  當您啟用來賓帳戶時，預設是任何人都可以存取伺服器上的共用資源。

##  <a name="common-processors-and-the-video-profiles-they-support"></a><a name="BKMK_CommonProcessors"></a> 一般處理器和其支援的影片設定檔
 若要從您的 Windows Server Essentials 伺服器串流媒體，您可以使用執行 Windows 7 或 Windows 8 作業系統或其他網路裝置的電腦 (例如數位媒體播放機) 或 Media Center 擴充項 (，例如 Xbox 360) 。 當您不在您網路的近端時，可以使用「遠端 Web 存取媒體播放機」來播放儲存在您伺服器上的檔案。

 您的資料傳送速率必須介於 200 KBps 到 10 MBps。 您必須使用您的電腦和裝置可以辨識和播放的媒體格式。 並非所有裝置都支援相同的媒體格式，因此必須有一個可供您的電腦和裝置播放您所擁有之媒體檔案的方法。

  Windows Server Essentials 包含可判斷電腦或裝置之功能的轉碼支援，然後會以動態方式將不支援的影片檔案轉換成支援的檔案。 一般而言，如果 Windows Media Player 12 可以在執行 Windows 7 或 Windows 8 的電腦上播放內容，伺服器上的內容就可以在網路連線裝置上播放。

 選擇的轉碼格式和位元速率多半取決於伺服器處理器的效能。 處理器效能會被視為「Windows 體驗指數」的一部分。 若要判斷您伺服器的效能分數，請執行下列其中一個動作：

- 在執行 Windows 7 或 Windows 8 與伺服器具有相同處理器的網路電腦上，移至 **主控台**，按一下 [ **效能資訊及工具**]，然後查看 [ **費率及改進您的電腦效能** ] 頁面上的資訊。

- 連絡處理器製造商。

  為了享有最佳的使用者體驗，請選擇適合您伺服器處理器的視訊串流處理解析度品質。 伺服器會自動將位元速率調整成下列其中一個設定：

- **低** 如果處理器分數低於 3.6。

- **中** 如果處理器分數在 3.6 到 4.2 之間。

- **高** 如果處理器分數在 4.2 到 6.0 之間。

- **最佳** 如果處理器分數高於 6.0。

  如果您選擇的視訊串流處理解析度需要比您伺服器更強的處理能力來處理，當您從伺服器進行媒體串流處理時，就可能會發生緩衝處理和停滯的情況。

> [!NOTE]
>  若要透過「遠端 Web 存取」進行高畫質視訊串流處理，您必須要有分數至少為 6.0 的處理器。

##  <a name="known-issues-with-media-file-types"></a><a name="BKMK_KnownIssues"></a> 媒體檔案類型的已知問題
 「遠端 Web 存取」中的媒體串流處理功能使用了「Windows Media Player 12 網路共用服務」。 「遠端 Web 存取」媒體串流處理支援 Windows Media Player 12 和 Silverlight 4 所支援的音訊、視訊及圖片檔案類型。

 下表列出「遠端 Web 存取」媒體串流處理所支援的檔案類型 (格式)。 如果您伺服器上有這個表格未包含的媒體檔案類型，您將無法透過「遠端 Web 存取」媒體串流處理對它們進行串流處理。

|檔案類型|檔案名稱副檔名|
|---------------|-------------------------|
|3GPP 檔案|.3gp、.3gpp、.3g2 及 .3gp2|
|音訊資料傳輸串流 (ADTS) 音訊檔案|.adts 和 .adt|
|AU 音訊檔案|.au 和 .snd|
|音訊交換檔案格式 (AIFF) 音訊檔案|.aif、.aifc 及 .aiff|
|AVCHD 檔案|.m2ts、.m2t 及 .mts|
|CD 音訊磁碟|.cda|
|DVD 視訊磁碟|.vob|
|JPEG 圖片檔案|.jpg|
|MP3 音訊檔案|.mp3 和 .m3u|
|MPEG 視訊檔案|.mpeg、.mpg、.m1v、.mpa、.mpe、.mp4、.mp4v、.m4v、.m4a 及 .mov|
|Windows 音訊和視訊檔案|.avi 和 .wav|
|Windows Media 音訊和視訊檔案|.asf、.asx、.wax、.wm、.wma、.wmd、.wmp、.wmv、.wmx、.wpl 及 .wvx|

> [!NOTE]
>  如果您無法使用這個表格中所列的檔案類型，表示檔案也可能是以 Windows Media Player 不支援的轉碼器所編碼。

 如需受支援檔案格式的其他資訊，請參閱 [Windows Media Player 支援的檔案類型](https://go.microsoft.com/fwlink/p/?LinkID=196118) 和 Silverlight [支援的媒體格式、通訊協定及記錄欄位](https://go.microsoft.com/fwlink/p/?LinkId=203339) 。

## <a name="additional-references"></a>其他參考資料

-   [播放數位媒體](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)

-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)