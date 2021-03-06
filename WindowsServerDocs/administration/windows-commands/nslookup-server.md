---
title: nslookup server
description: Nslookup server 命令的參考文章，此命令會將預設伺服器變更為指定的網域名稱系統 (DNS) 網域。
ms.topic: reference
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 64d1383dd5c4fa86a62fad91bae0ed5e4eb1454b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635623"
---
# <a name="nslookup-server"></a>nslookup server

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將預設伺服器變更為指定的網域名稱系統 (DNS) 網域。

此命令會使用目前的預設伺服器來查閱指定 DSN 網域的相關資訊。 如果您想要使用初始伺服器查閱資訊，請使用 [nslookup lserver](nslookup-lserver.md) 命令。

## <a name="syntax"></a>語法

```
server <DNSdomain>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<DNSdomain>` | 指定預設伺服器的 DNS 網域。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup lserver](nslookup-lserver.md)
