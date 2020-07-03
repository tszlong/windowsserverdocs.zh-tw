---
title: Prompt
description: 提示命令的參考文章，可自訂您的 Cmd.exe 命令提示字元。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3d98e965-02eb-46ad-9d0a-5dc44830373e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 72ed82c316faddba9486649497c8c48f88e6da81
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931139"
---
# <a name="prompt"></a>Prompt

變更 Cmd.exe 命令提示字元，包括顯示任何您想要的文字，例如目前目錄的名稱、時間和日期，或 Microsoft Windows 版本號碼。 如果使用時不含參數，此命令會將命令提示字元重設為預設設定，這是目前的磁碟機號和目錄，後面接著大於符號（ **>** ）。

## <a name="syntax"></a>語法

```
prompt [<text>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| `<text>` | 指定您想要包含在命令提示字元中的文字和資訊。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 您可以包含（而不是）*文字*參數中一或多個字元字串的字元組合，而不是（或）：

    | 字元 | 描述 |
    |--|--|
    | $q | = （等號） |
    | $$ | $ （貨幣符號） |
    | $t | 目前時間 |
    | $d | 目前日期 |
    | $p | 目前的磁片磁碟機和路徑 |
    | $v | Windows 版本號碼 |
    | $n | 目前磁片磁碟機 |
    | $g | > （大於符號） |
    | $l | < （小於符號） |
    | $b | `|`（管線符號） |
    | $_ | 輸入-換行字元 |
    | $e | ANSI 轉義碼（代碼27） |
    | $h | 倒退鍵（用來刪除已寫入命令列的字元） |
    | $a | & （連字號） |
    | $c | （左括弧） |
    | $f | ）（右括弧） |
    | $s | Space |

- 啟用命令延伸模組時， **prompt**命令會支援下列格式化字元：

    | 字元 | 描述 |
    |--|--|
    | $+ | 零或多個加號（ **+** ）字元，視**pushd**目錄堆疊的深度而定（每個已推送層級一個字元）。 |
    | $m | 與目前的磁碟機號相關聯的遠端名稱，如果目前的磁片磁碟機不是網路磁碟機機，則為空字串。 |

- 如果您在文字參數中包含 **$p**字元，則在輸入每個命令（以判斷目前的磁片磁碟機和路徑）之後，就會讀取磁片。 這可能需要額外的時間，特別是針對磁片磁碟機。

### <a name="examples"></a>範例

若要使用第一行的目前時間和日期，以及下一行的大於號來設定兩行命令提示字元，請輸入：

```
prompt $d$s$s$t$_$g
```

此提示的變更如下，其中日期和時間是最新的：

```
Fri 06/01/2007  13:53:28.91
```

若要將命令提示字元設為顯示為箭號（ `-->` ），請輸入：

```
prompt --$g
```

若要手動將命令提示字元變更為預設設定（目前的磁片磁碟機和路徑後面接著大於符號），請輸入：

```
prompt $p$g
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
