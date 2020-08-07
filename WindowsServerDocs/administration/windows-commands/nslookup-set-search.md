---
title: nslookup set search
description: Nslookup 設定搜尋命令的參考文章，會將網域名稱系統 (dns 網域搜尋清單中的 DNS) 功能變數名稱加入至要求，直到收到回應為止。
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91592daf702741fafcb88de792836f5183868101
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87885545"
---
# <a name="nslookup-set-search"></a>nslookup set search

將 DNS 網域搜尋清單中的網域名稱系統 (DNS) 功能變數名稱，附加至要求，直到收到回應為止。 這適用于集合和查閱要求至少包含一個期間，但結尾不是尾端句點的情況。

## <a name="syntax"></a>語法

```
set [no]search
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| nosearch | 停止在要求的 DNS 網域搜尋清單中附加網域名稱系統 (DNS) 功能變數名稱。 |
| 搜尋 | 在要求的 DNS 網域搜尋清單中，附加網域名稱系統 (DNS) 功能變數名稱，直到收到解答為止。 這是預設值。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
