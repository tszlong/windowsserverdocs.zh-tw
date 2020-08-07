---
title: color
description: Color 命令的參考文章，這會在目前會話的 [命令提示字元] 視窗中變更前景和背景色彩。
ms.topic: article
ms.assetid: f5b67131-d196-45ec-a3f9-b5d9f091fd86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2534273eebe7f8596b0e8f2ab3c90cfdcf824d00
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87892675"
---
# <a name="color"></a>color

在 [命令提示字元] 視窗中，變更目前會話的前景和背景色彩。 如果在沒有參數的情況下使用， **color**會還原預設的命令提示字元視窗前景和背景色彩。

## <a name="syntax"></a>語法

```
color [[<b>]<f>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<b>` | 指定背景色彩。 |
| `<f>` | 指定前景色彩。 |
| /? | 在命令提示字元顯示說明。 |

其中：

下表列出可用來做為和值的有效十六進位數位 `<b>` `<f>` ：

| 值 | Color |
| ----- | ----- |
| 0 | 黑色 |
| 1 | 藍色 |
| 2 | 綠色 |
| 3 | Aqua |
| 4 | 紅色 |
| 5 | 紫色 |
| 6 | 黃色 |
| 7 | 白色 |
| 8 | 灰色 |
| 9 | 淺藍色 |
| a | 淺綠 |
| b | 淺淺綠色 |
| c | 淺紅色 |
| d | 淺紫色 |
| e | 淺黃色 |
| f | 明亮白色 |

#### <a name="remarks"></a>備註

- 請勿在和之間使用空白字元 `<b>` `<f>` 。

- 如果您只指定一個十六進位數位，則會使用對應的色彩做為前景色彩，並將背景色彩設定為預設色彩。

- 若要設定預設的 [命令提示字元] 視窗色彩，請選取 [**命令提示**字元] 視窗的左上角，選取 [**預設值**]，選取 [**色彩**] 索引標籤，然後選取要用於**螢幕文字**和**螢幕背景**的色彩。

- 如果 `<b>` 和 `<f>` 都是相同的色彩值，則 ERRORLEVEL 會設定為 `1` ，而且前景或背景色彩都不會進行任何變更。

## <a name="examples"></a>範例

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

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
