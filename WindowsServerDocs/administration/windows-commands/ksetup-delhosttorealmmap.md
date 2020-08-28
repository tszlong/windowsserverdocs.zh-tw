---
title: ksetup delhosttorealmmap
description: '>ksetup delhosttorealmmap 命令的參考文章，此命令會移除所述主機和領域之間的服務主體名稱 (SPN) 對應。'
ms.topic: reference
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4401bc186a2471fdd300279b42d4eb1375fc1aa0
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033986"
---
# <a name="ksetup-delhosttorealmmap"></a>ksetup delhosttorealmmap

在指定的主機與領域之間，移除 (SPN) 對應的服務主體名稱。 此命令也會移除主機與領域 (之間的任何對應，或將多部主機移至領域) 。

對應會儲存在登錄中的底下 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm` 。 執行此命令之後，建議您確定對應會出現在登錄中。

## <a name="syntax"></a>語法

```
ksetup /delhosttorealmmap <hostname> <realmname>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<hostname>` | 指定電腦的完整功能變數名稱。 |
| `<realmname>` | 指定大寫 DNS 名稱，例如 CORP。CONTOSO.COM。 |

### <a name="examples"></a>範例

若要變更領域 CONTOSO 的設定，並刪除主機電腦 IPops897 與領域的對應，請輸入：

```
ksetup /delhosttorealmmap IPops897 CONTOSO
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [>ksetup 命令](ksetup.md)

- [>ksetup addhosttorealmmap 命令](ksetup-addhosttorealmmap.md)
