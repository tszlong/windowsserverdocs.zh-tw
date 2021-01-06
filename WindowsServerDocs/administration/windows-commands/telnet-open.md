---
title: telnet open
description: Telnet open 命令的參考文章，此命令會連接到 telnet 伺服器。
ms.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3ce606395530fee08de7ceaf6fe2a0f7243b5b97
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946884"
---
# <a name="telnet-open"></a>telnet：開啟

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

連接到 telnet 伺服器。

## <a name="syntax"></a>語法

```
o[pen] <hostname> [<port>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<hostname>` | 指定電腦名稱稱或 IP 位址。 |
| `[<port>]` | 指定 telnet 伺服器正在接聽的 TCP 通訊埠。 預設值為 TCP 埠23。 |

## <a name="examples"></a>範例

若要在 *telnet.microsoft.com* 連線到 telnet 伺服器，請輸入：

```
o telnet.microsoft.com
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
