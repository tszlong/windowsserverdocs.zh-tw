---
title: graftabl
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b08351d4-3d24-490c-86f6-1252da11d923
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d55df814cb962e82775a86e154a024c579987cf2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842411"
---
# <a name="graftabl"></a>graftabl



可讓 Windows 作業系統以圖形模式顯示擴充字元集。 如果使用時不含參數， **graftabl**會顯示上一個和目前的字碼頁。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
graftabl <CodePage>
graftabl /status
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<字碼頁 >|指定字碼頁，以定義圖形模式中擴充字元的外觀。</br>有效的字碼頁識別編號為：</br>437：美國</br>850：多語系（拉丁 I）</br>852：斯拉夫語（拉丁 II）</br>855：斯拉夫文（俄文）</br>857：土耳其文</br>860：葡萄牙文</br>861：冰島文</br>863：加拿大-法文</br>865：北歐</br>866：俄文</br>869：新式希臘文|
|/status|顯示**graftabl**目前使用的字碼頁。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   **Graftabl**只會影響您指定之字碼頁擴充字元的監視器顯示。 它不會變更實際的主控台輸入字碼頁。 若要變更控制台輸入字碼頁，請使用**mode**或**chcp**命令。
-   下表列出每個結束代碼和其簡短描述。  
    |結束代碼|描述|
    |---------|-----------|
    |0|字元集已成功載入。 未載入先前的字碼頁。|
    |1|指定了不正確的參數。 未採取任何動作。|
    |2|發生檔案錯誤。|
-   您可以在 batch 程式中使用 ERRORLEVEL 環境變數來處理**graftabl**所傳回的結束代碼。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要查看**graftabl**所使用的目前字碼頁，請輸入：
```
graftabl /status
```
若要將字碼頁437（美國）的圖形字元集載入記憶體中，請輸入：
```
graftabl 437
```
若要將字碼頁850（多語系）的圖形字元集載入記憶體中，請輸入：
```
graftabl 850
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

[Freedisk](freedisk.md)

[Chcp](chcp.md)