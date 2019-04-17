---
title: 發佈 Windows Admin Center 擴充功能
description: 發佈擴充功能，Windows Admin center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 762bd4613fa8ad6cdfb5b44745a7ce78b331499d
ms.sourcegitcommit: 3883eebbba70bfea0221e510863ee1a724a5f926
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "5783750"
---
# 發佈擴充功能

>適用於：Windows Admin Center、Windows Admin Center 預覽版

當您開發您的擴充功能時，您會想要將它發行，並讓它提供給其他人測試，或使用。 根據您的對象和發佈的目的，有幾個選項其中我們文件會介紹下方以及步驟和發佈的需求。

## 發佈選項

有三個主要支援 Windows Admin Center 的可設定的套件來源選項：
* Microsoft 的公用 Windows Admin Center NuGet 摘要
* 您自己私用的 NuGet 摘要
* 本機或網路檔案共用

### 發佈至 Windows Admin Center 擴充功能摘要

根據預設，Windows Admin Center 已連接到 NuGet 由 Microsoft Windows Admin Center 產品小組維護的摘要。 早期的預覽版本的 Microsoft 所開發的新擴充功能可能會發佈到此摘要，提供給 Windows Admin Center 使用者。 計劃要建置並公開發行延伸模組的外部開發人員也可以[提交要求](#publishing-your-extension-to-the-windows-admin-center-feed)發佈到此摘要。

### 發佈到不同的 NuGet 摘要

您也可能會建立您自己的 NuGet 摘要若要使用這其中許多[不同的選項設定為私用的來源或使用 NuGet 裝載服務](https://docs.microsoft.com/nuget/hosting-packages/overview)的發佈您的延伸模組。 NuGet 摘要必須支援 NuGet v2 API。 Windows Admin Center 目前不支援摘要的驗證，因為摘要需要設定，以允許任何人的讀取權限。

### 發佈至檔案共用

若要限制您的擴充功能的存取權，您的組織或一組有限的人員，您可以使用 SMB 檔案共用作為擴充功能摘要。 在此情況下，將會套用檔案共用及資料夾的權限允許其存取摘要。

## 準備發行您的擴充功能

請確定您閱讀並請考慮下列開發主題：

- [控制您的工具可見性](guides/dynamic-tool-display.md)
- [字串和當地語系化](guides/strings-localization.md)

### 可以考慮發行的預覽版本

如果您發行您的擴充功能，針對評估用途的預覽版本，我們建議您：

- 將 「 （預覽） 」 附加到.nuspec 檔案中的延伸模組之標題的結尾
- 說明在您的擴充功能描述.nuspec 檔案中的限制

## 建立延伸模組套件

Windows Admin Center 利用 NuGet 套件和散發和下載擴充功能的摘要。  為了讓您出貨的套件，您將需要產生包含您的外掛程式和延伸模組的 NuGet 套件。  單一套件可以包含 UI 延伸模組以及閘道外掛程式，和下一節將會逐步引導您完成此程序。

### 1.建置擴充功能

因為您已經準備好開始封裝您的擴充功能，建立新的目錄，您的檔案系統上，開啟主控台，並 CD 到其中。  這會是我們將用來包含所有 nuspec 和內容目錄會構成我們套件的根目錄。  我們會參考此資料夾做為 「 NuGet 套件 」，如本文件的持續時間。

#### UI 擴充功能

若要開始上收集所需的 UI 延伸模組的所有內容的程序，執行您的工具上的 「 gulp 組建 」 並確定會成功的組建。  此程序的套件，一起在名為"bundle"的資料夾中的所有元件都位於您的擴充功能 （在相同層級的 src 目錄） 的根目錄。  將此目錄和所有複製它的內容到 「 NuGet 套件 」 資料夾中。

#### 閘道外掛程式

使用您建置的基礎結構 （這可能是一樣簡單開啟 Visual Studio，然後按一下 [建置] 按鈕），編譯並建置您的外掛程式。  開啟您的建置輸出目錄，並複製 dll 代表您外掛程式，並將它們放在名為 「 套件 」 的 「 NuGet 套件 」 目錄內的新資料夾。  您不需要複製 FeatureInterface dll，只是代表您的程式碼的 dll。

### 2.建立.nuspec 檔案

若要建立的 NuGet 套件，您需要先建立.nuspec 檔案。 .Nuspec 檔案是包含 NuGet 套件中繼資料的 XML 資訊清單。 建置套件和資訊提供給消費者，則會使用此資訊清單。  將此檔案放在 「 NuGet 套件 」 資料夾的根目錄。

以下是範例.nuspec 檔案及需要或建議的屬性的清單。 完整的結構描述，請參閱[.nuspec 參考](https://docs.microsoft.com/nuget/reference/nuspec)。 .nuspec 將檔案儲存到您的專案根資料夾，以您選擇的檔案名稱。

> [!IMPORTANT]
> ```<id>``` .Nuspec 檔案中的值必須符合```"name"```在您的專案中的值```manifest.json```檔案，，否則您已發佈的擴充功能將不會成功以載入 Windows Admin Center。

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

#### 需要或建議屬性

| 屬性名稱 | 需要具備 / 建議 | 描述 |
| ---- | ---- | ---- |
| packageType | 必要 | 使用 「 WindowsAdminCenterExtension 」，也就是針對 Windows Admin Center 擴充功能定義的 NuGet 套件類型。 |
| id | 必要 | 在摘要中唯一套件識別碼。 這個值必須符合您的專案 manifest.json 檔中的 「 名稱 」 值。  如指導方針，請參閱[選擇套件的唯一識別碼](https://docs.microsoft.com/nuget/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number)。 |
| title | 所需的發佈至 Windows Admin Center 摘要 | Windows Admin Center 擴充功能管理員 」 中顯示之套件的易記名稱。 |
| 版本 | 必要 | 延伸模組的版本。 使用[語意式版本 （SemVer 慣例）](http://semver.org/spec/v1.0.0.html)是建議的但不是需要。 |
| 作者 | 必要 | 如果您的公司代表發行，使用您的公司名稱。 |
| description | 必要 | 提供延伸模組之功能的描述。 |
| iconUrl | 發佈至 Windows Admin Center 摘要時建議 | 若要顯示在 [擴充管理員] 中的圖示 URL。 |
| projectUrl | 所需的發佈至 Windows Admin Center 摘要 | 您的擴充功能的網站 URL。 如果您不需要另外的網站，使用上摘要 NuGet 套件網頁 URL。 |
| licenseUrl | 所需的發佈至 Windows Admin Center 摘要 | 您的擴充功能的使用者授權合約的 URL。 |
| 檔案 | 必要 | 這兩個設定設定 Windows Admin Center UI 擴充功能和閘道外掛程式的預期的資料夾結構。 |

### 3.建置延伸模組 NuGet 套件

使用您建立上述的.nuspec 檔案，您現在將建立您可以上傳並發佈至摘要 NuGet 的 NuGet 套件.nupkg 檔案。

1. [NuGet 用戶端工具網站](https://docs.microsoft.com/nuget/install-nuget-client-tools)下載 nuget.exe CLI 工具。
2. 若要建立.nupkg 檔案執行 「 nuget.exe 套件 [.nuspec 檔案名稱] 」。

### 4.簽署您的延伸模組 NuGet 套件

您的擴充功能中包含任何.dll 檔案是必要項目使用來自受信任的憑證授權單位 (CA) 憑證來簽署。 根據預設，將會封鎖不帶正負號的.dll 檔案時，在生產環境模式下執行 Windows Admin Center 正在執行。

也強烈建議您簽署的擴充功能 NuGet 套件，以確保 「 套件 」 的完整性，但這不是必要的步驟。

### 5.測試您的延伸模組 NuGet 套件

您的延伸模組套件現在已準備好進行測試 ！ .nupkg 檔案上傳至 NuGet 摘要或將它複製到的檔案共用。 若要檢視和下載套件從不同的摘要或檔案共用，您將需要[變更您的摘要的設定](../configure/using-extensions.md#installing-extensions-from-a-different-feed)以指向您 NuGet 摘要或檔案共用。 測試時，請確定內容正確出現在 [擴充管理員] 中成功安裝和解除安裝您的擴充功能。

## 將 Windows Admin Center 發佈您的擴充功能摘要

由發佈到 Windows Admin Center 摘要，您可以將您的擴充功能提供給任何 Windows Admin Center 使用者。 因為 Windows Admin Center SDK 是仍在預覽，所以我們想要與您協助解決開發問題，請確定您就可以產品的品質和給您的使用者體驗密切合作。

之前發行您的擴充功能的初始版本，我們建議您提交給 Microsoft 的擴充功能檢閱要求至少 2-3 週，以確保我們有足夠的時間來檢閱發行前，並為您對您的擴充功能進行任何變更，如有必要。 一旦準備好可以發佈您的擴充功能，您將需要將它傳送給我們的評論，並核准，如果我們會將它發行至您的摘要。

之後，如果您想要發行至您的擴充功能更新，您將必須提交評論的另一個要求。 雖然根據變更的範圍，更新的評論的往返時間通常應該較短。

### 提交給 Microsoft 的擴充功能檢閱要求

若要提交的延伸模組檢閱要求，輸入下列資訊並傳送電子郵件傳送給[wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Review%20Request)為。 我們將在一星期內回覆您的電子郵件。

```
Windows Admin Center Extension Review Request
1. Name and email address of extension owner/developer (up to 3 users). If you will be releasing an extension on behalf of your company, provide your company email address.
2. Company name (Only required if you are releasing an extension on behalf of your company):
3. Extension name:
4. Release target date (estimate):
5. For new extension submission - Extension description (early design wire frames, screen mockups or product screenshots are highly recommended):
6. For extension update review – Description of changes (include product screenshots if UI has been significantly changed):
```

### 提交您的延伸模組套件適用於檢閱和發佈

請確定您依照上述的指示來[建立延伸模組套件](#creating-an-extension-package)和.nuspec 檔案的正確定義和簽署的檔案。 我們也建議您有專案網站，包括下列：

- 您的擴充功能包括螢幕擷取畫面或視訊的詳細的說明
- 收到意見反應或問題的電子郵件地址或網站功能

當您準備好發佈您的擴充功能時，將電子郵件傳送給[wacextensionrequest@microsoft.com](mailto:wacextensionrequest@microsoft.com?subject=Windows%20Admin%20Center%20Extension%20Package%20Review) ，我們會提供有關如何將您的延伸模組套件傳送給我們的指示。 一旦我們收到您的套件，我們將檢閱並核准，如果發佈到 Windows Admin Center 摘要。