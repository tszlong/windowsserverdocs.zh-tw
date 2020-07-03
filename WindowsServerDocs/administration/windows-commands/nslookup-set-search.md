---
title: nslookup set search
description: Nslookup 設定搜尋命令的參考文章，其會將 DNS 網域搜尋清單中的網域名稱系統（DNS）功能變數名稱附加至要求，直到收到回應為止。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b1740dd9bb3eb35c4cd1ef4890fcb977b2dc1ff
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935500"
---
# <a name="nslookup-set-search"></a>nslookup set search

將 DNS 網域搜尋清單中的網域名稱系統（DNS）功能變數名稱附加至要求，直到收到回應為止。 這適用于集合和查閱要求至少包含一個期間，但結尾不是尾端句點的情況。

## <a name="syntax"></a>語法

```
set [no]search
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| nosearch | 停止在要求的 DNS 網域搜尋清單中附加網域名稱系統（DNS）功能變數名稱。 |
| 搜尋 | 在收到回應之前，在要求的 DNS 網域搜尋清單中附加網域名稱系統（DNS）功能變數名稱。 這是預設值。 |
| /? | 在命令提示字元顯示說明。 |
| /help | 在命令提示字元顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
