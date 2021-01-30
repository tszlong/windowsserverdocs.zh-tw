---
title: forfiles
description: Forfiles 命令的參考文章，會在檔案或一組檔案上選取並執行命令。
ms.topic: reference
ms.assetid: 43f6b004-446d-4fdd-91c5-5653613524a4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 05/20/2020
ms.openlocfilehash: 29858d1f8dcbb3a6ba99dec0c520fd147115a0ec
ms.sourcegitcommit: 1e94c10ff51f43325fa9184b09bbdfeb8c8fed36
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2021
ms.locfileid: "99081666"
---
# <a name="forfiles"></a>forfiles

選取並執行某個檔案或一組檔案的命令。 此命令最常用於批次檔中。

## <a name="syntax"></a>語法

```
forfiles [/P pathname] [/M searchmask] [/S] [/C command] [/D [+ | -] [{<date> | <days>}]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /P `<pathname>` | 指定開始搜尋的路徑。 依預設，搜尋會在目前的工作目錄中開始。 |
| 一樣 `<searchmask>` | 根據指定的搜尋遮罩來搜尋檔案。 預設 searchmask 為 `*` 。 |
| /S | 指示 **forfiles** 命令以遞迴方式在子目錄中搜尋。 |
| /C `<command>` | 在每個檔案上執行指定的命令。 命令字串應以雙引號括住。 預設的命令為 `"cmd /c echo @file"` 。 |
| /D `[{+\|-}][{<date> | <days>}]` | 選取在指定時間範圍內最後修改日期的檔案：<ul><li>選取上次修改日期晚于或等於 (**+**) 或早于或等於 (**-**) 指定日期的檔案，其中 *日期* 的格式為 MM/DD/YYYY。</li><li>選取上次修改日期晚于或等於 (**+**) 目前的日期加上指定的天數，或早于或等於 (**-**) 目前的日期減去指定的天數。</li><li>有效的 *天數* 值包含0到32768範圍中的任何數位。 如果未指定正負號， **+** 預設會使用。</li></ul> |
| /? | 在 cmd 視窗中顯示解說文字。 |

#### <a name="remarks"></a>備註

- 此 `forfiles /S` 命令類似于 `dir /S` 。

- 您可以使用命令字串中的下列變數，如 **/C** 命令列選項所指定：

    | 變數 | 描述 |
    | -------- | ----------- |
    | @FILE | 檔案名稱 |
    | @FNAME | 不含副檔名的檔案名。 |
    | @EXT | 副檔名。 |
    | @PATH | 檔案的完整路徑。 |
    | @RELPATH | 檔案的相對路徑。 |
    | @ISDIR | 如果檔案類型是目錄，則評估結果為 TRUE。 否則，此變數會評估為 FALSE。 |
    | @FSIZE | 檔案大小（以位元組為單位）。 |
    | @FDATE | 檔案的上次修改日期戳記。 |
    | @FTIME | 上次修改檔案的時間戳記。 |

- **Forfiles** 命令可讓您在上執行命令，或將引數傳遞至多個檔案。 例如，您可以對副檔名為 .txt 的樹狀目錄中的所有檔案執行 **類型** 命令。 或者，您可以在磁片磁碟機 C 上執行每個批次檔 ( * .bat) ，其檔案名 Myinput.txt 為第一個引數。

- 此命令可以：

    - 使用 **/d** 參數，依絕對日期或相對日期來選取檔案。

    - 使用和之類的變數來建立檔案的封存樹狀 @FSIZE 目錄 @FDATE 。

    - 使用變數區分目錄中的檔案 @ISDIR 。

    - 使用字元的十六進位代碼，在命令列中包含特殊字元（使用 0x *HH* 格式） (例如，0x09 用於索引標籤) 。

- 此命令的運作方式是 `recurse subdirectories` 在設計用來處理單一檔案的工具上執行旗標。

### <a name="examples"></a>範例

若要列出磁片磁碟機 C 上的所有批次檔，請輸入：

```
forfiles /P c:\ /S /M *.bat /C "cmd /c echo @file is a batch file"
```

若要列出磁片磁碟機 C 上的所有目錄，請輸入：

```
forfiles /P c:\ /S /M * /C "cmd /c if @isdir==TRUE echo @file is a directory"
```

若要列出目前目錄中至少有一年的所有檔案，請輸入：

```
forfiles /S /M *.* /D -365 /C "cmd /c echo @file is at least one year old."
```

若要顯示目前目錄中的每個檔案在2007年1月1日之前的每個檔案 *都已過期的文字檔，* 請輸入：

```
forfiles /S /M *.* /D -01/01/2007 /C "cmd /c echo @file is outdated."
```

若要以資料行格式列出目前目錄中所有檔案的副檔名，並在擴充功能前面加上一個索引標籤，請輸入：

```
forfiles /S /M *.* /C "cmd /c echo The extension of @file is 0x09@ext"
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
