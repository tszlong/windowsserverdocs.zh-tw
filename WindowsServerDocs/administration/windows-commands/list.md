---
title: list
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 57b6c8d0-872e-4dba-9715-1db8ab892e98
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d60c42b868a1e9a26e3168e4b489573f9f87e179
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841101"
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

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|寫入者|列出寫入器。 如需語法和參數，請參閱[清單寫入](list-writers.md)器。|
|shadows|列出持續性和現有的非持續陰影複製。 如需語法和參數，請參閱[清單陰影](list-shadows.md)。|
|提供者|列出目前已註冊的陰影複製提供者。 請參閱[清單提供者](list-providers.md)以取得語法和參數。|

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要列出所有陰影複製，請輸入：
```
list shadows all
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)