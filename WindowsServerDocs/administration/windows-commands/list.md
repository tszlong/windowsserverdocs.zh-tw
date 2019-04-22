---
title: list
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: aacc93e1c7a16a7327ddbd17515f19cf41a5b458
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825539"
---
# <a name="list"></a>list



清單寫入器、 陰影複製或在系統上目前已註冊的陰影複製提供者。 如果未指定參數，使用**清單**在命令提示字元中顯示說明。

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
|寫入器|列出寫入器。 請參閱[List](list-writers.md)語法和參數。|
|陰影|列出持續性和現有的非持續性陰影複製。 請參閱[列出 shadows](list-shadows.md)語法和參數。|
|提供者|列出目前登錄的陰影複製提供者。 請參閱[列出提供者](list-providers.md)語法和參數。|

## <a name="BKMK_examples"></a>範例

若要列出所有的陰影複製，請輸入：
```
list shadows all
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)