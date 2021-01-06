---
title: 使用 Windows PowerShell 設定非網域成員用戶端電腦
description: 瞭解如何針對分散式快取模式或託管快取模式手動設定 BranchCache 用戶端電腦。
manager: brianlic
ms.topic: get-started-article
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fac1b4ffe9b51d28b7bce917352335921d3b700e
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904483"
---
# <a name="use-windows-powershell-to-configure-non-domain-member-client-computers"></a>使用 Windows PowerShell 設定非網域成員用戶端電腦

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用這個程式，以手動方式設定 BranchCache 用戶端電腦的分散式快取模式或託管快取模式。

> [!NOTE]
> 如果您已使用群組原則設定 BranchCache 用戶端電腦，則群組原則設定會覆寫套用原則之用戶端電腦的任何手動設定。

若要執行此程式，至少需要 **Administrators** 的成員資格或同等許可權。

### <a name="to-enable-branchcache-distributed-or-hosted-cache-mode"></a>啟用 BranchCache 分散式或託管快取模式

1.  在您要設定的 BranchCache 用戶端電腦上，以系統管理員身分執行 Windows PowerShell，然後執行下列其中一項動作。

    -   若要設定用戶端電腦的 BranchCache 分散式快取模式，請輸入下列命令，然後按 ENTER。

        `Enable-BCDistributed`

    -   若要為 BranchCache 託管快取模式設定用戶端電腦，請輸入下列命令，然後按 ENTER。

        `Enable-BCHostedClient`

        > [!TIP]
        > 如果您想要指定可用的託管快取伺服器，請使用 `-ServerNames` 參數，並以逗號分隔的託管快取伺服器清單作為參數值。 例如，如果您有兩個名為 HCS1 和 HCS2 的託管快取伺服器，請使用下列命令為託管快取模式設定用戶端電腦。
        >
        > `Enable-BCHostedClient -ServerNames HCS1,HCS2`



