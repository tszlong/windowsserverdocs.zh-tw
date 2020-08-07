---
title: nslookup set root
description: Nslookup set root 命令的參考文章，它會變更用於查詢的根功能變數名稱伺服器名稱。
ms.topic: article
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 69dd99b00e186a4338ec5c176d8fed128325b89e
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885525"
---
# <a name="nslookup-set-root"></a>nslookup set root

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更用於查詢的根功能變數名稱稱。

> [!NOTE]
> 此命令支援[nslookup root](nslookup-root.md)命令。

## <a name="syntax"></a>語法

```
set root=<rootserver>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ---------- |
| `<rootserver>` | 指定根功能變數名稱伺服器的新名稱。 預設值為**ns.nic.ddn.mil**。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup root](nslookup-root.md)
