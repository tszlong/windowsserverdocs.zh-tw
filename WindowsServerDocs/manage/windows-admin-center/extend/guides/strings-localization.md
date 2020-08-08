---
title: Windows 系統管理中心的字串和當地語系化
description: '瞭解如何將您的字串準備好在 Windows Admin Center SDK 中進行當地語系化 (Project 檀香山) '
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 565e4da69466549538c380457269304c7f1cdd5a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944918"
---
# <a name="strings-and-localization-in-windows-admin-center"></a>Windows 系統管理中心的字串和當地語系化 #

>適用於：Windows Admin Center、Windows Admin Center 預覽版

讓我們更深入瞭解 Windows 管理中心延伸模組 SDK，並討論字串和當地語系化。

若要啟用呈現層上轉譯之所有字串的當地語系化，請利用/src/resources/strings 下的 resjson 檔案，它已經設定好了。 當您需要將新字串新增至延伸模組時，請將它新增至這個 resjson 檔做為新的專案。 現有的結構會遵循此格式：

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

您可以使用任何您喜歡的字串格式，但請注意，產生程式 (採用 resjson 的進程，並輸出可使用的 TypeScript 類別，) 將底線 (_) 轉換成句點 (. ) 。

例如，下列專案：
``` ts
"HelloWorld_cim_title": "CIM Component",
```
產生下列存取子結構：
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```

## <a name="add-other-languages-for-localization"></a>新增其他語言以進行當地語系化 ##

針對其他語言的當地語系化，必須為每種語言建立 resjson 檔案。 這些檔案必須放在中 ```\loc\output\{!ExtensionName}\{!LanguageFolder}\strings.resjson``` 。 具有對應資料夾的可用語言如下：

| 語言      | 資料夾      |
| ------------- |-------------|
| Čeština | cs-CZ |
| Deutsch | de-DE |
| 英文 | en-US |
| Español | es-ES |
| Français | fr-FR |
| Magyar | hu-HU |
| Italiano | it-IT |
| 日本語 | ja-JP |
| 한국어 | ko-KR |
| Nederlands | nl-NL |
| Polski | pl-PL |
| Português (Brasil) | pt-BR |
| Português (Portugal) | pt-PT |
| Русский | ru-RU |
| Svenska | sv-SE |
| Türkçe    | tr-TR |
| 中文 (簡體)  | zh-CN |
| 中文 (繁體)  | zh-TW |
> [!NOTE]
> 如果您的檔案結構需要在 loc/輸出中不同，您必須針對 gulpfile.js 中的 gulp 工作「產生-resjson-json-當地語系化」調整 localeOffset。 此位移是要開始搜尋 resjson 檔案之位置資料夾的深度。

每個 resjson 檔案的格式都會與本指南頂端所述的相同。

例如，若要包含 Español 的當地語系化，請在中包含此專案 ```\loc\output\HelloWorld\es-ES\strings.resjson``` ：
```json
"HelloWorld_cim_title": "CIM Componente",
```
每當您新增當地語系化字串時，必須再次執行 gulp 產生，才能讓它們出現。 請執行：
``` cmd
gulp generate
```

若要確認您的字串是否已產生，請流覽至 ```\src\app\assets\strings\{!LanguageFolder}\strings.resjson``` 。 新加入的專案會出現在這個檔案中。
現在，如果您在 Windows 系統管理中心內切換 [語言] 選項，就可以在延伸模組中看到當地語系化的字串。
