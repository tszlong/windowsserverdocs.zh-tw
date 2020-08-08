---
title: 新增 iFrame 至工具擴充功能
description: 開發工具擴充功能 Windows 管理中心 SDK (Project 檀香山) -將 iFrame 新增至工具延伸模組
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: da145d4ef3a58af51846d395081fb643af7c78ac
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87945105"
---
# <a name="add-an-iframe-to-a-tool-extension"></a>新增 iFrame 至工具擴充功能

>適用於：Windows Admin Center、Windows Admin Center 預覽版

在本文中，我們會將 iFrame 新增至我們以 Windows Admin Center CLI 建立的新空白工具擴充功能。

## <a name="prepare-your-environment"></a>準備您的環境 ##

如果您還沒有這麼做，請遵循[開發工具擴充](../develop-tool.md)功能中的指示來準備您的環境，並建立新的空白工具擴充功能。

## <a name="add-a-module-to-your-project"></a>將模組新增至您的專案 ##

將新的[空白模組](add-module.md)新增至您的專案，我們會在下一個步驟中新增 iFrame。

## <a name="add-an-iframe-to-your-module"></a>將 iFrame 新增至您的模組 ##

現在，我們會將 iFrame 新增至剛建立的新空白模組。

在 \src\app 中 \, ，流覽至您的模組資料夾，然後開啟 ```{!module-name}.component.html``` 具有下列命名慣例的檔案：

| 值 | 說明 | 範例檔案名稱 |
| ----- | ----------- | ------- |
| ```{!module-name}``` | 您的模組名稱 (小寫，空格以破折號取代) | ```manage-foo-works-portal.component.html``` |

將下列內容新增至 html 檔案：

``` html
<div>
  <iframe  style="height: 850px;" src="https://www.bing.com"></iframe>
</div>
```

這就是，您已將 iFrame 新增至您的延伸模組。  接下來，您可以在 Windows 系統管理中心內[建立和側邊載入](../develop-tool.md#build-and-side-load-your-extension)擴充功能，以查看結果。

> [!Note]
> 內容安全性原則 (CSP) 設定可能會讓某些網站無法在 Windows 系統管理中心內的 iFrame 中呈現。 您可以在[這裡](https://content-security-policy.com/)深入瞭解。
