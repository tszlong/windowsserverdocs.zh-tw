---
title: 保留
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b437e9f0c8d671e4378311d450aa0ac7639219f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852169"
---
# <a name="retain"></a>保留



準備現有動態簡單磁碟區來為開機或系統磁碟區。

## <a name="syntax"></a>語法

```
retain
```

## <a name="remarks"></a>備註

-   在主開機記錄 (MBR) 動態磁碟上，此命令會建立主開機記錄磁碟分割項目。
-   在 GUID 磁碟分割表格 (GPT) 動態磁碟上，此命令會建立 GUID 磁碟分割表格磁碟分割項目。

#### <a name="additional-references"></a>其他參考資料

