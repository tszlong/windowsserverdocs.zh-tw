---
title: bitsadmin cache and delete
description: Bitsadmin 快取和刪除命令的參考主題，其會刪除特定的快取專案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62c0c3d5b2cc188e8a8987c7ca502cdeaf932410
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718453"
---
# <a name="bitsadmin-cache-and-delete"></a>bitsadmin cache and delete

刪除特定的快取專案。

## <a name="syntax"></a>語法

```
bitsadmin /cache /delete recordID
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| recordID | 與快取專案相關聯的 GUID。 |

## <a name="examples"></a>範例

若要刪除 RecordID 為 {6511FB02-E195-40A2-B595-E8E2F8F47702} 的快取專案：

```
bitsadmin /cache /delete {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin cache 命令](bitsadmin-cache.md)
