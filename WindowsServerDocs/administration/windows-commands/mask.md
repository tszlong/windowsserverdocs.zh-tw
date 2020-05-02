---
title: 遮罩
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 816bcd932091b33ed897add5a13603e3a1eea925
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724012"
---
# <a name="mask"></a>遮罩



移除使用匯**入**命令匯入的硬體陰影複製。



## <a name="syntax"></a>語法

```
mask <ShadowSetID>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|ShadowSetID|移除屬於指定陰影複製組識別碼的陰影複製。|

## <a name="remarks"></a>備註

-   您可以使用現有的別名或環境變數來取代*ShadowSetID*。 請使用不含參數的**add**來查看現有的別名。

## <a name="examples"></a>範例

若要移除匯入的陰影複製% Import_1%，請輸入：
```
mask %Import_1%
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)