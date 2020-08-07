---
title: 選項
description: '[選擇] 命令的參考文章，會提示使用者從 batch 程式中的單一字元選項清單中選取一個專案，然後傳回所選選擇的索引。'
ms.topic: article
ms.assetid: c65a9119-410b-4dcf-9fa7-4e07d2a7238b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6aec3af1966d0b7952f0bbb5880e373356117e42
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880249"
---
# <a name="choice"></a>選項

提示使用者從 batch 程式中的單一字元選項清單中選取一個專案，然後傳回所選選擇的索引。 如果使用時不含參數， **choice**會顯示預設選項**Y**和**N**。

## <a name="syntax"></a>語法

```
choice [/c [<choice1><choice2><…>]] [/n] [/cs] [/t <timeout> /d <choice>] [/m <text>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /c`<choice1><choice2><…>` | 指定要建立的選項清單。 有效的選項包括 a-z、a-z、0-9 和擴充的 ASCII 字元， (128-254) 。 預設清單是 [YN]，它會顯示為 `[Y,N]?` 。 |
| /n | 隱藏選項清單，雖然仍會啟用選擇，而且如果) **/m**所指定的郵件內文 (，仍然會顯示。 |
| /cs | 指定選擇是否區分大小寫。 根據預設，這些選項不區分大小寫。 |
| 一起`<timeout>` | 指定在使用 **/d**所指定的預設選項之前，要暫停的秒數。 可接受的值為**0**到**9999**。 如果 **/t**設定為**0**，則在傳回預設選項之前，**選擇**不會暫停。 |
| /d`<choice>` | 指定在等候 **/t**指定的秒數之後，要使用的預設選項。 預設選項必須位於 **/c**所指定的挑選清單中。 |
| /m`<text>` | 指定要在挑選清單之前顯示的訊息。 如果未指定 **/m** ，則只會顯示選擇提示。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

- **ERRORLEVEL**環境變數會設定為使用者從選項清單中選取之索引鍵的索引。 清單中的第一個選擇會傳回的值 `1` ，第二個是的值 `2` ，依此類推。 如果使用者按下不是有效選擇的機碼，則**選擇**會發出警告嗶聲。

- 如果**選擇**偵測到錯誤狀況，則會傳回的**ERRORLEVEL**值 `255` 。 如果使用者按下 CTRL + BREAK 或 CTRL + C， **choice**會傳回的**ERRORLEVEL**值 `0` 。

> [!NOTE]
> 當您在 batch 程式中使用**ERRORLEVEL**值時，您必須以遞減順序列出它們。

## <a name="examples"></a>範例

若要顯示**Y**、 **N**和**C**的選項，請在批次檔中輸入下列程式程式碼：

```
choice /c ync
```

當批次檔執行**選擇**命令時，會出現下列提示：

```
[Y,N,C]?
```

若要隱藏選項**Y**、 **N**和**C**，但顯示 [**是]**、[**否**] 或 [**繼續**] 文字，請在批次檔中輸入下列程式程式碼：

```
choice /c ync /n /m Yes, No, or Continue?
```

> [!NOTE]
> 如果您使用 **/n**參數，但未使用 **/m**，當**選擇**等待輸入時，不會提示使用者。

若要顯示先前範例中所使用的文字和選項，請在批次檔中輸入下列程式程式碼：

```
choice /c ync /m Yes, No, or Continue
```

若要設定5秒的時間限制，並指定**N**做為預設值，請在批次檔中輸入下列程式程式碼：

```
choice /c ync /t 5 /d n
```

> [!NOTE]
> 在此範例中，如果使用者未在五秒內按下按鍵，**選擇**預設會選取**N** ，並傳回的錯誤值 `2` 。 否則， **choice**會傳回對應于使用者選擇的值。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
