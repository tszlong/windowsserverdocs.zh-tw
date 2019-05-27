---
title: color
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5b67131-d196-45ec-a3f9-b5d9f091fd86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8d23cc1bb5739b47c755d90c927cbcf82b8da7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825019"
---
# <a name="color"></a>color



變更前景和背景色彩在命令提示字元 視窗中目前的工作階段。 如果未指定參數，使用**色彩**還原預設命令提示字元 視窗前景和背景色彩。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
color [[<B>]<F>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<B>|指定背景色彩。|
|\<F>|指定前景色彩。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   下表列出有效的十六進位數字，您可以使用的值作為*B*並*F*。  
    |值|色彩|
    |-----|-----|
    |0|黑色|
    |1|藍色|
    |2|綠色|
    |3|青色|
    |4|紅色|
    |5|紫色|
    |6|黃色|
    |7|白皮書|
    |8|灰色|
    |9|淺藍色|
    |A|淺綠色|
    |B|淡藍|
    |C|淡紅色|
    |D|淺紫|
    |E|淺黃色|
    |F|亮白色|
-   請勿使用空格字元間*B*並*F*。
-   如果您指定只有一個十六進位數字，對應的色彩做為前景色彩和背景色彩會設為預設色彩。
-   若要設定預設的命令提示字元視窗色彩，請按一下 命令提示字元 視窗左上角，按一下 **預設值**，按一下**色彩**索引標籤，然後再按您想要用於的色彩**畫面文字**並**畫面背景**。
-   如果*B*並*F*相同，即**色彩**命令會設定 ERRORLEVEL 為 1，並不會變更前景或背景色彩。

## <a name="BKMK_examples"></a>範例

若要將命令提示字元視窗背景色彩變更為灰色，把前景色彩設為紅色，請輸入：
```
color 84
```
若要變更為淺黃色的命令提示字元 視窗的前景色彩，請輸入：
```
color e
```

> [!NOTE]
> 在此範例中，背景是設定為預設色彩，因為指定一個十六進位數字。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)