---
title: 遮罩
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f0dc83d7d9f7204f56e95c62b7cfad991f539ef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373708"
---
# <a name="mask"></a>遮罩



移除使用匯**入**命令匯入的硬體陰影複製。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
mask <ShadowSetID>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|ShadowSetID|移除屬於指定陰影複製組識別碼的陰影複製。|

## <a name="remarks"></a>備註

-   您可以使用現有的別名或環境變數來取代*ShadowSetID*。 請使用不含參數的**add**來查看現有的別名。

## <a name="BKMK_examples"></a>典型

若要移除匯入的陰影複製% Import_1%，請輸入：
```
mask %Import_1%
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)