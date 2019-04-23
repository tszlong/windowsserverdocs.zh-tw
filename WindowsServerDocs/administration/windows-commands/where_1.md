---
title: 其中
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0b3486a5-896b-4d92-84b8-e463a0b76487
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ff50405dd53ee383abc8e13f67befecf73e37c1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832449"
---
# <a name="where"></a>其中



顯示符合指定之搜尋模式之檔案的位置。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
where [/r <Dir>] [/q] [/f] [/t] [$<ENV>:|<Path>:]<Pattern>[ ...] 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/r \<Dir>|表示遞迴搜尋，開頭為指定的目錄。|
|/q|傳回的結束代碼 (**0**成功時，如**1**失敗) 而不會顯示相符的檔案清單。|
|/f|顯示的結果**其中**命令在引號內。|
|/t|顯示檔案大小和上次修改的日期和時間的每個相符的檔案。|
|[$\<ENV >:\|\<路徑 >:]\<模式 > [...]|指定檔案，以符合搜尋的模式。 至少一個模式，而且模式可以包含萬用字元 (**&#42;** 並 **？**)。 根據預設，**其中**搜尋目前的目錄與路徑環境變數中指定的路徑。 您可以指定不同的路徑，若要搜尋使用格式 $*ENV*:*模式*(其中*ENV*是現有環境變數，其中包含一或多個路徑) 或使用格式*路徑*:*模式*(其中*路徑*是您想要搜尋的目錄路徑)。 這些選擇性的格式不應使用 **/r**命令列選項。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   如果您未指定副檔名，PATHEXT 環境變數中所列的延伸模組預設會附加模式。
-   **其中**可以執行遞迴搜尋，顯示檔案的資訊，例如日期或大小，並接受環境變數來取代在本機電腦上的路徑。

## <a name="BKMK_examples"></a>範例

若要尋找名為 Test，目前的電腦和其子目錄的 C 磁碟機中的所有檔案，請輸入：
```
where /r c:\ test 
```
若要列出公用目錄中的所有檔案，請輸入：
```
where $public:*.*
```
若要尋找名為 「 記事本 」，遠端電腦的 C 磁碟機中的所有檔案，分別是 Computer1 和其子目錄中，輸入：
```
where /r \\computer1\c notepad.*
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)