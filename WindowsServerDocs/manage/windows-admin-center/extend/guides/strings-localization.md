---
title: 字串和當地語系化的 Windows Admin Center
description: 了解有關 Windows Admin Center SDK （專案檀香山） 中，取得您的字串準備好進行當地語系化
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fb328f86c98141a5a2a1c4fd05ec1d4c96a7bc5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845399"
---
# <a name="strings-and-localization-in-windows-admin-center"></a>字串和當地語系化的 Windows Admin Center #

>適用於：Windows Admin Center，Windows Admin Center 預覽

我們要進入更深入 Windows Admin Center 擴充功能 SDK，並討論字串和當地語系化。

若要啟用會轉譯展示層，請利用下 /src/resources/strings-strings.resjson 檔案的所有字串的當地語系化它已設定。 當您需要將新的字串新增至您的延伸模組時，將它加入此 resjson 檔案為新的項目。 現有的結構會遵循下列格式：

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

您可以使用任何您喜歡的格式字串，但請注意，產生程序 （處理序，會 resjson 並輸出使用的 TypeScript 類別） 會將底線 (_) 轉換成句號 （.）。

例如，此項目：
``` ts
"HelloWorld_cim_title": "CIM Component",
```
會產生存取子結構如下：
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```
