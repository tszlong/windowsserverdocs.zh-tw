---
title: nslookup set srchlist
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 39b28e7d43df2427caae46d323cd30f03b6b484c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436570"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

變更預設網域名稱系統 (DNS) 網域名稱和搜尋清單。

## <a name="syntax"></a>語法
```
Set srchlist=<DomainName>[/...]
```
## <a name="parameters"></a>參數

|    參數    |                                                                                        描述                                                                                        |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <DomainName>   | 指定新名稱的預設 DNS 網域和搜尋清單。 預設網域名稱值根據主機名稱。 您可以指定最多六個以斜線 （/） 分隔的名稱。 |
| {help &#124; ?} |                                                                   顯示的簡短摘要**nslookup**子命令。                                                                   |

## <a name="remarks"></a>備註
- **設定 srchlist**命令會覆寫的預設 DNS 網域名稱和搜尋清單**組網域**命令。 使用**全部設定**命令，以顯示清單。
  ## <a name="BKMK_examples"></a>範例
  下列範例會將 DNS 網域設定為 mfg.widgets.com 和三個名稱的 [搜尋] 清單：
  ```
  set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
  ```
  ## <a name="additional-references"></a>其他參考資料
  [命令列語法重點](command-line-syntax-key.md)
  [nslookup 設定網域](nslookup-set-domain.md)
  [nslookup 將所有設定](nslookup-set-all.md)
