---
title: mask
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1cb92caacb955449c1baaad411fdbe4cdf05b73
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839641"
---
# <a name="mask"></a>mask



移除使用匯**入**命令匯入的硬體陰影複製。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

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

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要移除匯入的陰影複製% Import_1%，請輸入：
```
mask %Import_1%
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)