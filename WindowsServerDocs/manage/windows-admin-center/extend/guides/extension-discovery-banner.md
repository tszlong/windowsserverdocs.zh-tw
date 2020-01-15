---
title: 啟用延伸模組探索橫幅
description: 啟用延伸模組探索橫幅
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: d761ba61ae5680373c334889799e82e5d092a0d4
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950102"
---
# <a name="enabling-the-extension-discovery-banner"></a>啟用延伸模組探索橫幅

>適用於：Windows Admin Center、Windows Admin Center 預覽版

Windows 管理中心 Preview 1903 中提供的新功能是延伸模組探索橫幅。 這項功能可讓延伸模組宣告所支援的伺服器硬體製造商和模型，以及當使用者連線到可用擴充功能的伺服器或叢集時，將會顯示通知橫幅，方便安裝延伸模組。 延伸模組開發人員將能夠取得其延伸模組的更多可見度，而且使用者將能夠輕鬆地探索其伺服器的更多管理功能。

![延伸模組探索橫幅](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## <a name="how-the-extension-discovery-banner-works"></a>延伸模組探索橫幅的運作方式

當 Windows 管理中心啟動時，它會連接到已註冊的擴充功能摘要，並提取可用擴充功能套件的中繼資料。 接著，當使用者連線到 Windows 系統管理中心的伺服器或叢集時，我們會閱讀伺服器硬體製造商和模型，以顯示在 [總覽] 工具中。 如果找到一個延伸模組，宣告它支援目前伺服器的製造商和/或型號，我們將會顯示橫幅，讓使用者知道。 按一下 [立即設定] 連結會將使用者帶到 [擴充管理員]，讓他們可以在其中安裝延伸模組。

## <a name="how-to-implement-the-extension-discovery-banner"></a>如何執行延伸模組探索橫幅

Nuspec 檔案中的「標記」中繼資料是用來宣告您的擴充功能支援哪些硬體製造商和/或型號。 標籤會以空格分隔，而您可以加入製造商或型號標記，或兩者都來宣告支援的製造商和/或型號。 標記格式是 ``"[value type]_[value condition]"``，其中 [數值型別] 是「製造商」或「模型」（區分大小寫），而 [值條件] 是定義製造商或模型字串的[JAVAscript 正則運算式](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Regular_Expressions)，而 [數值型別] 和 [值條件] 則以底線分隔。 接著會使用 URI 編碼來編碼此字串，並將其新增至. nuspec "tags" 中繼資料字串。

### <a name="example"></a>範例

假設我開發了一個擴充功能，它支援名為 Contoso Inc. 的公司伺服器，其模型名稱為 R3xx 和 R4xx。

1. 製造商的標記會是 ``"Manufacturer_/Contoso Inc./"``。 模型的標記可能 ``"Model_/^R[34][0-9]{2}$/"``。 視您要定義比對條件的嚴格程度而定，會有不同的方式來定義正則運算式。 您也可以將製造商或型號標記分隔成多個標記，例如，模型標記也可能 ``"Model_/R3../ Model_/R4../"``。
2. 您可以使用網頁瀏覽器的 DevTools 主控台來測試正則運算式。 在 Edge 或 Chrome 中，按 F12 開啟 [DevTools] 視窗，然後在 [主控台] 索引標籤中輸入下列內容，然後按 Enter 鍵：

   ```javascript
   var regex = /^R[34][0-9]{2}$/
   ```

   然後，如果您輸入並執行下列動作，它會傳回 ' true '。

   ```javascript
   regex.test('R300')
   ```

   而且，如果您執行下列動作，它會傳回 ' false '。

   ```javascript
   regex.test('R500')
   ```

3. 確認正則運算式之後，您也可以在 DevTools 主控台中使用下列 JAVAscript 方法來編碼它：

   ```javascript
   encodeURI(/^R[34][0-9]{2}$/)
   ```

   要新增至 nuspec 檔案的標記字串，其最終格式會是：

   ```
   <tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
   ```

> [!Tip]
> 我們瞭解硬體製造商可能會有非常廣泛的型號名稱，其中有些可能會受到支援，有些則否。 請記住，這項功能的目的是要協助**探索**您的延伸模組，但不需要是所有模型的完全最新清查。 您可以定義正則運算式，使其成為符合模型子集的較簡單運算式。 如果使用者第一次連線到不符合條件的伺服器模型，可能就不會看到探索橫幅，但較快或之後，它們會連接到另一部執行的伺服器，並將探索並安裝擴充功能。 您也可以考慮定義只符合製造商名稱的簡單正則運算式。 在某些情況下，您的延伸模組可能不會實際支援特定模型，但您可以使用[動態工具顯示功能](./dynamic-tool-display.md)來定義自訂的 PowerShell 腳本，以檢查模型支援並僅在適用時顯示您的擴充功能，或在延伸模組中為不支援所有功能的模型提供有限的功能。