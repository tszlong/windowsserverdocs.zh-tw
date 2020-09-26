---
title: schtasks 查詢
description: 適用于 schtasks.exe 查詢命令的參考文章，其中列出排程要在電腦上執行的所有工作。
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: c1bff666f762b21e8dbcf2bff59383d49cab6409
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390708"
---
# <a name="schtasks-query"></a>schtasks 查詢

列出排程要在電腦上執行的所有工作。

語法

```
schtasks [/query] [/fo {TABLE | LIST | CSV}] [/nh] [/v] [/s <computer> [/u [<domain>\]<user> [/p <password>]]]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /query | （選擇性）指定作業的名稱。 使用此查詢而不使用參數時，會執行查詢。 |
| /fo `<format>` | 指定輸出格式。 有效的值為 *資料表*、 *清單*或 *CSV*。 |
| /nh | 移除資料表顯示中的資料行標題。 此參數適用于 *資料表* 或 *CSV* 輸出格式。 |
| /v | 將工作的 advanced 屬性加入至顯示器。 此參數適用于 *清單* 或 *CSV* 輸出格式。 |
| /s `<computer>` | 指定遠端電腦的名稱或 IP 位址 (包含或不含反斜線) 。 預設是本機電腦。 |
| u `[<domain>]` | 使用指定之使用者帳戶的許可權來執行此命令。 根據預設，命令會以本機電腦目前使用者的許可權執行。 指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
| /p `<password>` | 指定 **/u** 參數中指定之使用者帳戶的密碼。 如果您在沒有 **/p**參數或 password 引數的情況下使用 **/u**參數，則 schtasks.exe 會提示您輸入密碼。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要列出所有針對本機電腦排程的工作，請輸入：

```
schtasks
schtasks /query
```

這些命令會產生相同的結果，而且可以交換使用。

若要要求在本機電腦上執行工作的詳細顯示，請輸入：

```
schtasks /query /fo LIST /v
```

此命令會使用 **/v** 參數來要求詳細的 (詳細資訊) 顯示，並使用 **/fo list** 參數將顯示格式化為清單，方便您閱讀。 您可以使用此命令來確認您所建立的工作具有預期的迴圈模式。

若要要求針對遠端電腦排程的工作清單，並將工作新增至本機電腦上的逗點分隔記錄檔，請輸入：

```
schtasks /query /s Reskit16 /fo csv /nh >> \\svr01\data\tasklogs\p0102.csv
```

您可以使用此命令格式來收集和追蹤針對多部電腦排程的工作。 此命令會使用 **/s** 參數來識別遠端電腦 *Reskit16*、指定格式的 **/fo** 參數，並使用 **/nh** 參數來隱藏資料行標題。 **>>** 附加符號會將輸出重新導向至本機電腦 *>woodgrove-svr01*上的工作記錄檔*p0102.csv*。 由於命令會在遠端電腦上執行，因此本機電腦路徑必須是完整的。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [schtasks 變更命令](schtasks-change.md)

- [schtasks create 命令](schtasks-create.md)

- [schtasks delete 命令](schtasks-delete.md)

- [schtasks end 命令](schtasks-end.md)

- [schtasks run 命令](schtasks-run.md)