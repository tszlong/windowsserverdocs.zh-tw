---
title: schtasks end
description: Schtasks end 命令的參考文章，此命令只會停止已排程工作啟動之程式的實例。
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 09/16/2020
ms.openlocfilehash: 7075c8689a39c7bc9eba917298678977916e29fa
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91390709"
---
# <a name="schtasks-end"></a>schtasks end

只停止排程工作所啟動之程式的實例。 若要停止其他處理常式，您必須使用 [TaskKill](taskkill.md) 命令。

## <a name="syntax"></a>語法

```
schtasks /end /tn <taskname> [/s <computer> [/u [<domain>\]<user> [/p <password>]]]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /tn `<taskname>` | 識別啟動程式的工作。 這是必要參數。 |
| /s `<computer>` | 指定遠端電腦的名稱或 IP 位址 (包含或不含反斜線) 。 預設是本機電腦。 |
| u `[<domain>]` | 使用指定之使用者帳戶的許可權來執行此命令。 根據預設，命令會以本機電腦目前使用者的許可權執行。 指定的使用者帳戶必須是遠端電腦上 Administrators 群組的成員。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
| /p `<password>` | 指定 **/u** 參數中指定之使用者帳戶的密碼。 如果您在沒有 **/p**參數或 password 引數的情況下使用 **/u**參數，則 schtasks.exe 會提示您輸入密碼。 只有當您使用 **/s**時， **/u**和 **/p**參數才有效。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要停止「 *我的記事本* 」工作啟動 Notepad.exe 的實例，請輸入：

```
schtasks /end /tn "My Notepad"
```

若要停止 *InternetOn* 工作在遠端電腦上啟動的 Internet Explorer 實例 *>woodgrove-svr01*，請輸入：

```
schtasks /end /tn InternetOn /s Svr01
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [schtasks 變更命令](schtasks-change.md)

- [schtasks create 命令](schtasks-create.md)

- [schtasks delete 命令](schtasks-delete.md)

- [schtasks 查詢命令](schtasks-query.md)

- [schtasks run 命令](schtasks-run.md)