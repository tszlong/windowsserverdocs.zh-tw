---
title: nslookup set domain
description: Nslookup set domain 命令的參考文章，此命令會將預設網域名稱系統 (DNS) 功能變數名稱變更為指定的名稱。
ms.topic: reference
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed002a7a6278d9bcd11a59c5708c723d7ffe91ef
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023442"
---
# <a name="nslookup-set-domain"></a>nslookup set domain

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將預設網域名稱系統 (DNS) 功能變數名稱變更為指定的名稱。

## <a name="syntax"></a>語法

```
set domain=<domainname>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<domainname>` | 指定預設 DNS 功能變數名稱的新名稱。 預設值是主機的名稱。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 根據 **defname** 和 **搜尋** 選項的狀態，會將預設的 DNS 功能變數名稱附加至查閱要求。

- 如果 DNS 網域搜尋清單的名稱中至少有兩個元件，則會包含預設 DNS 網域的父代。 例如，如果預設的 DNS 網域是 mfg.widgets.com，則搜尋清單會命名為 mfg.widgets.com 和 widgets.com。

- 使用 [nslookup set srchlist](nslookup-set-srchlist.md) 命令可指定不同的清單，以及 [nslookup set all](nslookup-set-all.md) 命令以顯示清單。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup set srchlist](nslookup-set-srchlist.md)

- [nslookup set all](nslookup-set-all.md)
