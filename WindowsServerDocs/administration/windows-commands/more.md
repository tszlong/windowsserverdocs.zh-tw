---
title: more
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ded14f6a-d82f-4aeb-a2d8-7ec1c94dfb8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/26/2019
ms.openlocfilehash: 291d98492f3f2b200ff0567c28a97927ca8c75be
ms.sourcegitcommit: e55e27143dad1d3fb956cfdac4c23ef4186af321
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2019
ms.locfileid: "68603228"
---
# <a name="more"></a>more



一次顯示一個輸出畫面。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
<Command> | more [/c] [/p] [/s] [/t<N>] [+<N>]
more [[/c] [/p] [/s] [/t<N>] [+<N>]] < [<Drive>:][<Path>]<FileName>
more [/c] [/p] [/s] [/t<N>] [+<N>] [<Files>]
```

## <a name="parameters"></a>參數

|           參數            |                               描述                               |
|--------------------------------|-------------------------------------------------------------------------|
|           \<命令 >           |      指定您想要顯示輸出的命令。      |
|               /c               |               在顯示頁面之前清除畫面。               |
|               /p               |                      展開表單換頁字元。                      |
|               /s               |          將多個空白行顯示成一行。          |
|             /t\<N >             |         將索引標籤顯示為*N*所指定的空間數目。         |
|             +\<N >              |     顯示第一個檔案, 該檔案是從*N*所指定的行開始。     |
| [\<磁片磁碟機 >:]\<[路徑 >\<] 檔案名 > |          指定要顯示之檔案的位置和名稱。          |
|            \<檔案 >            | 指定要顯示的檔案清單。 以空格分隔檔案名。 |
|               /?               |                  在命令提示字元顯示說明。                   |

## <a name="remarks"></a>備註

-   在**更多**提示 (`-- More --`) 中可接受下列子命令。 

    | Key | Action |
    | --- | ------ |
    | 空格鍵 | 顯示下一個頁面。 |
    | ENTER | 顯示下一行。 |
    | f | 顯示下一個檔案。 |
    | q | 退出 [**更多**] 命令。 |
    | = | 顯示行號。 |
    | p \<N > | 顯示接下來的*N*行。 |
    | s \<N > |S kips 下*N*行。 |
    | ? | 顯示可在**更多**提示字元中使用的命令。| 
    
-   使用重新導向字元 ( **<** ) 時, 您必須指定檔案名做為來源。 使用 [管道] ( **\|** ) 時, 您可以使用**目錄**、**排序**和**類型**之類的命令。
-   具有不同參數的命令**越多**, 就可以從修復主控台中取得。

## <a name="BKMK_examples"></a>典型

若要查看名為 [用戶端] 之檔案的第一個畫面, 請輸入下列其中一個命令:
```
more < clients.new
type clients.new | more
```
[**更多**] 命令會顯示用戶端的第一個畫面資訊。 [新增], 然後顯示下列提示:
```
-- More --
```
然後, 您可以按空格鍵查看下一個畫面的資訊。

若要清除畫面並移除所有額外的空白行, 然後再顯示檔案用戶端。新的, 請輸入下列其中一個命令:
```
more /c /s < clients.new
type clients.new | more /c /s
```
[**更多**] 命令會顯示用戶端的第一個畫面資訊。 [新增], 然後顯示下列提示:
```
-- More --
```

### <a name="using-more-subcommands"></a>使用更多子命令

下列範例可在**更多**提示 (`-- More --`) 中使用。
- 若要一次顯示一行檔案, 請按下 ENTER 鍵。
- 若要顯示下一個畫面, 請按下空格鍵 , 再按一次。
- 若要顯示命令列上列出的下一個檔案, 請在 [ 提示] 中輸入**f** 。
- 若要顯示可用的命令, 請輸入 **？** 會出現**更多**提示。
- 若要結束**更多**, 請在 [提示] 中輸入**q** 。
- 若要顯示目前的行號, **=** 請輸入**更多**的提示。 目前的行號已新增至**更多**提示, 如下所示:  
  ```
  -- More [Line: 24] --
  ```  
- 若要顯示特定的行數, 請在 [提示 ] 中輸入**p** 。 [**其他**] 會提示您輸入要顯示的行數, 如下所示:  
  ```
  -- More -- Lines:
  ```  
  輸入要顯示的行數, 然後按 ENTER 鍵。 [**更多**] 會顯示指定的行數。
- 若要略過特定的行數, 請在**更多**的提示中輸入**s** 。 [**其他**] 會提示您輸入要略過的行數, 如下所示:  
  ```
  -- More -- Lines:
  ```  
  輸入要略過的行數, 然後按 ENTER 鍵。 **更多**略過指定的行數, 並顯示下一個畫面的資訊。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
