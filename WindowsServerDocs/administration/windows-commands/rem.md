---
title: rem
description: Rem 命令的參考文章，會在腳本、批次或 config.sys 檔案中記錄批註。
ms.topic: reference
ms.assetid: 1a45b585-a83c-4ff6-badd-ff40f34cec23
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c56595a45eba3fd841f1f455c189164b240191e8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640645"
---
# <a name="rem"></a>rem

記錄腳本、批次或 config.sys 檔案中的批註。 如果未指定批註，則 **rem** 會新增垂直間距。

## <a name="syntax"></a>語法

```
rem [<comment>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<comment>` | 指定要包含為批註的字元字串。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Rem**命令不會在螢幕上顯示批註。 若要在螢幕上顯示批註，您必須在檔案中包含 **echo on** 命令。

- 您無法 `<` 在批次檔批註中使用重新導向字元 (或 `>`) 或管道 (`|`) 。

- 雖然您可以在沒有批註的情況下使用 **rem** 來將垂直間距新增至批次檔，您也可以使用空白行。 處理批次程式時，會忽略空白行。

### <a name="examples"></a>範例

若要透過批次檔批註新增垂直間距，請輸入：

```
@echo off
rem  This batch program formats and checks new disks.
rem  It is named Checknew.bat.
rem
rem echo Insert new disk in Drive B.
pause
format b: /v chkdsk b:
```

若要在 config.sys 檔案中的 **prompt** 命令之前包含說明批註，請輸入：

```
rem Set prompt to indicate current directory
prompt $p$g
```

若要提供腳本的相關批註，請輸入：

```
rem The commands in this script set up 3 drives.
rem The first drive is a primary partition and is
rem assigned the letter D. The second and third drives
rem are logical partitions, and are assigned letters
rem E and F.
create partition primary size=2048
assign d:
create partition extended
create partition logical size=2048
assign e:
create partition logical
assign f:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)