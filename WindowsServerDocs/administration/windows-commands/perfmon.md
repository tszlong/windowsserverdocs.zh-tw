---
title: perfmon
description: Perfmon 的參考主題
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9a8d5eca-8473-463e-a6e0-7bbd590b18e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/25/2018
ms.openlocfilehash: 5742b51ffd4fca16c1054e373636afbe39968c96
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723328"
---
# <a name="perfmon"></a>perfmon

以特定獨立模式啟動 Windows 可靠性和效能監視器。

## <a name="syntax"></a>語法

```
perfmon </res|report|rel|sys>
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/res|啟動資源檢視。|
|/report|啟動「系統診斷」資料收集器集合工具，並顯示結果的報告。|
|/rel|啟動可靠性監視器。|
|/sys|啟動效能監視器。|

## <a name="additional-references"></a>其他參考

[Windows 效能監視器](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc749154(v%3dws.11))
