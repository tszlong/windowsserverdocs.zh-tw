---
title: list
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57b6c8d0-872e-4dba-9715-1db8ab892e98
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91b42925fc822b10157bb488167d06fe82cfe1e3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374696"
---
# <a name="list"></a>list



列出系統上的寫入器、陰影複製或目前已註冊的陰影複製提供者。 如果使用時不含參數， **list**會在命令提示字元中顯示說明。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
list writers [metadata | detailed | status]
list shadows {all | set <SetID> | id <ShadowID>}
list providers
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|寫入器|列出寫入器。 如需語法和參數，請參閱[清單寫入](list-writers.md)器。|
|Shadows|列出持續性和現有的非持續陰影複製。 如需語法和參數，請參閱[清單陰影](list-shadows.md)。|
|都會|列出目前已註冊的陰影複製提供者。 請參閱[清單提供者](list-providers.md)以取得語法和參數。|

## <a name="BKMK_examples"></a>典型

若要列出所有陰影複製，請輸入：
```
list shadows all
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)