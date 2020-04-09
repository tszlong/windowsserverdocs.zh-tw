---
title: chcp
description: 適用于 chcp 的 Windows 命令主題，會變更使用中的主控台字碼頁。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc7b1c71-7b80-443d-9cf1-9bcf305aa1fd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e644cf8544d135c5d21c344b0fd0a3364c7f89c1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847941"
---
# <a name="chcp"></a>chcp

變更 active 主控台字碼頁。 如果使用時不含參數，則**chcp**會顯示作用中主控台字碼頁的數目。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
chcp [<NNN>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<NNN >|指定字碼頁。|
|/?|在命令提示字元顯示說明。|

下表列出每個支援的字碼頁和其國家/地區或語言：

|字碼頁|國家/地區或語言|
|---------|--------------------------|
|437|美國|
|850|多語系（拉丁 I）|
|852|斯拉夫語（拉丁 II）|
|855|斯拉夫文（俄文）|
|857|土耳其文|
|860|葡萄牙文|
|861|冰島文|
|863|加拿大-法文|
|865|北歐|
|866|俄文|
|869|新式希臘文|
|936|中文|

## <a name="remarks"></a>備註

-   只有與 Windows 一起安裝的原始設備製造商（OEM）字碼頁，才會在使用點陣字型的 [命令提示字元] 視窗中正確顯示。 其他字碼頁會以全螢幕模式或在使用 TrueType 字型的命令提示字元視窗中正確地顯示。
-   您不需要準備字碼頁（如 MS-DOS 中的）。
-   您在指派新字碼頁之後啟動的程式會使用新的字碼頁。 不過，在您指派新字碼頁之前啟動的程式（Cmd.exe 除外），會使用原始字碼頁。

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要查看作用中的字碼頁設定，請輸入：
```
chcp
```
此時會出現類似下列的訊息：

`Active code page: 437`

若要將使用中的字碼頁變更為850（多語系），請輸入：
```
chcp 850
```
如果指定的字碼頁無效，則會出現下列錯誤訊息：

`Invalid code page`

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
