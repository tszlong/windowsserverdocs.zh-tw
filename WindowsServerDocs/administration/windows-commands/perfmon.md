---
title: perfmon
description: Perfmon 命令的參考文章，它會以特定獨立模式啟動 Windows 可靠性和效能監視器。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9a8d5eca-8473-463e-a6e0-7bbd590b18e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/25/2018
ms.openlocfilehash: 5beaa1692f3fc4aa66eae81069f0ead5839d22e3
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922837"
---
# <a name="perfmon"></a>perfmon

以特定獨立模式啟動 Windows 可靠性和效能監視器。

## <a name="syntax"></a>語法

```
perfmon </res|report|rel|sys>
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| /res | 啟動資源檢視。 |
| /report | 啟動「系統診斷」資料收集器集合工具，並顯示結果的報告。 |
| /rel | 啟動可靠性監視器。 |
| /sys | 啟動效能監視器。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [Windows 效能監視器](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc749154(v%3dws.11))
