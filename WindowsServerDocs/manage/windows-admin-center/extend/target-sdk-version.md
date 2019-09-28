---
title: 以不同版本的 Windows Admin Center SDK 為目標
description: 以不同版本的 Windows Admin Center SDK 為目標（Project 檀香山）
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 0d3b7af5229f7b8487aa9f04eaf0d1756d8c02f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356975"
---
# <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>以不同版本的 Windows Admin Center SDK 為目標

>適用於：Windows Admin Center、Windows Admin Center 預覽版

使用 SDK 變更和平臺變更讓擴充功能保持最新狀態很容易。  我們會使用[NPM 標記](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk)，將新功能的發行組織成 SDK 版本。

有三種 SDK 版本可供您選擇：

* ```latest``` –此 SDK 套件與 Windows 管理中心的目前 GA 版本一致
* ```insider``` –此 SDK 套件與 Windows 管理中心的目前預覽版本一致（可從 Windows Server Insider Preview 取得）
* ```next``` –此 SDK 套件包含最新的功能

> [!NOTE]
> 深入瞭解可供下載的不同 Windows 系統管理中心[版本](https://aka.ms/WACDownloadPage)。

## <a name="targeting-sdk-version-on-a-new-project"></a>以新專案為目標的 SDK 版本

建立新的擴充功能時，您可以包含 ```--version``` 參數，以不同版本的 SDK 為目標：

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| 值 | 說明 | 範例 |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | 您的公司名稱（含空格） | ```Contoso Inc``` |
| ```{!Tool Name}``` | 您的工具名稱（含空格） | ```Manage Foo Works``` |
| ```{!version}``` | SDK 版本 | ```latest``` |

以下是建立新的延伸模組目標 ```insider``` 的範例：

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## <a name="targeting-sdk-version-on-an-existing-project"></a>以現有專案為目標的 SDK 版本

若要將現有的專案修改為以不同的 SDK 版本為目標，請修改 ```package.json``` 中的下列程式程式碼：

```
"@microsoft/windows-admin-center-sdk": "latest",
```
在此範例中，請以您所需的 SDK 版本（亦即 ```insider```）取代 ```latest```：

```
"@microsoft/windows-admin-center-sdk": "insider",
```

然後執行 ```npm install```，以更新整個專案中的參考。