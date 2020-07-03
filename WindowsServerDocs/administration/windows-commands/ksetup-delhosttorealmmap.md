---
title: ksetup delhosttorealmmap
description: Ksetup delhosttorealmmap 命令的參考文章，它會移除指定主機與領域之間的服務主體名稱（SPN）對應。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c55fe25a147c23026ddf97900d6da856f04314a3
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925490"
---
# <a name="ksetup-delhosttorealmmap"></a>ksetup delhosttorealmmap

移除所指定主機與領域之間的服務主體名稱（SPN）對應。 此命令也會移除主機與領域（或多部主機到領域）之間的任何對應。

對應會儲存在登錄中的底下 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm` 。 執行此命令之後，建議您確定對應出現在登錄中。

## <a name="syntax"></a>語法

```
ksetup /delhosttorealmmap <hostname> <realmname>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<hostname>` | 指定電腦的完整功能變數名稱。 |
| `<realmname>` | 指定大寫 DNS 名稱，例如 CORP。CONTOSO.COM。 |

### <a name="examples"></a>範例

若要變更領域 CONTOSO 的設定，以及刪除主機電腦 IPops897 與領域的對應，請輸入：

```
ksetup /delhosttorealmmap IPops897 CONTOSO
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)

- [ksetup addhosttorealmmap 命令](ksetup-addhosttorealmmap.md)
