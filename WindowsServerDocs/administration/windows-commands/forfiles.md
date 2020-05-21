---
title: forfiles
description: Forfiles 命令的參考主題，它會在檔案或一組檔案上選取並執行命令。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 43f6b004-446d-4fdd-91c5-5653613524a4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/20/2020
ms.openlocfilehash: 96ef7d016bd13961a4814ba4cd09095aed4f0e97
ms.sourcegitcommit: 29f7a4811b4d36d60b8b7c55ce57d4ee7d52e263
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/20/2020
ms.locfileid: "83716843"
---
# <a name="forfiles"></a>forfiles

選取並執行檔案或一組檔案的命令。 此命令最常用於批次檔中。

## <a name="syntax"></a>語法

```
forfiles [/P pathname] [/M searchmask] [/S] [/C command] [/D [+ | -] [{<date> | <days>}]]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| /P`<pathname>` | 指定要從中開始搜尋的路徑。 根據預設，搜尋會從目前的工作目錄開始。 |
| /M`<searchmask>` | 根據指定的搜尋遮罩搜尋檔案。 預設 searchmask 為 `*` 。 |
| /S | 指示**forfiles**命令以遞迴方式搜尋子目錄。 |
| /C`<command>` | 在每個檔案上執行指定的命令。 命令字串應該以雙引號括住。 預設的命令是 `"cmd /c echo @file"` 。 |
| /D`[{+\|-}][{<date> | <days>}]` | 選取在指定的時間範圍內，具有上次修改日期的檔案：<ul><li>選取上次修改日期晚于或等於（ **+** ）或早于或等於（ **-** ）指定日期的檔案，其中*date*的格式為 MM/DD/YYYY。</li><li>選取上次修改日期晚于或等於（ **+** ）目前日期加上指定的天數，或早于或等於（ **-** ）目前日期減去指定天數的檔案。</li><li>*Days*的有效值包括0–32768範圍內的任何數位。 如果未指定正負號， **+** 則預設會使用。</li></ul> |
| /? | 在 cmd 視窗中顯示解說文字。 |

#### <a name="remarks"></a>備註

- `forfiles /S`命令類似 `dir /S` 。

- 您可以在命令字串中使用下列變數，如 **/C**命令列選項所指定：

    | 變數 | 描述 |
    | -------- | ----------- |
    | @FILE | 檔案名稱 |
    | @FNAME | 不含副檔名的檔案名。 |
    | @EXT | 檔案名副檔名。 |
    | @PATH | 檔案的完整路徑。 |
    | @RELPATH | 檔案的相對路徑。 |
    | @ISDIR | 如果檔案類型是目錄，則評估結果為 TRUE。 否則，此變數會評估為 FALSE。 |
    | @FSIZE | 檔案大小（以位元組為單位）。 |
    | @FDATE | 上次修改檔案的日期戳記。 |
    | @FTIME | 上次修改檔案的時間戳記。 |

- **Forfiles**命令可讓您在上執行命令，或將引數傳遞至多個檔案。 例如，您可以在具有 .txt 副檔名的樹狀目錄中的所有檔案上執行**type**命令。 或者，您可以在磁片磁碟機 C 上執行每個批次檔（* .bat），檔案名 Myinput.x .txt 作為第一個引數。

- 此命令可以：

    - 使用 **/d**參數選取 [檔案] [絕對日期] 或 [相對日期]。

    - 使用和之類的變數，建立檔案的封存樹狀 @FSIZE 目錄 @FDATE 。

    - 使用變數來區分目錄中的檔案 @ISDIR 。

    - 在命令列中包含特殊字元，方法是使用字元的十六進位代碼，格式為 0x*HH* （例如，索引標籤的0x09）。

- 此命令的運作方式是 `recurse subdirectories` 在設計用來處理單一檔案的工具上，執行旗標。

### <a name="examples"></a>範例

若要列出 C 磁片磁碟機上的所有批次檔，請輸入：

```
forfiles /P c:\ /S /M *.bat /C "cmd /c echo @file is a batch file"
```

若要列出 C 磁片磁碟機上的所有目錄，請輸入：

```
forfiles /P c:\ /S /M *.* /C "cmd /c if @isdir==TRUE echo @file is a directory"
```

若要列出目前目錄中至少一年以前的所有檔案，請輸入：

```
forfiles /S /M *.* /D -365 /C "cmd /c echo @file is at least one year old."
```

若要顯示目前目錄中，2007年1月1日以前的每個檔案的*文字檔已過期*，請輸入：

```
forfiles /S /M *.* /D -01/01/2007 /C "cmd /c echo @file is outdated."
```

若要以欄格式列出目前目錄中所有檔案的副檔名，並在擴充功能前面加上索引標籤，請輸入：

```
forfiles /S /M *.* /C "cmd /c echo The extension of @file is 0x09@ext"
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
