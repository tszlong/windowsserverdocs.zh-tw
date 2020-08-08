---
title: BranchCache 網路殼層與 Windows PowerShell 命令
description: 本主題提供 Windows Server 2016 中 BranchCache 的 Network Shell 和 Windows PowerShell 命令參考資源連結
manager: brianlic
ms.topic: article
ms.assetid: a0726752-0a78-472b-9667-2f91636c1b3b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 13c62fd1c21cce98e5de2f6cc16f0edb6ee06a99
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87993796"
---
# <a name="branchcache-network-shell-and-windows-powershell-commands"></a>BranchCache 網路殼層與 Windows PowerShell 命令

>適用於：Windows Server (半年度管道)、Windows Server 2016

在 Windows Server 2016 中，您可以使用 Windows PowerShell 或網路命令介面 (適用于 BranchCache 的 Netsh) 命令來設定和管理 BranchCache。

在以後的 Windows 版本中，Microsoft 可能會移除 BranchCache 的 netsh 功能。 如果您目前使用 netsh 來設定和管理 BranchCache 和其他網路技術，Microsoft 建議您轉換到 Windows PowerShell。

Windows PowerShell 與 netsh 命令參考位於下列位置。 雖然 Windows Server 2016 之前的作業系統都已發行這兩個命令參考，但這些參考對此作業系統而言是正確的。

-   [Windows Server 2008 R2 中的 BranchCache netsh 命令](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd979561(v=ws.10))

-   [Windows PowerShell 中的 BranchCache Cmdlet](/powershell/module/branchcache/?view=win10-ps)

> [!TIP]
> 若要在 Windows PowerShell 命令提示字元下檢視 BranchCache 的 Windows PowerShell 命令清單，請在 Windows PowerShell 命令提示字元下輸入 `Get-Command -Module BranchCache`，然後按 ENTER。