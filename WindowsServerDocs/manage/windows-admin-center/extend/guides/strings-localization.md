---
title: 字串和當地語系化的 Windows Admin Center
description: 深入了解 Windows Admin Center SDK (Project Honolulu) 中，取得您的字串準備好進行當地語系化
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f0671160bd790d80e35f6d6c1586a07969939070
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296730"
---
# 字串和當地語系化的 Windows Admin Center #

>適用於：Windows Admin Center、Windows Admin Center 預覽版

讓我們前往更深入 Windows Admin Center 擴充功能 SDK，然後討論字串和當地語系化。

若要啟用的展示層，充分利用這些 /src/resources/strings-strings.resjson 檔案上所呈現的所有字串當地語系化它已經設定好。 當您需要將新的字串新增至您的擴充功能時，將它新增到此 resjson 檔案做為新的項目。 此格式的現有的結構如下：

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

您可以使用任何您喜歡的格式字串，但請注意，產生程序 （的程序會取得 resjson 並輸出可用 TypeScript 類別） 會將底線 (_) 轉換為句點 （.）。

例如，此項目：
``` ts
"HelloWorld_cim_title": "CIM Component",
```
會產生下列存取子結構：
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```

## 新增其他語言進行當地語系化 ## 

當地語系化成其他語言，strings.resjson 檔案必須為每一種語言建立。 這些檔案必須要在位置```\loc\output\{!ExtensionName}\{!LanguageFolder}\strings.resjson```。 可用的語言與相對應的資料夾是：

| Language      | 資料夾      |
| ------------- |-------------|
| Čeština | cs-CZ |
| 用以代表德文 | de-DE |
| 英文 | en-US |
| 西班牙文 | es-ES |
| Français | fr-FR | 
| Magyar | hu-HU | 
| 文 | it-IT |
| 日本語 | ja-JP | 
| 한국어 | ko-KR | 
| 荷蘭 | nl-NL |
| Polski | pl-PL |
| Português （巴西） | pt-BR |
| Português （葡萄牙） | pt-PT |
| РУССКИЙ | ru-RU |
| Svenska | sv-SE |
| Türkçe    | tr-TR |
| 中文(简体) | zh-CN |
| 中文(繁體) | zh-TW |
> [!NOTE]
> 如果您的檔案結構的需求是不同的內部的當地語系化/輸出，您將需要調整 gulp 使用工作 '產生 resjson-json-當地語系化' 中 gulpfile.js localeOffset。 這個位移是如何深入它應該開始搜尋 strings.resjson 檔案的當地語系化資料夾。

每個 strings.resjson 檔案將會格式化頂端的本文先前所述的方式相同。 

例如，以包含當地語系化的西班牙文包含此項目```\loc\output\HelloWorld\es-ES\strings.resjson```: 
```json
"HelloWorld_cim_title": "CIM Componente",
```
任何時候，您已新增當地語系化的字串，gulp 使用產生必須為了讓它們出現再次執行。 執行：
``` cmd
gulp generate 
```

若要確認您的字串，產生瀏覽至```\src\app\assets\strings\{!LanguageFolder}\strings.resjson```。 您剛新增的項目會出現在此檔案。
現在您切換 Windows Admin Center 中的語言選項，如果您將無法以查看您的擴充功能中的當地語系化的字串。 
