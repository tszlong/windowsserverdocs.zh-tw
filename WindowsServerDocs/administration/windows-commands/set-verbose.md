---
title: set verbose
description: Set verbose 命令的參考文章，這個命令會指定是否在建立陰影複製期間提供詳細資訊輸出。
ms.topic: reference
ms.assetid: 93cb93c9-666f-4c74-814b-1c404a949935
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2e0b4d4cca66b20125c1aa0bbb67a7806133f6d8
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389040"
---
# <a name="set-verbose"></a>設定詳細資訊

指定是否在建立陰影複製期間提供詳細資訊輸出。 如果使用時不含參數，請在命令提示字元中， **設定詳細** 資訊顯示說明。

## <a name="syntax"></a>語法

```
set verbose {on | off}
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| on | 在陰影複製建立過程中開啟詳細資訊輸出記錄。 如果開啟詳細資訊模式，則 **設定** 會提供寫入器包含或排除的詳細資料，以及中繼資料壓縮和解壓縮的詳細資料。 |
| 關 | 在陰影複製建立過程中關閉詳細資訊輸出記錄。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [設定內容命令](set-context.md)

- [設定中繼資料命令](set-metadata.md)

- [set option 命令](set-option.md)
