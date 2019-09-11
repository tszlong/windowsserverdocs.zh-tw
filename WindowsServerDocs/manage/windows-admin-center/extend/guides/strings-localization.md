---
title: Windows 系統管理中心的字串和當地語系化
description: 瞭解如何在 Windows 管理中心 SDK 中讓您的字串準備好進行當地語系化（Project 檀香山）
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4cc624dcc985f13f97b7bbc767de6bbb9c72217a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869709"
---
# <a name="strings-and-localization-in-windows-admin-center"></a>Windows 系統管理中心的字串和當地語系化 #

>適用於：Windows Admin Center、Windows Admin Center 預覽版

讓我們更深入瞭解 Windows 管理中心延伸模組 SDK，並討論字串和當地語系化。

若要啟用呈現層上轉譯之所有字串的當地語系化，請利用/src/resources/strings 下的 resjson 檔案，它已經設定好了。 當您需要將新字串新增至延伸模組時，請將它新增至這個 resjson 檔做為新的專案。 現有的結構會遵循此格式：

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

您可以使用任何您喜歡的字串格式，但請注意，產生進程（取得 resjson 並輸出可用 TypeScript 類別的程式）會將底線（_）轉換成句點（.）。

例如，下列專案：
``` ts
"HelloWorld_cim_title": "CIM Component",
```
產生下列存取子結構：
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```

## <a name="add-other-languages-for-localization"></a>新增其他語言以進行當地語系化 ## 

針對其他語言的當地語系化，必須為每種語言建立 resjson 檔案。 這些檔案必須放在中```\loc\output\{!ExtensionName}\{!LanguageFolder}\strings.resjson```。 具有對應資料夾的可用語言如下：

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
| Português Brasileiro | pt-BR |
| Português (Portugal) | pt-PT |
| Русский | ru-RU |
| Svenska | sv-SE |
| Türkçe    | tr-TR |
| 中文(简体) | zh-CN |
| 中文(繁體) | zh-TW |
> [!NOTE]
> 如果您的檔案結構需要在 loc/輸出中不同，則必須調整 gulpfile.js 中 gulp 工作 ' resjson-json-當地語系化 ' 的 localeOffset。 此位移是要開始搜尋 resjson 檔案之位置資料夾的深度。

每個 resjson 檔案的格式都會與本指南頂端所述的相同。 

例如，若要包含 Español 的當地語系化，請在中```\loc\output\HelloWorld\es-ES\strings.resjson```包含此專案： 
```json
"HelloWorld_cim_title": "CIM Componente",
```
每當您新增當地語系化字串時，必須再次執行 gulp 產生，才能讓它們出現。 執行：
``` cmd
gulp generate 
```

若要確認您的字串是否已產生， ```\src\app\assets\strings\{!LanguageFolder}\strings.resjson```請流覽至。 新加入的專案會出現在這個檔案中。
現在，如果您在 Windows 系統管理中心內切換 [語言] 選項，就可以在延伸模組中看到當地語系化的字串。 
