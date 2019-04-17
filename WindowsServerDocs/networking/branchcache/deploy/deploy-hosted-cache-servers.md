---
title: 部署裝載快取伺服器（選擇性）
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 96d03b42-6cd9-4905-b6a2-dc36130dd24f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d7345b9acf5ef5003cc2a811569083d7c12894a1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-hosted-cache-servers-optional"></a>部署裝載快取伺服器（選擇性）

>適用於：Windows Server（以每年次管道）、Windows Server 2016

安裝和 BranchCache 裝載快取伺服器位於分公司您想要部署 BranchCache 裝載快取模式中的設定，您可以使用此程序。 與 Windows Server 2016 中 BranchCache，您可以部署一個分公司中的多個裝載快取伺服器。  
  
> [!IMPORTANT]  
> 因為分散式快取模式不需要在分公司裝載快取伺服器電腦此步驟是選擇性的。 如果您不計劃在部署裝載快取任何分公司模式，您不需要部署裝載快取伺服器，並您不需要執行這個程序中的步驟。  
  
您必須成員的**系統管理員**，或相當於執行此程序。  
  
### <a name="to-install-and-configure-a-hosted-cache-server"></a>安裝和設定的裝載快取伺服器  
  
1.  在電腦上您想要為裝載快取伺服器設定，請安裝 BranchCache 功能的 Windows PowerShell 命令提示字元中，執行下列命令。  
  
    `Install-WindowsFeature BranchCache -IncludeManagementTools`  
  
2.  設定為裝載快取伺服器的電腦使用其中一項下列命令：  
  
    -   若要非網域結合將電腦設定為裝載快取伺服器，在 Windows PowerShell 命令提示字元中，輸入下列命令，然後按 ENTER 鍵。  
  
        `Enable-BCHostedServer`  
  
    -   若要設定的網域加入為裝載快取伺服器、電腦與服務連接點 Active Directory 中登記參加並自動裝載快取伺服器探索 client 電腦，在 Windows PowerShell 命令提示字元中，輸入下列命令，然後按 ENTER 鍵。  
  
        `Enable-BCHostedServer -RegisterSCP`  
  
3.  若要確認裝載快取伺服器的正確設定，Windows PowerShell 命令提示字元中，輸入下列命令，然後按 ENTER 鍵。  
  
    `Get-BCStatus`  
  
    > [!NOTE]  
    > 您一節中執行此命令之後**HostedCacheServerConfiguration**的值為**HostedCacheServerIsEnabled**為**True**。 如果您設定在 Active Directory 的值為登記服務連接點 (SCP) 網域結合裝載快取伺服器**HostedCacheScpRegistrationEnabled**是**，則為 True**。  
  

