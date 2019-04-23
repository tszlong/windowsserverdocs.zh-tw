---
title: 新增 iFrame 至工具擴充功能
description: 開發工具擴充功能的 Windows Admin Center SDK （專案檀香山）-將 iFrame 加入至 [工具] 延伸模組
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 7cf1dcec1bc8e187b6db789c5402ca8119ca8b6c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850759"
---
# <a name="add-an-iframe-to-a-tool-extension"></a>新增 iFrame 至工具擴充功能

>適用於：Windows Admin Center，Windows Admin Center 預覽

在本文中，我們會新增一個 iframe，連結至新的空白工具延伸模組，我們建立了使用 Windows Admin Center CLI。

## <a name="prepare-your-environment"></a>準備您的環境 ##

如果您還沒有這麼做，請依照下列中的指示[開發工具 延伸模組](..\develop-tool.md)若要準備您的環境，並建立新的清空 [工具] 延伸模組。

## <a name="add-a-module-to-your-project"></a>將模組新增至您的專案 ##

加入新[空的模組](add-module.md)至您的專案下, 一步我們會新增一個 iframe，連結。  

## <a name="add-an-iframe-to-your-module"></a>將 iFrame 加入至您的模組 ##

現在我們將新增一個 iframe，連結到我們剛剛建立的新的空白模組。

在 \src\app\,瀏覽至您的模組資料夾，然後開啟檔案```{!module-name}.component.html```，找到下列命名慣例：

| 值 | 說明 | 範例檔案名稱 |
| ----- | ----------- | ------- |
| ```{!module-name}``` | 您的模組名稱 (小寫，空格以破折號取代) | ```manage-foo-works-portal.component.html``` |
    
Html 檔案中加入下列內容：

``` html
<div>
  <iframe  style="height: 850px;" src="https://www.bing.com"></iframe>
</div>
```

就這麼簡單，您已將 iFrame 加入您的延伸模組。  接下來，您可以[建置，並端負載](..\develop-tool.md#build-and-side-load-your-extension)您 Windows Admin Center，來查看結果的延伸模組。

> [!Note]
> 內容安全性原則 (CSP) 設定可防止某些站台內 Windows Admin Center 的 iFrame 中轉譯。 您可以深入了解這[此處](https://content-security-policy.com/)。 
