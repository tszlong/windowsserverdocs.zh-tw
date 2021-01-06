---
title: 部署託管快取伺服器 (選擇性)
description: 瞭解如何安裝及設定 BranchCache 託管快取伺服器，這些伺服器位於您想要部署 BranchCache 託管快取模式的分公司。
manager: brianlic
ms.topic: how-to
ms.assetid: 96d03b42-6cd9-4905-b6a2-dc36130dd24f
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: 9ed41c9455abb2bcd9417d393206f6f6ca6bb71c
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946724"
---
# <a name="deploy-hosted-cache-servers-optional"></a>部署託管快取伺服器 (選擇性)

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用此程式來安裝及設定 BranchCache 託管快取伺服器，這些伺服器位於您想要部署 BranchCache 託管快取模式的分公司。 使用 Windows Server 2016 中的 BranchCache，您可以在一個分公司部署多部託管快取伺服器。

> [!IMPORTANT]
> 此步驟是選擇性的，因為分散式快取模式不需要分公司的託管快取伺服器電腦。 如果您不打算在任何分公司部署託管快取模式，則不需要部署託管快取伺服器，也不需要執行此程式中的步驟。

您必須是系統 **管理員** 的成員，或等同于執行此程式。

### <a name="to-install-and-configure-a-hosted-cache-server"></a>安裝及設定託管快取伺服器

1.  在您想要設定為託管快取伺服器的電腦上，于 Windows PowerShell 提示字元中執行下列命令，以安裝 BranchCache 功能。

    `Install-WindowsFeature BranchCache -IncludeManagementTools`

2.  使用下列其中一個命令將電腦設定為託管快取伺服器：

    -   若要將未加入網域的電腦設定為託管快取伺服器，請在 Windows PowerShell 提示字元中輸入下列命令，然後按 ENTER 鍵。

        `Enable-BCHostedServer`

    -   若要將加入網域的電腦設定為託管快取伺服器，並在用戶端電腦的 Active Directory 中註冊服務連接點，請在 Windows PowerShell 提示字元中輸入下列命令，然後按 ENTER 鍵。

        `Enable-BCHostedServer -RegisterSCP`

3.  若要確認託管快取伺服器的設定是否正確，請在 Windows PowerShell 提示字元中輸入下列命令，然後按 ENTER。

    `Get-BCStatus`

    > [!NOTE]
    > 執行此命令之後，在 **HostedCacheServerConfiguration** 一節中， **HostedCacheServerIsEnabled** 的值為 **True**。 如果您設定加入網域的託管快取伺服器來註冊服務連接點 (SCP) 在 Active Directory 中，則 **HostedCacheScpRegistrationEnabled** 的值為 **True**。


