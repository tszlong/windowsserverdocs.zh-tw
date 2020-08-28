---
title: help
description: 說明命令的參考文章，此命令會顯示所指定命令的可用命令或詳細說明資訊的清單。
ms.topic: reference
ms.assetid: c65b5ac3-711a-4368-95b8-ba82e2d00713
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 701b3a1840ac56e399bb4febf0765c2f69a84633
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035456"
---
# <a name="help"></a>help

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示指定命令的可用命令或詳細說明資訊的清單。 如果未使用任何參數 **，請** 說明清單並簡短描述每個系統命令。

## <a name="syntax"></a>語法

```
help [<command>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<command>` | 指定要顯示詳細說明資訊的命令。 |

### <a name="examples"></a>範例

若要查看 **robocopy** 命令的相關資訊，請輸入：

```
help robocopy
```

若要顯示 DiskPart 中可用之所有命令的清單，請輸入：

```
help
```

若要顯示有關如何在 DiskPart 中使用 **create partition primary** 命令的詳細說明資訊，請輸入：

```
help create partition primary
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
