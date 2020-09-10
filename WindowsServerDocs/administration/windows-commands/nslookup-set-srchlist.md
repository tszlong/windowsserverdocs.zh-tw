---
title: nslookup set srchlist
description: Nslookup set srchlist 命令的參考文章，此命令會變更預設網域名稱系統 (DNS) 功能變數名稱和搜尋清單。
ms.topic: reference
ms.assetid: 8486266d-22ac-4ce5-aad6-1cd0c08110a2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0d2798cd97b561ec5da7e515587978a4b19403c0
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640533"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更預設網域名稱系統 (DNS) 功能變數名稱和搜尋清單。 此命令會覆寫 [nslookup set domain](nslookup-set-domain.md) 命令的預設 DNS 功能變數名稱和搜尋清單。

## <a name="syntax"></a>語法

```
set srchlist=<domainname>[/...]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<domainname>` | 指定預設 DNS 網域和搜尋清單的新名稱。 預設的功能變數名稱值是以主機名稱為基礎。 您可以指定最多六個名稱，並以斜線分隔 (/) 。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 使用 [nslookup set all](nslookup-set-all.md) 命令以顯示清單。

### <a name="examples"></a>範例

若要將 DNS 網域設定為 *mfg.widgets.com* ，並將搜尋清單設定為三個名稱：

```
set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [nslookup set domain](nslookup-set-domain.md)

- [nslookup set all](nslookup-set-all.md)
