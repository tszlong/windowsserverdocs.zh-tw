---
title: 恢復
description: Revert 命令的參考文章，可將磁片區還原回指定的陰影複製。
ms.topic: reference
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cc80890604b5ad1a308d1cd4df9cd23465c970d2
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027276"
---
# <a name="revert"></a>恢復

將磁片區還原回指定的陰影複製。 只有 CLIENTACCESSIBLE 內容中的陰影複製才支援此功能。 這些陰影複製是永久性的，而且只能由系統提供者進行。 如果使用時不含參數，則 **revert** 會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
revert <shadowcopyID>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<shadowcopyID>` | 指定要將磁片區還原成的陰影複製識別碼。 如果您未使用此參數，命令會在命令提示字元中顯示說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
