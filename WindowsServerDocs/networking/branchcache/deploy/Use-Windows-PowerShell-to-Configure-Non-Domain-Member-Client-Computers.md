---
title: 使用 Windows PowerShell 設定非網域成員用戶端電腦
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量
manager: brianlic
ms.topic: get-started-article
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 57fbe66168ccf1db5f4136d805206036405b3848
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964294"
---
# <a name="use-windows-powershell-to-configure-non-domain-member-client-computers"></a>使用 Windows PowerShell 設定非網域成員用戶端電腦

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式，針對分散式快取模式或託管快取模式手動設定 BranchCache 用戶端電腦。

> [!NOTE]
> 如果您已使用群組原則設定 BranchCache 用戶端電腦，則群組原則設定會覆寫套用原則之用戶端電腦的任何手動設定。

若要執行此程式，至少需要**Administrators**的成員資格或同等許可權。

### <a name="to-enable-branchcache-distributed-or-hosted-cache-mode"></a>若要啟用 BranchCache 分散式或託管快取模式

1.  在您要設定的 BranchCache 用戶端電腦上，以系統管理員身分執行 Windows PowerShell，然後執行下列其中一項動作。

    -   若要設定 BranchCache 分散式快取模式的用戶端電腦，請輸入下列命令，然後按 ENTER。

        `Enable-BCDistributed`

    -   若要設定 BranchCache 託管快取模式的用戶端電腦，請輸入下列命令，然後按 ENTER。

        `Enable-BCHostedClient`

        > [!TIP]
        > 如果您想要指定可用的託管快取伺服器，請使用 `-ServerNames` 參數，並以逗號分隔的託管快取伺服器清單做為參數值。 例如，如果您有兩個名為 HCS1 和 HCS2 的託管快取伺服器，請使用下列命令來設定適用于託管快取模式的用戶端電腦。
        >
        > `Enable-BCHostedClient -ServerNames HCS1,HCS2`



