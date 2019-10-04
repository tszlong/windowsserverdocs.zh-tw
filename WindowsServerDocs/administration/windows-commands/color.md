---
title: color
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ed792e4626897945e688f1c54767d7680ade6d99
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379236"
---
# <a name="color"></a>color



在 [命令提示字元] 視窗中，變更目前會話的前景和背景色彩。 如果在沒有參數的情況下使用， **color**會還原預設的命令提示字元視窗前景和背景色彩。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
color [[<B>]<F>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<B >|指定背景色彩。|
|\<F >|指定前景色彩。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   下表列出有效的十六進位數字，您可以使用的值作為*B*並*F*。

|值|色彩|
|-----|-----|
|0|黑色|
|1|藍天|
|2|綠色|
|3|綠色|
|4|紅色|
|5|變成|
|6|黃色|
|7|白皮書|
|8|灰色|
|9|淺藍色|
|A|淺綠色|
|B|淺淺綠色|
|C|淺紅色|
|D|淺紫色|
|E|淺黃色|
|F|明亮白色|
    
-   請勿在*B*和*F*之間使用空白字元。
-   如果您只指定一個十六進位數位，則會使用對應的色彩做為前景色彩，並將背景色彩設定為預設色彩。
-   若要設定預設的 [命令提示字元] 視窗色彩，請按一下 [命令提示字元] 視窗的左上角，按一下 [**預設值**]，按一下 [**色彩**] 索引標籤，然後按一下您要用於**螢幕文字**和螢幕背景的色彩。.
-   如果*B*和*F*相同， **color**命令會將 ERRORLEVEL 設定為1，而且不會對前景或背景色彩進行任何變更。

## <a name="BKMK_examples"></a>典型

若要將 [命令提示字元] 視窗的背景色彩變更為灰色，並將前景色彩變更為紅色，請輸入：
```
color 84
```
若要將 [命令提示字元] 視窗的前景色彩變更為淺黃色，請輸入：
```
color e
```

> [!NOTE]
> 在此範例中，背景設定為預設色彩，因為只指定一個十六進位數位。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
