---
title: 選項
description: Choice 命令的參考文章，此命令會提示使用者從 batch 程式中的單一字元挑選清單中選取一個專案，然後傳回所選選項的索引。
ms.topic: reference
ms.assetid: c65a9119-410b-4dcf-9fa7-4e07d2a7238b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 367f52ee41e72fe1c4c94c37a07e3a4227dec8a7
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026042"
---
# <a name="choice"></a>選項

提示使用者從 batch 程式的單一字元挑選清單中選取一個專案，然後傳回所選選項的索引。 如果使用時不含參數，[ **選擇** ] 會顯示預設選項 [ **Y** ] 和 [ **N**]。

## <a name="syntax"></a>語法

```
choice [/c [<choice1><choice2><…>]] [/n] [/cs] [/t <timeout> /d <choice>] [/m <text>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /c `<choice1><choice2><…>` | 指定要建立之選項的清單。 有效的選項包括 a-z、a-z、0-9 及擴充的 ASCII 字元 (128-254) 。 預設清單為 [YN]，其顯示為 `[Y,N]?` 。 |
| /n | 隱藏選項清單，雖然仍會啟用選擇，而且如果有 **/m** 指定的郵件內文 (，) 仍會顯示。 |
| /cs | 指定選擇區分大小寫。 依預設，選擇不區分大小寫。 |
| 一起 `<timeout>` | 指定使用 **/d**指定的預設選項之前要暫停的秒數。 可接受的值是從 **0** 到 **9999**。 如果 **/t** 設定為 **0**，則在傳回預設選項之前， **選擇** 不會暫停。 |
| /d `<choice>` | 指定在等候 **/t**指定的秒數之後，所要使用的預設選項。 預設選項必須在 **/c**指定的選項清單中。 |
| 一樣 `<text>` | 指定要在挑選清單之前顯示的訊息。 如果未指定 **/m** ，則只會顯示選擇提示。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

- **ERRORLEVEL**環境變數會設定為使用者從選項清單中選取的索引鍵索引。 清單中的第一個選擇會傳回的值 `1` 、的第二個值 `2` ，依此類推。 如果使用者按下不是有效選擇的按鍵，則 **選擇** 會聽到警告嗶聲。

- 如果 **choice** 偵測到錯誤狀況，則會傳回的 **ERRORLEVEL** 值 `255` 。 如果使用者按下 CTRL + BREAK 或 CTRL + C， **choice** 會傳回的 **ERRORLEVEL** 值 `0` 。

> [!NOTE]
> 當您在 batch 程式中使用 **ERRORLEVEL** 值時，您必須以遞減順序列出它們。

## <a name="examples"></a>範例

若要顯示 **Y**、 **N**和 **C**選項，請在批次檔中輸入下列程式程式碼：

```
choice /c ync
```

當批次檔執行 **choice** 命令時，會出現下列提示：

```
[Y,N,C]?
```

若要隱藏選擇 [ **Y**]、[ **N**] 和 [ **C**]，但顯示 [**是]、[****否**] 或 [**繼續**]，請在批次檔中輸入下列程式程式碼：

```
choice /c ync /n /m Yes, No, or Continue?
```

> [!NOTE]
> 如果您使用 **/n** 參數，但不使用 **/m**，則當 **選擇** 等候輸入時，不會提示使用者。

若要顯示先前範例中所使用的文字和選項，請在批次檔中輸入下列程式程式碼：

```
choice /c ync /m Yes, No, or Continue
```

若要設定五秒的時間限制，並將 **N** 指定為預設值，請在批次檔中輸入下列程式程式碼：

```
choice /c ync /t 5 /d n
```

> [!NOTE]
> 在此範例中，如果使用者未在五秒內按下按鍵， **選擇** 預設會選取 [ **N** ]，並傳回的錯誤值 `2` 。 否則， **choice** 會傳回對應至使用者選擇的值。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
