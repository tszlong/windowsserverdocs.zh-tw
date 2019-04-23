---
title: more
description: '適用於 Windows 命令主題 * * *- '
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
ms.date: 10/16/2017
ms.openlocfilehash: d4957f9455c6caed027331a939db0c2fefbe1961
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857569"
---
# <a name="more"></a>more



一次顯示一個螢幕的輸出。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
<Command> | more [/c] [/p] [/s] [/t<N>] [+<N>]
more [[/c] [/p] [/s] [/t<N>] [+<N>]] < [<Drive>:][<Path>]<FileName>
more [/c] [/p] [/s] [/t<N>] [+<N>] [<Files>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<命令 >|指定您要顯示輸出的命令。|
|/c|清除之前顯示頁面的螢幕。|
|/p|展開換頁字元。|
|/s|以單一的空白列中顯示多個空白行。|
|/t\<N>|顯示索引標籤，為所指定的空格數目*N*。|
|+\<N>|在所指定的列中顯示的第一個檔案開頭*N*。|
|[\<Drive>:] [<Path>]<FileName>|指定要顯示之檔案的名稱與位置。|
|\<檔案 >|指定要顯示檔案的清單。 以空格分隔的檔案名稱。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   下列子命令會接受**更多**提示字元 (`-- More --`)。  
    |Key|動作|
    |---|------|
    |空格鍵|顯示下一個頁面。|
    |ENTER|顯示的下一行。|
    |f|顯示下一個檔案。|
    |q|結束**更多**命令。|
    |=|顯示行號。|
    |p \<N>|顯示下一步*N*行。|
    |s \<N>|略過下一個*N*行。|
    |?|顯示在可用的命令**更多**提示字元。|
-當使用重新導向字元 (**<**)，您必須指定檔案名稱做為來源。 當使用管道 (* *|*），您可以使用這類命令作為**dir**，**排序**，並**型別**。
-   **更多**命令，使用不同的參數，可從修復主控台。

## <a name="BKMK_examples"></a>範例

若要檢視名為 Clients.new 檔案資訊的第一個畫面，輸入下列命令之一：
```
more < clients.new
type clients.new | more
```
**更多**命令會顯示來自 Clients.new，資訊的第一個畫面，並接著會顯示下列提示：
```
-- More --
```
然後，您可以按空格鍵以查看資訊的下一個畫面。

若要清除螢幕，並移除所有額外空白的行之前顯示檔案 Clients.new，輸入下列命令之一：
```
more /c /s < clients.new
type clients.new | more /c /s
```
**更多**命令會顯示來自 Clients.new，資訊的第一個畫面，並接著會顯示下列提示：
```
-- More --
```

### <a name="using-more-subcommands"></a>使用更多的子命令

下列範例可在**更多**提示字元 (`-- More --`)。
-   若要一次顯示一行，在按 ENTER 鍵**更多**提示字元。
-   若要顯示下一個畫面中，按一下 在空格鍵**更多**提示字元。
-   若要顯示在命令列上所列的下一個檔案，請輸入**f**在**詳細**提示字元。
-   若要顯示可用的命令，請輸入**嗎？** 在**更多**提示字元。
-   結束**更多**，型別**q**在**詳細**提示字元。
-   若要顯示目前的行號，請輸入**=** 在**詳細**提示字元。 若要加入目前的行號**更多**提示，如下所示：  
    ```
    -- More [Line: 24] --
    ```  
-   若要顯示特定的行數，請輸入**p**在**詳細**提示字元。 **更多**會提示您輸入要顯示，如下所示的行數：  
    ```
    -- More -- Lines:
    ```  
    輸入要顯示的行數，然後按 ENTER 鍵。 **更多**會顯示指定的行數。
-   若要跳過特定的行數，請輸入**s**在**詳細**提示字元。 **更多**會提示您輸入的略過，如下所示的行數：  
    ```
    -- More -- Lines:
    ```  
    輸入的略過行數，然後按 ENTER 鍵。 **更多**略過指定的行數，並顯示資訊的下一個畫面。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)