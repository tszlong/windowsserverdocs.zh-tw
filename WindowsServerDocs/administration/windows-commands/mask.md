---
title: 遮罩
description: Mask 命令的參考文章，此命令會移除使用匯入命令匯入的硬體陰影複製。
ms.topic: reference
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c5d8f86de3019e47da3aff56aa370972de3288fa
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037866"
---
# <a name="mask"></a>遮罩

移除使用匯 **入** 命令匯入的硬體陰影複製。

## <a name="syntax"></a>語法

```
mask <shadowsetID>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| shadowsetID | 移除屬於指定之陰影複製集識別碼的陰影複製。 |

#### <a name="remarks"></a>備註

- 您可以使用現有的別名或環境變數來取代 *ShadowSetID*。 使用不含參數的 **add** 來查看現有的別名。

### <a name="examples"></a>範例

若要移除匯入的陰影複製 *% Import_1%*，請輸入：

```
mask %Import_1%
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)