---
title: 遮罩
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 353e6080d1f6c548bc907b58655f31d0bce6de8b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858019"
---
# <a name="mask"></a>遮罩



移除已匯入所使用的硬體陰影複製**匯入**命令。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
mask <ShadowSetID>
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|ShadowSetID|移除陰影複製屬於指定的陰影複製設定識別碼。|

## <a name="remarks"></a>備註

-   您可以使用現有的別名或環境變數的位置*ShadowSetID*。 使用**新增**不含參數，若要查看現有的別名。

## <a name="BKMK_examples"></a>範例

若要移除已匯入的陰影複製 %import_1，請輸入：
```
mask %Import_1%
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)