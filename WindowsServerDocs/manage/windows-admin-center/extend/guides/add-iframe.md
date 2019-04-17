---
title: 將 iFrame 新增到工具擴充功能
description: 開發工具擴充功能 Windows Admin Center SDK (Project Honolulu)-將 iFrame 新增到工具擴充功能
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 7b4a7b688e4b2d9f52e44395c19211b91b964578
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080925"
---
# 將 iFrame 新增到工具擴充功能

>適用於：Windows Admin Center、Windows Admin Center 預覽版

在本文中，我們將新增 iFrame 我們已使用 Windows Admin Center CLI 建立新的空白工具擴充功能。

## 準備您的環境 ##

如果您還沒有這樣做，請依照[開發工具擴充功能](..\develop-tool.md)來準備您的環境，並建立新的空白工具擴充功能的指示。

## 模組新增到您的專案 ##

將新的[空白模組](add-module.md)新增到您的專案，，我們將在下一個步驟中新增 iFrame。  

## 將 iFrame 新增到您的模組 ##

現在我們將新增 iFrame 我們剛建立的新的空白模組。

在 \src\app\，瀏覽到您的模組資料夾，然後開啟檔案```{!module-name}.component.html```使用下列命名慣例找到：

| 值 | 說明 | 範例檔案名稱 |
| ----- | ----------- | ------- |
| ```{!module-name}``` | 您的模組名稱 (小寫，空格以破折號取代) | ```manage-foo-works-portal.component.html``` |
    
Html 檔案中加入下列內容：

``` html
<div>
  <iframe  style="height: 850px;" src="https://www.bing.com"></iframe>
</div>
```

這樣就完成了，您已新增 iFrame 到您的擴充功能。  接下來，您可以[建置並側載入](..\develop-tool.md#build-and-side-load-your-extension)您的擴充功能，在 Windows Admin Center，以查看結果。
