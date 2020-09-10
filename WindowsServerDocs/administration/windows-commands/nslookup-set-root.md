---
title: nslookup set root
description: Nslookup set root 命令的參考文章，此命令會變更用於查詢之根功能變數名稱伺服器的名稱。
ms.topic: reference
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d7e1251b7e13320a77a4c59736a6bd985bd248af
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637485"
---
# <a name="nslookup-set-root"></a>nslookup set root

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更用於查詢之根伺服器的名稱。

> [!NOTE]
> 此命令支援 [nslookup 根](nslookup-root.md) 命令。

## <a name="syntax"></a>語法

```
set root=<rootserver>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| ---------- | ---------- |
| `<rootserver>` | 指定根伺服器的新名稱。 預設值為 **ns.nic.ddn.mil**。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup root](nslookup-root.md)
