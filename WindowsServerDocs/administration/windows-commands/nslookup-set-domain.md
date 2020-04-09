---
title: nslookup set domain
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fa433383e23fd19779960348e0af88e0a405ff83
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838461"
---
# <a name="nslookup-set-domain"></a>nslookup set domain

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將預設網域名稱系統（DNS）功能變數名稱變更為指定的名稱。
## <a name="syntax"></a>語法
```
set domain=<DomainName>
```
### <a name="parameters"></a>參數

|    參數    |                                           描述                                           |
|-----------------|-------------------------------------------------------------------------------------------------|
|  <DomainName>   | 指定預設 DNS 功能變數名稱的新名稱。 預設的功能變數名稱是主機名稱。 |
| {help &#124; ？} |                      顯示**nslookup**子命令的簡短摘要。                      |

## <a name="remarks"></a>備註
- 預設 DNS 功能變數名稱會附加至查閱要求，視**defname**和**搜尋**選項的狀態而定。 如果 DNS 網域搜尋清單的名稱中至少有兩個元件，則會包含預設 DNS 網域的父系。 例如，如果預設的 DNS 網域是 mfg.widgets.com，搜尋清單會同時命名為 mfg.widgets.com 和 widgets.com。 使用**set srchlist**命令來指定不同的清單，以及**設定 all**命令以顯示清單。
  ## <a name="additional-references"></a>其他參考資料
  - [命令列語法索引鍵](command-line-syntax-key.md)
  [nslookup set srchlist](nslookup-set-srchlist.md)
  [nslookup 全部設定](nslookup-set-all.md)
