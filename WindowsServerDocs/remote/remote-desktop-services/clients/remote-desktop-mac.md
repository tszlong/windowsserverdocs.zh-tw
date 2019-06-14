---
title: 開始使用在 Mac 上的遠端桌面
description: 了解如何設定遠端桌面用戶端的 Mac
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7afc65f8-3158-49c9-9d48-4dab1c69afba
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 10/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: a54ffd3d5596ba8c71deab668e4952da445ca12e
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66804937"
---
# <a name="get-started-with-remote-desktop-on-mac"></a>開始使用在 Mac 上的遠端桌面

>適用於：Windows 10，Windows 8.1，Windows Server 2012 R2 中，Windows Server 2016

您可以使用 Mac 的遠端桌面用戶端來從您的 Mac 電腦使用 Windows 應用程式、 資源及桌上型電腦。 使用下列資訊來開始建置，並查看[常見問題集](remote-desktop-client-faq.md)如有任何問題。

>[!NOTE]
> - 想知道 macOS 用戶端的新版本嗎？ 請參閱[Mac 上的遠端桌面最新消息？](mac-whatsnew.md)
> - 執行 macOS 10.10 及更新版本的電腦上，執行 Mac 用戶端。
> - 這篇文章中的資訊主要適用於完整版的 Mac 用戶端-Mac AppStore 中可用的版本。 下載我們預覽應用程式中試用新功能： [beta 用戶端版本資訊](https://go.microsoft.com/fwlink/?LinkID=619698&clcid=0x409)。

## <a name="get-the-remote-desktop-client"></a>取得遠端桌面用戶端
請遵循下列步驟來開始使用您的 Mac 上的遠端桌面：

1. 下載的 Microsoft 遠端桌面用戶端[Mac App Store](https://itunes.apple.com/us/app/microsoft-remote-desktop/id1295203466?mt=12)。
2. [您的電腦設定成接受遠端連接](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop)。 （如果您略過此步驟中，您無法連接到您的電腦）。
3. 將遠端桌面連線或存取遠端資源。 您可以使用連線來直接連接至 Windows 電腦和遠端資源，若要使用的 RemoteApp 程式、 工作階段為基礎的桌面，或虛擬桌面發佈內部部署使用 RemoteApp 與桌面連線。 在公司環境中通常使用這項功能。

## <a name="what-about-the-mac-beta-client"></a>Mac beta 用戶端呢？
我們在 HockeyApp 我們預覽通道上測試新功能。 想要簽出嗎？ 移至[適用於 Mac 的 Microsoft 遠端桌面](https://go.microsoft.com/fwlink/?LinkID=619698)然後按一下**下載**。 您不需要建立帳戶或登入以下載 beta 版的用戶端的 HockeyApp。

如果您已經在用戶端，您可以檢查更新，以確保您擁有的最新版本。 在 beta 用戶端中，按一下**Microsoft 遠端桌面 Beta**上方，，然後按一下**檢查是否有更新**。 

## <a name="add-a-remote-desktop-connection"></a>新增遠端桌面連線
若要建立遠端桌面連線：

1. 在 「 連線中心 」 中，按一下 **+** ，然後按一下**桌面**。
2. 輸入下列資訊：
   - **電腦名稱**-電腦的名稱。
      - 這可以是 Windows 的電腦名稱 (在中找到**系統**設定)，使用網域名稱或 IP 位址。
      - 您也可以將連接埠資訊加入這個名稱，結尾像是*MyDesktop:3389*。
   - **使用者帳戶**-新增您用來存取遠端電腦的使用者帳戶。
     - 電腦或本機帳戶，已加入 Active Directory (AD)，使用其中一種格式： *user_name*， *domain\user_name*，或<em>user_name@domain.com</em>。
     - 針對 Azure Active Directory (AAD) 加入的電腦，使用其中一種格式：*AzureAD\user_name*或是<em>AzureAD\user_name@domain.com</em>。
     - 您也可以選擇是否需要密碼。
     - 在管理多個具有相同的使用者名稱的使用者帳戶時，設定來區分之帳戶的易記名稱。
     - 管理應用程式的喜好設定中儲存的使用者帳戶。 

3. 您也可以設定連接這些選擇性設定：
   - 設定好記的名稱 
   - 新增閘道
   - 將聲音輸出設定
   - 交換滑鼠按鈕
   - 啟用管理員模式
   - 本機資料夾重新導向到遠端工作階段
   - 正向的本機印表機
   - 正向的智慧卡
4. 按一下 [儲存]  。

若要啟動的連線，只要連按兩下它。 這也適用於的遠端資源。

### <a name="export-and-import-connections"></a>匯出和匯入連線
您可以匯出的遠端桌面連線的定義，並使用不同的裝置上。 遠端桌面會儲存在不同。RDP 檔案。

1. 在 「 連線中心 」 中，以滑鼠右鍵按一下的遠端桌面。
2. 按一下 **匯出**。
3. 瀏覽至您要儲存的遠端桌面的位置。RDP 檔案。
4. 按一下 [確定]  。

您可以使用下列步驟，匯入遠端桌面。RDP 檔案。

1. 在功能表列中，按一下**檔案** > **匯入**。
2. 瀏覽至。RDP 檔案。
3. 按一下 [開啟]  。

## <a name="add-a-remote-resource"></a>新增遠端資源
遠端資源是 RemoteApp 程式、 工作階段為基礎的桌面及發佈使用 RemoteApp 與桌面連線的虛擬桌面。

- URL 會顯示連結，可讓您存取 RemoteApp 與桌面連線的 RD Web 存取伺服器。
- 設定 RemoteApp 與桌面連線會列出。

若要新增的遠端資源：

1. 在 「 連線中心 」 中，按一下 **+** ，然後按一下**新增遠端資源**。 
2. 輸入遠端資源的資訊：
   - **摘要 URL** -RD Web 存取伺服器的 URL。 您也可以輸入您的公司電子郵件帳戶，在此欄位 – 這會告訴用戶端搜尋與您的電子郵件地址相關聯的 RD Web 存取伺服器。
   - **使用者名稱**-要用於您要連接到 RD Web 存取伺服器的使用者名稱。
   - **密碼**-針對您要連接到 RD Web 存取伺服器使用的密碼。
3. 按一下 [儲存]  。


遠端資源將會顯示在 「 連線中心。


## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>連接至 RD 閘道存取內部資產

遠端桌面閘道 （RD 閘道） 可讓您從任何地方連線到公司網路上的遠端電腦在網際網路上。 您可以建立和管理您的應用程式，或設定新的桌面連線時，喜好設定中的閘道。

若要設定新的閘道，在 喜好設定：

1. 在 「 連線中心 」 中，按一下**喜好設定 > 閘道**。 
2. 按一下  **+** 按鈕底部的資料表輸入下列資訊：
   - **伺服器名稱**– 您想要作為閘道使用的電腦名稱。 這可以是 Windows 電腦名稱、 網際網路網域名稱或 IP 位址。 您也可以新增連接埠資訊的伺服器名稱 (例如：**RDGateway:443**或是**10.0.0.1:443**)。
   - **使用者名稱**-使用者名稱和密碼用於您所連接的遠端桌面閘道。 您也可以選取**使用的連接認證**所使用的遠端桌面連線使用的相同使用者名稱和密碼。


## <a name="manage-your-user-accounts"></a>管理您的使用者帳戶

當您連線到桌面或遠端資源時，您可以儲存一次從選取的使用者帳戶。 您可以使用遠端桌面用戶端來管理您的使用者帳戶。

若要建立新的使用者帳戶：

1. 在 「 連線中心 」 中，按一下**設定** > **帳戶**。
2. 按一下 **新增使用者帳戶**。
3. 輸入下列資訊：
   - **使用者名稱**-要儲存以供使用，透過遠端連線的使用者名稱。 您可以輸入使用者名稱，任何下列格式： user_name domain\user_name，或user_name@domain.com。
   - **密碼**-您指定的使用者的密碼。 您想要將儲存到使用遠端連線的每個使用者帳戶必須具有與其相關聯的密碼。
   - **易記名稱**-如果您使用相同的使用者帳戶與不同的密碼，設定易記的名稱，以便區別這些使用者帳戶。
4. 點選**儲存**，然後點選**設定**。

## <a name="customize-your-display-resolution"></a>自訂顯示解析度
您可以在遠端桌面工作階段中指定的顯示解析度。

1. 在 「 連線中心 」 中，按一下**喜好設定**。
2. 按一下 **解析度**。 
3. 按一下  **+** 。
4. 解析高度和寬度，輸入，然後按一下 **[確定]。**

若要刪除的解析度，請加以選取，然後再按一下 **-** 。

** 蒩厞柈個別空格**如果您執行 Mac OS X 10.9 和停用** 蒩厞柈個別空格**Mavericks 中 (**系統喜好設定 > 的任務控制**)，您需要設定這項設定遠端桌面用戶端使用相同的選項。

### <a name="drive-redirection-for-remote-resources"></a>遠端資源的磁碟機重新導向
磁碟機重新導向支援遠端資源，以便您可以儲存檔案的建立與遠端應用程式在本機上至您的 mac。 重新導向的資料夾一律是您在遠端工作階段中顯示為的網路磁碟機的主目錄。

> [!NOTE]
> 若要使用這項功能，系統管理員必須在伺服器上設定適當的設定。


## <a name="use-a-keyboard-in-a-remote-session"></a>在 遠端工作階段中使用鍵盤

Mac 鍵盤配置與不同的 Windows 的鍵盤配置。 

- Mac 鍵盤上的 Command 機碼值等於 Windows 鍵。
- 若要執行在 Mac 使用命令按鈕的動作，您必須使用 Windows 中的控制項按鈕 (例如：複製 = Ctrl + C)。
- 功能鍵可以啟動工作階段中，按此外 FN 鍵 (例如：FN + F1）。
- Mac 鍵盤上的 [空間] 列右邊的 Alt 鍵等於 Alt Gr/右 Alt 鍵，在 Windows 中。

根據預設，遠端工作階段會使用相同的鍵盤地區設定做為您要執行用戶端的 OS。 (如果您的 Mac 執行的 en-us-我們 OS，用於遠端工作階段。 如果未使用的 OS 鍵盤地區設定，請檢查鍵盤在遠端電腦上設定和變更設定以手動方式。 請參閱[遠端桌面用戶端常見問題集](remote-desktop-client-faq.md)的鍵盤和地區設定的詳細資訊。


## <a name="support-for-remote-desktop-gateway-pluggable-authentication-and-authorization"></a>支援遠端桌面閘道的隨插即用驗證和授權

Windows Server 2012 R2 引進了新的驗證方法、 遠端桌面閘道的隨插即用驗證和授權，可提供更大的彈性的自訂驗證常式的支援。 您現在可以使用 Mac 用戶端驗證模型。 

> [!IMPORTANT]
> 不支援 Windows 8.1 之前的自訂驗證和授權模型，但上述文章討論它們。

若要深入了解這項功能，請參閱[ http://aka.ms/paa-sample ](http://aka.ms/paa-sample)。


> [!TIP]
> 提出問題或意見都歡迎畫面。 不過，請不會回傳要求，以取得疑難排解說明，在這篇文章結尾處使用的註解功能。 相反地，請移至[遠端桌面用戶端論壇](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc)並啟動新的執行緒。 有功能建議嗎？ 告訴我們在[用戶端使用者之聲論壇](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android)。

