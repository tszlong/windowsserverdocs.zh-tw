---
title: bitsadmin util 和版本
description: 適用于**bitsadmin util 和 version**的 Windows 命令主題，其會顯示 BITS 服務的版本。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c2518eb7a8f15d9a592ed9a77dd67a6f8d8afac
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122460"
---
# <a name="bitsadmin-util-and-version"></a>bitsadmin util 和版本

顯示 BITS 服務的版本（例如，2.0）。

> [!NOTE]
> BITS 1.5 和更早版本不支援此命令。

## <a name="syntax"></a>語法

```
bitsadmin /util /version [/verbose]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /verbose | 使用此參數可顯示每個位相關 DLL 的檔案版本，並確認 BITS 服務是否可以啟動。|

## <a name="examples"></a>範例

下列範例是 BITS 服務的版本。

```
C:\>bitsadmin /util /version
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)