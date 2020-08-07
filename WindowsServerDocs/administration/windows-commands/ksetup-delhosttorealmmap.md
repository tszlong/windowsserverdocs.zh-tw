---
title: ksetup delhosttorealmmap
description: Ksetup delhosttorealmmap 命令的參考文章，它會移除服務主體名稱 (SPN) 在所述主機與領域之間的對應。
ms.topic: article
ms.assetid: 3faee482-a96c-4614-86fd-aaa446643ec4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 953a8d33a65bb9c5aafd4d549f762772bc059ba4
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888004"
---
# <a name="ksetup-delhosttorealmmap"></a>ksetup delhosttorealmmap

移除服務主體名稱， (指定的主機與領域之間的 SPN) 對應。 此命令也會移除主機到領域 (或多部主機與領域) 之間的任何對應。

對應會儲存在登錄中的底下 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentContolSet\Lsa\Kerberos\HostToRealm` 。 執行此命令之後，建議您確定對應出現在登錄中。

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

若要變更領域 CONTOSO 的設定，以及刪除主機電腦 IPops897 與領域的對應，請輸入：

```
ksetup /delhosttorealmmap IPops897 CONTOSO
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ksetup 命令](ksetup.md)

- [ksetup addhosttorealmmap 命令](ksetup-addhosttorealmmap.md)
