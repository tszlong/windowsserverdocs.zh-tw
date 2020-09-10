---
title: perfmon
description: Perfmon 命令的參考文章，此命令會啟動 Windows 可靠性，並在特定的獨立模式中效能監視器。
ms.topic: reference
ms.assetid: 9a8d5eca-8473-463e-a6e0-7bbd590b18e1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 07/25/2018
ms.openlocfilehash: c68f0c2160621ed38ea013697f178ec461c8d44c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627445"
---
# <a name="perfmon"></a>perfmon

以特定的獨立模式啟動 Windows 可靠性及效能監視器。

## <a name="syntax"></a>語法

```
perfmon </res|report|rel|sys>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| /res | 啟動資源檢視。 |
| /report | 啟動系統診斷資料收集器集合工具，並顯示結果的報告。 |
| /rel | 啟動可靠性監視器。 |
| /sys | 啟動效能監視器。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [Windows 效能監視器](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc749154(v%3dws.11))
