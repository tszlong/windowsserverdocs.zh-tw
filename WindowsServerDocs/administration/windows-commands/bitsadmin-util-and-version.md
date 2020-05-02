---
title: bitsadmin util and version
description: Bitsadmin util 和 version 命令的參考主題，其會顯示 BITS 服務的版本。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98f17328-dfbd-4cbb-93c1-b8d424bc3f0a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 20c3db6e6fcd5ef3d00287f36c9f9624ab5224dd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707597"
---
# <a name="bitsadmin-util-and-version"></a>bitsadmin util and version

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

以顯示 BITS 服務的版本。

```
bitsadmin /util /version
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [bitsadmin util 命令](bitsadmin-util.md)

- [bitsadmin 命令](bitsadmin.md)
