---
title: 發行 Windows Admin Center 的擴充功能
description: 發行 Windows Admin Center （專案檀香山） 的擴充功能
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 762bd4613fa8ad6cdfb5b44745a7ce78b331499d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820419"
---
# <a name="publishing-extensions"></a>發行擴充功能

>適用於：Windows Admin Center，Windows Admin Center 預覽

當您開發您的延伸模組時，您會想要將它發行，並將它提供給其他人來測試或使用。 根據您的對象和目的發行，還有我們會介紹以下的步驟和發行需求以及幾個選項。

## <a name="publishing-options"></a>發行選項

有三個 Windows Admin Center 支援的可設定的套件來源的主要選項：
* Microsoft 的公用 Windows Admin Center NuGet 摘要
* 您自己的私用 NuGet 摘要
* 本機或網路檔案共用

### <a name="publishing-to-the-windows-admin-center-extension-feed"></a>發佈到摘要的 Windows Admin Center 延伸模組

依預設，Windows Admin Center 連線至 NuGet 由 Microsoft 的 Windows Admin Center 產品小組所維護的摘要。 發佈到此摘要，並可供 Windows Admin Center 使用者，可能是由 Microsoft 所開發的新擴充功能的早期預覽版本。 外部計劃要建置及公開發行延伸模組的開發人員也可以[申請](#publishing-your-extension-to-the-windows-admin-center-feed)發佈到此摘要。

### <a name="publishing-to-a-different-nuget-feed"></a>發行至不同的 NuGet 摘要

您也可以建立您自己的 NuGet 摘要，以將您的延伸模組發行至使用任何一種[不同的選項，設定 私用的來源，或使用 NuGet，裝載服務](https://docs.microsoft.com/nuget/hosting-packages/overview)。 NuGet 摘要必須支援 NuGet v2 API。 由於 Windows Admin Center 目前不支援驗證摘要，摘要必須設定為允許任何人都能讀取權限。

### <a name="publishing-to-a-file-share"></a>發行至檔案共用

若要限制存取您的延伸模組，到您的組織，或為一組有限的人員，您可以使用 SMB 檔案共用作為擴充功能摘要。 在此情況下，將套用的檔案共用和資料夾權限，允許存取摘要。

## <a name="preparing-your-extension-for-release"></a>準備您的延伸模組可供發行

請確認您已閱讀並考慮下列的開發主題：

- [控制工具的可見性](guides/dynamic-tool-display.md)
- [字串和當地語系化](guides/strings-localization.md)

### <a name="consider-releasing-as-a-preview-release"></a>請考慮釋放的預覽版本

如果您要釋出您的延伸模組，用於評估的預覽版本，我們建議您：

- .Nuspec 檔案中的擴充功能的標題結尾附加"(Preview)"
- 說明在.nuspec 檔案中的擴充功能的描述中的限制

## <a name="creating-an-extension-package"></a>建立延伸模組套件

NuGet 封裝和散發及下載擴充功能的摘要，則會利用 Windows Admin Center。  為了讓您的出貨的套件，您必須產生 NuGet 封裝，其中包含您的增益集和延伸模組。  UI 延伸模組以及閘道外掛程式，可以包含單一封裝下, 一節將逐步引導您完成程序。

### <a name="1-build-your-extension"></a>1.建置您的延伸模組

當您準備好要開始封裝您的延伸模組，在您的檔案系統上建立新的目錄，開啟主控台，並 CD 入它。  這是我們將使用包含所有 nuspec 及內容目錄將構成我們套件的根目錄。  期間本文件，我們將 「 NuGet 套件 」 的形式參考此資料夾。

#### <a name="ui-extensions"></a>UI 延伸模組

若要開始上收集所需的 UI 延伸模組的所有內容的程序，請執行您的工具上的 「 gulp 組建 」，請確定組建已成功。  此程序的封裝，一起在名為 「 組合 」 的資料夾中的所有元件都位於您的延伸模組 （相同層級的 src 目錄） 的根目錄。  複製此目錄及其所有的內容 [NuGet 封裝] 資料夾。

#### <a name="gateway-plugins"></a>閘道外掛程式

使用您的建置基礎結構 （這可能很簡單，只要開啟 Visual Studio，並按一下 [建立] 按鈕），編譯和建置您的外掛程式。  開啟您的組建輸出目錄，並複製 dll 代表您的外掛程式，將它們放在 [NuGet 封裝] 目錄中稱為 「 套件 」 的新資料夾中。  您不需要複製 FeatureInterface dll 時，就表示您的程式碼的 dll。

### <a name="2-create-the-nuspec-file"></a>2.建立.nuspec 檔案

若要建立 NuGet 套件，您必須先建立.nuspec 檔案。 .Nuspec 檔案是包含 NuGet 套件中繼資料的 XML 資訊清單。 若要建置的套件和資訊提供給取用者，則會使用此資訊清單。  將此檔案放在 [NuGet 封裝] 資料夾的根目錄。

以下是範例.nuspec 檔案以及必要或建議的屬性清單。 針對完整的結構描述，請參閱[.nuspec 參考](https://docs.microsoft.com/nuget/reference/nuspec)。 .Nuspec 檔案儲存至您的專案根資料夾中，使用您選擇的檔案名稱。

> [!IMPORTANT]
> ```<id>``` .Nuspec 檔案中的值必須符合```"name"```在您的專案中的值```manifest.json```或是您已發佈的延伸模組的檔案，就不會載入已成功在 Windows Admin Center。

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
  <metadata>
    <packageTypes>
      <packageType name="WindowsAdminCenterExtension" />
    </packageTypes>  
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

| 屬性名稱 | 必要 / 建議 | 描述 |
| ---- | ---- | ---- |
| packageType | 必要項 | 使用 「 WindowsAdminCenterExtension"，也就是針對 Windows Admin Center 延伸模組所定義的 NuGet 套件類型。 |
| id | 必要項 | 摘要中的唯一封裝識別碼。 此值必須符合您的專案 manifest.json 檔案中的"name"值。  請參閱[選擇唯一的套件識別碼](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number)指導方針。 |
| title | 所需的發佈到摘要的 Windows Admin Center | 顯示在 Windows Admin Center 延伸模組管理員中之封裝的易記名稱。 |
| version | 必要項 | 延伸模組版本。 使用[語意版本控制 （SemVer 慣例）](http://semver.org/spec/v1.0.0.html) ，建議使用但非必要。 |
| 作者 | 必要項 | 如果發行代表您的公司，使用您的公司名稱。 |
| description | 必要項 | 提供延伸模組的功能的描述。 |
| iconUrl | 發佈到摘要的 Windows Admin Center 時，建議使用 | 若要顯示在延伸模組管理員圖示 URL。 |
| projectUrl | 所需的發佈到摘要的 Windows Admin Center | 擴充功能的網站 URL。 如果您還沒有個別的網站，用於封裝網頁上的 NuGet 摘要的 URL。 |
| licenseUrl | 所需的發佈到摘要的 Windows Admin Center | 您的擴充功能使用者授權合約的 URL。 |
| 檔案 | 必要項 | 這兩項設定會設定 Windows Admin Center UI 延伸模組和閘道外掛程式所預期的資料夾結構。 |

### <a name="3-build-the-extension-nuget-package"></a>3.建置延伸模組的 NuGet 套件

使用您先前建立的.nuspec 檔案，您現在將建立您可上傳及發佈至 NuGet 摘要的 NuGet 套件.nupkg 檔案。

1. 下載 nuget.exe CLI 工具，從[NuGet 用戶端工具網站](https://docs.microsoft.com/nuget/install-nuget-client-tools)。
2. 您可以執行 [nuget.exe 套件 [.nuspec 檔案名稱]] 以建立.nupkg 檔案。

### <a name="4-signing-your-extension-nuget-package"></a>4.簽署您延伸模組的 NuGet 套件

需要使用從受信任憑證授權單位 (CA) 憑證來簽署任何包含在您的延伸模組的.dll 檔案。 根據預設，不帶正負號的.dll 檔案將會封鎖 Windows Admin Center 在生產模式中執行時執行。

我們也強烈建議您登入延伸模組的 NuGet 套件，以確保套件的完整性，但這不是必要的步驟。

### <a name="5-test-your-extension-nuget-package"></a>5.測試您的延伸模組 NuGet 套件

現在已準備好進行測試擴充功能封裝 ！ 將.nupkg 檔案上傳至 NuGet 摘要，或將它複製到檔案共用。 若要檢視，並從其他摘要或檔案共用下載套件，您將需要[變更摘要的設定](../configure/using-extensions.md#installing-extensions-from-a-different-feed)指向您的 NuGet 摘要，或檔案共用。 測試時，請確定屬性會正確顯示在 [延伸模組管理員] 中，而且您可以成功地安裝和解除安裝您的延伸模組。

## <a name="publishing-your-extension-to-the-windows-admin-center-feed"></a>您的延伸模組發行至 Windows Admin Center 摘要

發行至摘要的 Windows Admin Center，您可以將您的擴充功能提供給 Windows Admin Center 中的任何使用者。 因為 Windows Admin Center SDK 仍處於預覽狀態，我們想要密切合作，與您合作協助解決開發問題和，請確定您將可提供高品質的產品，並為您的使用者體驗。

之前釋出您的延伸模組的初始版本，我們建議您提交給 Microsoft 的擴充功能檢閱要求至少 2-3 個星期之前版本，以確保有足夠的時間來檢閱，並為您如有必要，對您的延伸模組進行任何變更。 準備要發行您的延伸模組之後，您必須進行審查，將它傳送給我們，如果核准，我們會將它發行到您的摘要。

之後，如果您想要釋放您的延伸模組的更新，您必須提交另一個檢閱要求。 根據變更的範圍，而更新檢閱迴轉時間通常應該短。

### <a name="submit-an-extension-review-request-to-microsoft"></a>提交給 Microsoft 的擴充功能檢閱要求

若要提交的延伸模組檢閱要求，請輸入下列資訊和傳送電子郵件給[ wacextensionrequest@microsoft.com ](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request)。 我們將一週內回覆您的電子郵件。

```
Windows Admin Center Extension Review Request
1. Name and email address of extension owner/developer (up to 3 users). If you will be releasing an extension on behalf of your company, provide your company email address.
2. Company name (Only required if you are releasing an extension on behalf of your company):
3. Extension name:
4. Release target date (estimate):
5. For new extension submission - Extension description (early design wire frames, screen mockups or product screenshots are highly recommended):
6. For extension update review – Description of changes (include product screenshots if UI has been significantly changed):
```

### <a name="submit-your-extension-package-for-review-and-publishing"></a>提交您的延伸模組套件，供您檢閱和發佈

請確定您遵循上述指示，如[建立延伸模組套件](#creating-an-extension-package)和.nuspec 檔案已正確定義，而且檔案會經過簽署。 我們也建議您有專案網站，包括下列：

- 詳細的描述，包括螢幕擷取畫面或影片擴充功能
- 接收意見反應或問題的電子郵件地址或網站功能

當您準備好發佈您的延伸模組時，傳送電子郵件[ wacextensionrequest@microsoft.com ](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review) ，我們將提供有關如何將您的延伸模組套件傳送給我們的指示。 一旦我們收到您的套件，我們將檢閱並一經核准，將發佈到摘要的 Windows Admin Center。