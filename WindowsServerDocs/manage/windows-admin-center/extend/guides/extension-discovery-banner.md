---
title: 啟用擴充功能探索橫幅
description: 啟用擴充功能探索橫幅
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fafc16d6acae3c5a7a58a379d2f88998b8e98c3d
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811876"
---
# <a name="enabling-the-extension-discovery-banner"></a>啟用擴充功能探索橫幅

>適用於：Windows Admin Center，Windows Admin Center 預覽

在 Windows Admin Center 預覽 1903年中提供的新功能是擴充功能探索橫幅。 這項功能可讓擴充功能來宣告的伺服器硬體製造商和其支援的模型，當使用者連線到伺服器或延伸模組是可用的叢集，就輕鬆安裝延伸模組會顯示通知橫幅。 延伸模組開發人員將能夠取得更多可見性，其延伸模組，使用者將能夠輕鬆地找出其伺服器的多個管理功能。

![擴充功能探索橫幅](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## <a name="how-the-extension-discovery-banner-works"></a>擴充功能探索的橫幅中的運作方式

啟動 Windows Admin Center 時，它會連線到已註冊擴充功能摘要，並擷取可用的延伸模組套件的中繼資料。 當使用者連線到伺服器或 Windows Admin Center 中的叢集，我們會讀取伺服器硬體製造商和型號，顯示在 [概觀] 工具。 如果我們發現擴充功能會宣告它支援目前的伺服器製造商及 （或） 模型，我們會顯示橫幅，讓使用者知道。 按一下 [設定現在] 連結會讓使用者延伸模組管理員 」 以他們可以在其中安裝擴充功能。

## <a name="how-to-implement-the-extension-discovery-banner"></a>如何實作的擴充功能探索橫幅

.Nuspec 檔案中的"tags"中繼資料用來宣告哪些硬體製造商及/或延伸模組支援的模型。 標記以空格分隔，您可以新增製造商或模型的標籤，或兩者都宣告支援的製造商和 （或） 模型。 標記格式``"[value type]_[value condition]"``[實值型別] 是 「 製造商 」 或 「 模型 」 （區分大小寫），而 [值條件] 是[Javascript 規則運算式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)定義製造商或模型的字串和 [實值型別] 和 [值條件] 會以底線分隔。 使用編碼方式，並加入.nuspec"tags"中繼資料字串的 URI，然後編碼此字串。

### <a name="example"></a>範例

假設我開發了延伸模組支援從公司名稱 Contoso Inc.為模型的伺服器名稱 R3xx R4xx。

1. 製造商的標記會``"Manufacturer_/Contoso Inc./"``。 模型的標記可能是``"Model_/^R[34][0-9]{2}$/"``。 根據嚴格程度，您要定義比對條件，將會有不同的方式，來定義您的規則運算式。 您也可以將製造商或型號標記分成多個標記，比方說，也可能是模型標籤``"Model_/R3../ Model_/R4../"``。
2. 您可以測試網頁瀏覽器的 DevTools 主控台為規則的運算式。 在 Edge 或 Chrome 中，按 F12 開啟 DevTools 視窗中，並在 主控台 索引標籤中，輸入下列文字，然後按 Enter:

   ```javascript
   var regex = /^R[34][0-9]{2}$/
   ```

   如果您輸入並執行下列命令，它會傳回 'true'。

   ```javascript
   regex.test('R300')
   ```

   如果您執行下列命令，會傳回 'false'。

   ```javascript
   regex.test('R500')
   ```

3. 一旦您確認的規則運算式，您可以將它編碼為 DevTools 主控台，使用下列 Javascript 方法：

   ```javascript
   encodeURI(/^R[34][0-9]{2}$/)
   ```

   要新增至您的.nuspec 檔案中的標記字串的最後一個格式會是：

   ```
   <tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
   ```

> [!Tip]
> 我們了解硬體製造商可能會有非常廣泛的其中一些可能支援而有些則不是模型名稱。 請記住，這項功能為了解決**探索**延伸模組，但它並沒有為您的模型完全最新清查。 您可以定義您的規則運算式更簡單的運算式符合您的模型子集。 使用者可能無法看到探索橫幅，如果他們第一次連接到伺服器模型不符合條件，但很快地將連線至另一部伺服器，並和會探索並安裝擴充功能。 您也可以考慮定義簡單的規則運算式只會比對您的製造商名稱。 在某些情況下，您的延伸模組可能實際上支援特定的模型，但是您可以使用[動態工具顯示功能](./dynamic-tool-display.md)來定義自訂的 PowerShell 指令碼來檢查模型支援，並只顯示您的延伸模組時，或不支援所有功能的模型提供有限的功能，在您的延伸模組。