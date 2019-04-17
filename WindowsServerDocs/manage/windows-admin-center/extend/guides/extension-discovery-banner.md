---
title: 啟用擴充功能探索橫幅
description: 啟用擴充功能探索橫幅
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/08/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 389acba6d1fe6f65320bd780c9fde6469b387e0b
ms.sourcegitcommit: e558dda2774345e9ad17ff04b759f68c59d88139
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2019
ms.locfileid: "9262922"
---
# 啟用擴充功能探索橫幅 #

>適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows Admin Center 預覽版 1903年中可用的新功能是擴充功能探索橫幅。 這項功能可讓延伸模組來宣告的伺服器硬體製造商和它所支援的模型，並通知橫幅輕鬆地安裝擴充功能，當使用者連接到伺服器或叢集延伸模組是可用，將會顯示。 擴充功能開發人員將能夠取得他們的延伸模組的更多的可見性，使用者無法輕易地找到更多針對伺服器的管理功能。

![擴充功能探索橫幅](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## 擴充功能探索橫幅的運作方式 ##

啟動 Windows Admin Center 時，它將會連線到已註冊的擴充功能摘要，並擷取可用的延伸模組套件的中繼資料。 然後當使用者連接到伺服器或叢集 Windows Admin Center 中的，我們會讀取概觀工具中顯示的伺服器硬體製造商和型號。 如果我們找到宣告它支援目前伺服器的製造商和/或模型的擴充功能，我們會顯示，讓使用者知道橫幅。 按一下 「 立即設定 」 的連結將使用者帶到延伸模組管理員他們可以在其中安裝延伸模組。

## 如何實作延伸模組探索橫幅 ##

.Nuspec 檔案中的 「 標記 」 中繼資料用來宣告哪些硬體製造商和/或模型化您的延伸支援。 以空格分隔標記，您可以新增為製造商或模型標記，或兩者宣告支援的製造商和/或模型。 標記格式是``"[value type]_[value condition]"``[實值型別] 上述任一種 「 製造商 」 或 「 模型 」 （區分大小寫），而 [值條件] 是[Javascript 規則運算式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)定義製造商或模型字串與 [實值型別] 和 [value 條件] 是以底線分隔。 使用 URI 編碼，並新增到.nuspec 「 標記 」 中繼資料字串，然後編碼此字串。

### 範例 ###

比方說 [我已開發擴充功能支援的模型，且名為 Contoso Inc.公司伺服器 R3xx 和 R4xx 名稱。

1. 製造商的標記會是``"Manufacturer_/Contoso Inc./"``。 模型的標記可能是``"Model_/^R[34][0-9]{2}$/"``。 根據嚴格您想要定義符合條件時，會有不同的方式來定義您的規則運算式。 您也可以分隔製造商或模型標記到多個標記，例如，也可能是模型標記``"Model_/R3../ Model_/R4../"``。
2. 您可以測試規則運算式與您的網頁瀏覽器 DevTools 主控台。 在 Edge 或 Chrome，叫用 F12 以開啟 DevTools] 視窗中，並在主控台索引標籤中，輸入下列，然後按 Enter:

```javascript
var regex = /^R[34][0-9]{2}$/
```

如果您輸入，並執行下列命令，它會傳回 '，則為 true'。

```javascript
regex.test('R300')
```

如果您執行下列命令，它將會傳回 false' 和。

```javascript
regex.test('R500')
```

3. 一旦您已確認規則運算式時，您可以加以編碼在 DevTools 主控台中，使用下列 Javascript 方法：

```javascript
encodeURI(/^R[34][0-9]{2}$/)
```

就是字串的最終的標記，用以新增.nuspec 檔案格式：

```
<tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
```

> [!Tip]
> 我們了解硬體製造商可能會有很大範圍的型號名稱，其中有些可能支援而有些不是。 請記住，這項功能為了協助進行**探索**您的擴充功能，但其不具會完全保持最新狀態的詳細目錄的所有您的模型。 您可以定義您的規則運算式是較簡單的運算式符合您的模型的子集。 使用者可能不會看到探索橫幅如果他們第一次連線到伺服器的模型，不符合條件，但很快他們將會連線到另一部伺服器，並將會探索和安裝和延伸模組。 您也可以考慮定義簡單的規則運算式只符合您製造商的名稱。 在某些情況下，您的擴充功能可能無法實際支援特定的模型，但您可以使用[動態工具顯示功能](./dynamic-tool-display.md)來定義自訂 PowerShell 指令碼來檢查模型支援，並只會顯示您的擴充功能時適用，或提供限制您的模型不支援的所有功能的擴充功能中的功能。