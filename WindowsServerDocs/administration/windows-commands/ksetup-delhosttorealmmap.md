---
title: ksetup delhosttorealmmap
description: Ksetup delhosttorealmmap 命令的參考主題，它會移除所指定主機與領域之間的服務主體名稱（SPN）對應。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17fc30e76247c570c653d5ec38501a2199435c7f
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817858"
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

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)

- [ksetup addhosttorealmmap 命令](ksetup-addhosttorealmmap.md)
