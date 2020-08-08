---
title: 部署託管快取伺服器 (選擇性)
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量
manager: brianlic
ms.topic: get-started-article
ms.assetid: 96d03b42-6cd9-4905-b6a2-dc36130dd24f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 75f82b660edeac538bda511d71526aacc2433b5b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964244"
---
# <a name="deploy-hosted-cache-servers-optional"></a>部署託管快取伺服器 (選擇性)

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式，在您想要部署 BranchCache 託管快取模式的分公司中，安裝和設定 BranchCache 託管快取伺服器。 有了 Windows Server 2016 中的 BranchCache，您就可以在一個分公司部署多部託管快取伺服器。

> [!IMPORTANT]
> 此步驟是選擇性的，因為分散式快取模式不需要分公司的託管快取伺服器電腦。 如果您不打算在任何分公司部署託管快取模式，就不需要部署託管快取伺服器，也不需要執行此程式中的步驟。

您必須是系統**管理員**的成員或同等許可權，才能執行此程式。

### <a name="to-install-and-configure-a-hosted-cache-server"></a>安裝和設定託管快取伺服器

1.  在您要設定為託管快取伺服器的電腦上，于 Windows PowerShell 提示字元中執行下列命令，以安裝 BranchCache 功能。

    `Install-WindowsFeature BranchCache -IncludeManagementTools`

2.  使用下列其中一個命令，將電腦設定為託管快取伺服器：

    -   若要將未加入網域的電腦設定為託管快取伺服器，請在 Windows PowerShell 提示字元中輸入下列命令，然後按 ENTER。

        `Enable-BCHostedServer`

    -   若要將加入網域的電腦設定為託管快取伺服器，並在 Active Directory 用戶端電腦的自動託管快取伺服器探索中註冊服務連接點，請在 Windows PowerShell 提示字元中輸入下列命令，然後按 ENTER 鍵。

        `Enable-BCHostedServer -RegisterSCP`

3.  若要驗證託管快取伺服器的正確設定，請在 Windows PowerShell 提示字元中輸入下列命令，然後按 ENTER。

    `Get-BCStatus`

    > [!NOTE]
    > 執行此命令之後，在**HostedCacheServerConfiguration**區段中， **HostedCacheServerIsEnabled**的值為**True**。 如果您已將加入網域的託管快取伺服器設定為在 Active Directory 中註冊服務連接點 (SCP) ，則**HostedCacheScpRegistrationEnabled**的值為**True**。


