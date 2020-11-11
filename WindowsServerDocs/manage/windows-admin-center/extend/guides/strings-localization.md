---
title: Windows Admin Center 中的字串和當地語系化
description: '深入瞭解如何在 Windows Admin Center SDK (專案 Honolulu 中準備好您的字串以進行當地語系化) '
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: a59e6e1d12844e809f5e4d9f85109c601dabfe26
ms.sourcegitcommit: b39ea3b83280f00e5bb100df0dc8beaf1fb55be2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/11/2020
ms.locfileid: "94520491"
---
# <a name="strings-and-localization-in-windows-admin-center"></a>Windows Admin Center 中的字串和當地語系化 #

>適用於：Windows Admin Center、Windows Admin Center 預覽版

讓我們更深入探索 Windows Admin Center Extensions SDK，並討論字串和當地語系化。

若要啟用展示層上轉譯之所有字串的當地語系化，請利用/src/resources/strings 下的 resources.resjson 檔案-已設定。 當您需要將新字串新增至您的延伸模組時，請將它新增至這個 resources.resjson 檔案作為新專案。 現有的結構會遵循此格式：

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

您可以使用任何您喜歡的字串格式，但請注意，產生程式 (採用 resources.resjson 的程式，並輸出可用的 TypeScript 類別) 將底線 (_) 轉換為 ( ) 。

例如，這個專案：
``` ts
"HelloWorld_cim_title": "CIM Component",
```
產生下列存取子結構：
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```

## <a name="add-other-languages-for-localization"></a>新增其他語言以進行當地語系化 ##

針對其他語言的當地語系化，必須針對每個語言建立 resources.resjson 檔案。 這些檔案必須放入 ```\loc\output\{!ExtensionName}\{!LanguageFolder}\strings.resjson``` 。 具有對應資料夾的可用語言如下：

| Language      | 資料夾      |
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
> 如果您的檔案結構需求在 loc/輸出中不同，您將需要調整 gulpfile.js 中 gulp 工作 ' resources.resjson-json-當地語系化 ' 的 localeOffset。 此位移是從「位置」資料夾開始搜尋 resources.resjson 檔案的深度。

Resources.resjson 檔案的格式將會與本指南頂端所述的方式相同。

例如，若要包含 Español 的當地語系化，請在中加入 ```\loc\output\HelloWorld\es-ES\strings.resjson``` 下列專案：
```json
"HelloWorld_cim_title": "CIM Componente",
```
每當您加入當地語系化的字串時，都必須再次執行 gulp 產生，才能讓它們出現。 執行：
``` cmd
gulp generate
```

若要確認已產生您的字串，請流覽至 ```\src\app\assets\strings\{!LanguageFolder}\strings.resjson``` 。 您新增的專案會出現在這個檔案中。
現在，如果您在 Windows Admin Center 中切換 language 選項，就可以在延伸模組中看到當地語系化的字串。
