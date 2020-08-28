---
title: bitsadmin cache and info
description: Bitsadmin cache 和 info 命令的參考文章，此命令會傾印特定的快取專案。
ms.topic: reference
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cce96d25b3968c1f975b6426ce8c25e7420f7bd3
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028886"
---
# <a name="bitsadmin-cache-and-info"></a>bitsadmin cache and info

傾印特定的快取專案。

## <a name="syntax"></a>語法

```
bitsadmin /cache /info recordID [/verbose]
```

### <a name="parameters"></a>參數

| Paramreter | 描述 |
| -------------- | -------------- |
| 記錄識別碼 | 與快取專案相關聯的 GUID。 |

## <a name="examples"></a>範例

若要傾印記錄識別碼值為 {6511FB02-E195-40A2-B595-E8E2F8F47702} 的快取專案：

```
bitsadmin /cache /info {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin cache 命令](bitsadmin-cache.md)
