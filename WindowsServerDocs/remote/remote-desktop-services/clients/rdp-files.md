---
title: 支援的遠端桌面 RDP 檔案設定
description: 深入了解遠端桌面的 RDP 檔案設定
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: fbec5c27b7cb3ef0ef750a5ebd7d7ea2654c47b3
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2019
ms.locfileid: "65983458"
---
# <a name="supported-remote-desktop-rdp-file-settings"></a>支援遠端桌面的 RDP 檔案設定

下表包含支援您可以使用以 Windows 和 HTML 用戶端的 RDP 檔案設定的清單。 "X"平台的資料行中指出，支援設定。 不過，這份清單不支援 Windows 和 HTML5 用戶端設定的完整清單。 我們將持續更新這個資料表，以包含更多支援的 RDP 設定 Windows 和 HTML5 的用戶端，以及 macOS、 iOS 和 Android 用戶端。

| RDP 設定                        | 描述            | 值                 | 預設值          | Windows 虛擬桌面 | Windows | HTML5   |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|:-------:|:-------:|
| 替代的完整位址： s:value | 指定的替代名稱或遠端電腦的 IP 位址。 | 任何有效的名稱或 IP 位址的遠端電腦，例如 「 10.10.15.15" | | x | x | x |
| 替代殼層： s:value        | 判斷程式自動啟動時，您使用 RDP 連線。 此設定對應於 [程式路徑和檔案名稱] 方塊在 [程式] 索引標籤上的遠端桌面連線用戶端。 若要指定替代的殼層，請輸入有效的路徑可執行檔的值，例如"C:\ProgramFiles\Office\word.exe 」。 這項設定也會決定哪些路徑或遠端的應用程式，以啟動在連接時，如果已啟用 Remoteapplicationmode=i:<0 的別名。 | "C:\ProgramFiles\Office\word.exe" || x | x | x |
| audiocapturemode:i:value | 指出是否已啟用音訊的輸入/輸出重新導向。 此設定會對應到遠端音訊中的遠端桌面連線用戶端。 | - 0:停用從本機裝置的音訊擷取<br>- 1:啟用從本機裝置和重新導向至音訊的應用程式在遠端工作階段中的音訊擷取 | 0 | x | x | |
| audiomode:i:value | 判斷是否會本機或遠端電腦播放音訊。 | - 0:在本機電腦 （在此電腦上的 [播放]） 上播放音效<br>- 1:在遠端電腦 （遠端電腦上的 [播放]） 上播放音效<br>- 2:不會播放的音效 （並不會影響） | 0 | x | x | x |
| 驗證層級： i： 值 | 定義伺服器的驗證層級設定。 | - 0:如果伺服器驗證失敗，連線到電腦，而不發出警告 (Connect 且不用警告我)<br>- 1:如果伺服器驗證失敗，未建立連線 （不連線）<br>- 2:如果伺服器驗證失敗，顯示警告，並容我連接或拒絕的連線 （我的警告）<br>- 3:指定沒有任何驗證要求。 | 3 | x | x ||
| 啟用自動重新連線： i： 值 | 此設定會決定是否在用戶端電腦會自動嘗試重新連線到遠端電腦的連線中斷，例如，如果沒有網路連線中斷時。 此設定會對應到重新連接，如果連接已卸除的核取方塊底下的 [體驗] 索引標籤**選項**中遠端桌面連線 (RDC)。| - 0:用戶端電腦不會自動嘗試重新連線<br>- 1:用戶端電腦會自動嘗試重新連線| 1 | x | x | x |
| bandwidthautodetect:i:value | 決定是否要啟用自動網路類型偵測 | - 0:停用自動網路類型偵測<br>- 1:啟用自動網路類型偵測 | 1 | x | x | x |
| compression:i:value | 此設定會決定是否要在傳輸的 RDP 到本機電腦時，啟用大量壓縮。|- 0:停用 RDP 大量壓縮<br>- 1:啟用 RDP 大量壓縮 | 1 | x | x | x |
| 桌面大小識別碼： i： 值 | 指定維度的 遠端工作階段桌面，從一組預先定義的選項。 如果指定了 desktopheight 或 desktopwidth，這項設定會覆寫。| -0:640×480<br>- 1:800×600<br>- 2:1024×768<br>- 3:1280×1024<br>- 4:1600×1200 | 0 | x | x | x |
| desktopheight:i:value | 當您使用遠端桌面連線連接時，此設定會決定解析高度 （以像素為單位） 的遠端電腦上。 此設定會對應到在 RDC 選項下的 [顯示] 索引標籤上的顯示設定滑桿中的選取範圍。 | 200 到 2048年之間的數字值 | 預設值設定為在本機電腦上的解析 | x | x | x |
| desktopwidth:i:value | 當您使用遠端桌面連線連接時，此設定會決定解析寬度 （以像素為單位） 的遠端電腦上。 此設定會對應到在 RDC 選項下的 [顯示] 索引標籤上的顯示設定滑桿中的選取範圍。 | 介於 200 到 4096 之間的數值 | 預設值設定為在本機電腦上的解析 | x | x | x |
| disableclipboardredirection:i:value | 此設定會決定是否要在連接到遠端電腦時，啟用剪貼簿重新導向。 | - 0:已啟用剪貼簿重新導向<br>- 1:未啟用剪貼簿重新導向 | x | x | x |
| disableconnectionsharing:i:value | 決定遠端桌面用戶端重新連線到任何現有開放連線或 RemoteApp 或桌面啟動時起始新的連接 | - 0:重新連線到任何現有的工作階段<br>- 1:初始化新的連線 | 0 | x | x | x |
| disableprinterredirection:i:value | 此設定會決定是否要在連接到遠端電腦時，啟用 「 輕鬆列印印表機重新導向。 | - 0:輕鬆列印印表機重新導向已啟用<br>- 1:輕鬆列印印表機重新導向已停用 | x | x | x |
| domain:s:value | 此設定會指定將用於登入遠端電腦使用遠端桌面連線 (RDC) 的使用者帳戶所在的網域的名稱。 此設定，以及 使用者名稱 設定中，值的值會出現在 RDC 中的 選項 下的 一般 索引標籤上的使用者名稱 文字方塊中。 | 有效的網域名稱，例如"CONTOSO" | 沒有預設值 | x | x | x |
| drivestoredirect:s:value | 決定哪些用戶端電腦上的本機磁碟機將會重新導向，而且可在遠端工作階段。 | 未指定值： 未將任何磁碟機重新導向<br>* :重新導向所有的磁碟機，包括 稍後連接的磁碟機<br> DynamicDrives： 重新導向任何稍後連接的磁碟機<br>磁碟機和一或多個磁碟機的標籤： 重新導向指定的磁碟機| 未指定值： 未將任何磁碟機重新導向 | x | x    | |
| enablecredsspsupport:i:value | 此設定會決定 RDP 會使用認證安全性支援提供者 (CredSSP) 驗證是否可用。 | - 0:RDP 不會使用 CredSSP，即使作業系統支援 CredSSP<br>- 1:如果作業系統支援 CredSSP RDP 將會使用 CredSSP | 1 | x | x | |
| 完整的地址： s:value | 此設定指定的名稱或您想要連接到遠端電腦的 IP 位址 | 有效的電腦名稱、 IPv4 位址或 IPv6 位址。 | | x | x | x |
| gatewaycredentialssource:i:value | 指定或擷取的 RD 閘道驗證方法。 | - 0:詢問密碼 (NTLM)<br>- 1:使用智慧卡<br>- 4:允許使用者稍後選取 | 0 | x | x | x |
| gatewayhostname:s:value | 指定的 RD 閘道器主機名稱。 | 有效的閘道伺服器位址。 ||x|x|x|
| gatewayprofileusagemethod:i:value | 指定是否要使用預設的 RD 閘道設定 | - 0:使用系統管理員所指定的預設設定檔模式<br>- 1:使用指定由使用者明確設定， | 0 | x | x | x |
| gatewayusagemethod:i:value | 指定何時要使用的 RD 閘道伺服器 | - 0:請勿使用 RD 閘道伺服器<br>- 1:永遠使用 RD 閘道伺服器<br>- 2:如果無法建立直接連線，RD 工作階段主機，請使用 RD 閘道伺服器<br>- 3:使用預設的 RD 閘道伺服器設定<br>- 4:不使用 RD 閘道，略過本機位址的伺服器<br>設定此屬性值 0 或 4 都相當有效，但此屬性設定為 4 啟用這個選項可略過本機位址。 | | x | x | x |
| networkautodetect:i:value | 決定要使用自動化的網路頻寬偵測。 需要設定選項 bandwidthautodetect 並將相互關聯連接類型為 7。 | - 0:請勿使用自動化的網路頻寬偵測<br> - 1:使用自動化的網路頻寬偵測 | 1 | x ||x|
| promptcredentialonce:i:value | 判斷使用者的認證會儲存及用於 RD 閘道和遠端電腦。|- 0:遠端工作階段不會使用相同的認證<br>- 1:遠端工作階段會使用相同的認證|1|x|x||
| redirectclipboard:i:value | 此設定會決定是否在本機電腦上的剪貼簿將會重新導向並可在遠端工作階段。 此設定對應於 **剪貼簿** RDC 中的 [選項] 下的 [本機資源] 索引標籤上的核取方塊。 | - 0:在本機電腦上的剪貼簿將無法使用遠端工作階段<br>- 1:在本機電腦上的剪貼簿位於遠端工作階段|1|x|x|x|
| redirectdrives:i:value | 此設定會決定是否在用戶端電腦上的磁碟機將會重新導向並可在遠端工作階段。 此設定會對應到磁碟機下的選取項目 **更多** RDC 中的 [選項] 下的 [本機資源] 索引標籤。|- 0:在本機電腦上的磁碟機無法在遠端工作階段<br>- 1:在本機電腦上的磁碟機可在遠端工作階段|0|x|x| |
| redirectprinters:i:value | 此設定會決定是否在用戶端電腦上設定的印表機會重新導向，而且可在當您連接到遠端電腦使用遠端桌面連線的遠端工作階段。 此設定會對應至在選取範圍 **印表機** RDC 中的 [選項] 下的 [本機資源] 索引標籤上的核取方塊。 | - 0:在本機電腦上的印表機都無法使用遠端工作階段<br>- 1:在本機電腦上的印表機可在遠端工作階段|1|x|x|x|
| redirectsmartcards:i:value | 此設定會決定用戶端電腦上的智慧卡裝置是否會重新導向，而且可在遠端工作階段，當您連接到遠端電腦使用遠端桌面連線 (RDC)。 此設定會對應至在選取範圍 **智慧卡** 下在 RDC 選項下的 [本機資源] 索引標籤上的多個核取方塊。|- 0:在本機電腦上的智慧卡裝置不適用於遠端工作階段<br>- 1:在本機電腦上的智慧卡裝置是在遠端工作階段可用|1|x|x||
| remoteapplicationcmdline:s:value | RemoteApp 的選擇性命令列參數。||x|x|x|
| remoteapplicationexpandcmdline:i:value| 決定在本機或遠端，是否應該展開 RemoteApp 命令列參數中包含的環境變數。|- 0:在本機電腦的值應該展開的環境變數<br>- 1:應該在遠端電腦的值在遠端電腦上展開環境變數||x|x|x|
| remoteapplicationexpandworkingdir | 決定在本機或遠端，是否應該展開 RemoteApp 工作目錄參數中包含的環境變數。 | - 0:在本機電腦的值應該展開的環境變數<br> - 1:應該在遠端電腦之值的遠端電腦上展開環境變數。<br>RemoteApp 工作目錄是透過殼層使用 directory 參數指定。||x|x|x|
|remoteapplicationfile:s:value | 指定要對 RemoteApp 開啟遠端電腦上的檔案。<br>若要開啟的本機檔案，您還必須啟用來源磁碟機的磁碟機重新導向。||x|x|x|
|remoteapplicationicon:s:value | 指定要啟動 RemoteApp 時，用戶端 UI 中顯示的圖示檔。 如果沒有指定檔案名稱時，用戶端會使用標準的 [遠端桌面] 圖示。 支援 「.ico"的檔案。||x|x|x|
|remoteapplicationmode:i:value | 決定是否 RemoteApp 連線就會啟動為 RemoteApp 工作階段。| - 0:不啟動 RemoteApp 工作階段<br>- 1:啟動 RemoteApp 工作階段|1|x|x|x|
|remoteapplicationname:s:value | 指定用戶端介面中的 RemoteApp 的名稱，在啟動 RemoteApp 時。| 例如，"Excel 2016。 」|x|x|x|
|remoteapplicationprogram:s:value | 指定 RemoteApp 別名或可執行檔名稱。 | 例如，"EXCEL"。 |x|x|x|
|畫面模式識別碼： i： 值 | 此設定會決定當您連接到遠端電腦使用遠端桌面連線時，[遠端工作階段] 視窗是否顯示全螢幕。 此設定會對應到在 RDC 選項下的 [顯示] 索引標籤上的顯示設定滑桿中的選取範圍。|- 0:遠端工作階段會出現在視窗<br>- 1:遠端工作階段會顯示全螢幕|2|x|x|x|
|智慧型的調整大小： i： 值 | 此設定會決定用戶端電腦可以調整以符合視窗大小，用戶端電腦的遠端電腦上的內容。|- 0:在調整大小時，不會調整用戶端視窗顯示<br>- 1:用戶端視窗顯示會相應調整大小時|0|x|x||
| 使用 multimon:i:value | 當您連接到遠端電腦使用遠端桌面連線時，此設定會設定多重監視器支援。|- 0:請勿啟用多重監視器支援<br>- 1:啟用多重監視器支援|0|x|x||
| username:s:value | 此設定會指定將用於使用遠端桌面連線登入遠端電腦的使用者帳戶的名稱。 此設定，以及 [網域] 設定中，值的值會出現在 RDC 選項下的 [一般] 索引標籤上的 [使用者名稱] 方塊中。| 任何有效的使用者名稱。 ||x|x|x|
| videoplaybackmode:i:value| 此設定會決定是否遠端桌面連線會使用 RDP 有效率多媒體適用於串流視訊的播放。|- 0:不使用 RDP 有效率多媒體串流為視訊播放<br>- 1:使用 RDP 有效率多媒體適用於串流視訊的播放，盡可能|1|x|x||
| workspaceid:s:value | 此設定可定義 RemoteApp 與桌面識別碼包含這項設定的 RDP 檔案。 | 有效的 RemoteApp 和桌面連線識別碼|x|x||
