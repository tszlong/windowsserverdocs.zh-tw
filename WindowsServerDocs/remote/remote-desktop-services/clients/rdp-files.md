---
title: 支援的遠端桌面 RDP 檔案設定
description: 了解遠端桌面的 RDP 檔案設定
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: btaintor
manager: dongill
ms.author: helohr
ms.date: 05/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 6a596bcd29fdb80f8b2b0fa86ee808755dd09f43
ms.sourcegitcommit: f0fcfee992b76f1ad5dad460d4557f06ee425083
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/11/2020
ms.locfileid: "77125090"
---
# <a name="supported-remote-desktop-rdp-file-settings"></a>支援的遠端桌面 RDP 檔案設定

下表包含支援的 RDP 檔案設定清單，您可以搭配 Windows 和 HTML 用戶端來使用這些設定。 平台欄中的 "x" 表示是支援的設定。 不過，此清單不是 Windows 和 HTML5 用戶端支援的設定完整清單。 我們將持續更新此表格，以包含 Windows 和 HTML5 用戶端，以及 macOS、iOS 和 Android 用戶端支援的更多 RDP 設定。

請參閱[此文件](https://go.microsoft.com/fwlink/?linkid=2098243&clcid=0x409)以了解如何使用 PowerShell 自訂主機集區的 RDP 屬性。

| RDP 設定                        | 說明            | 值                 | 預設值          | Windows 虛擬桌面 | Windows | HTML5   |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|:-------:|:-------:|
| alternate full address:s:value | 指定遠端電腦的替代名稱或 IP 位址。 | 遠端電腦的任何有效名稱或 IP 位址，例如 "10.10.15.15" | | x | x | x |
| alternate shell:s:value        | 決定當您使用 RDP 進行連線時是否自動啟動程式。 若要指定替代殼層，請輸入可執行檔的有效路徑作為值，例如 "C:\ProgramFiles\Office\word.exe"。 如果啟用 RemoteApplicationMode，此設定也會決定連線時要啟動的遠端應用程式路徑或別名。 | "C:\ProgramFiles\Office\word.exe" || x | x | x |
| audiocapturemode:i:value | 指出是否啟用音訊輸入/輸出重新導向。 | - 0：停用本機裝置的音訊擷取<br>- 1：啟用本機裝置的音訊擷取，並重新導向至遠端工作階段中的音訊應用程式 | 0 | x | x | |
| audiomode:i:value | 決定本機或遠端電腦是否播放音訊。 | - 0：在本機電腦上播放聲音 (在這部電腦上播放)<br>- 1：在遠端電腦上播放聲音 (在遠端電腦上播放)<br>- 2：不要播放聲音 (不要播放) | 0 | x | x | x |
| authentication level:i:value | 定義伺服器驗證等級設定。 | - 0：如果伺服器驗證失敗，則連線到電腦而不顯示警告 (連線且不要警告我)<br>- 1：如果伺服器驗證失敗，不要建立連線 (不要連線)<br>- 2：如果伺服器驗證失敗，則顯示警告並允許我連線或拒絕連線 (警告我)<br>- 3：未指定驗證需求。 | 3 | x | x ||
| autoreconnection enabled:i:value | 決定如果連線中斷 (例如當網路連線中斷時)，用戶端電腦是否會自動嘗試重新連線到遠端電腦。 | - 0：用戶端電腦不會自動嘗試重新連線<br>- 1：用戶端電腦會自動嘗試重新連線| 1 | x | x | x |
| bandwidthautodetect:i:value | 決定是否啟用自動網路類型偵測 | - 0：停用自動網路類型偵測<br>- 1：啟用自動網路類型偵測 | 1 | x | x | x |
| camerastoredirect:s:value | 設定要重新導向的相機。 此設定使用已啟用相機的 KSCATEGORY_VIDEO_CAMERA 介面清單 (以分號分隔) 進行重新導向。 | - * :重新導向所有相機<br> - 一個可以排除特定相機，方法是在符號連結字串前面加上 "-"，例如 camerastoredirect:s:\\?\usb#vid_0bda&pid_58b0&mi | | x | x | |
| compression:i:value | 決定當透過 RDP 傳輸到本機電腦時是否啟用大量壓縮。|- 0：停用 RDP 大量壓縮<br>- 1：啟用 RDP 大量壓縮 | 1 | x | x | x |
| desktop size id:i:value | 從一組預先定義的選項指定遠端工作階段桌面尺寸。 如果指定了 desktopheight 或 desktopwidth，則會覆寫此設定。| \- 0：640×480<br>- 1：800×600<br>- 2：1024×768<br>- 3：1280×1024<br>- 4：1600×1200 | 0 | x | x | x |
| desktopheight:i:value | 決定當您使用遠端桌面連線進行連線時，遠端桌面上的解析度高度 (以像素為單位)。 此設定相當於 RDC [選項] 下 [顯示] 索引標籤上 [顯示器設定] 滑桿中的選項。 | 介於 200 到 2048 之間的數值 | 預設值會設定為本機電腦上的解析度 | x | x | x |
| desktopwidth:i:value | 決定當您使用遠端桌面連線進行連線時，遠端桌面上的解析度寬度 (以像素為單位)。 此設定相當於 RDC [選項] 下 [顯示] 索引標籤上 [顯示器設定] 滑桿中的選項。 | 介於 200 到 4096 之間的數值 | 預設值會設定為本機電腦上的解析度 | x | x | x |
| devicestoredirect:s:value | 決定用戶端電腦上要重新導向至遠端工作階段以供使用的裝置。 | - *:重新導向所有支援的裝置，包括稍後連線的裝置<br> - 一或多個裝置的有效硬體識別碼 | | x | x | x |
| disableconnectionsharing:i:value | 決定當啟動 RemoteApp 或桌面時，遠端桌面用戶端是否會重新連線到任何現有開啟連線或起始新的連線 | - 0：重新連線到任何現有的工作階段<br>- 1：起始新的連線 | 0 | x | x | x |
| domain:s:value | 指定登入遠端電腦時，用於登入的使用者帳戶所在網域名稱。 | 有效的網域名稱，例如 "CONTOSO" | 沒有預設值 | x | x | x |
| drivestoredirect:s:value | 決定用戶端電腦上要重新導向至遠端工作階段以供使用的本機磁碟機。 | - 未指定值：不要重新導向任何磁碟機<br>- * :重新導向所有磁碟機，包括稍後連接的磁碟機<br>- DynamicDrives：重新導向稍後連接的任何磁碟機<br>- 一或多個磁碟機的磁碟機和標籤 (例如 "drivestoredirect:s:C:;E:;")：重新導向指定的磁碟機| 未指定值：不要重新導向任何磁碟機 | x | x    | |
| enablecredsspsupport:i:value | 決定 RDP 是否會使用認證安全性支援提供者 (CredSSP) 進行驗證 (如果有的話)。 | - 0：RDP 不會使用 CredSSP，即使作業系統支援 CredSSP 也一樣<br>- 1：如果作業系統支援 CredSSP，則 RDP 會使用 CredSSP | 1 | x | x | |
| 編碼重新導向的影片擷取: i: 值 | 啟用或停用重新導向影片的編碼。 | - 0：停用重新導向影片的編碼<br>- 1：啟用重新導向影片的編碼 | 1 | x | x | x |
| full address:s:value | 此設定指定您想要連線的遠端電腦名稱或 IP 位址 | 有效的電腦名稱、IPv4 位址或 IPv6 位址。 | | x | x | x |
| gatewaycredentialssource:i:value | 指定或擷取 RD 閘道驗證方法。 | - 0：詢問密碼 (NTLM)<br>- 1：使用智慧卡<br>- 4：允許使用者稍後再選取 | 0 | x | x | x |
| gatewayhostname:s:value | 指定 RD 閘道主機名稱。 | 有效的閘道伺服器位址。 ||x|x|x|
| gatewayprofileusagemethod:i:value | 指定是否使用預設的 RD 閘道設定 | - 0：使用由系統管理員指定的預設設定檔模式<br>- 1：使用由使用者指定的明確設定 | 0 | x | x | x |
| gatewayusagemethod:i:value | 指定何時使用 RD 閘道伺服器 | - 0：不要使用 RD 閘道伺服器<br>- 1：一律使用 RD 閘道伺服器<br>- 2：如果無法建立與 RD 工作階段主機的直接連線，請使用 RD 閘道伺服器<br>- 3：使用預設的 RD 閘道伺服器設定<br>- 4：不要使用 RD 閘道，略過本機位址的伺服器<br>將此屬性值設定為 0 或 4 會有同等效果，但將此屬性設定為 4 會啟用略過本機位址的選項。 | | x | x | x |
| networkautodetect:i:value | 決定是否使用自動網路頻寬偵測。 需要設定選項 bandwidthautodetect 並與連線類型 7 相互關聯。 | - 0：不要使用自動網路頻寬偵測<br> - 1：使用自動網路頻寬偵測 | 1 | x ||x|
| promptcredentialonce:i:value | 決定是否儲存使用者的認證以同時用於 RD 閘道和遠端電腦。|- 0：遠端工作階段不會使用相同的認證<br>- 1：遠端工作階段會使用相同的認證|1|x|x||
| redirectclipboard:i:value | 決定是否啟用剪貼簿重新導向。 | - 0：無法在遠端工作階段中使用本機電腦上的剪貼簿<br>- 1：可以在遠端工作階段中使用本機電腦上的剪貼簿|1|x|x|x|
| 重新導向影片擷取編碼品質:i:value | 控制編碼影片的品質。 | - 0：高度壓縮影片。 當有很多動作時，品質可能會受影響 <br>- 1：中度壓縮<br>- 2：高圖片品質的低度壓縮影片 | 0 | x | x | x |
| redirectprinters:i:value | 決定當您使用遠端桌面連線來連線到遠端電腦時，用戶端電腦上設定的印表機是否要重新導向至遠端工作階段以供使用。 | - 0：無法在遠端工作階段中使用本機電腦上的印表機<br>- 1：可以在遠端工作階段中使用本機電腦上的印表機|1|x|x|x|
| redirectsmartcards:i:value | 決定當您連線到遠端電腦時，用戶端電腦上的智慧卡裝置是否要重新導向至遠端工作階段以供使用。 |- 0：無法在遠端工作階段中使用本機電腦上的智慧卡裝置<br>- 1：可以在遠端工作階段中使用本機電腦上的智慧卡裝置|1|x|x||
| remoteapplicationcmdline:s:value | RemoteApp 的選擇性命令列參數。||x|x|x|
| remoteapplicationexpandcmdline:i:value| 決定 RemoteApp 命令列參數內含的環境變數是否應該在本機或遠端擴充。|- 0：環境變數應該擴充為本機電腦的值<br>- 1：環境變數應該在遠端電腦上擴充為遠端電腦的值||x|x|x|
| remoteapplicationexpandworkingdir | 決定 RemoteApp 工作目錄參數內含的環境變數是否應該在本機或遠端擴充。 | - 0：環境變數應該擴充為本機電腦的值<br> - 1：環境變數應該在遠端電腦上擴充為遠端電腦的值。<br>RemoteApp 工作目錄是透過殼層工作目錄參數指定。||x|x|x|
|remoteapplicationfile:s:value | 指定 RemoteApp 要在遠端電腦上開啟的檔案。<br>若要開啟本機檔案，您還必須啟用來源磁碟機的磁碟機重新導向。||x|x|x|
|remoteapplicationicon:s:value | 指定當啟動 RemoteApp 時要顯示在用戶端 UI 中的圖示檔。 如果未指定檔案名稱，用戶端會使用標準遠端桌面圖示。 僅支援 ".ico" 檔案。||x|x|x|
|remoteapplicationmode:i:value | 決定是否以 RemoteApp 工作階段啟動 RemoteApp 連線。| - 0：不要啟動 RemoteApp 工作階段<br>- 1：啟動 RemoteApp 工作階段|1|x|x|x|
|remoteapplicationname:s:value | 指定當啟動 RemoteApp 時，用戶端介面中的 RemoteApp 名稱。| 例如，"Excel 2016"。|x|x|x|
|remoteapplicationprogram:s:value | 指定 RemoteApp 的別名或可執行檔名稱。 | 例如，"EXCEL"。 |x|x|x|
|screen mode id:i:value | 決定當您使用遠端桌面連線來連線到遠端電腦時，遠端工作階段視窗是否以全螢幕顯示。 |- 1：遠端工作階段會顯示在視窗中<br>- 2：遠端工作階段會以全螢幕顯示|2|x|x|x|
|smart sizing:i:value | 決定用戶端電腦是否可以調整遠端電腦上的內容大小，以符合用戶端電腦的視窗大小。|- 0：用戶端視窗顯示不會在調整大小後縮放<br>- 1：用戶端視窗顯示會在調整大小後縮放|0|x|x||
| use multimon:i:value | 可在您使用遠端桌面連線來連線到遠端電腦時設定多重監視器支援。|- 0：不要啟用多重監視器支援<br>- 1：啟用多重監視器支援|0|x|x||
| username:s:value | 指定將用於登入遠端電腦的使用者帳戶名稱。 | 任何有效的使用者名稱。 ||x|x|x|
| videoplaybackmode:i:value| 決定遠端桌面連線是否會使用 RDP 高效多媒體串流來播放視訊。|- 0：不要使用 RDP 高效多媒體串流來播放視訊<br>- 1：盡可能使用 RDP 高效多媒體串流來播放視訊|1|x|x||
| workspaceid:s:value | 定義與 RDP 檔案 (其中包含此設定) 建立關聯的 RemoteApp 和桌面識別碼。 | 有效的 RemoteApp 和桌面連線識別碼|x|x||
