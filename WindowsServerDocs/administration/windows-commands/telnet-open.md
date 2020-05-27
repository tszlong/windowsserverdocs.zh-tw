---
title: telnet 開啟
description: Telnet 開啟的參考主題，會連線到 telnet 伺服器。
ms.prod: windows-server
ms.technology: manage-windows-commands
s.topic: article
ms.assetid: e30ad68c-2366-4754-ac36-311a2392902a vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed2c00b44143942ba1ccb77eec17ca975f23b498
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821318"
---
# <a name="telnet-open"></a>telnet：開啟

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

連接到 telnet 伺服器。

## <a name="syntax"></a>語法
```
o[pen] <hostname> [<Port>]
```
#### <a name="parameters"></a>參數

| 參數  |                                        說明                                         |
|------------|--------------------------------------------------------------------------------------------|
| <hostname> |                         指定電腦名稱稱或 IP 位址。                         |
|  [<Port>]  | 指定 telnet 伺服器正在接聽的 TCP 通訊埠。 預設值為 TCP 埠23。 |

## <a name="examples"></a>範例
連接到 telnet 伺服器，網址為 telnet.microsoft.com。
```
o telnet.microsoft.com
```
## <a name="additional-references"></a>其他參考
- [命令列語法關鍵](command-line-syntax-key.md)
