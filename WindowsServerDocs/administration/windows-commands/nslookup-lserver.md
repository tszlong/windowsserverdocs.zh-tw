---
title: nslookup lserver
description: Nslookup lserver 命令的參考文章，此命令會將初始伺服器變更為指定的網域名稱系統 (DNS) 網域。
ms.topic: reference
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d1f507b1a61e9fd4fc506fb4e74f869c83e95335
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635674"
---
# <a name="nslookup-lserver"></a>nslookup lserver

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將初始伺服器變更為指定的網域名稱系統 (DNS) 網域。

此命令會使用初始伺服器來查閱指定 DSN 網域的相關資訊。 如果您想要使用目前的預設伺服器查閱資訊，請使用 [nslookup server](nslookup-server.md) 命令。

## <a name="syntax"></a>語法

```
lserver <DNSdomain>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<DNSdomain>` | 指定初始伺服器的 DNS 網域。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup server](nslookup-server.md)
