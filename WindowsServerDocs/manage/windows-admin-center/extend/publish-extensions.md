---
title: 發行 Windows 管理中心的擴充功能
description: '發行 Windows 系統管理中心的擴充功能 (Project 檀香山) '
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 910ed2733d01fe502a93d43f46530d781ba8c7e5
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87992698"
---
# <a name="publishing-extensions"></a>發行延伸模組

>適用於：Windows Admin Center、Windows Admin Center 預覽版

開發擴充功能之後，您會想要發佈並讓其他人可以進行測試或使用。 根據您的物件和發佈目的而定，我們將在下面介紹一些選項，以及發行的步驟和需求。

## <a name="publishing-options"></a>發行選項

Windows 系統管理中心支援的可設定套件來源有三個主要選項：
* Microsoft 的公用 Windows 管理中心 NuGet 摘要
* 您自己的私用 NuGet 摘要
* 本機或網路檔案共用

### <a name="publishing-to-the-windows-admin-center-extension-feed"></a>發行至 Windows 管理中心延伸模組摘要

根據預設，Windows 管理中心會連線到 Microsoft Windows 管理中心產品小組所維護的 NuGet 摘要。 Microsoft 所開發的新擴充功能的早期預覽版本可能會發佈至此摘要，並可供 Windows 管理中心使用者使用。 計畫公開建立和發行延伸模組的外部開發人員，也可能會提交發佈到此摘要[的要求](#publishing-your-extension-to-the-windows-admin-center-feed)。

### <a name="publishing-to-a-different-nuget-feed"></a>發行至不同的 NuGet 摘要

您也可以建立自己的 NuGet 摘要來發行您的擴充功能，以使用[設定私人來源或使用 NuGet 主機服務的許多不同選項](/nuget/hosting-packages/overview)之一。 NuGet 摘要必須支援 NuGet v2 API。 由於 Windows 管理中心目前不支援摘要驗證，因此必須將摘要設定為允許任何人的讀取權限。

### <a name="publishing-to-a-file-share"></a>發行至檔案共用

若要將您的延伸模組存取限制為您的組織或有限的人員群組，您可以使用 SMB 檔案共用作為擴充功能摘要。 在此情況下，將會套用檔案共用和資料夾的許可權，以允許存取摘要。

## <a name="preparing-your-extension-for-release"></a>準備發行的擴充功能

請確定您已閱讀並考慮下列開發主題：

- [控制您的工具可見性](guides/dynamic-tool-display.md)
- [字串和當地語系化](guides/strings-localization.md)

### <a name="consider-releasing-as-a-preview-release"></a>考慮以預覽版本發行

如果您要發行延伸模組的預覽版本以供評估之用，我們建議您：

- 在 nuspec 檔案中，將 " (Preview) " 附加至延伸模組標題的結尾
- 說明 nuspec 檔案中延伸模組描述的限制

## <a name="creating-an-extension-package"></a>建立擴充功能套件

Windows 管理中心會利用 NuGet 套件和摘要來散發和下載延伸模組。  為了讓您的套件出貨，您必須產生包含外掛程式和擴充功能的 NuGet 套件。  單一套件可以同時包含 UI 延伸模組和閘道外掛程式，下一節將逐步引導您完成整個程式。

### <a name="1-build-your-extension"></a>1. 建立擴充功能

當您準備好要開始封裝延伸模組時，請在您的檔案系統上建立新的目錄，開啟主控台，然後將它放入其中。  這會是我們將用來包含所有 nuspec 和內容目錄的根目錄，而這些目錄會構成封裝。  在此檔的持續期間，我們將以「NuGet 套件」的形式參考此資料夾。

#### <a name="ui-extensions"></a>UI 延伸模組

若要開始收集 UI 延伸模組所需的所有內容，請在您的工具上執行 "gulp build"，並確認組建成功。  此程式會將所有元件一起封裝在一個名為「配套」的資料夾中，位於您擴充功能的根目錄中， (位於相同的 src 目錄層級) 。  將此目錄及其所有內容複寫到 [NuGet 套件] 資料夾。

#### <a name="gateway-plugins"></a>閘道外掛程式

使用您的組建基礎結構 (這可以簡單地開啟 Visual Studio 然後按一下 [組建] 按鈕) 、編譯及建立您的外掛程式。  開啟您的組建輸出目錄，並複製代表您外掛程式的 Dll () ，並將它們放在名為 "Package" 的「NuGet 套件」目錄內的新資料夾中。  您不需要複製 FeatureInterface dll，只有代表程式碼的 Dll () 。

### <a name="2-create-the-nuspec-file"></a>2. 建立 nuspec 檔案

若要建立 NuGet 封裝，您必須先建立 nuspec 檔案。 Nuspec 檔案是包含 NuGet 套件中繼資料的 XML 資訊清單。 此資訊清單用來建置套件和向取用者提供資訊。  將此檔案放在「NuGet 套件」資料夾的根目錄。

以下是 nuspec 檔案的範例，以及必要或建議屬性的清單。 如需完整的架構，請參閱[nuspec 參考](/nuget/reference/nuspec)。 將 nuspec 檔案儲存到您的專案根資料夾中，並使用您選擇的檔案名。

> [!IMPORTANT]
> ```<id>```Nuspec 檔案中的值必須符合專案檔案中的 ```"name"``` 值 ```manifest.json``` ，否則您發行的延伸模組將不會在 Windows 系統管理中心成功載入。

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="https://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
  <metadata>
    <id>contoso.project.extension</id>
    <version>1.0.0</version>
    <title>Contoso Hello Extension</title>
    <authors>Contoso</authors>
    <owners>Contoso</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <projectUrl>https://msft-sme.myget.org/feed/windows-admin-center-feed/package/nuget/contoso.sme.hello-extension</projectUrl>
    <licenseUrl>http://YourLicenseLink</licenseUrl>
    <iconUrl>http://YourLogoLink</iconUrl>
    <description>Hello World extension by Contoso</description>
    <copyright>(c) Contoso. All rights reserved.</copyright>
    <tags></tags>
  </metadata>
  <files>
    <file src="bundle\**\*.*" target="ux" />
    <file src="package\**\*.*" target="gateway" />
  </files>
</package>
```

#### <a name="required-or-recommended-properties"></a>必要或建議的屬性

| 屬性名稱 | 必要/建議 | 描述 |
| ---- | ---- | ---- |
| packageType | 必要 | 使用 "WindowsAdminCenterExtension"，這是針對 Windows 管理中心延伸模組所定義的 NuGet 套件類型。 |
| id | 必要 | 摘要內的唯一封裝識別碼。 這個值必須符合專案的 manifest.js檔案中的 "name" 值。  如需指導方針，請參閱[選擇唯一的套件識別碼](/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number)。 |
| title | 發行至 Windows 系統管理中心摘要所需 | 顯示在 Windows 管理中心擴充管理員中之套件的易記名稱。 |
| version | 必要 | 延伸模組版本。 建議使用[ (SemVer 慣例) 的語義版本](http://semver.org/spec/v1.0.0.html)設定，但並非必要。 |
| authors | 必要 | 如果代表您的公司發行，請使用您的公司名稱。 |
| description | 必要 | 提供延伸模組功能的描述。 |
| iconUrl | 發行至 Windows 系統管理中心摘要時建議使用 | 要在擴充管理員中顯示的圖示 URL。 |
| projectUrl | 發行至 Windows 系統管理中心摘要所需 | 擴充功能網站的 URL。 如果您沒有個別的網站，請使用 NuGet 摘要上封裝網頁的 URL。 |
| licenseUrl | 發行至 Windows 系統管理中心摘要所需 | 延伸模組之使用者授權合約的 URL。 |
| files | 必要 | 這兩個設定會設定 Windows 管理中心所預期的資料夾結構，以供 UI 延伸模組和閘道外掛程式之用。 |

### <a name="3-build-the-extension-nuget-package"></a>3. 建立擴充功能 NuGet 封裝

使用您在上面建立的 nuspec 檔案，您現在將建立 NuGet nupkg 檔案，您可以將其上傳併發布至 NuGet 摘要。

1. 從[NuGet 用戶端工具網站](/nuget/install-nuget-client-tools)下載 nuget.exe CLI 工具。
2. 執行 "nuget.exe pack [. nuspec file name]" 以建立 nupkg 檔案。

### <a name="4-signing-your-extension-nuget-package"></a>4. 簽署您的擴充功能 NuGet 套件

延伸模組中包含的任何 .dll 檔案都必須使用來自受信任憑證授權單位單位 (CA) 的憑證進行簽署。 根據預設，當 Windows 管理中心在生產模式中執行時，不會封鎖未簽署的 .dll 檔案。

我們也強烈建議您簽署延伸模組 NuGet 封裝，以確保封裝的完整性，但這不是必要的步驟。

### <a name="5-test-your-extension-nuget-package"></a>5. 測試延伸模組 NuGet 套件

您的延伸模組套件現在已準備好進行測試！ 將 nupkg 檔案上傳至 NuGet 摘要，或將它複製到檔案共用。 若要從不同的摘要或檔案共用中查看及下載套件，您必須[變更](../configure/using-extensions.md#installing-extensions-from-a-different-feed)摘要設定以指向您的 NuGet 摘要或檔案共用。 測試時，請確定屬性會正確顯示在 [擴充管理員] 中，而且您可以成功安裝並卸載擴充功能。

## <a name="publishing-your-extension-to-the-windows-admin-center-feed"></a>將您的擴充功能發行至 Windows 管理中心摘要

藉由發行至 Windows 管理中心摘要，您可以將擴充功能提供給任何 Windows 系統管理中心使用者。 由於 Windows 管理中心 SDK 仍處於預覽狀態，因此我們想要與您密切合作，以協助解決開發問題，並確保您能夠為使用者提供品質的產品和體驗。

發行延伸模組的初始版本之前，建議您先將延伸模組審查要求提交至 Microsoft 至少2-3 個星期，以確保我們有足夠的時間來進行審查，並視需要對您的擴充功能進行任何變更。 當您的延伸模組準備好要發佈之後，您必須將它傳送給我們以供審查，並在核准之後，將其發佈至您的摘要。

之後，如果您想要發行延伸模組的更新，就必須提交另一個審查要求。 根據變更範圍而定，更新審查的周轉時間通常應該較短。

### <a name="submit-an-extension-review-request-to-microsoft"></a>提交延伸模組審查要求給 Microsoft

若要提交延伸模組審查要求，請輸入下列資訊，並以電子郵件傳送至 [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request) 。 我們會在一周內回復您的電子郵件。

```
Windows Admin Center Extension Review Request
1. Name and email address of extension owner/developer (up to 3 users). If you will be releasing an extension on behalf of your company, provide your company email address.
2. Company name (Only required if you are releasing an extension on behalf of your company):
3. Extension name:
4. Release target date (estimate):
5. For new extension submission - Extension description (early design wire frames, screen mockups or product screenshots are highly recommended):
6. For extension update review – Description of changes (include product screenshots if UI has been significantly changed):
```

### <a name="submit-your-extension-package-for-review-and-publishing"></a>提交您的延伸模組套件以供審查及發行

請確定您遵循上述的指示來[建立延伸模組套件](#creating-an-extension-package)，而且 nuspec 檔案已正確定義且檔案已簽署。 我們也建議您有一個專案網站，包括下列各項：

- 延伸模組的詳細描述，包括螢幕擷取畫面或影片
- 接收意見反應或問題的電子郵件地址或網站功能

當您準備好要發佈延伸模組時，請傳送電子郵件至， [wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review) 我們將提供有關如何將您的延伸模組套件傳送給我們的指示。 當我們收到您的套件之後，我們會進行審查，並在核准後發佈至 Windows 管理中心摘要。