---
title: 以不同版本的 Windows Admin Center SDK 為目標
description: '將目標設為不同版本的 Windows Admin Center SDK (專案 Honolulu) '
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 3d6b08e69d69a37b31b616994b3bdb67666cb2bb
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766931"
---
# <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>以不同版本的 Windows Admin Center SDK 為目標

>適用於：Windows Admin Center、Windows Admin Center 預覽版

使用 SDK 變更和平臺變更讓您的延伸模組保持最新狀態很簡單。  我們會使用 [NPM 標記](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk) ，將新功能的發行組織成 SDK 版本。

有三個 SDK 版本可供您選擇：

* ```latest``` –此 SDK 套件符合 Windows Admin Center 的目前 GA 版本
* ```insider``` –此 SDK 套件符合 Windows Server Insider Preview 目前可用 Windows Admin Center (預覽版本) 
* ```next``` –此 SDK 套件包含最新的功能

> [!NOTE]
> 深入瞭解可供下載之不同 [版本](../overview.md) 的 Windows Admin Center。

## <a name="targeting-sdk-version-on-a-new-project"></a>在新專案上以 SDK 版本為目標

建立新的擴充功能時，您可以包含 ```--version``` 參數，將目標設為不同版本的 SDK：

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 您的公司名稱 (空格)  | ```Contoso Inc``` |
| ```{!Tool Name}``` | 您的工具名稱 (空格)  | ```Manage Foo Works``` |
| ```{!version}``` | SDK 版本 | ```latest``` |

以下是建立新擴充功能目標的範例 ```insider``` ：

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## <a name="targeting-sdk-version-on-an-existing-project"></a>以現有專案為目標的 SDK 版本

若要修改現有的專案，以不同的 SDK 版本為目標，請修改下列程式 ```package.json``` 程式碼：

```
"@microsoft/windows-admin-center-sdk": "latest",
```
在此範例中，請 ```latest``` 將取代為您想要的 SDK 版本，亦即 ```insider``` ：

```
"@microsoft/windows-admin-center-sdk": "insider",
```

然後執行 ```npm install``` 以更新整個專案中的參考。