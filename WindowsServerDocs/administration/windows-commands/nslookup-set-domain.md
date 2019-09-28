---
title: nslookup set domain
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f140a371a6374baa7921ca823df469156593423c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372933"
---
# <a name="nslookup-set-domain"></a>nslookup set domain

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將預設網域名稱系統（DNS）功能變數名稱變更為指定的名稱。
## <a name="syntax"></a>語法
```
set domain=<DomainName>
```
## <a name="parameters"></a>參數

|    參數    |                                           描述                                           |
|-----------------|-------------------------------------------------------------------------------------------------|
|  <DomainName>   | 指定預設 DNS 功能變數名稱的新名稱。 預設的功能變數名稱是主機名稱。 |
| {help &#124; ？} |                      顯示**nslookup**子命令的簡短摘要。                      |

## <a name="remarks"></a>備註
- 預設 DNS 功能變數名稱會附加至查閱要求，視**defname**和**搜尋**選項的狀態而定。 如果 DNS 網域搜尋清單的名稱中至少有兩個元件，則會包含預設 DNS 網域的父系。 例如，如果預設的 DNS 網域是 mfg.widgets.com，搜尋清單會同時命名為 mfg.widgets.com 和 widgets.com。 使用**set srchlist**命令來指定不同的清單，以及**設定 all**命令以顯示清單。
  ## <a name="additional-references"></a>其他參考
  [命令列語法索引鍵](command-line-syntax-key.md)
  [nslookup set srchlist](nslookup-set-srchlist.md)
  [nslookup set all](nslookup-set-all.md)
