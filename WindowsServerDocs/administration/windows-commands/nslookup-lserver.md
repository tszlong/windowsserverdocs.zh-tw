---
title: nslookup lserver
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aee5ea0b-bb17-4c14-bde7-2f7a91f2f22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30c5ba8b7fef9b09d854aca998948f7891d99a02
ms.sourcegitcommit: ee8e0b217be6f6b2532ee7265fb4be00c106e124
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878124"
---
# <a name="nslookup-lserver"></a>nslookup lserver

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將預設伺服器變更為指定的網域名稱系統（DNS）網域。
## <a name="syntax"></a>語法
```
lserver <DNSDomain> 
```
## <a name="parameters"></a>參數

|    參數    |                      說明                      |
|-----------------|-------------------------------------------------------|
|   <DNSDomain>   | 指定預設伺服器的新 DNS 網域。  |
| {help &#124; ？} | 顯示**nslookup**子命令的簡短摘要。 |

## <a name="remarks"></a>備註
- **Lserver**命令會使用初始伺服器來查閱指定之 DNS 網域的相關資訊。 這與使用目前預設伺服器的**伺服器**命令相反。
  ## <a name="additional-references"></a>其他參考
  [命令列語法關鍵](command-line-syntax-key.md)
   [nslookup 伺服器](nslookup-server.md)
