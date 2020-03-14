---
title: 開始使用 iOS 用戶端
description: 了解如何設定適用於 iOS 的遠端桌面用戶端
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 03ec5a3d-d3f2-4afd-9405-ae58b6ecc91c
author: Heidilohr
manager: lizross
ms.author: helohr
date: 02/11/2020
ms.localizationpriority: medium
ms.openlocfilehash: ef13227a9f7b83f01786bbb11498da912c86581b
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323460"
---
# <a name="get-started-with-the-ios-client"></a>開始使用 iOS 用戶端

>適用於：Windows 10、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2

您可以使用適用於 iOS 的遠端桌面用戶端，從您的 iOS 裝置 (iPhone 和 iPad) 使用 Windows 應用程式、資源及桌面。

請使用下列資訊立即開始。 如果您有任何問題，請務必查看[常見問題集](remote-desktop-client-faq.md)。

> [!NOTE]
> - 想知道適用於 iOS 用戶端的新版本嗎？ 請參閱 [iOS 上的遠端桌面有什麼新功能？](ios-whatsnew.md)
> - iOS 用戶端支援執行 iOS 6.x 和更新版本的裝置。

## <a name="get-the-remote-desktop-client-and-start-using-it"></a>取得遠端桌面用戶端，並開始使用它

### <a name="download-the-remote-desktop-client-from-the-ios-store"></a>從 iOS 商店下載遠端桌面用戶端

依照下列步驟，在 iOS 裝置上開始使用遠端桌面：

1. 從 [iOS App Store](https://aka.ms/rdios) 或 [iTunes](https://itunes.apple.com/app/microsoft-remote-desktop/id714464092?mt=8) 下載 Microsoft 遠端桌面用戶端。
2. [將您的電腦設定成接受遠端連線](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop)。
3. 新增[遠端桌面連線](#add-a-remote-desktop-connection)或[遠端資源](#add-a-remote-resource)。 您可以使用連線直接連線至 Windows 電腦和遠端資源，以使用 RemoteApp 程式、工作階段型桌面，或使用 RemoteApp 和桌面連線在內部部署發佈的虛擬桌面。 在公司環境中通常可以使用這項功能。

### <a name="add-a-remote-desktop-connection"></a>新增遠端桌面連線

若要建立遠端桌面連線：

1. 在 [連線中心] 內，點選 **+** ，然後點選 [新增電腦或伺服器]  。
2. 為遠端桌面連線輸入下列資訊：
   - **電腦名稱** – 電腦的名稱。 這可以是 Windows 電腦名稱、網際網路網域名稱或 IP 位址。 您也可以將連接埠資訊附加到電腦名稱 (例如 **MyDesktop:3389** 或 **10.0.0.1:3389**)。
   - **使用者名稱** – 用來存取遠端電腦的使用者名稱。 您可以使用下列格式：*user_name*、*domain\user_name* 或 `user_name@domain.com`。 您也可以指定是否要提示輸入使用者名稱與密碼。
3. 您也可以設定下列其他選項：
   - **易記名稱 (選擇性)** – 您要連線電腦的易記名稱。 您可以使用任何字串，但如果您未指定易記名稱，會顯示電腦名稱。
   - **閘道 (選擇性)** – 您要用來連線至公司內部網路上的虛擬桌面、RemoteApp 程式以及工作階段型桌面的遠端桌面閘道。 向您的系統管理員取得閘道的相關資訊。
   - **音效** – 選擇要在遠端工作階段期間用於音訊的裝置。 您可以選擇在本機裝置、遠端裝置上播放音效，或者完全不要播放音效。
   - **交換滑鼠按鈕** – 每當滑鼠手勢使用滑鼠左按鈕傳送命令時，它會改為使用滑鼠右按鈕傳送相同的命令。 如果遠端電腦是針對慣用左手的滑鼠模式而設定，則這是必要的。
   - **系統管理員模式** - 連線到執行 Windows Server 2003 或更新版本的伺服器上的系統管理工作階段。
4. 點選 [儲存]  。

需要編輯這些設定嗎？ 按住您要編輯的桌面，然後點選 [設定] 圖示。

### <a name="add-a-remote-resource"></a>新增遠端資源

遠端資源包括 RemoteApp 程式、工作階段型桌面，以及使用 RemoteApp 與桌面連線發佈的虛擬桌面。

- URL 會顯示可讓您存取 RemoteApp 與桌面連線的 RD Web 存取伺服器的連結。
- 系統會列出已設定的 RemoteApp 與桌面連線。

若要新增遠端資源：

1. 在 [連線中心] 畫面上，點選 **+** ，然後點選 [新增遠端資源]  。
2. 輸入遠端資源的資訊：
   - **摘要 URL** - RD Web 存取伺服器的 URL。 您也可以在此欄位中輸入您的公司電子郵件帳戶，如此會告訴用戶端搜尋與您電子郵件地址相關聯的 RD Web 存取伺服器。
   - **使用者名稱** - 您要連線的 RD Web 存取伺服器所要使用的使用者名稱。
   - **密碼** - 用於您要連線之 RD Web 存取伺服器的密碼。
3. 點選 [儲存]  。

遠端資源將會顯示於連線中心。

## <a name="manage-your-user-accounts"></a>管理您的使用者帳戶

當您連線至桌面或遠端資源時，您可以儲存使用者帳戶以便再次從中選取。

若要建立新的使用者帳戶：

1. 在 [連線中心] 內，點選 [設定]  ，然後點選 [使用者帳戶]  。
2. 點選 [新增使用者帳戶]  。
3. 輸入下列資訊：
   - **使用者名稱** - 要儲存以用於遠端連線的使用者名稱。 您可以使用下列任何一種格式輸入使用者名稱：user_name、domain\user_name 或 user_name@domain.com。
   - **密碼** - 您指定的使用者所使用的密碼。 您想要儲存以用於遠端連線的每個使用者帳戶都必須有相關聯的密碼。
4. 點選 [儲存]  。

刪除使用者帳戶：

1. 在 [連線中心] 內，點選 [設定]  ，然後點選 [使用者帳戶]  。
2. 選取您要刪除的帳戶。
3. 點選 [刪除]  。

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>連線至 RD 閘道以存取內部資產

遠端桌面閘道 (RD 閘道) 可讓您從網際網路上的任何地方連線到公司網路上的遠端電腦。 您可以使用遠端桌面用戶端來建立和管理閘道。

設定新閘道：

1. 在 [連線中心] 內，點選 [設定]   > [閘道]  。
2. 點選 [新增遠端桌面閘道]  。
3. 輸入下列資訊：
   - **伺服器名稱** - 您想要用來作為閘道的電腦名稱。 這可以是 Windows 電腦名稱、網際網路網域名稱或 IP 位址。 您也可以將連接埠資訊新增至伺服器名稱 (例如：**RDGateway:443** or **10.0.0.1:443**)。
   - **使用者名稱** - 您要連線的遠端桌面閘道所要使用的使用者名稱和密碼。 您也可以選取 [使用連線認證]  ，以使用針對遠端桌面連線而使用的相同使用者名稱和密碼。

## <a name="navigate-the-remote-desktop-session"></a>瀏覽遠端桌面工作階段

當您開始遠端桌面工作階段時，提供一些您可以用來瀏覽工作階段的工具。

### <a name="start-a-remote-desktop-connection"></a>開始遠端桌面連線

1. 點選遠端桌面連線以開始遠端桌面工作階段。
2. 如果系統要求您驗證遠端桌面的憑證，點選 [接受]  。 您也可以將 [不要再詢問我是否連線到此電腦]  切換至 [開啟]  ，一律接受憑證。

### <a name="connection-bar"></a>連線列

連線列可讓您存取其他瀏覽控制項。

- **移動瀏覽控制項**：移動瀏覽控制項可讓畫面放大及四處移動。 請注意，只有直接輕觸才可以使用移動瀏覽控制項。
   - 啟用/停用移動瀏覽控制項：點選連線列中的 [移動瀏覽] 圖示，可顯示移動瀏覽控制項並縮放畫面。 再次點選連線列中的 [移動瀏覽] 圖示可隱藏控制項，並將畫面回復到其原始解析度。
   - 使用移動瀏覽控制項：點選並按住移動瀏覽控制項，然後以您想要移動畫面的方向拖曳。
   - 移動移動瀏覽控制項：點兩下並按住移動瀏覽控制項，將控制項移動到畫面上。
- **連線名稱**：顯示目前的連線名稱。 點選連線名稱可顯示工作階段選取列。
- **鍵盤**：點選 [鍵盤] 圖示以顯示或隱藏鍵盤。 移動瀏覽控制項會在鍵盤顯示時自動顯示。
- **移動連線列**：點選並按住連線列，然後拖放到畫面頂端的新位置。

### <a name="session-selection"></a>工作階段選取

您可以同時向不同的電腦開啟多個連線。 點選連線列可顯示畫面左側的工作階段選取列。 工作階段選取列可讓您檢視已開啟的連線，並在兩者之間切換。

- 在開啟的遠端資源工作階段中的應用程式之間切換。

    當您連線到遠端資源時，可以在該工作階段中點選 [展開工具] 功能表，然後從可用項目的清單中選擇，以便在開啟的應用程式之間切換。
- 啟動新的工作階段

  您可以從目前的連線中啟動新的應用程式或桌面工作階段：點選 [啟動新工作階段]  ，然後從可用項目的清單中選擇。

- 中斷工作階段的連線

  若要中斷工作階段的連線，點選 [工作階段] 圖格左側的 X。

### <a name="command-bar"></a>命令列

從 8.0.1 版開始，命令列會取代公用程式列。 您可以切換滑鼠模式，並返回命令列中的連線中心。

## <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>在遠端工作階段中使用輕觸手勢和滑鼠模式

用戶端會使用標準的輕觸手勢。 您也可以使用輕觸手勢來複寫遠端桌面上的滑鼠動作。 下表中定義可用的滑鼠模式。

> [!NOTE]
> 與 Windows 8 或更新版本互動時，原生輕觸手勢在直接輕觸模式中受到支援。 如需有關 Windows 8 手勢的詳細資訊，請參閱[觸控：撥動、點選等等](https://windows.microsoft.com/windows-8/touch-swipe-tap-beyond)。

| 滑鼠模式    | 滑鼠操作      | 手勢                                                    |
|---------------|----------------------|------------------------------------------------------------|
| 直接觸控  | 按滑鼠左鍵           | 1 指點選                                               |
| 直接觸控  | 按滑鼠右鍵          | 1 指點選並按住                                      |
| 滑鼠指標 | 按滑鼠左鍵           | 1 指點選                                               |
| 滑鼠指標 | 按住滑鼠左鍵並拖曳  | 用 1 指點兩下並按住，然後拖曳                    |
| 滑鼠指標 | 按滑鼠右鍵          | 2 指點選                                               |
| 滑鼠指標 | 按滑鼠右鍵並拖曳 | 用 2 指點兩下並按住，然後拖曳                    |
| 滑鼠指標 | 滑鼠滾輪          | 用 2 指點兩下並按住，然後向上或向下拖曳                |
| 滑鼠指標 | 縮放                 | 捏合 2 隻手指可放大；張開 2 隻手指可縮小 |

## <a name="supported-input-devices"></a>支援的輸入裝置

iOS 13 和 iPadOS 中的基本[藍牙滑鼠支援](https://support.apple.com/HT210546)可作為協助工具功能。 您可以使用 Swiftpoint GT 和 ProPoint 滑鼠來取得 RD 用戶端中更深入的滑鼠整合。 此外，系統也支援與 iOS 和 iPadOS 相容的外部鍵盤。

如需裝置支援的詳細資訊，請參閱 [iOS 用戶端的新功能](ios-whatsnew.md)和 [iOS App Store](https://aka.ms/rdios)。

> [!TIP]
> Swiftpoint 針對適用於 iOS 用戶端的使用者，提供 [ProPoint 滑鼠的獨家折扣](https://www.swiftpoint.com/microsoft)。

## <a name="use-a-keyboard-in-a-remote-session"></a>在遠端工作階段中使用鍵盤

您可以在遠端工作階段中使用螢幕小鍵盤或實體鍵盤。

若是螢幕小鍵盤，請使用鍵盤上方那一列右邊緣的按鈕，在標準與其他鍵盤之間切換。

如果您的 iOS 裝置啟用藍牙，則用戶端會自動偵測藍牙鍵盤。

雖然某些按鍵組合在遠端工作階段中可能無法如預期般運作，但許多常見的 Windows 按鍵組合 (例如 CTRL+C、CTRL+V 和 ALT+TAB) 都可以運作。

> [!IMPORTANT]
> 隨時都歡迎提出問題和意見。 不過，請「勿」使用本文結尾的留言功能來張貼需要疑難排解協助的要求。 請改為前往[遠端桌面用戶端論壇](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc)，並開始新的對話。 有功能建議嗎？ 請在[用戶端使用者意見論壇](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android)中告訴我們。
