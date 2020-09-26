---
title: schtasks 刪除
description: Schtasks delete 命令的參考文章，此命令會從排程中刪除排程工作。
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: a5b090ac9520aeae5e603e0c47c0c225b8bd0eff
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390717"
---
# <a name="schtasks-delete"></a>schtasks 刪除

從排程中刪除排程工作。 此命令不會刪除工作執行的程式，也不會中斷正在執行的程式。

## <a name="syntax"></a>語法

```
schtasks /delete /tn {<taskname> | *} [/f] [/s <computer> [/u [<domain>\]<user> [/p <password>]]]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /tn `{<taskname> | *}` | 識別要刪除的工作。 如果您使用 `*` ，此命令會刪除所有已排程的電腦工作，而不只是目前使用者排定的工作。 |
| /f | 抑制確認訊息。 這項工作會在沒有警告的情況下刪除。 |
| /s `<computer>` | 指定遠端電腦的名稱或 IP 位址 (包含或不含反斜線) 。 預設是本機電腦。 |
| u `[<domain>]` | 使用指定之使用者帳戶的許可權來執行此命令。 根據預設，命令會以本機電腦目前使用者的許可權執行。 指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
| /p `<password>` | 指定 **/u** 參數中指定之使用者帳戶的密碼。 如果您在沒有 **/p**參數或 password 引數的情況下使用 **/u**參數，則 schtasks.exe 會提示您輸入密碼。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

從遠端電腦的排程刪除 *開始郵件* 工作。

```
schtasks /delete /tn Start Mail /s Svr16
```

此命令會使用 **/s** 參數來識別遠端電腦。

刪除本機電腦排程中的所有工作，包括其他使用者排定的工作。

```
schtasks /delete /tn * /f
```

此命令會使用 **/tn &#42;** 參數來代表電腦上的所有工作，並使用 **/f** 參數來抑制確認訊息。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [schtasks 變更命令](schtasks-change.md)

- [schtasks create 命令](schtasks-create.md)

- [schtasks end 命令](schtasks-end.md)

- [schtasks 查詢命令](schtasks-query.md)

- [schtasks run 命令](schtasks-run.md)
