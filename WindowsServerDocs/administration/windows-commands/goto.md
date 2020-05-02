---
title: goto
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e0de1458-1f78-48ff-a746-c285a945a510
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dd61575b8b31ed47463db464f4aad0a048e755b2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724999"
---
# <a name="goto"></a>goto



將 cmd.exe 導向至 batch 程式中的標記行。 在 batch 程式中， **goto**會將命令處理導向至標籤所識別的一行。 找到標籤時，處理會繼續從下一行開始的命令開始。



## <a name="syntax"></a>語法

```
goto <Label> 
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<標籤>|指定在 batch 程式中當做標籤使用的文字字串。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   使用命令延伸模組

    如果已啟用命令延伸模組（預設值），而且您使用 [ **goto** ] 命令搭配目標標籤 **： EOF**，則您會將控制權轉移到目前批次腳本檔案的結尾，並結束批次腳本檔案，但不定義標籤。 當您使用**goto**搭配 **： EOF**標籤時，您必須在標籤之前插入冒號。 例如：  
    ```
    goto:EOF
    ```  
-   使用有效的*標籤*值

    您可以使用*標籤*參數中的空格，但不能包含其他分隔符號（例如，分號或等號）。
-   將*標籤*與 batch 程式中的標籤相符

    您指定的*標籤*值必須符合 batch 程式中的標籤。 Batch 程式內的標籤必須以冒號（:) 開頭。 如果一行的開頭是冒號，則會將它視為標籤，而該行上的任何命令都會被忽略。 如果您的 batch 程式不包含您在 [*標籤*] 中指定的標籤，則 batch 程式會停止並顯示下列訊息：  
    ```
    Label not found
    ```  
-   使用**goto**進行條件式作業

    您可以搭配其他命令使用**goto**來執行條件式作業。 如需使用**goto**進行條件式運算的詳細資訊，請參閱[If](if.md)命令參考。

## <a name="examples"></a>範例

下列 batch 程式會將磁片磁碟機 A 中的磁片格式化為系統磁片。 如果作業成功， **goto**命令會將處理導向至 **： end**標籤：
```
echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program. 
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

[Cmd](cmd.md)

[只有](if.md)