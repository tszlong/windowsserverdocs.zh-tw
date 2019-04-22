---
title: 部署託管快取伺服器 (選擇性)
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 96d03b42-6cd9-4905-b6a2-dc36130dd24f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b19680e933e7a33871816578b63c5a141db0ce00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826209"
---
# <a name="deploy-hosted-cache-servers-optional"></a>部署託管快取伺服器 (選擇性)

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用此程序來安裝和設定 BranchCache 託管快取伺服器位於您想要用來部署 BranchCache 託管快取模式的分公司。 Windows Server 2016 中的 branchcache，您可以部署一個分公司中的多部託管快取伺服器。  
  
> [!IMPORTANT]  
> 這個步驟是選擇性的因為分散式快取模式不需要在分公司的託管快取伺服器電腦。 如果您不打算進行部署託管在任何的分公司中的快取模式，您不需要部署託管快取伺服器，和您不需要執行此程序中的步驟。  
  
您必須是隸屬**系統管理員**，或同等權限才能執行此程序。  
  
### <a name="to-install-and-configure-a-hosted-cache-server"></a>安裝和設定託管快取伺服器  
  
1.  在您想要設定為託管快取伺服器的電腦，執行下列命令在 Windows PowerShell 提示字元來安裝 BranchCache 功能。  
  
    `Install-WindowsFeature BranchCache -IncludeManagementTools`  
  
2.  將電腦設定為託管快取伺服器，使用下列命令之一：  
  
    -   若要設定非網域聯結的電腦做為託管快取伺服器，請在 Windows PowerShell 提示字元中，輸入下列命令，然後按 ENTER 鍵。  
  
        `Enable-BCHostedServer`  
  
    -   若要設定加入的網域的電腦做為託管快取伺服器，並在 Active Directory 註冊服務連接點自動託管快取伺服器探索用戶端電腦，請在 Windows PowerShell 提示字元中，輸入下列命令，然後按 ENTER 鍵。  
  
        `Enable-BCHostedServer -RegisterSCP`  
  
3.  若要確認正確設定託管快取伺服器，請在 Windows PowerShell 提示字元中，輸入下列命令，然後按 ENTER 鍵。  
  
    `Get-BCStatus`  
  
    > [!NOTE]  
    > 一節中執行這個命令中後, **HostedCacheServerConfiguration**，值**HostedCacheServerIsEnabled**會**True**。 如果您設定網域聯結的託管快取伺服器註冊服務連接點 (SCP) 在 Active Directory 的值**HostedCacheScpRegistrationEnabled**是 **，則為 True**。  
  

