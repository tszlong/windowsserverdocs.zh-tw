---
title: chcp
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7b1c71-7b80-443d-9cf1-9bcf305aa1fd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 622d4b64128c7e39cc761e4f5e9d69cf54383760
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819199"
---
# <a name="chcp"></a>chcp



變更作用中的主控台字碼頁。 如果未指定參數，使用**chcp**顯示作用中的主控台字碼頁的數目。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
chcp [<NNN>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<NNN>|指定的字碼頁。|
|/?|在命令提示字元顯示說明。|

下表列出每個支援的字碼頁和國家/地區或語言：

|字碼頁|國家/地區或語言|
|---------|--------------------------|
|437|美國|
|850|多語系 (拉丁文 I)|
|852|斯拉夫文 (拉丁文 II)|
|855|斯拉夫文 （俄文）|
|857|土耳其文|
|860|葡萄牙文|
|861|冰島文|
|863|加拿大法文|
|865|北歐|
|866|俄文|
|869|現代希臘文|
|936|中文|

## <a name="remarks"></a>備註

-   只有原始設備製造商 (OEM) 字碼頁與 Windows 一起安裝會正確出現在命令提示字元視窗中，使用點陣字型。 在全螢幕模式或使用 TrueType 字型的 [命令提示字元] 視窗中，會正確出現其他字碼頁。
-   您不需要準備 （就像在 MS-DOS) 的字碼頁。
-   之後啟動的程式會指派新的程式碼頁面使用新的字碼頁。 不過，您開始之前，您的程式 （Cmd.exe) 除外指派新的程式碼頁面使用的原始字碼頁。

## <a name="BKMK_examples"></a>範例

若要檢視作用中的字碼頁設定，請輸入：
```
chcp
```
此時會出現類似下面的訊息：

`Active code page: 437`

若要變更作用中的字碼頁 850 （多國語言），請輸入：
```
chcp 850
```
如果指定的字碼頁無效，則會出現下列錯誤訊息：

`Invalid code page`

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
