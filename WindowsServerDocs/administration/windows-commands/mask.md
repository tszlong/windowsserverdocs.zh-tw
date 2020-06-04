---
title: 遮罩
description: Mask 命令的參考主題，其會移除使用匯入命令匯入的硬體陰影複製。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bf301474-d74a-44e7-9fad-c8a11e7ca3bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ee01bb74b1fef1bb31a266c01a9e9bde7743691d
ms.sourcegitcommit: 5e313a004663adb54c90962cfdad9ae889246151
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2020
ms.locfileid: "84354628"
---
# <a name="mask"></a>遮罩

移除使用匯**入**命令匯入的硬體陰影複製。

## <a name="syntax"></a>語法

```
mask <shadowsetID>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| shadowsetID | 移除屬於指定陰影複製組識別碼的陰影複製。 |

#### <a name="remarks"></a>備註

- 您可以使用現有的別名或環境變數來取代*ShadowSetID*。 請使用不含參數的**add**來查看現有的別名。

### <a name="examples"></a>範例

若要移除匯入的陰影複製 *% Import_1%*，請輸入：

```
mask %Import_1%
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)