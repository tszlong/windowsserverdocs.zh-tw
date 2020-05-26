---
title: help
description: '[說明] 命令的參考主題，它會顯示可用命令的清單，或指定命令的詳細說明資訊。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c65b5ac3-711a-4368-95b8-ba82e2d00713
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9b73ef32b49b834a91f24e943749eb21398c8588
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818658"
---
# <a name="help"></a>help

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示指定命令的可用命令或詳細說明資訊的清單。 如果在沒有**參數的情況**下使用，說明會列出並簡短地描述每個系統命令。

## <a name="syntax"></a>語法

```
help [<command>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<command>` | 指定要顯示其詳細說明資訊的命令。 |

### <a name="examples"></a>範例

若要查看**robocopy**命令的相關資訊，請輸入：

```
help robocopy
```

若要顯示 DiskPart 中所有可用命令的清單，請輸入：

```
help
```

若要顯示有關如何在 DiskPart 中使用 [**建立資料分割主要**] 命令的詳細說明資訊，請輸入：

```
help create partition primary
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
