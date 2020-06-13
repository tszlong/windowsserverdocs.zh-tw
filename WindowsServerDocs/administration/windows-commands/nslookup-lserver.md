---
title: nslookup lserver
description: Nslookup lserver 命令的參考主題，它會將初始伺服器變更為指定的網域名稱系統（DNS）網域。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 868142f251d62ebc3efd7913aded8e22aa077bd3
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721586"
---
# <a name="nslookup-lserver"></a>nslookup lserver

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將初始伺服器變更為指定的網域名稱系統（DNS）網域。

此命令會使用初始伺服器來查閱指定 DSN 網域的相關資訊。 如果您想要使用目前的預設伺服器查閱資訊，請使用[nslookup server](nslookup-server.md)命令。

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

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup server](nslookup-server.md)
