---
title: nslookup set srchlist
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8486266d-22ac-4ce5-aad6-1cd0c08110a2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb93a9f7cf969161536e88bec929b7e6ba0f0e5d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372766"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更預設網域名稱系統（DNS）功能變數名稱和搜尋清單。

## <a name="syntax"></a>語法
```
Set srchlist=<DomainName>[/...]
```
## <a name="parameters"></a>參數

|    參數    |                                                                                        描述                                                                                        |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <DomainName>   | 指定預設 DNS 網域和搜尋清單的新名稱。 預設的功能變數名稱值是以主機名稱為基礎。 您最多可以指定六個名稱，並以斜線（/）分隔。 |
| {help &#124; ？} |                                                                   顯示**nslookup**子命令的簡短摘要。                                                                   |

## <a name="remarks"></a>備註
- **Set srchlist**命令會覆寫 [**設定網域**] 命令的預設 DNS 功能變數名稱和搜尋清單。 使用 [**全部設定**] 命令來顯示清單。
  ## <a name="BKMK_examples"></a>典型
  下列範例會將 DNS 網域設為 mfg.widgets.com，並將搜尋清單設定為三個名稱：
  ```
  set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
  ```
  ## <a name="additional-references"></a>其他參考
  [命令列語法索引鍵](command-line-syntax-key.md)
  [nslookup 設定網域](nslookup-set-domain.md)
  [nslookup 設定全部](nslookup-set-all.md)
