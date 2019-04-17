---
title: 目標為不同版本的 Windows Admin Center SDK
description: 目標為不同版本的 Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 47ae669e517f963762ee6267594e18f3413a72ff
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081035"
---
# 目標為不同版本的 Windows Admin Center SDK

>適用於：Windows Admin Center、Windows Admin Center 預覽版

讓您的擴充功能保持在最新的 SDK 的變更和平台變更非常容易。  我們使用[NPM 標記](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk)來將為發行版本的新功能的組織成 SDK 版本。

有三個 SDK 版本，您可以選擇：

* ```latest``` – 此 SDK 套件會配合目前的 GA 版本的 Windows Admin Center
* ```insider``` – 此 SDK 套件會配合目前預覽版本的 Windows Admin Center （可在 Windows Server Insider Preview）
* ```next``` – 此 SDK 套件包含最新功能

> [!NOTE]
> 深入了解不同[版本](https://aka.ms/WACDownloadPage)的 Windows Admin Center 可供下載。

## 在新的專案上的 SDK 版本為目標

在建立新的擴充功能時，您可以包含```--version```參數，以目標為不同版本的 SDK:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 您的公司名稱 （以空格） | ```Contoso Inc``` |
| ```{!Tool Name}``` | 您的工具名稱 （以空格） | ```Manage Foo Works``` |
| ```{!version}``` | SDK 版本 | ```latest``` |

以下是建立新的擴充功能為目標的範例```insider```:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## 在現有的專案上的 SDK 版本為目標

若要修改現有的專案以目標為不同的 SDK 版本，修改以下這一行中的```package.json```:

```
"@microsoft/windows-admin-center-sdk": "latest",
```
在此範例中，取代```latest```與您想要的 SDK 版本，也就是```insider```:

```
"@microsoft/windows-admin-center-sdk": "insider",
```

然後執行```npm install```要更新您的專案參考。