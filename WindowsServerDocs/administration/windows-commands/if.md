---
title: if
description: If 命令的參考文章，此命令會在 batch 程式中執行條件式處理。
ms.topic: reference
ms.assetid: 698b3fb9-532b-4c2b-af7f-179f8dc57131
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9bb3c29b7d77b6b1e07e647735701be3171cfb85
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89634572"
---
# <a name="if"></a>if

在 batch 程式中執行條件式處理。

## <a name="syntax"></a>語法

```
if [not] ERRORLEVEL <number> <command> [else <expression>]
if [not] <string1>==<string2> <command> [else <expression>]
if [not] exist <filename> <command> [else <expression>]
```

如果命令延伸模組已啟用，請使用下列語法：

```
if [/i] <string1> <compareop> <string2> <command> [else <expression>]
if cmdextversion <number> <command> [else <expression>]
if defined <variable> <command> [else <expression>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- |------------ |
| 否 | 指定只有當條件為 false 時，才執行命令。 |
| errorlevel `<number>` | 只有當先前的程式執行 Cmd.exe 傳回等於或大於 *數位*的結束代碼時，才指定 true 條件。 |
| `<command>` | 指定符合上述條件時應執行的命令。 |
| `<string1>==<string2>` | 只有當 *string1* 和 *string2* 相同時，才指定 true 條件。 這些值可以是常值字串或批次變數 (例如 `%1`) 。 您不需要用引號括住常值字串。 |
| 存在 `<filename>` | 如果指定的檔案名存在，則指定 true 條件。 |
| `<compareop>` | 指定三個字母的比較運算子，包括：<ul><li>**等於** -等於</li><li>**NEQ** -不等於</li><li>**LSS** -小於</li><li>**LEQ** -小於或等於</li><li>**GTR** -大於</li><li>**GEQ** -大於或等於</li></ul> |
| /i | 強制字串比較忽略大小寫。 您可以使用的形式來使用 **/i** `string1==string2` 。 **if** 這些比較是泛型的，在這種情況下，如果 *string1* 和 *string2* 都只包含數位，則會將字串轉換成數位，並執行數位比較。 |
| cmdextversion `<number>` | 只有當與 Cmd.exe 的命令延伸功能相關聯的內部版本號碼等於或大於指定的數目時，才會指定 true 條件。 第一個版本是1。 當將大幅增強功能新增至命令延伸模組時，它會遞增一。 當命令延伸模組停用時， **cmdextversion** 條件永遠不成立 (預設會啟用命令延伸模組) 。 |
| defined `<variable>` | 如果已定義 *變數* ，則指定 true 條件。 |
| `<expression>` | 指定要傳遞給 **else** 子句中命令的命令列命令和任何參數。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如果 **if** 子句中指定的條件為 true，則會執行遵循條件的命令。如果條件為 false，則會忽略 **if** 子句中的命令，而且命令會執行 **else** 子句中指定的任何命令。

- 當程式停止時，它會傳回結束代碼。 若要使用結束代碼作為條件，請使用 **errorlevel** 參數。

- 如果您使用 **已定義**的，則會將下列三個變數新增至環境： **% errorlevel%**、 **% cmdcmdline%** 和 **% cmdextversion%**。

  - **% errorlevel%**：展開為 errorlevel 環境變數目前值的字串表示。 此變數假設尚未有名稱為 ERRORLEVEL 的現有環境變數。 如果有，您會改為取得該 ERRORLEVEL 值。

  - **% cmdcmdline%**：展開至原始命令列，以 Cmd.exe 處理之前傳遞給 Cmd.exe。 這會假設尚未有名稱為 CMDCMDLINE 的現有環境變數。 如果有，您會改為取得該 CMDCMDLINE 值。

  - **% cmdextversion%**：展開為目前值 **cmdextversion**的字串表示。 這會假設尚未有名稱為 CMDEXTVERSION 的現有環境變數。 如果有，您會改為取得該 CMDEXTVERSION 值。

- 在**if**之後，您必須在與命令相同的行上使用**else**子句。

### <a name="examples"></a>範例

若 **找不到 File Product**，若要顯示訊息找不到資料檔案，請輸入：

```
if not exist product.dat echo Cannot find data file
```

若要格式化磁片磁碟機 A 中的磁片並在格式化程式期間發生錯誤時顯示錯誤訊息，請在批次檔中輸入下列幾行：

```
:begin
@echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program.
```

若要從目前目錄中刪除檔案 Product，或在找不到 Product 時顯示訊息，請在批次檔中輸入下列幾行：

```
IF EXIST Product.dat (
del Product.dat
) ELSE (
echo The Product.dat file is missing.
)
```

> [!NOTE]
> 這些線條可以合併成單一行，如下所示：
> ```
> IF EXIST Product.dat (del Product.dat) ELSE (echo The Product.dat file is missing.)
> ```

若要在執行批次檔之後回應 ERRORLEVEL 環境變數的值，請在批次檔中輸入下列幾行：

```
goto answer%errorlevel%
:answer1
echo The program returned error level 1
goto end
:answer0
echo The program returned error level 0
goto end
:end
echo Done!
```

若要在 ERRORLEVEL 環境變數的值小於或等於1時移至 [確定] 標籤，請輸入：

```
if %errorlevel% LEQ 1 goto okay
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [goto 命令](goto.md)
