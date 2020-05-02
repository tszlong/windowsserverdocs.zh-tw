---
title: dfsutil
description: 適用于 dfsutil 的參考主題，可管理 DFS 命名空間、伺服器和用戶端。 dfsutil 命令會使用原始的分散式檔案系統術語，並提供更新的 DFS 命名空間術語，做為大部分命令的說明。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 999eef79227d4531ba724c9cac40127297ea38a0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719519"
---
# <a name="dfsutil"></a>dfsutil

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Dfsutil 命令會管理 DFS 命名空間、伺服器和用戶端。

>[!NOTE]
>**DFS 命名空間 PowerShell 模組**提供部分 Dfsutil 參數的替代專案，有些則仍然需要您使用 Dfsutil。 如需有關更新的 PowerShell 對應專案的詳細資訊，請參閱[DFSN](https://docs.microsoft.com/powershell/module/dfsn/?view=win10-ps)。

## <a name="parameters-available-in-powershell"></a>PowerShell 中可用的參數

您可以使用 PowerShell 中的下列參數：

| 參數 | 描述 |
| --------- | ----------- |
| root | 顯示、建立、移除、匯入、匯出命名空間根。 |
| link | 顯示、建立、移除或移動資料夾（連結）。 |
| 目標 | 顯示、建立、移除資料夾目標或命名空間伺服器。 |
| 屬性 | 顯示或修改資料夾目標或命名空間伺服器。 |
| 伺服器 | 顯示或修改命名空間設定。 |
| 網域 | 顯示網域中所有的網域型命名空間。 |

## <a name="parameters-only-available-in-dfsutil"></a>僅適用于 dfsutil 的參數

您只能從 dfsutil 使用下列參數。

| 參數 | 描述 |
| --------- | ----------- |
| 用戶端 | 顯示或修改用戶端資訊或登錄機碼。 |
| diag | 執行診斷或 view dfsdirs/dfspath。 |
| 快取 | 顯示或排清用戶端快取。 |

如需每個命令的詳細資訊，請在已安裝 DFS 命名空間管理工具的伺服器上開啟命令提示字元，然後`dfsutil client /?`輸入`dfsutil diag /?`、或`dfsutil cache /?`。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)