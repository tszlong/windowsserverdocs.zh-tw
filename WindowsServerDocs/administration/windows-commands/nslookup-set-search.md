---
title: nslookup set search
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e3f5bce42d3614b535b2dfb00c4c9ea9cac2346
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723559"
---
# <a name="nslookup-set-search"></a>nslookup set search



將 DNS 網域搜尋清單中的網域名稱系統（DNS）功能變數名稱附加至要求，直到收到回應為止。 這適用于集合和查閱要求至少包含一個期間，但結尾不是尾端句點的情況。

## <a name="syntax"></a>語法

```
set [no]search
```

### <a name="parameters"></a>參數

|  參數   |                                                                          描述                                                                          |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **nosearch** |                            停止將 DNS 網域搜尋清單中的網域名稱系統（DNS）功能變數名稱附加至要求。                            |
|  **search**  | 將 DNS 網域搜尋清單中的網域名稱系統（DNS）功能變數名稱附加至要求，直到收到回應為止。 預設語法為 [**搜尋**]。 |
|    {說明     |                                                                              ?}                                                                               |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)