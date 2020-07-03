---
title: nslookup server
description: Nslookup server 命令的參考文章，它會將預設伺服器變更為指定的網域名稱系統（DNS）網域。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ec66534d475502ee68f9fabb58b214d25e6e0aaf
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931259"
---
# <a name="nslookup-server"></a>nslookup server

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將預設伺服器變更為指定的網域名稱系統（DNS）網域。

此命令會使用目前的預設伺服器來查閱指定 DSN 網域的相關資訊。 如果您想要使用初始伺服器查閱資訊，請使用[nslookup lserver](nslookup-lserver.md)命令。

## <a name="syntax"></a>語法

```
server <DNSdomain>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<DNSdomain>` | 指定預設伺服器的 DNS 網域。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup lserver](nslookup-lserver.md)
