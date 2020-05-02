---
title: nslookup lserver
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2054c0fd427b41e7d6076258b29ab78d0fb7892
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723669"
---
# <a name="nslookup-lserver"></a>nslookup lserver

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將預設伺服器變更為指定的網域名稱系統（DNS）網域。
## <a name="syntax"></a>語法
```
lserver <DNSDomain> 
```
### <a name="parameters"></a>參數

|    參數    |                      描述                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | 指定預設伺服器的新 DNS 網域。  |
| {help &#124;？} | 顯示**nslookup**子命令的簡短摘要。 |

## <a name="remarks"></a>備註
- **Lserver**命令會使用初始伺服器來查閱指定之 DNS 網域的相關資訊。 這與使用目前預設伺服器的**伺服器**命令相反。
  ## <a name="additional-references"></a>其他參考
  - [命令列語法關鍵](command-line-syntax-key.md)
  [nslookup 伺服器](nslookup-server.md)
