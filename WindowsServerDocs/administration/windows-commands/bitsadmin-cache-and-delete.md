---
title: bitsadmin cache and delete
description: Bitsadmin cache 和 delete 命令的參考文章，可刪除特定的快取專案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 739215722eac761aed45d6b4dba32b2b001450b3
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927041"
---
# <a name="bitsadmin-cache-and-delete"></a>bitsadmin cache and delete

刪除特定的快取專案。

## <a name="syntax"></a>語法

```
bitsadmin /cache /delete recordID
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| -------------- | -------------- |
| recordID | 與快取專案相關聯的 GUID。 |

## <a name="examples"></a>範例

若要刪除 RecordID 為 {6511FB02-E195-40A2-B595-E8E2F8F47702} 的快取專案：

```
bitsadmin /cache /delete {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin cache 命令](bitsadmin-cache.md)
