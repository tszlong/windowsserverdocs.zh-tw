---
title: bitsadmin 快取與刪除
description: 適用于 bitsadmin 快取**和刪除**的 Windows 命令主題，會刪除特定的快取專案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0fd7f1db83a62dd9c1085d6afdcf509c1c3ac8cf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850941"
---
# <a name="bitsadmin-cache-and-delete"></a>bitsadmin 快取與刪除

刪除特定的快取專案。

## <a name="syntax"></a>語法

```
bitsadmin /cache /delete recordID
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| -------------- | -------------- |
| recordID | 與快取專案相關聯的 GUID。 |

## <a name="examples"></a><a name=BKMK_examples></a>典型

下列範例會刪除 RecordID 為 {6511FB02-E195-40A2-B595-E8E2F8F47702} 的快取專案。

```
C:\>bitsadmin /cache /delete {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)