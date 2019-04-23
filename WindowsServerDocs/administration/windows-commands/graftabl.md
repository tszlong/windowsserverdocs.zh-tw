---
title: graftabl
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b08351d4-3d24-490c-86f6-1252da11d923
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 395873cf3dbeb574dd9abc69f45b410bece80c25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848439"
---
# <a name="graftabl"></a>graftabl



可讓 Windows 作業系統，顯示在圖形模式中設定的擴充的字元。 如果未指定參數，使用**graftabl**會顯示先前和目前的字碼頁。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
graftabl <CodePage>
graftabl /status
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<CodePage>|指定字碼頁在圖形模式下定義的擴充字元的外觀。</br>有效的字碼頁識別號碼如下：</br>437:美國</br>850:多語系 (拉丁文 I)</br>852:斯拉夫文 (拉丁文 II)</br>855:斯拉夫文 （俄文）</br>857:土耳其文</br>860:葡萄牙文</br>861:冰島文</br>863:加拿大法文</br>865:北歐</br>866:俄文</br>869:現代希臘文|
|/status|顯示目前字碼頁所**graftabl**使用。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   **Graftabl**只會影響監視顯示您所指定的字碼頁的擴充字元。 它不會變更實際的主控台輸入的字碼頁。 若要變更主控台輸入的字碼頁，請使用**模式**或是**chcp**命令。
-   下表列出每個結束代碼和它的簡短描述。  
    |結束代碼|描述|
    |---------|-----------|
    |0|已成功載入字元集。 已不載入任何先前的字碼頁。|
    |1|指定了不正確的參數。 未採取任何動作。|
    |2|發生檔案錯誤。|
-   您可以使用 ERRORLEVEL 環境變數中的批次程式，以處理序所傳回的結束代碼**graftabl**。

## <a name="BKMK_examples"></a>範例

若要檢視目前所使用的字碼頁**graftabl**，型別：
```
graftabl /status
```
若要載入到記憶體中的字碼頁 437 則 （美國） 的圖形字元集，請輸入：
```
graftabl 437
```
若要載入圖形所設定的字元集字碼頁 850 （多語言） 載入記憶體，請輸入：
```
graftabl 850
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

[freedisk](freedisk.md)

[Chcp](chcp.md)