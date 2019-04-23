---
title: 以不同版本的 Windows Admin Center SDK 為目標
description: 目標不同版本的 Windows Admin Center （專案檀香山） SDK
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 47ae669e517f963762ee6267594e18f3413a72ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833619"
---
# <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>以不同版本的 Windows Admin Center SDK 為目標

>適用於：Windows Admin Center，Windows Admin Center 預覽

保持最新的 SDK 變更與平台變更您的延伸模組很簡單。  我們會使用[NPM 標記](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk)將發行的新功能組織成 SDK 版本。

有三個 SDK 版本，您可以選擇：

* ```latest``` -此 SDK 套件與目前 GA 版本的 Windows Admin Center
* ```insider``` -此 SDK 套件與目前的預覽版本的 Windows Admin Center （位於 Windows Server Insider Preview）
* ```next``` -此 SDK 套件包含最新功能

> [!NOTE]
> 深入了解不同[版本](https://aka.ms/WACDownloadPage)的 Windows Admin Center 可供下載。

## <a name="targeting-sdk-version-on-a-new-project"></a>以新專案的 SDK 版本為目標

當建立新的延伸模組，您可以包含```--version```不同的 sdk 版本為目標的參數：

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 您的公司名稱 （含空格） | ```Contoso Inc``` |
| ```{!Tool Name}``` | 您的工具名稱 （含空格） | ```Manage Foo Works``` |
| ```{!version}``` | SDK 版本 | ```latest``` |

以下是建立新的延伸模組為目標的範例```insider```:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## <a name="targeting-sdk-version-on-an-existing-project"></a>以現有的專案的 SDK 版本為目標

若要修改現有的專案以不同的 SDK 版本為目標，修改中的下行```package.json```:

```
"@microsoft/windows-admin-center-sdk": "latest",
```
在此範例中，取代```latest```與您所需的 SDK 版本，也就是```insider```:

```
"@microsoft/windows-admin-center-sdk": "insider",
```

然後執行```npm install```更新整個專案的參考。