---
title: 回應
description: Echo 命令的參考文章，會顯示訊息或開啟或關閉命令回應功能。
ms.topic: reference
ms.assetid: fb9fcd0f-5e73-4504-aa95-78204e1a79d3
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6adafeeca8284aa240a59db0eb6c64553203ca12
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636219"
---
# <a name="echo"></a>回應

顯示訊息，或開啟或關閉命令回顯功能。 如果使用時不含參數， **echo** 會顯示目前的 echo 設定。

## <a name="syntax"></a>語法

```
echo [<message>]
echo [on | off]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| [開啟 \| ] | 開啟或關閉命令回顯功能。 命令回顯預設為開啟。 |
| `<message>` | 指定要在螢幕上顯示的文字。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- `echo <message>`當**回應**關閉時，此命令特別有用。 若要顯示有數行的訊息，而不顯示任何命令，您可以 `echo <message>` 在 batch 程式的 [ **echo off** ] 命令之後包含數個命令。

- 關閉 **echo** 之後，命令提示字元視窗中不會出現命令提示字元。 若要顯示命令提示字元，請 **在上輸入 echo。**

- 如果用於批次檔中， **回應開啟** 和 **關閉** 時，不會影響命令提示字元的設定。

- 若要避免在批次檔中回應特定命令，請 `@` 在命令前面插入一個登入。 若要避免回應批次檔中的所有命令，請在檔案開頭包含 **echo off** 命令。

- 若要顯示管道 (`|`) 或重新導向字元 (`<` 或在 `>` 使用 **echo**時) ，請使用插入 `^` 號 () 緊接在管道或重新導向字元之前。 例如，、 `^|` `^>` 或 `^<`) 。 若要顯示插入號，請在連續 () 中輸入兩個游標 `^^` 。

### <a name="examples"></a>範例

若要顯示目前的 **echo** 設定，請輸入：

```
echo
```

若要回應畫面上的空白行，請輸入：

```
echo.
```

> [!NOTE]
> 請勿在句號之前加上空格。 否則，會顯示句號，而不是空白行。

若要在命令提示字元中防止回顯命令，請輸入：

```
echo off
```

> [!NOTE]
> 當 **echo** 關閉時，命令提示字元視窗中不會出現命令提示字元。 若要再次顯示命令提示字元，請 **在上輸入 echo**。

若要防止批次檔中的所有命令 (包括 [ **echo off** ] 命令) 無法在螢幕上顯示，請在批次處理檔案類型的第一行：

```
@echo off
```

您可以使用 **echo** 命令作為 **if** 語句的一部分。 例如，若要在目前的目錄中搜尋副檔名為 rpt 的檔案，並在找到這類檔案時回應訊息，請輸入：

```
if exist *.rpt echo The report has arrived.
```

下列批次檔會在目前的目錄中搜尋副檔名為 .txt 的檔案，並顯示一則訊息，指出搜尋結果：

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

如果在執行批次檔時找不到 .txt 檔案，則會顯示下列訊息：

```
This directory contains no text files.
```

當批次檔執行時，如果找到 .txt 檔案，則會顯示此範例 (的 File1.txt、File2.txt 和 File3.txt 檔案存在) ：

```
This directory contains the following text files:
File1.txt
File2.txt
File3.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
