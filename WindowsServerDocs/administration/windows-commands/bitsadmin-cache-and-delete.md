---
title: bitsadmin 快取與刪除
description: Bitsadmin 快取**和刪除**的 Windows 命令主題-刪除特定的快取專案。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87c3ffd7e0c9c43e8e2eb6e5d5a1d98610a4d9ad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382069"
---
# <a name="bitsadmin-cache-and-delete"></a>bitsadmin 快取與刪除



刪除特定的快取專案。

## <a name="syntax"></a>語法

```
bitsadmin /Cache /Delete RecordID 
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|RecordID|與快取專案相關聯的 GUID。|

## <a name="BKMK_examples"></a>典型

下列範例會刪除 RecordID 為 {6511FB02-E195-40A2-B595-E8E2F8F47702} 的快取專案。
```
C:\>bitsadmin /Cache /Delete {6511FB02-E195-40A2-B595-E8E2F8F47702} 
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)