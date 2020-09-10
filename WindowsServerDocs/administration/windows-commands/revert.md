---
title: 恢復
description: Revert 命令的參考文章，可將磁片區還原回指定的陰影複製。
ms.topic: reference
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3c909cec1e503552f68cad55489529585a5eaf51
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640621"
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
