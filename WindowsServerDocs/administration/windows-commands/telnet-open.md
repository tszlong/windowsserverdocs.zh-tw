---
title: telnet open
description: Telnet open 的參考文章，可連接到 telnet 伺服器。
s.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 892d797c4b56acb46e8119237fd38296e4ae411c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636785"
---
# <a name="telnet-open"></a>telnet：開啟

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

連接到 telnet 伺服器。

## <a name="syntax"></a>語法
```
o[pen] <hostname> [<Port>]
```
#### <a name="parameters"></a>參數

| 參數  |                                        描述                                         |
|------------|--------------------------------------------------------------------------------------------|
| <hostname> |                         指定電腦名稱稱或 IP 位址。                         |
|  [<Port>]  | 指定 telnet 伺服器正在接聽的 TCP 通訊埠。 預設值為 TCP 埠23。 |

## <a name="examples"></a>範例
在 telnet.microsoft.com 連線到 telnet 伺服器。
```
o telnet.microsoft.com
```
## <a name="additional-references"></a>其他參考資料
- [命令列語法關鍵](command-line-syntax-key.md)
