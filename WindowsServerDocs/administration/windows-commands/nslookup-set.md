---
title: nslookup set
description: Nslookup set 命令的參考文章，會變更影響查閱行為的設定設定。
ms.topic: reference
ms.assetid: 1fe5b36d-e93e-468b-abca-43b0204b32d1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 70850cd40f7eb54212b112a6e1410a25628a3d7f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636470"
---
# <a name="nslookup-set"></a>nslookup set

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更影響查閱功能的設定。

## <a name="syntax"></a>語法

```
set all [class | d2 | debug | domain | port | querytype | recurse | retry | root | search | srchlist | timeout | type | vc] [options]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| [nslookup set all](nslookup-set-all.md) | 列出所有目前的設定。 |
| [nslookup set class](nslookup-set-class.md) | 變更查詢類別，指定資訊的通訊協定群組。 |
| [nslookup set d2](nslookup-set-d2.md) | 開啟或關閉 [詳細資訊] 偵錯模式。 |
| [nslookup set debug](nslookup-set-debug.md) | 完全關閉偵錯模式。 |
| [nslookup set domain](nslookup-set-domain.md) | 將預設網域名稱系統 (DNS) 功能變數名稱變更為指定的名稱。 |
| [nslookup set port](nslookup-set-port.md) | 將預設的 TCP/UDP 網域名稱系統 (DNS) 名稱伺服器埠變更為指定的值。
| [nslookup set querytype](nslookup-set-querytype.md) | 變更查詢的資源記錄類型。 |
| [nslookup set recurse](nslookup-set-recurse.md) | 告知網域名稱系統 (DNS) 名稱伺服器，以查詢其他伺服器（如果找不到任何資訊）。 |
| [nslookup set retry](nslookup-set-retry.md) | 設定重試次數。 |
| [nslookup set root](nslookup-set-root.md) | 變更用於查詢之根伺服器的名稱。 |
| [nslookup set search](nslookup-set-search.md) | 將 DNS 網域搜尋清單中的網域名稱系統 (DNS) 功能變數名稱加入要求，直到收到解答為止。 |
| [nslookup set srchlist](nslookup-set-srchlist.md) | 變更預設網域名稱系統 (DNS) 功能變數名稱和搜尋清單。 |
| [nslookup set timeout](nslookup-set-timeout.md) | 變更等候查閱要求回復的初始秒數。 |
| [nslookup set type](nslookup-set-type.md) | 變更查詢的資源記錄類型。 |
| [nslookup set vc](nslookup-set-vc.md) | 指定將要求傳送到伺服器時，是否要使用虛擬電路。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
