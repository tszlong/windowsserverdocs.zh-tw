---
title: nslookup server
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 608267f8-f7b4-412a-8dcd-e08b5ffc2085
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e24e55026d12a0d8afc5b6f1bef926ece9087bd0
ms.sourcegitcommit: 6423dfa9cecb3b06bdd563cae113c3e80a4ec330
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71105015"
---
# <a name="nslookup-server"></a>nslookup server

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將預設伺服器變更為指定的網域名稱系統（DNS）網域。
## <a name="syntax"></a>語法
```
server <DNSDomain>
```
## <a name="parameters"></a>參數

|    參數    |                          描述                           |
|-----------------|----------------------------------------------------------------|
|   <DNSDomain>   | 必要。 指定預設伺服器的新 DNS 網域。 |
| {help &#124; ？} |     顯示**nslookup**子命令的簡短摘要。      |

## <a name="remarks"></a>備註
- **伺服器**命令會使用目前的預設伺服器來查閱指定之 DNS 網域的相關資訊。 這與使用初始伺服器的**lserver**命令相反。
  ## <a name="additional-references"></a>其他參考
  [命令列語法索引鍵](command-line-syntax-key.md)
   [nslookup lserver](nslookup-lserver.md)
