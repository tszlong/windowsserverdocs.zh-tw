---
ms.assetid: 0379abc3-25c7-46ab-9a6b-80a5152365b0
title: AD FS 中的自訂 Web 主題
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 99ea2ef8d2a4fca6733b5d314ab138adb55277aa
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956405"
---
# <a name="custom-web-themes-in-ad-fs"></a>AD FS 中的自訂 Web 主題

現成推出的主題 \- \- \- 稱為預設值。 您可以匯出預設佈景主題，並利用它加快開始的過程。 您可以自訂包含配置的外觀和行為，方法是修改 .css 檔案、匯入並套用這個新佈景主題，然後就可以使用自訂的外觀和行為。 使用 .css 檔案也可讓您更輕鬆地使用 Web 設計工具。

下列 Cmdlet 建立自訂網頁佈景主題，它會複製預設網頁佈景主題。

```powershell
New-AdfsWebTheme –Name custom –SourceName default
```

您可以修改 .css 檔案，並使用新的 .css 檔案來設定新的網頁佈景主題。 若要匯出網頁佈景主題，請使用下列 Cmdlet。

```powershell
Export-AdfsWebTheme –Name default –DirectoryPath c:\theme
```

若要將 .css 檔案套用至新的佈景主題，請使用下列 Cmdlet。

```powershell
Set-AdfsWebTheme –TargetName custom –StyleSheet @{path="c:\NewTheme.css"}
```

下列 Cmdlet 會從新的樣式表建立自訂網頁佈景主題。

```powershell
New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"} –RTLStyleSheetPath c:\NewRtlTheme.css
```

若要將自訂網頁主題套用至 AD FS，請使用下列 Cmdlet。

```powershell
Set-AdfsWebConfig -ActiveThemeName custom
```

若要將 JavaScript 新增至 AD FS，請使用下列 Cmdlet。

```powershell
Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=' /adfs/portal/script/onload.js';path="D:\inetpub\adfsassets\script\onload.js"}
```

## <a name="additional-references"></a>其他參考資料

[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)
