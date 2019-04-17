---
title: 管理多 Active Directory 森林中的資源
description: 本主題是在 Windows Server 2016 的 IP 位址管理 (IPAM) 管理組節目表的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82f8f382-246e-4164-8306-437f7a019e0f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b01680d4b35461ff85965781ebc60e7a613d1cb8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="manage-resources-in-multiple-active-directory-forests"></a>管理多 Active Directory 森林中的資源

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解如何使用 IPAM 管理網域控制站、DHCP 伺服器，並在多個 Active Directory 樹系的 DNS 伺服器。  
  
若要使用 IPAM 管理遠端 Active Directory 森林中的資源，每個您想要管理樹系必須兩種方式信任的樹系安裝 IPAM 的位置。  
  
若要探索不同的 Active Directory 樹系的程序，請打開伺服器管理員並按一下 IPAM。 在 IPAM client 主控台中，按一下 [**設定伺服器探索**，然後按一下 [**取得森林**。 在這樣的背景工作探索受信任的樹系與他們的網域。 探索程序完成之後，請按一下**設定伺服器探索**，這會下列。  
  
![設定伺服器探索](../../media/Manage-Resources-in-Multiple-Active-Directory-Forests/ipam_serverdiscovery.jpg)  

>[!NOTE]
>適用於群組 Policy\ 型提供 Active Directory 跨樹系案例，確保您執行下列 Windows PowerShell cmdlet IPAM 伺服器上，而不是在 Dc 信任的網域。 做為範例，如果 IPAM 伺服器樹系 corp.contoso.com 所加入，且信任的樹系 fabrikam.com，您可以執行下列 Windows PowerShell cmdlet IPAM 在伺服器上的群組 Policy\ 型提供 fabrikam.com 樹 corp.contoso.com。 若要執行下列 cmdlet，您必須是 fabrikam.com 森林中網域管理群組成員。

    
    Invoke-IpamGpoProvisioning -Domain fabrikam.COM -GpoPrefixName IPAMSERVER -IpamServerFqdn IPAM.CORP.CONTOSO.COM
    

在**設定伺服器探索**對話方塊中，按一下 [**選取樹系**，然後選擇您想要管理 IPAM 的樹系。 也選取您想要管理，然後按一下 [網域**新增**。

在**選擇要探索伺服器角色**，為您想要管理每個網域，指定的伺服器來探索類型。 這些選項**網域控制站**，**DHCP 伺服器**，並**的 DNS 伺服器**。

根據預設，發現網域控制站、DHCP 伺服器和 DNS 伺服器-，如果您不想要找出這類的伺服器的其中一個，請確定您取消選取核取方塊，該選項。

範例圖例上述 contoso.com、樹系會安裝 IPAM 伺服器，並根 fabrikam.com 樹系的網域新增 IPAM 管理。 選取的伺服器角色允許 IPAM 探索及管理網域控制站、DHCP 伺服器及 fabrikam.com 根網域 contoso.com 根網域中的 DNS 伺服器。

森林、網域及伺服器角色指定您之後，請按**[確定]**。 IPAM 執行探索並探索完成時，您可以管理本機和遠端森林中的資源。
