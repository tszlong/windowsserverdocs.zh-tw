---
title: set metadata
description: 設定中繼資料命令的參考文章，此命令會設定陰影建立中繼資料檔案的名稱和位置，以用來將陰影複製從一部電腦傳送到另一部電腦。
ms.topic: reference
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2ff03661d953ae7afc39117ced89734c5df02c14
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389072"
---
# <a name="set-metadata"></a>set metadata

設定用來將陰影複製從一部電腦傳送到另一部電腦的陰影建立中繼資料檔案的名稱和位置。 如果使用時不含參數， **設定中繼資料** 會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
set metadata [<drive>:][<path>]<metadata.cab>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| `[<drive>:][<path>]` | 指定要建立中繼資料檔案的位置。 |
| `<metadata.cab>` | 指定要儲存陰影建立中繼資料的 cab 檔案名。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [設定內容命令](set-context.md)

- [set option 命令](set-option.md)

- [設定詳細資訊命令](set-verbose.md)
