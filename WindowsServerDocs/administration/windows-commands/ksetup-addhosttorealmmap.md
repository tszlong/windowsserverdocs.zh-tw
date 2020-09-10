---
title: ksetup addhosttorealmmap
description: '>ksetup addhosttorealmmap 命令的參考文章，此命令會在指定的主機和領域之間新增服務主體名稱 (SPN) 對應。'
ms.topic: reference
ms.assetid: 237742d5-fa68-466c-b97e-636f489248ea
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 16ffe4431167ef63c73d4889febed49c40344e8b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639757"
---
# <a name="ksetup-addhosttorealmmap"></a>ksetup addhosttorealmmap

在所述主機和領域之間的 (SPN) 對應中新增服務主體名稱。 此命令也可讓您將共用相同 DNS 尾碼的主機或多部主機對應至領域。

對應會儲存在登錄中的 **HKEY_LOCAL_MACHINE \system\currentcontolset\lsa\kerberos\hosttorealm**下。

## <a name="syntax"></a>語法

```
ksetup /addhosttorealmmap <hostname> <realmname>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- |------------ |
| `<hostname>` | 主機名稱是電腦名稱稱，而且可以指定為電腦的完整功能變數名稱。 |
| `<realmname>` | 領域名稱會指定為大寫 DNS 名稱，例如 CORP。CONTOSO.COM。 |

### <a name="examples"></a>範例

若要將主機電腦 *IPops897* 對應至 *CONTOSO* 領域，請輸入：

```
ksetup /addhosttorealmmap IPops897 CONTOSO
```

檢查登錄以確定對應是否如預期執行。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [>ksetup 命令](ksetup.md)

- [>ksetup delhosttorealmmap 命令](ksetup-delhosttorealmmap.md)
