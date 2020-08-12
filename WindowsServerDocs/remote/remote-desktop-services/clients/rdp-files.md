---
title: 支援的遠端桌面 RDP 檔案設定
description: 了解遠端桌面的 RDP 檔案設定
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 06/30/2020
ms.localizationpriority: medium
ms.openlocfilehash: 5303bb696131d4e122da11c2d72152bf304716b4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961932"
---
# <a name="supported-remote-desktop-rdp-file-settings"></a>支援的遠端桌面 RDP 檔案設定

下表包含支援的 RDP 檔案設定清單，您可以搭配遠端桌面用戶端來使用這些設定。 進行設定時，請檢查[用戶端比較](./remote-desktop-app-compare.md)，已確認每個用戶端支援的重新導向。

這份表格也會點出支援作為 Windows 虛擬桌面自訂屬性的設定。 您可以參閱[此文件](/azure/virtual-desktop/customize-rdp-properties/)，以了解如何使用 PowerShell 來自訂 Windows 虛擬桌面主機集區的 RDP 屬性。

## <a name="connection-information"></a>連線資訊

| RDP 設定                        | 說明            | 值                 | 預設值          | Windows 虛擬桌面支援 |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|
| full address:s:value | 電腦名稱：</br>此設定指定您想要連線的遠端電腦名稱或 IP 位址。</br></br>這是 RDP 檔案中唯一需要的設定。 | 有效的名稱、IPv4 位址或 IPv6 位址。 | | 否 |
| alternate full address:s:value | 指定遠端電腦的替代名稱或 IP 位址。 | 有效的名稱、IPv4 位址或 IPv6 位址。 | | 否 |
| username:s:value | 指定將用於登入遠端電腦的使用者帳戶名稱。 | 任何有效的使用者名稱。 | | 否 |
| domain:s:value | 指定登入遠端電腦時，用於登入的使用者帳戶所在網域名稱。 | 有效的網域名稱，例如 "CONTOSO"。 | | 否 |
| gatewayhostname:s:value | 指定 RD 閘道主機名稱。 | 有效的名稱、IPv4 位址或 IPv6 位址。 | | 否 |
| gatewaycredentialssource:i:value | 指定 RD 閘道驗證方法。 | - 0：詢問密碼 (NTLM)</br>- 1：使用智慧卡</br>- 2：使用目前登入之使用者的認證。</br>- 3：提示使用者輸入其認證並使用基本驗證</br>- 4：允許使用者稍後再選取</br>- 5：使用以 Cookie 為基礎的驗證 | 0 | 否 |
| gatewayprofileusagemethod:i:value | 指定是否使用預設的 RD 閘道設定。 | - 0：使用由系統管理員指定的預設設定檔模式</br>- 1：使用由使用者指定的明確設定 | 0 | 否 |
| gatewayusagemethod:i:value | 指定何時要在連線中使用 RD 閘道。 | - 0：不使用 RD 閘道</br>- 1：一律使用 RD 閘道</br>- 2：如果無法直接與 RD 工作階段主機連線，就使用 RD 閘道</br>- 3：使用預設的 RD 閘道設定</br>- 4：不使用 RD 閘道，略過本機位址的閘道</br>將此屬性值設定為 0 或 4 會有同等效果，但將此屬性設定為 4 會啟用略過本機位址的選項。 | 0 | 否 |
| promptcredentialonce:i:value | 決定是否儲存使用者的認證以同時用於 RD 閘道和遠端電腦。 | - 0：遠端工作階段不會使用相同的認證</br>- 1：遠端工作階段會使用相同的認證 | 1 | 否 |
| authentication level:i:value | 定義伺服器驗證等級設定。 | - 0：如果伺服器驗證失敗，則連線到電腦而不顯示警告 (連線且不要警告我)</br>- 1：如果伺服器驗證失敗，不要建立連線 (不要連線)</br>- 2：如果伺服器驗證失敗，則顯示警告並允許我連線或拒絕連線 (警告我)</br>- 3：未指定驗證需求。 | 3 | 否 |
| enablecredsspsupport:i:value | 決定用戶端是否會使用認證安全性支援提供者 (CredSSP) 進行驗證 (如果有的話)。 | - 0：RDP 不會使用 CredSSP，即使作業系統支援 CredSSP 也一樣</br>- 1：如果作業系統支援 CredSSP，則 RDP 會使用 CredSSP | 1 | 是 |
| disableconnectionsharing:i:value | 決定在有新連線啟動時，用戶端是要重新連線到任何現有的已中斷連線工作階段，還是要起始新的連線。 | - 0：重新連線到任何現有的工作階段</br>- 1：起始新的連線 | 0 | 是 |
| alternate shell:s:value | 指定要在遠端工作階段中以命令介面而非總管形式自動啟動的程式。 | 有效的可執行檔路徑，例如 "C:\ProgramFiles\Office\word.exe" | | 是 |

## <a name="session-behavior"></a>工作階段行為

| RDP 設定                        | 說明            | 值                 | 預設值          | Windows 虛擬桌面支援 |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|
| autoreconnection enabled:i:value | 決定如果連線中斷 (例如當網路連線中斷時)，用戶端是否會自動嘗試重新連線到遠端電腦。 | - 0：用戶端不會自動嘗試重新連線</br>- 1：用戶端會自動嘗試重新連線 | 1 | 是 |
| bandwidthautodetect:i:value | 決定是否啟用自動網路類型偵測 | - 0：停用自動網路類型偵測</br>- 1：啟用自動網路類型偵測 | 1 | 是 |
| networkautodetect:i:value | 決定是否使用自動網路頻寬偵測。 需要將 bandwidthautodetect 設定為 1。 | - 0：不要使用自動網路頻寬偵測</br> - 1：使用自動網路頻寬偵測 | 1 | 是 |
| compression:i:value | 決定當透過 RDP 傳輸到本機電腦時是否啟用大量壓縮。|- 0：停用 RDP 大量壓縮</br>- 1：啟用 RDP 大量壓縮 | 1 | 是 |
| videoplaybackmode:i:value| 決定連線是否會使用 RDP 高效多媒體串流來播放視訊。|- 0：不要使用 RDP 高效多媒體串流來播放視訊</br>- 1：盡可能使用 RDP 高效多媒體串流來播放視訊 | 1 | 是 |

## <a name="device-redirection"></a>裝置重新導向

| RDP 設定                        | 說明            | 值                 | 預設值          | Windows 虛擬桌面支援 |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|
| audiocapturemode:i:value | 麥克風重新導向：</br>指出是否啟用音訊輸入重新導向。 | - 0：停用本機裝置的音訊擷取</br>- 1：啟用本機裝置的音訊擷取，並重新導向至遠端工作階段中的音訊應用程式 | 0 | 是 |
| 編碼重新導向的影片擷取: i: 值 | 啟用或停用重新導向影片的編碼。 | - 0：停用重新導向影片的編碼</br>- 1：啟用重新導向影片的編碼 | 1 | 是 |
| 重新導向影片擷取編碼品質:i:value | 控制編碼影片的品質。 | - 0：高度壓縮影片。 當有很多動作時，品質可能會受影響。 </br>- 1：中度壓縮。</br>- 2：高圖片品質的低度壓縮影片。 | 0 | 是 |
| audiomode:i:value | 音訊輸出位置：</br>決定本機或遠端電腦是否播放音訊。 | - 0：在本機電腦上播放聲音 (在這部電腦上播放)</br>- 1：在遠端電腦上播放聲音 (在遠端電腦上播放)</br>- 2：不要播放聲音 (不要播放) | 0 | 是 |
| camerastoredirect:s:value | 相機重新導向：</br>設定要重新導向的相機。 此設定使用已啟用相機的 KSCATEGORY_VIDEO_CAMERA 介面清單 (以分號分隔) 進行重新導向。 | - * :重新導向所有相機</br> - 相機清單，例如 camerastoredirect:s:\\?\usb#vid_0bda&pid_58b0&mi</br>- 若要排除特定相機，請在符號連結字串前面加上 "-" | 不要重新導向任何相機 | 是 |
| devicestoredirect:s:value | 隨插即用的裝置重新導向：</br>決定本機電腦上要重新導向至遠端工作階段以供使用的裝置。 | - *:重新導向所有支援的裝置，包括稍後連線的裝置</br> - 一或多個裝置的有效硬體識別碼</br> - DynamicDevices：重新導向稍後連線的所有支援裝置 | 不要重新導向任何裝置 | 是 |
| drivestoredirect:s:value | 磁碟機/儲存體重新導向：</br>決定本機電腦上要重新導向至遠端工作階段以供使用的磁碟機。 | - 未指定值：不要重新導向任何磁碟機</br>- * :重新導向所有磁碟機，包括稍後連接的磁碟機</br>- DynamicDrives：重新導向稍後連接的任何磁碟機</br>- 一或多個磁碟機的磁碟機和標籤 (例如 "drivestoredirect:s:C:;E:;")：重新導向指定的磁碟機 | 不要重新導向任何磁碟機 | 是 |
| redirectclipboard:i:value | 剪貼簿重新導向：</br>決定是否啟用剪貼簿重新導向。 | - 0：無法在遠端工作階段中使用本機電腦上的剪貼簿</br>- 1：可以在遠端工作階段中使用本機電腦上的剪貼簿 | 1 | 是 |
| redirectprinters:i:value | 印表機重新導向：</br>決定是否將本機電腦上所設定的印表機重新導向至遠端工作階段以供使用 | - 0：無法在遠端工作階段中使用本機電腦上的印表機</br>- 1：可以在遠端工作階段中使用本機電腦上的印表機 | 1 | 是 |
| redirectsmartcards:i:value | 智慧卡重新導向：</br>決定是否將本機電腦上的智慧卡裝置重新導向至遠端工作階段以供使用。 |- 0：無法在遠端工作階段中使用本機電腦上的智慧卡裝置</br>- 1：可以在遠端工作階段中使用本機電腦上的智慧卡裝置 | 1 | 是 |
| usbdevicestoredirect:s:value | USB 重新導向 | - *:將尚未由另一個高階重新導向來重新導向的所有 USB 裝置，進行重新導向</br> - {Device Setup Class GUID}：將屬於指定[裝置安裝類別](/windows-hardware/drivers/install/system-defined-device-setup-classes-available-to-vendors/)成員的所有裝置進行重新導向</br> - USBInstanceID：將執行個體識別碼所識別的特定 USB 裝置進行重新導向| 不要重新導向任何 USB 裝置 | 是 |

## <a name="display-settings"></a>顯示器設定

| RDP 設定                        | 說明            | 值                 | 預設值          | Windows 虛擬桌面支援 |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|
| use multimon:i:value | 決定遠端工作階段會使用本機電腦上的一部顯示器還是多部顯示器。 | - 0：不要啟用多部顯示器支援</br>- 1：啟用多部顯示器支援 | 1 | 是 |
| selectedmonitors:s:value | 指定要從遠端工作階段使用的本機顯示器。 所選取的顯示器必須是連續的。 需要將 multimon 設定為 1。</br></br>僅適用於 Windows 收件匣 (MSTSC) 和 Windows 桌面 (MSRDC) 用戶端。 | 以逗號分隔的電腦特定顯示器識別碼清單。 您可以藉由呼叫 mstsc.exe /l 來擷取識別碼。 第一個列出的識別碼將會設定為工作階段中的主顯示器。 | 所有顯示器 | 是 |
| maximizetocurrentdisplays:i:value | 決定在最大化時，遠端工作階段要在哪部顯示器上進入全螢幕模式。 需要將 multimon 設定為 1。</br></br>僅適用於 Windows 桌面 (MSRDC) 用戶端。 | - 0：最大化時，工作階段會在一開始選取的顯示器上進入全螢幕模式</br>- 1：最大化時，會動態地在工作階段視窗所觸及的顯示器上進入全螢幕模式 | 0 | 是 |
| singlemoninwindowedmode:i:value | 決定在結束全螢幕模式時，多部顯示器遠端工作階段是否會自動切換為單一顯示器。 需要將 multimon 設定為 1。</br></br>僅適用於 Windows 桌面 (MSRDC) 用戶端。 | - 0：結束全螢幕模式時，工作階段會保留所有顯示器</br>- 1：結束全螢幕模式時，工作階段會切換為單一顯示器 | 0 | 是 |
| screen mode id:i:value | 決定當您啟動連線時，遠端工作階段視窗是否會以全螢幕模式來顯示。 | - 1：遠端工作階段會顯示在視窗中</br>- 2：遠端工作階段會以全螢幕顯示 | 2 | 是 |
| smart sizing:i:value | 決定本機電腦是否要擴縮遠端工作階段的內容，使其符合視窗大小。 | - 0：不會在調整大小時擴縮本機視窗內容</br>- 1：會在調整大小時擴縮本機視窗內容 | 0 | 是 |
| dynamic resolution:i:value | 決定在調整本機視窗大小時，是否要自動更新遠端工作階段的解析度。 | - 0：工作階段運作期間，工作階段的解析度會保持不變</br>- 1：工作階段的解析度會隨著本機視窗大小的調整而隨之更新 | 1 | 是 |
| desktop size id:i:value | 從一組預先定義的選項指定遠端工作階段桌面尺寸。 如果指定了 desktopheight 和 desktopwidth，則會覆寫此設定。| \- 0：640×480</br>- 1：800×600</br>- 2：1024×768</br>- 3：1280×1024</br>- 4：1600×1200 | 1 | 是 |
| desktopheight:i:value | 指定遠端工作階段的解析度高度 (以像素為單位)。 | 介於 200 到 8192 之間的數值 | 符合本機電腦 | 是 |
| desktopwidth:i:value | 指定遠端工作階段的解析度寬度 (以像素為單位)。 | 介於 200 到 8192 之間的數值 | 符合本機電腦 | 是 |
| desktopscalefactor:i:value | 指定遠端工作階段的擴縮比例，讓內容變得更大。 | 下列清單中的數值：100、125、150、175、200、250、300、400、500 | 100 | 是 |

## <a name="remoteapp"></a>RemoteApp

| RDP 設定                        | 說明            | 值                 | 預設值          | Windows 虛擬桌面支援 |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|
| remoteapplicationcmdline:s:value | RemoteApp 的選擇性命令列參數。 | 有效的命令列參數。 | | 否 |
| remoteapplicationexpandcmdline:i:value | 決定 RemoteApp 命令列參數內含的環境變數是否應該在本機或遠端擴充。 | - 0：環境變數應該擴充為本機電腦的值</br>- 1：環境變數應該擴充為遠端電腦的值 | 1 | 否 |
| remoteapplicationexpandworkingdir:i:value | 決定 RemoteApp 工作目錄參數內含的環境變數是否應該在本機或遠端擴充。 | - 0：環境變數應該擴充為本機電腦的值</br> - 1：環境變數應該擴充為遠端電腦的值。</br>RemoteApp 工作目錄是透過殼層工作目錄參數指定。 | 1 | 否 |
| remoteapplicationfile:s:value | 指定 RemoteApp 要在遠端電腦上開啟的檔案。</br>若要開啟本機檔案，您還必須啟用來源磁碟機的磁碟機重新導向。 | 有效的檔案路徑。 | | 否 |
| remoteapplicationicon:s:value | 指定當啟動 RemoteApp 時要顯示在用戶端 UI 中的圖示檔。 如果未指定檔案名稱，用戶端會使用標準遠端桌面圖示。 僅支援 ".ico" 檔案。 | 有效的檔案路徑。 | | 否 |
| remoteapplicationmode:i:value | 決定是否以 RemoteApp 工作階段啟動連線。 | - 0：不要啟動 RemoteApp 工作階段</br>- 1：啟動 RemoteApp 工作階段 | 1 | 否 |
| remoteapplicationname:s:value | 指定當啟動 RemoteApp 時，用戶端介面中的 RemoteApp 名稱。| 應用程式顯示名稱。 例如，"Excel 2016"。 | | 否 |
| remoteapplicationprogram:s:value | 指定 RemoteApp 的別名或可執行檔名稱。 | 有效的別名或名稱。 例如，"EXCEL"。 | | 否 |
