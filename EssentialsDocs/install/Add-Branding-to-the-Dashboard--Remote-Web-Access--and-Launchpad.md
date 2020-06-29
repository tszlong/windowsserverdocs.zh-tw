---
title: 將商標新增至儀表板、遠端 Web 存取和啟動列
description: 如何將商標材料新增至您的儀表板、遠端 Web 存取和啟動列畫面。
ms.date: 04/10/2014
ms.prod: windows-server
ms.topic: article
ms.assetid: 166262f8-b2a5-4b1c-a4a7-a141e1c54f10
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 056c7264fc90adbf115c3c6587081a449240a98a
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85471083"
---
# <a name="add-branding-to-the-dashboard-remote-web-access-and-launchpad"></a>將商標新增至儀表板、遠端 Web 存取和啟動列

> 適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

您可以透過新增登錄項目來新增許多商標。 作業系統登錄中的所有商標專案都位於底下 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OEM` 。

針對共同品牌的用途，Windows Server Essentials 標誌的最小寬度必須為**170 圖元**，並保持正確的外觀比例（ **96 DPI**）。

## <a name="to-add-branding-by-changing-the-registry"></a>若要變更登錄來新增商標

1. 在伺服器上，將滑鼠移至畫面的右上角，然後按一下 **[搜尋]**。

2. 在 [搜尋] 方塊中，輸入 **regedit**，然後按一下 [Regedit]**** 應用程式。

3. 在瀏覽窗格中，依序展開 **HKEY_LOCAL_MACHINE**、**SOFTWARE**、**Microsoft** 及 **Windows Server**。 如果**OEM**機碼不存在，您可以建立它：

    1. 以滑鼠右鍵按一下 **[Windows Server]**，按一下 **[新增]**，然後按一下 **[機碼]**。

    2. 輸入 **OEM** 作為機碼名稱。

4. 選擇性如果您要建立標誌的專案，您可以建立不同的金鑰來區別標誌的語言版本。 例如，如果您同時有英文版和德文版的標誌，可以建立 en-us 機碼和 de-de 機碼。 因為所有的標誌檔都會儲存在同一個資料夾，所以您必須提供標誌影像檔的多個執行個體，每個語言的檔案具備唯一的名稱。 例如，您可以建立名為 DashboardLogo_en.png 和 DashboardLogo_de.png 的檔案。

5. 以滑鼠右鍵按一下**OEM** ，或以滑鼠右鍵按一下適當的語言機碼，按一下 [**新增**]，然後按一下 [**字串值**]。

6. 輸入字串的名稱，然後按 ENTER 鍵。 如需有關字串名稱和資料值的詳細資訊，請參閱本文的登錄**字串和值**一節。

7. 輸入字串的名稱，然後按 ENTER 鍵。 如需有關字串名稱和資料值的詳細資訊，請參閱本文的登錄**字串和值**一節。

8. 在滑鼠右鍵上按一下新字串，然後按一下 **[修改]**。

9. 根據該表格輸入與字串名稱相關聯的值，然後按一下 **[確定]**。

10. 如果您要為標誌影像或附加的連結建立專案，請將檔案複製到 `%programFiles%\Windows Server\Bin\OEM` 。 如果 OEM 目錄不存在，請建立該目錄。

11. 如果您在客戶擁有伺服器之後對遠端 Web 存取進行變更，則必須透過下列方式開啟遠端 Web 存取：

    1. 在儀表板上，按一下 **[設定]**，然後按一下 **[隨處存取]** 索引標籤。

    2. 如果已開啟 [**隨處存取**]，**請按一下 [** 設定]，然後在 [**設定隨處存取**嚮導] 的 [**選擇要啟用的隨處存取功能**] 頁面上，清除 [遠端 Web 存取] 核取方塊。

    3. 按一下 [設定]****。

### <a name="registry-strings-and-values"></a>登錄字串和值

下表提供可用字串名稱和資料值的相關資訊。

| 商標位置 | 描述 | 名稱字串 | 資料值 |
|--|--|--|--|
| 儀表板標誌 | 將標誌影像新增到儀表板。 儀表板標誌必須是 .png 格式，而且寬高比不得大於 350 像素 x 38 像素。<p>**重要事項：** 若要將儀表板與您的標誌共同品牌，您必須編輯 OPK DVD 上提供的 [插圖] 磚，並在遵循適當的空白字元需求時，將您的公司標誌附加至影像。 | DashboardLogo | 輸入標誌影像檔的名稱。 |
| DashboardClientLogo | 將標誌影像新增至儀表板用戶端登入畫面。 | DashboardClientLogo | 輸入標誌影像檔的名稱。 |
| 網站背景圖片 | 變更 [遠端 Web 存取登入] 頁面上顯示的背景影像。 一般的解析度會如下列方式顯示：<ul><li>**1024x768 圖元**-此解析度會精確填滿登入頁面。</li><li>**800x600 圖元**-此解析度會將影像中央放在頁面上，並以黑色框線顯示。</li><li>**1280x720 圖元**-此解析度會將影像置中。 超出1024x720 的任何圖元都不會出現。 | LogonBackground | 輸入背景影像檔案的名稱。 |
| 網站標題 | 將遠端 Web 存取網站的標題從 Windows Server Essentials 取代為您指定的標題。 | WebsiteName | 輸入新的遠端 Web 存取網站標題。 |
| 網站標誌 | 變更遠端 Web 存取網站上的預設標誌。 預期的標誌大小是 32 x 32 像素。 如果您的標誌小於或等於此，則會伸展或縮小以符合這些維度。 | WebsiteLogo | 輸入標誌影像檔的名稱 |
| 附加的網站標誌 | 您的合作夥伴標誌將會顯示在出現在 [遠端 Web 存取] 網站上的 Microsoft 標誌正下方。 預期的標誌大小寬高比是 200 像素 x 50 像素。 如果您的標誌大於這個大小，標誌將會依照原始外觀比例縮小成符合這個大小。 如果您的標誌小於這個大小，標誌將會置於這個 200 x 50 像素空間的中央，且不會變更大小或外觀比例。 | OEMLogo | 輸入標誌影像檔的名稱。 |
| 網站首頁和登入頁面上的連結 | 將連結附加至遠端 Web 存取網站的 [登入] 頁面和 [首頁]。 包含連結資訊的 XML 檔案必須位於 `%programFiles%\Windows Server\Bin\OEM` 資料夾中。 | LinksXML | 請參考 LinksXML 元素表格，取得元件與描述的清單。 |
| 啟動列標誌 | 將標誌影像新增到啟動列。 啟動列標誌必須是 .png 格式並且不能高於 64 像素。 | LaunchpadLogo | 輸入標誌影像檔的名稱。 |

#### <a name="xml-linking-format"></a>XML 連結格式

您必須將網站**首頁**和登入頁面上的連結格式化為：

```xml
<OemLinks>
    <LogonLinks>
        <Link Name=LogonLinkName>
        <Text>LogonLinkDescription</Text>
        <Url>LogonLinkURL</Url>
        <Icon>LinkIcon</Icon>
        </Link>
    </LogonLinks>

    <HomepageLinks>
        <Link Name=HomepageLinkName>
        <Text>HomepageLinkDescription</Text>
        <Url>HomepageLinkURL</Url>
        <Icon>HomepageLinkIcon</Icon>
        </Link>
    </HomepageLinks>
</OemLinks>
```

### <a name="linksxml-elements"></a>LinksXML 元素

| LinksXML 元素 | 描述 |
|--|--|
| LogonLinks | 登入連結的父專案。 |
| 連結名稱 | 登入連結名稱。 |
| Text | 顯示為登入頁面連結的文字。 |
| URL | 解析為登入頁面連結的 URL。 |
| 圖示 | 登入連結的圖示檔案名。 這個檔案應與 .xml 檔案放在相同的資料夾位置中。 圖示影像應為16x16 圖元和 .png 格式。 如果您未提供圖示，則會使用預設連結圖示影像。 |
| HomepageLinks | **首頁**的父專案。 |
| 連結名稱 | 首頁**連結**名稱。 |
| Text | 顯示為**首頁**連結的文字。 |
| URL | 解析為**首頁**連結的 URL。 |
| 圖示 | **首頁**連結的圖示檔名稱。 這個檔案應與 .xml 檔案放在相同的資料夾位置中。 圖示影像應為16x16 圖元和 .png 格式。 如果您未提供圖示，則會使用預設的**首頁**連結圖示影像。 |

## <a name="additional-references"></a>其他參考資料

- [建立和自訂映像](Creating-and-Customizing-the-Image.md)

- [其他自訂項目](Additional-Customizations.md)

- [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)

- [測試客戶經驗](Testing-the-Customer-Experience.md)