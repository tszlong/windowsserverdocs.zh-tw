---
title: echo
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fb9fcd0f-5e73-4504-aa95-78204e1a79d3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eb5c9650b95703f1316e6f5f179b910d22574f68
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222959"
---
# <a name="echo"></a>echo



顯示訊息，或開啟或關閉回應功能的命令。 如果未指定參數，使用**echo**會顯示目前的回應設定。

如需如何使用此命令的範例，請參閱[範例](#examples)。

## <a name="syntax"></a>語法

```
echo [<Message>]
echo [on | off]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[在\|關閉]|開啟或關閉回應功能的命令。 命令回應預設為開啟。|
|\<訊息 >|指定要在螢幕上顯示的文字。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   **Echo** *訊息*命令時特別有用**echo**已關閉。 若要顯示的訊息數行長而不會顯示任何命令，您可以包含數個**echo** *訊息*命令之後**關閉回應**命令，在您批次的程式。
-   當**echo**已關閉，不會顯示命令提示字元中，這是在命令提示字元視窗。 若要顯示命令提示字元中，輸入**上回應。**
-   如果在批次檔中使用**上回應**並**關閉回應**不會影響在命令提示字元設定。
-   若要避免回應特定的命令批次檔中，插入 at 符號 (@) 前面的命令。 若要避免回應的批次檔中的所有命令，包括**echo off**命令在檔案開頭。
-   若要顯示的管道 ( **|** ) 或重新導向字元 (**<** 或是**>**) 當您使用**echo**，使用管道或重新導向字元正前方的插入號 (^) (例如**^|**， **^>**，或 **^<** ). 若要顯示插入號，請連續輸入兩個插入號 ( **^^** )。

## <a name="examples"></a>範例

若要顯示目前**echo**設定中，輸入：
```
echo
```
若要回應在螢幕上的空白列，輸入：
```
echo.
```

> [!NOTE]
> 請勿句點的前面加上空格。 否則，期間將會顯示而不是空白的行。

若要避免回應命令在命令提示字元中，輸入：
```
echo off 
```

> [!NOTE]
> 當**echo**已關閉，不會顯示命令提示字元中，這是在命令提示字元視窗。 若要再次顯示命令提示字元中，輸入**回應上**。

若要避免的批次檔中的所有命令 (包括**關閉回應**命令) 無法顯示在畫面上，在第一行的批次檔案類型：
```
@echo off
```
您可以使用**echo**命令的一部分**如果**陳述式。 例如，若要搜尋目前目錄.rpt 副檔名，以及回應訊息的任何檔案，如果找到這類檔案，請輸入：
```
if exist *.rpt echo The report has arrived.
```
下列批次檔會搜尋目前目錄的檔案副檔名為.txt 的檔案名稱，並顯示訊息，指出搜尋的結果：
```
@echo off
if not exist *.txt (
echo This directory contains no text files.
) else (
   echo This directory contains the following text files:
   echo.
   dir /b *.txt
   )
```
如果不找到任何.txt 檔案的批次檔執行時，會顯示下列訊息：
```
This directory contains no text files.
```
如果.txt 檔案找不到批次檔執行時，會顯示下列輸出 （在此範例中，假設 File1.txt、 File2.txt 和 File3.txt 存在的檔案）：
```
This directory contains the following text files:
File1.txt
File2.txt
File3.txt
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)
