---
title: goto
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e0de1458-1f78-48ff-a746-c285a945a510
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1ad0190519d58bd879ae391f378d800760c204f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857529"
---
# <a name="goto"></a>goto



指示要加上標籤的列 cmd.exe 批次程式中。 批次在程式內**goto**將導向至由標籤列的命令處理。 找到標籤時，處理作業會繼續從下一行開始的命令。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
goto <Label> 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Label>|指定的文字字串做為批次程式中的標籤。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   使用命令延伸模組

    如果命令延伸模組為啟用 （預設值），而且您使用**goto**命令的目標標籤 **: EOF**，您將控制權移轉給目前的批次指令碼檔案的結尾，並結束批次指令碼檔案但不要定義標籤。 當您使用**goto**具有 **: EOF**標籤，您必須插入該標籤之前的冒號。 例如:   
    ```
    goto:EOF
    ```  
-   使用有效*標籤*值

    您可以使用中的空格*標籤*參數，但您不能包含其他分隔符號 （比方說，分號或等號）。
-   比對*標籤*批次程式中的標籤

    *標籤*您指定的值必須符合批次程式中的標籤。 批次程式中的標籤必須以冒號 （:） 開頭。 如果一條線是以冒號開頭，它會被視為標籤，並會忽略該行上的任何命令。 如果您的 batch 程式不包含您在中指定的標籤*標籤*，批次程式停止，並顯示下列訊息：  
    ```
    Label not found
    ```  
-   使用**goto**條件式作業

    您可以使用**goto**與執行條件式作業的其他命令。 如需使用詳細資訊**goto**條件式作業，請參閱[如果](if.md)命令參考。

## <a name="BKMK_examples"></a>範例

下列批次程式做為系統磁碟機格式化磁碟機 A 中的磁碟。 如果作業成功， **goto**命令會指示要處理 **： 結束**標籤：
```
echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program. 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)

[Cmd](cmd.md)

[If](if.md)