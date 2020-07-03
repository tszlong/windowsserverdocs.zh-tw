---
title: 設定詳細資訊
description: 設定詳細資訊的參考文章，指定是否在建立陰影複製時提供詳細資訊輸出。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 93cb93c9-666f-4c74-814b-1c404a949935
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 309053339ca6ee354dda95860be7f77f3f885da2
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935917"
---
# <a name="set-verbose"></a>設定詳細資訊

指定是否在建立陰影複製時提供詳細資訊輸出。 如果使用時不含參數， **set verbose**會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
set verbose {on | off}
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|-----------|-------------|
|    {開啟    |    off}     |

## <a name="remarks"></a>備註

-   如果 verbose 模式是 on，則**set**會提供寫入器包含或排除的詳細資料，以及中繼資料壓縮與解壓縮的詳細資訊。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)