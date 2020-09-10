---
title: ksetup delhosttorealmmap
description: '>ksetup delhosttorealmmap 命令的參考文章，此命令會移除所述主機和領域之間的服務主體名稱 (SPN) 對應。'
ms.topic: reference
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 321d468169005d5b183e3b3d4149872a731b8cfa
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639694"
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
