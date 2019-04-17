---
title: 遠端桌面的用戶端應用程式的比較
description: 了解不同的 RD 應用程式時，它可以支援的功能和函式的比較。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/22/2018
ms.localizationpriority: medium
ms.openlocfilehash: fc20d1a2c51abddb8ae014efc621f4f0b36c3677
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297407"
---
# 比較用戶端應用程式

>適用於： Windows 10、 Windows 8.1、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2

我們通常詢問不同的遠端桌面用戶端應用程式如何相比較。 它們都執行相同的動作嗎？ 以下是這些問題的答案。

## 重新導向支援

下列表格比較裝置和其他遠端桌面連線的應用程式、 通用應用程式、 Android 應用程式、 iOS 應用程式、 macOS 應用程式和 web 用戶端上的重新導向的支援。 這些表格涵蓋一次是在遠端工作階段，您可以存取的重新導向。 

如果您遠端到您的個人桌面，有額外的重新導向，您可以設定**其他設定**工作階段。 如果您的遠端桌面或應用程式由您的組織所管理，您的系統管理員可以啟用或停用重新導向，透過群組原則設定。

### 輸入重新導向

| 重新導向 | 遠端桌面<br> [連線] | 通用 | Android | iOS | macOS | web 用戶端 |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| 鍵盤    | X                             | X         | X       | X   | X     | X          |
| 滑鼠       | X                             | X         | X       | X *    | X     | X          |
| 觸控       | X                             | X         | X       | X   |       |            |
| Other       | 手寫筆                           |           |         |     |       |            |
* 檢視[針對遠端桌面 iOS 搶鮮版用戶端支援的輸入裝置的清單](remote-desktop-ios.md#supported-input-devices)。

### 連接埠重新導向   

| 重新導向 | 遠端桌面 <br>[連線] | 通用 | Android | iOS | macOS | Web 用戶端 |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| 序列連接埠 | X                             |           |         |     |       |            |
| USB         | X                             |           |         |     |       |            |

當您啟用 USB 連接埠重新導向時，任何附加至 USB 連接埠的 USB 裝置會自動辨識遠端工作階段中。

### 其他重新導向 （裝置等等）



| 重新導向         | 遠端桌面連線 | 通用   | Android | iOS         | macOS                                    | Web 用戶端    |
|---------------------|---------------------------|-------------|---------|-------------|------------------------------------------|---------------|
| 相機             | X                         |             |         |             |                                          |               |
| 剪貼簿           | X                         | 文字、 影像 | 文字    | 文字、 影像 | X                                        | 文字          |
| 本機磁碟機/存放區 | X                         |             | X       |             | x                                        |               |
| 位置            | X                         |             |         |             |                                          |               |
| 麥克風         | X                         |X            |         |             | X                                        |               |
| 印表機            | X                         |             |         |             | X （僅附近）                            | PDF 列印     |
| 掃描器            | X                         |             |         |             |                                          |               |
| 智慧卡         | X                         |             |         |             | X （不支援 Windows 驗證） |               |
| 喇叭            | X                         | X           | X       | X           | X                                        | X （除了 IE) |

* 針對印表機重新導向-將 macOS 應用程式的發行者網印表機驅動程式預設支援。 它們不支援重新導向原生印表機驅動程式。

### 支援的 RDP 設定
下表包含可用於 Windows 和 HTML 的用戶端的支援 RDP 檔案設定的清單。 為 「 x 」 中的平台欄會指出支援該設定。 請注意這不是完整的清單的支援適用於 Windows 與 HTML5 的用戶端的設定。 我們會持續更新此表格將包含更多支援的 RDP 設定適用於 Windows 與 HTML5 的用戶端，以及 macOS、 iOS 及 Android 用戶端。

| RDP 設定                        | 描述            | 值                 | 預設值          | Windows 虛擬桌面 | Windows | HTML5   |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|:-------:|:-------:|
| 替代的完整地址： s:value | 指定替代名稱或遠端電腦的 IP 位址。 | 任何有效的名稱或 IP 位址的遠端電腦，例如，「 10.10.15.15 」 | | x | x | x |
| 替代的殼層： s:value        | 決定是否程式自動啟動時您使用 RDP 連線。 此設定會對應至程式路徑和檔案名稱] 方塊程式] 索引標籤上的遠端桌面連線用戶端。 若要指定替代的殼層，輸入的值，例如 「 」 C:\ProgramFiles\Office\word.exe 」 」 的可執行檔是有效的路徑。 這項設定也會決定的路徑或如果已啟用 RemoteApplicationMode 在連線時啟動的遠端應用程式的別名。 | 例如，「 C:\ProgramFiles\Office\word.exe 」 || x | x | x |
| audiocapturemode:i:value | 指出是否已啟用音訊輸入/輸出重新導向。 此設定會對應至遠端桌面連線用戶端的遠端音訊方塊。 | 從本機裝置; (0) 停用音訊擷取（1） 啟用音訊擷取從本機裝置及重新導向至遠端工作階段中音訊應用程式 | 0 | x | x | |
| audiomode:i:value | 決定哪些電腦 （也就是本機或遠端） 會播放音訊。 | （在此電腦上播放）; 在本機電腦上 (0) 的音效（1） （播放遠端電腦上;） 的遠端電腦上播放音效（2） 不播放的聲音 （無法播放） | 0 | x | x | x |
| 驗證層級： i:value | 定義伺服器驗證層級的設定。 | (0) 如果伺服器驗證失敗，連接到電腦沒有警告 (Connect 且不要警告我);（1） 如果伺服器驗證失敗，不會建立連線 （請勿連接）;（2） 如果伺服器驗證失敗，顯示警告，並允許我連線或拒絕連線 （我的警告）;（指定 3） 不會驗證要求。 | 3 | x | x ||
| 啟用 autoreconnection: i:value | 此設定會決定是否在用戶端電腦會自動嘗試重新連線至遠端電腦連線中斷; 如果例如，沒有網路連線中斷時。 如果連線是捨棄的選項中遠端桌面連線 (RDC) 下的 [體驗] 索引標籤上的核取方塊，此設定會對應到重新連線。| (0) 的用戶端電腦就不會自動嘗試重新連線;（1） 用戶端電腦會自動嘗試重新連線| 1 | x | x | x |
| bandwidthautodetect:i:value | 決定是否要啟用自動網路偵測類型 | (0) 停用自動網路偵測類型;（1） 啟用自動網路類型偵測 | 1 | x | x | x |
| 壓縮： i:value | 此設定會決定由 RDP 傳輸到本機電腦時，是否已啟用大量壓縮。|(0) 停用 RDP 大量壓縮;（1） 啟用 RDP 大量壓縮 | 1 | x | x | x |
| 識別碼： i:value 桌面大小 | 指定一組預先定義的選項從遠端工作階段桌面的維度。 如果 desktopheight 或 desktopwidth 已指定，會覆寫此設定。| (0) 640 x 480;（1) 800 x 600;（2) 1024 x 768;（3) 1280 x 1024;（4) 1600 x 1200 | 0 | x | x | x |
| desktopheight:i:value | 當您使用遠端桌面連線 (RDC) 連線時，此設定會決定解析度高度 （以像素） 遠端電腦上。 此設定會對應到上中 RDC theDisplaytab underOptions theDisplay configurationslider 中選取項目。 | 數值 200 到 2048 | 預設值設定為在本機電腦上的解析度 | x | x | x |
| desktopwidth:i:value | 當您使用遠端桌面連線 (RDC) 連線時，此設定會決定解析度 （像素寬度） 遠端電腦上。 此設定會對應到上中 RDC theDisplaytab underOptions theDisplay configurationslider 中選取項目。 | 數值 200 到 4096 | 預設值設定為在本機電腦上的解析度 | x | x | x |
| disableclipboardredirection:i:value | 此設定可決定連線至遠端電腦時，是否已啟用剪貼簿重新導向。 | 已啟用 (0) 剪貼簿重新導向;（未啟用 1） 剪貼簿重新導向 | x | x | x |
| disableconnectionsharing:i:value | 決定是否遠端桌面用戶端重新連線至任何現有的開啟連線，或啟動 RemoteApp 或桌面時，初始化新的連線 | (0) 重新連線至任何現有的工作階段;（1） 初始化新的連線 | 0 | x | x | x |
| disableprinterredirection:i:value | 此設定可決定連線至遠端電腦時，是否已啟用輕鬆列印印表機重新導向。 | 已啟用 (0) 輕鬆列印印表機重新導向;（已停用 1） 輕鬆列印印表機重新導向 | x | x | x |
| 網域： s:value | 此設定指定使用者帳戶將用來登入遠端電腦使用遠端桌面連線 (RDC) 所在的網域的名稱。 這項設定，以及使用者名稱] 設定值的值會出現在 theUser nametext theGeneraltab underOptionsin RDC] 方塊中。 | 有效的網域名稱，例如，"CONTOSO" | 沒有預設值 | x | x | x |
| drivestoredirect:s:value | 決定重新導向和遠端工作階段中可用，將會在用戶端電腦上的本機磁碟機。 | 未指定-值不重新導向任何磁碟機;*-將所有的磁碟機，包括更新版本; 已連接的磁碟機重新導向DynamicDrives-重新導向的任何更新版本; 已連接的磁碟機磁碟機和標籤的一或多個磁碟機-重新導向的指定的磁碟機。| 未指定-值不重新導向任何磁碟機 | x | x    | |
| enablecredsspsupport:i:value | 此設定可決定 RDP 是否將使用認證的安全性支援提供者 (CredSSP) 進行驗證是否可用。 | (0) RDP 不會使用 CredSSP，即使您的作業系統支援 CredSSP;（如果作業系統支援 CredSSP RDP 1) 將會使用 CredSSP | 1 | x | x | |
| 完整的地址： s:value | 此設定會指定的名稱或您想要連線到遠端電腦的 IP 位址 | 有效的電腦名稱、 IPv4 位址或 IPv6 位址-指定您要連線的遠端電腦。 | | x | x | x |
| gatewaycredentialssource:i:value | 指定或擷取的 RD 閘道驗證方法。 | (0) 詢問密碼 (NTLM);（1） 使用智慧卡;（4） 可讓使用者稍後選取 | 0 | x | x | x |
| gatewayhostname:s:value | 指定的 RD 閘道主機名稱。 | 有效的閘道伺服器位址。 ||x|x|x|
| gatewayprofileusagemethod:i:value | 指定是否要使用 RD 閘道的預設設定 | (0) 使用預設設定檔的模式，所指定的系統管理員;（1） 使用明確設定，所指定的使用者。 | 0 | x | x | x |
| gatewayusagemethod:i:value | 指定使用 RD 閘道伺服器的時機 | (0) 不使用 RD 閘道伺服器;（1） 一律使用 RD 閘道伺服器;（2） 使用 RD 閘道伺服器，如果無法進行直接連線到 RD 工作階段主機;（使用預設的 RD 閘道伺服器設定; 3）（4） 不要使用 RD 閘道，略過本機位址; 伺服器設定此屬性值以 0 或 4 都是有效地對等，但此屬性設定為 4 啟用的選項來略過本機位址。 | | x | x | x |
| networkautodetect:i:value | 決定是否要或不使用自動的網路頻寬偵測。 需要設定 optionbandwidthautodetectto 和 withconnection type7 相互關聯。 | (0) 不會使用自動的網路頻寬偵測;（1） 使用自動網路頻寬偵測 | 1 | x ||x|
| promptcredentialonce:i:value | 決定是否儲存與用於 「 RD 閘道和遠端電腦使用者的認證。|(0) 的遠端工作階段將不會使用相同的認證。（1） 遠端工作階段將會使用相同的認證|1|x|x||
| redirectclipboard:i:value | 此設定可決定是否在本機電腦上的剪貼簿會重新導向和遠端工作階段中可用。 此設定會對應到 theClipboardcheck 接到 Resourcestab underOptionsin RDC] 方塊。 | (0) 剪貼簿在本機電腦上的沒有可用的遠端工作階段;（1） 在本機電腦上的剪貼簿中有提供遠端工作階段|1|x|x|x|
| redirectdrives:i:value | 此設定可決定是否在用戶端電腦上的磁碟機會重新導向和遠端工作階段中可用。 此設定會對應到選取項目 forDrivesunderMoreon 接到 Resourcestab underOptionsin RDC。|(0)，在本機電腦上的磁碟機未提供在遠端工作階段中;（1），在本機電腦上的磁碟機是遠端工作階段中可用|0|x|x| |
| redirectprinters:i:value | 此設定可決定是否在用戶端電腦上設定的印表機將會重新導向，並可用於當您連線至遠端電腦使用遠端桌面連線 (RDC) 遠端工作階段。 此設定會對應到 thePrinterscheck 接到 Resourcestab underOptionsin RDC] 方塊中選取項目。 | (0) 在本機電腦上的印表機未提供在遠端工作階段中;（在本機電腦上 1） 的印表機可在遠端工作階段|1|x|x|x|
| redirectsmartcards:i:value | 此設定可決定是否在用戶端電腦上的智慧卡裝置會重新導向，並可用於當您連線至遠端電腦使用遠端桌面連線 (RDC) 遠端工作階段。 此設定會對應到選取項目在 theSmart cardscheck 方塊上接到 Resourcestab underOptionsin RDC underMore。|(0) 在本機電腦上的智慧卡裝置沒有可用的遠端工作階段中;（在本機電腦上 1） 的智慧卡裝置是在遠端工作階段中可用|1|x|x||
| remoteapplicationcmdline:s:value | RemoteApp 選用的命令列參數。||x|x|x|
| remoteapplicationexpandcmdline:i:value| 判斷在本機或遠端電腦時，是否展開 RemoteApp 命令列參數中所包含的環境變數。|(0) 的環境變數應展開本機電腦; 的值（1） 環境變數應展開遠端電腦的遠端電腦的值||x|x|x|
| remoteapplicationexpandworkingdir | 判斷在本機或遠端電腦時，是否展開 RemoteApp 工作目錄參數中所包含的環境變數。 | (0) 的環境變數應展開本機電腦; 的值（1） 環境變數應展開遠端電腦的遠端電腦的值。 請注意： 透過殼層工作目錄參數指定 RemoteApp 工作目錄。||x|x|x|
|remoteapplicationfile:s:value | 指定要由 RemoteApp 遠端電腦上開啟的檔案。 注意： 對於要開啟本機檔案，您也必須啟用為來源磁碟機的磁碟機重新導向。||x|x|x|
|remoteapplicationicon:s:value | 指定要時啟動 RemoteApp 用戶端 UI 中顯示的圖示檔案。 如果未不指定任何檔案名稱，用戶端會使用標準的遠端桌面圖示。 支援僅 」.ico 」 檔案。||x|x|x|
|remoteapplicationmode:i:value | 決定是否要作為 RemoteApp 工作階段啟動 RemoteApp 連線。| (0) 不會啟動 RemoteApp 工作階段中;（1） 啟動 RemoteApp 工作階段|1|x|x|x|
|remoteapplicationname:s:value | 啟動 RemoteApp 時，在用戶端介面指定 RemoteApp 的名稱。| 例如，「 Excel 2016 」|x|x|x|
|remoteapplicationprogram:s:value | 指定 RemoteApp 別名或可執行檔名稱。 | 例如，「 EXCEL 」 |x|x|x|
|螢幕模式識別碼： i:value | 這項設定會決定是否遠端工作階段視窗顯示全螢幕，當您使用遠端桌面連線 (RDC) 連線至遠端電腦。 此設定會對應到上 theDisplaytab underOptionsin RDC theDisplay configurationslider 中選取項目。|（1） 遠端工作階段會出現在視窗中;（2） 遠端工作階段會顯示全螢幕|2|x|x|x|
|智慧的大小調整： i:value | 此設定可決定用戶端電腦可以調整以符合用戶端電腦的視窗大小的遠端電腦上的內容。|(0) 的用戶端視窗顯示將不時重新調整大小; 縮放（將縮放 1） 的用戶端視窗顯示，當重新調整大小|0|x|x||
| 使用 multimon:i:value | 當您使用遠端桌面連線 (RDC) 連線至遠端電腦時，此設定會設定多個監視器的支援。|(0) 不會啟用多重監視器支援;（1） 啟用多個監視器支援|0|x|x||
| username:s:value | 此設定會指定將用來登入遠端電腦使用遠端桌面連線 (RDC) 的使用者帳戶的名稱。 這項設定，以及將網域設定的值的值會出現在 theGeneraltab underOptionsin RDC 上 theUser 名稱] 方塊。| 任何有效的使用者名稱。 ||x|x|x|
| videoplaybackmode:i:value| 此設定可決定是否遠端桌面連線 (RDC) 將會使用 RDP 有效率多媒體的串流視訊播放。|(0) 不使用 RDP 有效率多媒體的串流視訊播放;（1） 使用 RDP 有效率多媒體的串流時可能的視訊播放|1|x|x||
| workspaceid:s:value | 此設定會定義 RemoteApp 和桌面識別碼相關聯的 RDP 檔案，其中包含這項設定。 | 有效的 RemoteApp 和桌面連線識別碼|x|x||