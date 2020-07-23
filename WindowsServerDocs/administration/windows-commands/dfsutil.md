---
title: dfsutil
description: 用於管理 DFS 命名空間、伺服器和用戶端之 dfsutil 命令的參考文章。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cfb3d221e275a688f5c18a960681257077fb4f7f
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86958370"
---
# <a name="dfsutil"></a>dfsutil

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Dfsutil 命令會管理 DFS 命名空間、伺服器和用戶端。

## <a name="functionality-available-in-powershell"></a>PowerShell 中可用的功能

[DFSN](/powershell/module/dfsn/?view=win10-ps) PowerShell 模組為下列的 dfsutil 參數提供對等的功能。

| 參數 | 描述 |
| --------- | ----------- |
| root | 顯示、建立、移除、匯入、匯出命名空間根。 |
| link | 顯示、建立、移除或移動資料夾（連結）。 |
| 目標 | 顯示、建立、移除資料夾目標或命名空間伺服器。 |
| 屬性 | 顯示或修改資料夾目標或命名空間伺服器。 |
| 伺服器 | 顯示或修改命名空間設定。 |
| 網域 | 顯示網域中所有的網域型命名空間。 |

## <a name="functionality-available-only-in-dfsutil"></a>僅適用于 dfsutil 的功能

下列功能僅適用于 dfsutil 參數：

| 參數 | 描述 |
| --------- | ----------- |
| 用戶端 | 顯示或修改用戶端資訊或登錄機碼。 |
| diag | 執行診斷或 view dfsdirs/dfspath。 |
| 快取 | 顯示或排清用戶端快取。 |

如需每個命令的詳細資訊，請在已安裝 DFS 命名空間管理工具的伺服器上開啟命令提示字元，然後輸入 `dfsutil client /?` 、 `dfsutil diag /?` 或 `dfsutil cache /?` 。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
