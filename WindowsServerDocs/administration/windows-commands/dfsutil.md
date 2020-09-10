---
title: dfsutil.exe
description: 用於管理 DFS 命名空間、伺服器和用戶端之 dfsutil 命令的參考文章。
ms.topic: reference
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9fb3883403dd0f4a88b612655247c4f4a2df7de7
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635045"
---
# <a name="dfsutil"></a>dfsutil.exe

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Dfsutil 命令會管理 DFS 命名空間、伺服器和用戶端。

## <a name="functionality-available-in-powershell"></a>PowerShell 提供的功能

[DFSN](/powershell/module/dfsn/?view=win10-ps) PowerShell 模組會提供對等的功能給下列 dfsutil 參數。

| 參數 | 描述 |
| --------- | ----------- |
| root | 顯示、建立、移除、匯入、匯出命名空間根。 |
| link | 顯示、建立、移除或移動 (連結) 的資料夾。 |
| 目標 | 顯示、建立、移除資料夾目標或命名空間伺服器。 |
| 屬性 | 顯示或修改資料夾目標或命名空間伺服器。 |
| 伺服器 | 顯示或修改命名空間設定。 |
| 網域 | 顯示網域中所有以網域為基礎的命名空間。 |

## <a name="functionality-available-only-in-dfsutil"></a>僅適用于 dfsutil 的功能

下列功能只可作為 dfsutil 參數使用：

| 參數 | 描述 |
| --------- | ----------- |
| 用戶端 | 顯示或修改用戶端資訊或登錄機碼。 |
| diag | 執行診斷或 view dfsdirs/dfspath。 |
| 快取 | 顯示或清除用戶端快取。 |

如需每個命令的詳細資訊，請在已安裝 DFS 命名空間管理工具的伺服器上開啟命令提示字元，然後輸入 `dfsutil client /?` 、 `dfsutil diag /?` 或 `dfsutil cache /?` 。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
