---
title: 在 iOS 上開始使用遠端桌面
description: 了解如何設定適用於 iOS 的遠端桌面用戶端
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 03ec5a3d-d3f2-4afd-9405-ae58b6ecc91c
author: lizap
manager: dongill
ms.author: elizapo
date: 01/13/2017
ms.localizationpriority: medium
ms.openlocfilehash: 71fe969de4d21f7fa3c134b0f80fc7f69e5b2da8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446701"
---
# <a name="get-started-with-remote-desktop-on-ios"></a>在 iOS 上開始使用遠端桌面

>適用於：Windows 10，Windows 8.1、 Windows Server 2019、 Windows Server 2016、windows Server 2012 R2

您可以使用適用於 iOS 的遠端桌面用戶端來從您的 iOS 裝置 （Iphone 和 Ipad） 使用 Windows 應用程式、 資源及桌上型電腦。

若要開始使用下列資訊。 請務必查看[常見問題集](remote-desktop-client-faq.md)如果您有任何問題。

> [!NOTE]
> - 想知道 iOS 用戶端的新版本嗎？ 請參閱[遠端桌面在 iOS 上最新消息？](ios-whatsnew.md)
> - IOS 用戶端支援執行 iOS 之裝置 6.x 和更新版本。

## <a name="get-the-remote-desktop-client-and-start-using-it"></a>取得遠端桌面用戶端，並開始使用

### <a name="download-the-remote-desktop-client-from-the-ios-store"></a>從 iOS store 下載遠端桌面用戶端
請遵循下列步驟來開始在您的 iOS 裝置上使用遠端桌面：

1. 下載的 Microsoft 遠端桌面用戶端[iTunes](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8)。
2. [您的電腦設定成接受遠端連接](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop)。
3. 新增[遠端桌面連線](#add-a-remote-desktop-connection)或是[遠端資源](#add-a-remote-resource)。 您使用連線來連線到直接到 Windows 電腦和遠端資源使用的 RemoteApp 程式，工作階段為基礎的桌面或虛擬桌面發佈內部部署使用 RemoteApp 與桌面連線。 在公司環境中通常使用這項功能。

### <a name="download-the-remote-desktop-ios-beta-client"></a>下載遠端桌面 iOS Beta 用戶端
在 您的 iOS 裝置，請依照[這些指示](https://aka.ms/rdiosbeta)下載遠端桌面 iOS Beta 用戶端。

### <a name="add-a-remote-desktop-connection"></a>新增遠端桌面連線

若要建立遠端桌面連線： 
1. 在 「 連線中心 」 點選 **+** ，然後點選**新增電腦或伺服器**。
2. 輸入遠端桌面連線的下列資訊：
   - **電腦名稱**– 電腦的名稱。 這可以是 Windows 電腦名稱、 網際網路網域名稱或 IP 位址。 您也可以在電腦名稱加連接埠資訊 (例如**MyDesktop:3389**或是**10.0.0.1:3389**)。
   - **使用者名稱**– 要用來存取遠端電腦的使用者名稱。 您可以使用下列格式： *user_name*， *domain\user_name*，或<em>user_name@domain.com</em>。 您也可以指定是否要提示輸入使用者名稱和密碼。
3. 您也可以設定下列其他選項：
   - **易記名稱 （選擇性）** – 您要連接到電腦以易記名稱。 您可以使用任何字串，但如果您未指定好記的名稱，會顯示電腦名稱。
   - **閘道 （選擇性）** – 您想要用於連線到虛擬桌面、 RemoteApp 程式以及以工作階段為基礎的桌面，在公司內部網路上的遠端桌面閘道。 從您的系統管理員取得閘道的相關資訊。
   - **音效**– 選擇要用於遠端工作階段期間的音訊裝置。 您可以選擇播放音效，在本機裝置，也就是遠端裝置上，或者完全不要。
   - **交換滑鼠按鈕**– 每當滑鼠筆勢會傳送命令，以滑鼠左鍵，以改為傳送相同的命令，以滑鼠右鍵。 這是需要遠端電腦設定為慣用左手滑鼠模式。
   - **系統管理員模式**-連接到執行 Windows Server 2003 或更新版本的伺服器上的系統管理工作階段。
4. 點選**儲存**。

需要編輯這些設定嗎？ 按住您要編輯的桌面，然後點選 [設定] 圖示。 

### <a name="add-a-remote-resource"></a>新增遠端資源
遠端資源是 RemoteApp 程式、 工作階段為基礎的桌面及發佈使用 RemoteApp 與桌面連線的虛擬桌面。

- URL 會顯示連結，可讓您存取 RemoteApp 與桌面連線的 RD Web 存取伺服器。
- 設定 RemoteApp 與桌面連線會列出。

若要新增的遠端資源：

1. 在 「 連線中心 」 畫面上，點選 **+** ，然後點選**新增遠端資源**。 
2. 輸入遠端資源的資訊：
   - **摘要 URL** -RD Web 存取伺服器的 URL。 您也可以輸入您的公司電子郵件帳戶，在此欄位 – 這會告訴用戶端搜尋與您的電子郵件地址相關聯的 RD Web 存取伺服器。
   - **使用者名稱**-要用於您要連接到 RD Web 存取伺服器的使用者名稱。
   - **密碼**-針對您要連接到 RD Web 存取伺服器使用的密碼。
3. 點選**儲存**。

遠端資源將會顯示在 「 連線中心。


## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>連接至 RD 閘道存取內部資產

遠端桌面閘道 （RD 閘道） 可讓您從任何地方連線到公司網路上的遠端電腦在網際網路上。 您可以建立和管理您使用遠端桌面用戶端的閘道。

若要設定新的閘道：

1. 在 「 連線中心 」 中，點選**設定 > 閘道**。 
2. 點選**新增遠端桌面閘道**。
3. 輸入下列資訊：
   - **伺服器名稱**– 您想要作為閘道使用的電腦名稱。 這可以是 Windows 電腦名稱、 網際網路網域名稱或 IP 位址。 您也可以新增連接埠資訊的伺服器名稱 (例如：**RDGateway:443**或是**10.0.0.1:443**)。
   - **使用者名稱**-使用者名稱和密碼用於您所連接的遠端桌面閘道。 您也可以選取**使用的連接認證**所使用的遠端桌面連線使用的相同使用者名稱和密碼。


## <a name="manage-your-user-accounts"></a>管理您的使用者帳戶 

當您連線到桌面或遠端資源時，您可以儲存一次從選取的使用者帳戶。 您可以使用遠端桌面用戶端來管理您的使用者帳戶。

若要建立新的使用者帳戶：

1. 在 「 連線中心 」 中，點選**設定**，然後點選**使用者名稱**。
2. 點選**新增使用者帳戶**。
3. 輸入下列資訊：
   - **使用者名稱**-要儲存以供使用，透過遠端連線的使用者名稱。 您可以輸入使用者名稱，任何下列格式： user_name domain\user_name，或user_name@domain.com。
   - **密碼**-您指定的使用者的密碼。 您想要將儲存到使用遠端連線的每個使用者帳戶必須具有與其相關聯的密碼。
4. 點選**儲存**，然後點選**設定**。
5. 點選**完成**以儲存新的組態。

若要刪除的使用者帳戶：

1. 在 「 連線中心 」 中，點選**設定 > 使用者名稱**。
2. 撥動從右到左來選取使用者的資料列。
3. 點選**刪除**。



## <a name="navigate-the-remote-desktop-session"></a>瀏覽遠端桌面工作階段
當您啟動遠端桌面工作階段時，有一些工具可用，您可以使用瀏覽工作階段。

### <a name="start-a-remote-desktop-connection"></a>啟動 遠端桌面連線

1. 點選 遠端桌面連線，並啟動遠端桌面工作階段。 
2. 如果系統要求您驗證遠端桌面憑證時，點選**接受**。 您可以選擇一律接受滑動**不要再詢問我連線到此電腦**切換至**ON**。 

### <a name="connection-bar"></a>連線列

連接列可讓您存取其他導覽控制項。 

- **移動瀏覽控制項**:取景位置調整控制項讓螢幕可以放大及四處移動。 請注意取景位置調整控制項只會使用直接觸控。
   - 啟用/停用移動瀏覽控制項：點選 [移動瀏覽列中的圖示連線顯示取景位置調整控制項中與縮放] 畫面。 點選 移動瀏覽列中的圖示連線一次，若要隱藏控制項，並傳回其原始的解決方式的畫面。
   - 使用移動瀏覽控制項：點選 按住移動瀏覽控制項，然後在您想要移動螢幕的方向拖曳。
   - 移動取景位置調整控制項：Double 點選並按住 取景位置調整控制項移動螢幕上的控制項。
- **連線名稱**:會顯示目前的連接名稱。 點選 連線名稱，以顯示工作階段選取項目列。
- **鍵盤**:點選 [鍵盤] 圖示以顯示或隱藏鍵盤。 顯示鍵盤時，會自動顯示取景位置調整控制項。
- **移動連線列**:點選並按住 [連線] 列中，然後拖放到新位置上方的螢幕。

### <a name="session-selection"></a>工作階段選取項目
您可以有多個同時開啟到不同的電腦的連線。 點選 [連線] 列左手邊的畫面上顯示的工作階段選取項目列。 工作階段選取項目列可讓您檢視開啟的連接，以及兩者間切換。 

- 切換開啟的遠端資源的工作階段中的應用程式。

    當您連接到遠端資源時，您可以切換開啟的應用程式中點選 [展開工具] 功能表，然後從可用的項目清單中選擇該工作階段。
- 啟動新的工作階段

  您可以啟動新的應用程式或桌面工作階段內目前的連接： 點選**啟動新**，然後選擇 從清單中可用的項目。

- 工作階段中斷連線

  若要中斷連線工作階段點選 X 在左手邊的 [工作階段] 圖格。

### <a name="command-bar"></a>命令列

命令列取代列從 8.0.1 版公用程式。 您可以切換滑鼠模式並返回命令列中的 「 連線中心 」。

## <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>使用 遠端工作階段中的 觸控筆勢與滑鼠模式

用戶端會使用標準的觸控筆勢。 您也可以使用觸控筆勢來複寫的遠端桌面的滑鼠動作。 下表中定義可用的滑鼠模式。

> [!NOTE]
> 互動 Windows 8 或更新版本的原生的觸控筆勢支援直接觸控模式。 如需詳細資訊，在 Windows 8 上看到筆勢[觸控：撥動、 點選，以後](https://windows.microsoft.com/en-US/windows-8/touch-swipe-tap-beyond)。

| 滑鼠模式    | 滑鼠作業      | 手勢                                                    |
|---------------|----------------------|------------------------------------------------------------|
| 直接觸控  | 按一下滑鼠左鍵           | 1 的手指點選                                               |
| 直接觸控  | 按一下滑鼠右鍵          | 1 手指點選並按住                                      |
| 滑鼠指標 | 按一下滑鼠左鍵           | 1 的手指點選                                               |
| 滑鼠指標 | 按一下滑鼠左鍵並拖曳  | 1 的手指 double 點選並按住，然後拖曳                    |
| 滑鼠指標 | 按一下滑鼠右鍵          | 2 個手指點選                                               |
| 滑鼠指標 | 按一下滑鼠右鍵並拖曳 | 2 個手指 double 點選並按住，然後拖曳                    |
| 滑鼠指標 | 滑鼠滾輪          | 2 個手指點選並按住不放，然後向上或向下拖曳                |
| 滑鼠指標 | 縮放                 | 縮小 2 的手指來放大或散佈在 2 個根手指，以縮小 |

## <a name="supported-input-devices"></a>支援的輸入的裝置

[遠端桌面 iOS beta 用戶端](https://aka.ms/rdiosbeta)支援 Swiftpoint 和 ProPoint 實體滑鼠。 提供 Swiftpoint[獨家折扣](https://www.swiftpoint.com/microsoft/)iOS beta 用戶端使用者 t 上。

IOS 用戶端時，目前僅支援 Swiftpoint 滑鼠。 請參閱[iOS 用戶端在最新消息](ios-whatsnew.md)頁面並[iOS App Store](https://aka.ms/rdios)如需有關支援的其他裝置在未來的消息。

## <a name="use-a-keyboard-in-a-remote-session"></a>在 遠端工作階段中使用鍵盤

您可以使用螢幕小鍵盤] 或 [遠端工作階段中的實體鍵盤。

螢幕小鍵盤，使用在鍵盤上方列的右邊緣標準與其他鍵盤之間切換 按鈕。

如果您的 iOS 裝置啟用藍芽，用戶端會自動偵測藍芽鍵盤。

請注意，由於在作業系統上的限制，例如 Ctrl、 選項和函式的特殊按鍵將無法如預期運作與藍芽鍵盤。 下列金鑰的運作方式：

- 英數字元的索引鍵
- 資料指標索引鍵
- 索引標籤：索引標籤上的運作方式，但無法運作 Shift + Tab
- 首頁 / Pos1:Alt + 向左鍵 = 首頁
- 結束:Alt + 向右鍵 = 結束
- Page Up:Alt + 向上 = Page Up
- Page down 鍵：Alt + 向下 = Page down 鍵
- 選取所有：命令 + A = Ctrl + A （在大部分程式中全選）
- 剪下：命令 + X = Ctrl + X （在大部分程式中的剪下）
- 複製：Command + C = Ctrl + C （在大部分程式中的複製）
- 貼上：命令 + V = Ctrl + V （貼上在大多數程式中）
- 符號：Alt + 英數字元的索引鍵將會產生不同的符號，根據所設定的語言

> [!TIP]
> 提出問題或意見都歡迎畫面。 不過，請不會回傳要求，以取得疑難排解說明，在這篇文章結尾處使用的註解功能。 相反地，請移至[遠端桌面用戶端論壇](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc)並啟動新的執行緒。 有功能建議嗎？ 告訴我們在[用戶端使用者之聲論壇](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android)。

