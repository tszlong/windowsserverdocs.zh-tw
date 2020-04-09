---
title: echo
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fb9fcd0f-5e73-4504-aa95-78204e1a79d3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e6e9c3c79cc8006efba0c97a574e3d6d94a6f7e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845241"
---
# <a name="echo"></a>echo



顯示訊息，或開啟或關閉命令回顯功能。 如果使用時不含參數， **echo**會顯示目前的回應設定。

如需如何使用此命令的範例，請參閱[範例](#examples)。

## <a name="syntax"></a>語法

```
echo [<Message>]
echo [on | off]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|[on \| off]|開啟或關閉命令回顯功能。 命令回顯預設為開啟。|
|\<訊息 >|指定要在螢幕上顯示的文字。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   當**echo**關閉時， **echo** *Message*命令特別有用。 若要顯示長達幾行的訊息，但不顯示任何命令，您可以在 batch 程式中的 [**回應關閉**] 命令之後包含數個**回應***消息*命令。
-   當**echo**關閉時，命令提示字元就不會出現在 [命令提示字元] 視窗中。 若要顯示命令提示字元，請**在上輸入 echo。**
-   如果在批次檔中使用， **echo on**和**echo**不會影響命令提示字元的設定。
-   若要避免回應批次檔中的特定命令，請在命令前面插入 @ 符號。 若要避免回應批次檔中的所有命令，請在檔案的開頭包含**echo off**命令。
-   當您使用**echo**時，若要顯示管道（ **|** ）或重新導向字元（ **<** 或 **>** ），請在管道或重新導向字元之前使用插入號（^）（例如， **^|** 、 **^>** 或 **^<** ）。 若要顯示插入號，請連續輸入兩個插入號（ **^^** ）。

## <a name="examples"></a>範例

若要顯示目前的**回應**設定，請輸入：

```
echo
```

若要在螢幕上回應空白行，請輸入：

```
echo.
```

> [!NOTE]
> 不要在句號前面加上空格。 否則，將會顯示期間，而不是空白行。

若要在命令提示字元中避免回顯命令，請輸入：

```
echo off 
```

> [!NOTE]
> 當**echo**關閉時，命令提示字元就不會出現在 [命令提示字元] 視窗中。 若要再次顯示命令提示字元，請**在上輸入 echo**。

若要防止批次檔中的所有命令（包括 [**回應關閉**] 命令）在畫面上顯示，請在批次處理檔案類型的第一行：

```
@echo off
```

您可以使用**echo**命令作為**if**語句的一部分。 例如，若要在目前的目錄中搜尋副檔名為 rpt 的檔案，並在找到這類檔案時回應訊息，請輸入：

```
if exist *.rpt echo The report has arrived.
```

下列批次檔會在目前的目錄中搜尋副檔名為 .txt 的檔案，並顯示指出搜尋結果的訊息：

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

如果執行批次檔時找不到 .txt 檔案，則會顯示下列訊息：

```
This directory contains no text files.
```

當批次檔執行時，如果找到 .txt 檔案，就會顯示下列輸出（在此範例中，假設檔案 File1 .txt、File2 .txt 和 File3 存在）：

```
This directory contains the following text files:
File1.txt
File2.txt
File3.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
