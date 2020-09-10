---
title: diskperf
description: Diskperf 命令的參考文章，可用來在執行 Windows 的電腦上遠端啟用或停用實體或邏輯磁片效能計數器。
ms.topic: reference
ms.assetid: f06916e8-069b-4ec8-a6eb-59f1d9f77111
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f0e7700b3fbe1cec79aa9cf2b5c93eab9bd8372d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624819"
---
# <a name="diskperf"></a>diskperf

**Diskperf**命令會在執行 Windows 的電腦上遠端啟用或停用實體或邏輯磁片效能計數器。

## <a name="syntax"></a>語法

```
diskperf [-y[d|v] | -n[d|v]] [\\computername]
```

## <a name="options"></a>選項。

| 選項 | 描述 |
| ------ | ----------- |
| -y | 當電腦重新開機時，啟動所有磁片效能計數器。 |
| -yd | 當電腦重新開機時，啟用實體磁片磁碟機的磁片效能計數器。 |
| -yv | 當電腦重新開機時，可啟用邏輯磁碟機或存放磁片區的磁片效能計數器。 |
| -n | 當電腦重新開機時，停用所有磁片效能計數器。 |
| -nd | 當電腦重新開機時，請停用實體磁片磁碟機的磁片效能計數器。 |
| -nv | 當電腦重新開機時，請停用邏輯磁碟機或存放磁片區的磁片效能計數器。 |
| `\\<computername>` | 指定您要啟用或停用磁片效能計數器的電腦名稱稱。 |
| -? | 顯示即時線上說明。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
