---
title: 開始使用 macOS 用戶端
description: 了解如何設定適用於 Mac 的遠端桌面用戶端
ms.topic: article
ms.assetid: 7afc65f8-3158-49c9-9d48-4dab1c69afba
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 07/16/2020
ms.localizationpriority: medium
ms.openlocfilehash: 2a0c93ddc6bf891a3f8157d36b67f60241d4d41b
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87991598"
---
# <a name="get-started-with-the-macos-client"></a>開始使用 macOS 用戶端

>適用於：Windows 10、Windows 8.1、Windows Server 2012 R2、Windows Server 2016

您可以使用適用於 Mac 的遠端桌面用戶端，從您的 Mac 電腦使用 Windows 應用程式、資源及桌面。 若要開始使用，請使用下列資訊 - 如有任何問題，請參閱[常見問題集](remote-desktop-client-faq.md)。

>[!NOTE]
> - 想知道適用於 macOS 用戶端的新版本嗎？ 請參閱 [macOS 上的遠端桌面有什麼新功能？](mac-whatsnew.md)
> - Mac 用戶端須在執行 macOS 10.10 及更新版本的電腦上執行。
> - 本文中的資訊主要適用於完整版的 Mac 用戶端 - Mac AppStore 中提供的版本。 請下載我們此處的預覽應用程式以試用新功能：[Beta 用戶端版本資訊](https://go.microsoft.com/fwlink/?LinkID=619698&clcid=0x409)。

## <a name="get-the-remote-desktop-client"></a>取得遠端桌面用戶端

依照下列步驟，在 Mac 上開始使用遠端桌面：

1. 從 [Mac App Store](https://itunes.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12) 下載 Microsoft 遠端桌面用戶端。
2. [將您的電腦設定成接受遠端連線](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop)。 (如果您略過此步驟，即無法連線至您的電腦。)
3. 新增遠端桌面連線或遠端資源。 您可以使用連線直接連線至 Windows 電腦和遠端資源，以使用 RemoteApp 程式、工作階段型桌面，或使用 RemoteApp 和桌面連線在內部部署發佈的虛擬桌面。 在公司環境中通常可以使用這項功能。

## <a name="what-about-the-mac-beta-client"></a>Mac Beta 用戶端呢？

我們正在 AppCenter 的預覽通道上測試新功能。 想一探究竟嗎？ 請前往[適用於 Mac 的 Microsoft 遠端桌面](https://aka.ms/rdmacbeta)，然後選取 [下載]。 您不需要建立帳戶或登入 AppCenter，即可下載 Beta 用戶端。

如果您已有用戶端，您可以檢查更新，以確定您擁有最新版本。 在 Beta 用戶端中，選取頂端的 [Microsoft 遠端桌面 Beta 版]，然後選取 [檢查更新]。

## <a name="add-a-workspace"></a>新增工作區

訂閱系統管理員提供給您的摘要，以取得可供您在 macOS 裝置上使用的受控資源清單。

若要訂閱摘要：

1. 選取主頁面上的 [新增摘要]，以連線至服務並擷取您的資源。
2. 輸入摘要 URL。 這可以是 URL 或電子郵件地址：
   - 此 URL 通常是 Windows 虛擬桌面 URL。 您要使用的版本取決於正在使用的 Windows 虛擬桌面版本。
      - 若為 2019 秋季版，請使用 `https://rdweb.wvd.microsoft.com/api/feeddiscovery/webfeeddiscovery.aspx`。
      - 若為 2020 春季版，請使用 `https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery`。
   - 若要使用電子郵件，請輸入您的電子郵件地址。 這會告訴用戶端搜尋與您的電子郵件地址相關聯的 URL (如果系統管理員是這樣設定伺服器的話)。
3. 選取 [訂閱]。
4. 出現提示時，使用您的使用者帳戶登入。

登入之後，您應該會看到可用資源的清單。

訂閱摘要之後，摘要的內容便會定期自動更新。 根據系統管理員所做的變更，可能會新增、變更或移除資源。

### <a name="export-and-import-connections"></a>匯出和匯入連線

您可以匯出遠端桌面連線定義，並在不同的裝置上加以使用。 遠端桌面會儲存在個別的 RDP 檔案中。

若要匯出 RDP 檔案：

1. 在連線中心，以滑鼠右鍵按一下遠端桌面。
2. 選取 [匯出]。
3. 瀏覽至要儲存遠端桌面 RDP 檔案的位置。
4. 選取 [確定]。

若要匯入 RDP 檔案：

1. 在功能表列中，選取 [檔案] > [匯入]。
2. 瀏覽至 RDP 檔案。
3. 選取 [開啟]。

## <a name="add-a-remote-resource"></a>新增遠端資源

遠端資源包括 RemoteApp 程式、工作階段型桌面，以及使用 RemoteApp 與桌面連線發佈的虛擬桌面。

- URL 會顯示可讓您存取 RemoteApp 與桌面連線的 RD Web 存取伺服器的連結。
- 系統會列出已設定的 RemoteApp 與桌面連線。

若要新增遠端資源：

1. 在 [連線中心] 中，選取 [+]，然後選取 [新增遠端資源]。
2. 輸入遠端資源的資訊：
   - **摘要 URL** - RD Web 存取伺服器的 URL。 您也可以在此欄位中輸入您的公司電子郵件帳戶，如此會告訴用戶端搜尋與您電子郵件地址相關聯的 RD Web 存取伺服器。
   - **使用者名稱** - 您要連線的 RD Web 存取伺服器所要使用的使用者名稱。
   - **密碼** - 您要連線的 RD Web 存取伺服器所要使用的密碼。
3. 選取 \[儲存\]。

遠端資源將會顯示於連線中心。

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>連線至 RD 閘道以存取內部資產

遠端桌面閘道 (RD 閘道) 可讓您從網際網路上的任何位置連線至公司網路上的遠端電腦。 您可以在應用程式的喜好設定中，或是在設定新的桌面連線時建立和管理閘道。

若要在喜好設定中設定新的閘道：

1. 在連線中心，選取 [喜好設定 > 閘道]。
2. 選取表格底部的 **+** 按鈕，並輸入下列資訊：
   - **伺服器名稱** - 您想要用來作為閘道的電腦名稱。 這可以是 Windows 電腦名稱、網際網路網域名稱或 IP 位址。 您也可以將連接埠資訊新增至伺服器名稱 (例如：**RDGateway:443** 或 **10.0.0.1:443**)。
   - **使用者名稱** - 您要連線的遠端桌面閘道所要使用的使用者名稱和密碼。 您也可以選取 [使用連線認證]，以使用針對遠端桌面連線而使用的相同使用者名稱和密碼。

## <a name="manage-your-user-accounts"></a>管理您的使用者帳戶

當您連線至桌面或遠端資源時，您可以儲存使用者帳戶以便再次從中選取。 您可以使用遠端桌面用戶端來管理您的使用者帳戶。

若要建立新的使用者帳戶：

1. 在連線中心選取 [設定] > [帳戶]。
2. 選取 [新增使用者帳戶]。
3. 輸入下列資訊：
   - **使用者名稱** - 要儲存以用於遠端連線的使用者名稱。 您可以使用下列任何一種格式輸入使用者名稱：user_name、domain\user_name 或 user_name@domain.com。
   - **密碼** - 您指定的使用者所使用的密碼。 您想要儲存以用於遠端連線的每個使用者帳戶都必須有相關聯的密碼。
   - **易記名稱** - 如果您使用具有不同密碼的相同使用者帳戶，請設定易記名稱以區別這些使用者帳戶。
4. 選取 [儲存]，然後選取 [設定]。

## <a name="customize-your-display-resolution"></a>自訂顯示器解析度

您可以指定遠端桌面工作階段的顯示器解析度。

1. 在連線中心選取 [喜好設定]。
2. 選取 [解析度]。
3. 選取 [+]。
4. 輸入解析度的高度和寬度，然後選取 [確定]。

若要刪除解析度，請加以選取，然後選取 [-]。

## <a name="displays-have-separate-spaces"></a>顯示器有個別空間

如果您正在執行 Mac OS X 10.9，並且在 Mavericks 中 ([系統喜好設定] > [任務控制項]) 停用了 [顯示器有個別空間]，則必須在遠端桌面用戶端中使用相同選項設定這項設定。

### <a name="drive-redirection-for-remote-resources"></a>遠端資源的磁碟機重新導向

磁碟機重新導向支援遠端資源，因此您可以在本機將透過遠端應用程式建立的檔案儲存至您的 Mac。 重新導向的資料夾一律為您在遠端工作階段中顯示為主目錄的網路磁碟機。

> [!NOTE]
> 若要使用這項功能，系統管理員必須在伺服器上設定適當的設定。

## <a name="use-a-keyboard-in-a-remote-session"></a>在遠端工作階段中使用鍵盤

Mac 的鍵盤配置與 Windows 的鍵盤配置不同。

- Mac 鍵盤上的 Command 鍵等同於 Windows 鍵。
- 若要在 Mac 上執行使用 Command 按鈕的動作，則必須使用 Windows 中的 Control 按鈕 (例如：Copy = Ctrl+C)。
- 同時按下 FN 鍵 (例如：FN+F1) 則可在工作階段中啟用功能鍵。
- 在 Mac 鍵盤上，空格鍵右側的 Alt 鍵等同於 Windows 中的 Alt Gr/右 Alt 鍵。

根據預設，遠端工作階段會使用與您執行用戶端所在的 OS 相同的鍵盤地區設定。 (如果您的 Mac 執行 en-us OS，則遠端工作階段也會使用 en-us。)如果未使用 OS 鍵盤地區設定，請檢查遠端電腦上的鍵盤設定，並手動進行變更。 如需鍵盤和地區設定的詳細資訊，請參閱[遠端桌面用戶端常見問題集](remote-desktop-client-faq.md)。

## <a name="support-for-remote-desktop-gateway-pluggable-authentication-and-authorization"></a>支援遠端桌面閘道插入式驗證和授權

Windows Server 2012 R2 導入了新式驗證方法的支援，也就是遠端桌面閘道插入式驗證和授權，可為自訂驗證常式提供更大的彈性。 現在，您可以在 Mac 用戶端試用此驗證模型。

> [!IMPORTANT]
> 雖然本文的前半段有所討論，但 Windows 8.1 之前的自訂驗證和授權模型不受支援。

若要深入了解這項功能，請參閱 [https://aka.ms/paa-sample](https://aka.ms/paa-sample)。

> [!TIP]
> 隨時都歡迎提出問題和意見。 不過，請「勿」使用本文結尾的留言功能來張貼需要疑難排解協助的要求。 請改為前往[遠端桌面用戶端論壇](/answers/topics/windows-remote-desktop-client.html)，並開始新的對話。 有功能建議嗎？ 請在[用戶端使用者意見論壇](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android)中告訴我們。