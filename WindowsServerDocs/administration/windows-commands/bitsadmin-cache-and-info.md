---
title: bitsadmin cache and info
description: Bitsadmin cache 和 info 命令的參考文章，會傾印特定的快取專案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dabf9b229138bf1d39863643574c5509ffcfcd91
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923269"
---
# <a name="bitsadmin-cache-and-info"></a>bitsadmin cache and info

傾印特定的快取專案。

## <a name="syntax"></a>語法

```
bitsadmin /cache /info recordID [/verbose]
```

### <a name="parameters"></a>參數

| Paramreter | Description |
| -------------- | -------------- |
| recordID | 與快取專案相關聯的 GUID。 |

## <a name="examples"></a>範例

若要傾印具有 {6511FB02-E195-40A2-B595-E8E2F8F47702} recordID 值的快取專案：

```
bitsadmin /cache /info {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin cache 命令](bitsadmin-cache.md)
