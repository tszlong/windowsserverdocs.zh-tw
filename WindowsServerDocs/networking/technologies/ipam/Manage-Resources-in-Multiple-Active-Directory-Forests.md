---
title: 管理多個 Active Directory 樹系中的資源
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
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
ms.openlocfilehash: c6752d87be2e689e517b287092a2570e27943c18
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815969"
---
# <a name="manage-resources-in-multiple-active-directory-forests"></a>管理多個 Active Directory 樹系中的資源

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題以了解如何使用 IPAM 來管理網域控制站、 DHCP 伺服器和多個 Active Directory 樹系中的 DNS 伺服器。  
  
若要使用 IPAM 來管理遠端的 Active Directory 樹系中的資源，您想要管理每個樹系必須有兩個 IPAM 安裝所在的樹系具有雙向信任。  
  
若要開始探索程序適用於不同的 Active Directory 樹系，開啟 伺服器管理員，並按一下 IPAM。 在 IPAM 用戶端主控台中，按一下**設定伺服器探索**，然後按一下**取得樹系**。 這會起始探索受信任的樹系和其網域的背景工作。 在探索程序完成之後，請按一下**設定伺服器探索**，這會開啟下列對話方塊。  
  
![設定伺服器探索](../../media/Manage-Resources-in-Multiple-Active-Directory-Forests/ipam_serverdiscovery.jpg)  

>[!NOTE]
>為群組原則\-型佈建的 Active Directory 跨樹系案例，請確定您執行下列 Windows PowerShell cmdlet，在 IPAM 伺服器上並不信任網域上的網域控制站。 例如，如果樹系 corp.contoso.com 已加入您的 IPAM 伺服器，而且信任的樹系是 fabrikam.com，您可以執行下列 Windows PowerShell cmdlet 在 corp.contoso.com 中的 IPAM 伺服器上為群組原則\-基礎上佈建fabrikam.com 樹系。 若要執行這個指令程式，您必須是 fabrikam.com 樹系中的 Domain Admins 群組的成員。

    
    Invoke-IpamGpoProvisioning -Domain fabrikam.COM -GpoPrefixName IPAMSERVER -IpamServerFqdn IPAM.CORP.CONTOSO.COM
    

在 [**設定伺服器探索**] 對話方塊中，按一下**選取的樹系**，然後選擇您想要使用 IPAM 管理的樹系。 選取 您要管理，然後按一下 網域 而且**新增**。

在 **選取伺服器角色探索**，每個您想要管理的網域，指定要探索的伺服器類型。 選項包括**網域控制站**， **DHCP 伺服器**，並**DNS 伺服器**。

根據預設，探索到的網域控制站、 DHCP 伺服器和 DNS 伺服器，-因此如果您不想探索其中一個這些類型的伺服器，請確定您取消選取該選項的核取方塊。

在上述範例圖中，IPAM 伺服器安裝在 contoso.com 樹系並 fabrikam.com 樹系根網域已新增 IPAM 管理的。 選取的伺服器角色，可讓 IPAM 探索及管理網域控制站、 DHCP 伺服器和在 fabrikam.com 根網域，而 contoso.com 的根網域的 DNS 伺服器。

您已指定樹系、 網域和伺服器角色之後，請按一下**確定**。 IPAM 會執行探索，並探索完成時，您可以管理本機和遠端樹系中的資源。
