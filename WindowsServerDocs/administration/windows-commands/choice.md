---
title: choice
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c65a9119-410b-4dcf-9fa7-4e07d2a7238b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78d2816bff754ef04558cf37eaada7c7fafba823
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841549"
---
# <a name="choice"></a>choice



會提示使用者從清單中的單一字元選項，在批次程式中，選取一個項目，則會傳回選取的選項的索引。 如果未指定參數，使用**選擇**會顯示預設選項**Y**並**N**。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
choice [/c [<Choice1><Choice2><…>]] [/n] [/cs] [/t <Timeout> /d <Choice>] [/m <"Text">]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/c \<Choice1><Choice2><…>|指定要建立選擇清單。 有效的選項包括 a-z、 A-Z、 0-9，以及延伸的 ASCII 字元 (128 254)。 預設清單是 「 YN"，其中會顯示為`[Y,N]?`。|
|/n|雖然仍會啟用這些選項會隱藏的選擇，清單和訊息文字 (如果指定 **/m**) 仍會顯示。|
|/cs|指定這些選項包括區分大小寫。 根據預設，這些選項不會區分大小寫。|
|/t \<Timeout>|指定要使用所指定的預設選擇之前暫停的秒數 **/d**。 可接受的值是從**0**要**9999**。 如果 **/t**設為**0**，**選擇**才不會暫停之前傳回的預設選項。|
|/d \<Choice>|指定預設選項來使用等候所指定的秒數之後 **/t**。 預設選項必須是所指定的選項清單 **/c**。|
|/m <"text">|指定要顯示的選項清單之前的訊息。 如果 **/m**未指定，只有選擇提示會顯示。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   ERRORLEVEL 環境變數設為 使用者從選項清單中選取的索引鍵的索引。 在清單中的第一個選擇傳回值為 1，第二個值為 2，依此類推。 如果使用者按下按鍵，就不是有效的選擇**選擇**發出警告的嗶聲。 如果**選擇**偵測到錯誤狀況，它會傳回的 ERRORLEVEL 值為 255。 如果使用者按下 CTRL + BREAK 或 CTRL + C**選擇**傳回的 ERRORLEVEL 值為 0。

> [!NOTE]
> 當您在批次程式中使用 ERRORLEVEL 值時，以遞減順序列出。

## <a name="BKMK_examples"></a>範例

若要呈現的選擇，Y，N，C，輸入批次檔中的下面這一行：
```
choice /c ync
```
當批次檔執行時，會顯示下列提示**選擇**命令：
```
[Y,N,C]?
```
隱藏選項 Y、 N 和 C，但顯示文字"是、 否] 或 [繼續 」，批次檔中輸入下列命令：
```
choice /c ync /n /m "Yes, No, or Continue?"
```
當批次檔執行時，會顯示下列提示**選擇**命令：
```
Yes, No, or Continue?
```

> [!NOTE]
> 如果您使用 **/n**參數，但是不會使用 **/m**，使用者不是提示當**選擇**正在等候輸入。

若要顯示的文字和上一個範例中使用的選項，請在 批次檔中輸入下面這一行：
```
choice /c ync /m "Yes, No, or Continue"
```
當批次檔執行時，會顯示下列提示**選擇**命令：
```
Yes, No, or Continue [Y,N,C]?
```
若要設定時間限制為五秒，並指定**N**作為預設值，請輸入下列命令批次檔中：
```
choice /c ync /t 5 /d n
```
當批次檔執行時，會顯示下列提示**選擇**命令：
```
[Y,N,C]?
```

> [!NOTE]
> 在此範例中，如果使用者不會不按任一鍵在五秒內， **choice**選取**N**依預設，並傳回錯誤值為 2。 否則，請**選擇**傳回對應至使用者選擇的值。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)