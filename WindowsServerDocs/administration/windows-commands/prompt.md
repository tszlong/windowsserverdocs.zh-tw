---
title: Prompt
description: Prompt 命令的參考文章，可自訂您的 Cmd.exe 命令提示字元。
ms.topic: reference
ms.assetid: 3d98e965-02eb-46ad-9d0a-5dc44830373e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/11/2018
ms.openlocfilehash: 8cf91a2a2e78191f035545bc897eceae4c897457
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640326"
---
# <a name="prompt"></a>Prompt

變更 Cmd.exe 的命令提示字元，包括顯示您想要的任何文字，例如目前目錄的名稱、時間和日期，或 Microsoft Windows 版本號碼。 如果使用時不含參數，此命令會將命令提示重設為預設設定，也就是目前的磁碟機號和目錄，後面接著大於符號的 (**>**) 。

## <a name="syntax"></a>語法

```
prompt [<text>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<text>` | 指定要包含在命令提示字元中的文字和資訊。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 您可以包含的字元組合（而不是，或除了） *text* 參數中的一個或多個字元字串：

    | 字元 | 描述 |
    |--|--|
    | $q | = (等號)  |
    | $$ | $ (貨幣正負號)  |
    | $t | 目前時間 |
    | $d | 目前日期 |
    | $p | 目前的磁片磁碟機和路徑 |
    | $v | Windows 版本號碼 |
    | $n | 目前磁片磁碟機 |
    | $g | > (大於簽署)  |
    | $l | < (小於正負號)  |
    | $b | `|` (管道符號)  |
    | $_ | 輸入-換行 |
    | $e | ANSI 轉義碼 (代碼 27)  |
    | $h | 倒退鍵 (刪除已寫入命令列的字元)  |
    | $a | & (連字號)  |
    | $c |  ( (左括弧)  |
    | $f | )  (右括弧)  |
    | $s | Space |

- 啟用命令延伸模組時， **prompt** 命令支援下列格式化字元：

    | 字元 | 描述 |
    |--|--|
    | $+ | 零或多個加號 (**+**) 字元，視 **pushd** 目錄堆疊的深度而定， (推送) 的每個層級一個字元。 |
    | $m | 與目前磁碟機號相關聯的遠端名稱，如果目前磁片磁碟機不是網路磁碟機機，則為空字串。 |

- 如果您在 text 參數中包含 **$p** 字元，則在輸入每個命令 (之後，您的磁片會被讀取，以判斷目前的磁片磁碟機和路徑) 。 這可能需要額外的時間，特別是針對磁片磁碟機。

### <a name="examples"></a>範例

若要使用第一行的目前時間和日期來設定雙行命令提示字元，並在下一行輸入大於正負號的命令提示字元，請輸入：

```
prompt $d$s$s$t$_$g
```

提示會變更，如下所示，其中的日期和時間是最新的：

```
Fri 06/01/2007  13:53:28.91
```

若要將命令提示字元設為以箭號顯示 (`-->`) ，請輸入：

```
prompt --$g
```

若要手動將命令提示字元變更為預設設定 (目前的磁片磁碟機和路徑後面加上大於符號) ，請輸入：

```
prompt $p$g
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
