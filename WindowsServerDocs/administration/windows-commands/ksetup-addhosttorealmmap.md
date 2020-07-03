---
title: ksetup addhosttorealmmap
description: Ksetup addhosttorealmmap 命令的參考文章，它會在指定的主機和領域之間新增服務主體名稱（SPN）對應。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 237742d5-fa68-466c-b97e-636f489248ea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 830db84e210b94088e74fd08909f7c47ff84df98
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925570"
---
# <a name="ksetup-addhosttorealmmap"></a>ksetup addhosttorealmmap

在指定的主機和領域之間新增服務主體名稱（SPN）對應。 此命令也可讓您將共用相同 DNS 尾碼的主機或多部主機對應到領域。

對應會儲存在登錄中的**HKEY_LOCAL_MACHINE \system\currentcontolset\lsa\kerberos\hosttorealm**之下。

## <a name="syntax"></a>語法

```
ksetup /addhosttorealmmap <hostname> <realmname>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- |------------ |
| `<hostname>` | 主機名稱是電腦名稱稱，可以指定為電腦的完整功能變數名稱。 |
| `<realmname>` | 領域名稱會指定為大寫 DNS 名稱，例如 CORP。CONTOSO.COM。 |

### <a name="examples"></a>範例

若要將主機電腦*IPops897*對應到*CONTOSO*領域，請輸入：

```
ksetup /addhosttorealmmap IPops897 CONTOSO
```

請檢查登錄，確定對應是否如預期發生。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)

- [ksetup delhosttorealmmap 命令](ksetup-delhosttorealmmap.md)
