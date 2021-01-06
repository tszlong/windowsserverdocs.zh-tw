---
title: 管理多個 Active Directory 樹系中的資源
description: 本主題是 Windows Server 2016 中的 IP 位址管理 (IPAM) 管理指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: 82f8f382-246e-4164-8306-437f7a019e0f
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 073e187435dabdc328c0549e072865b5d2978765
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948344"
---
# <a name="manage-resources-in-multiple-active-directory-forests"></a>管理多個 Active Directory 樹系中的資源

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解如何使用 IPAM 來管理多個 Active Directory 樹系中的網域控制站、DHCP 伺服器和 DNS 伺服器。

若要使用 IPAM 來管理遠端 Active Directory 樹系中的資源，您要管理的每個樹系都必須與安裝 IPAM 的樹系有雙向信任。

若要啟動不同 Active Directory 樹系的探索程式，請開啟伺服器管理員然後按一下 [IPAM]。 在 IPAM 用戶端主控台中，按一下 [ **設定伺服器探索**]，然後按一下 [ **取得** 樹系]。 這會起始會探索信任的樹系和其網域的背景工作。 探索程式完成之後，請按一下 [ **設定伺服器探索**]，這會開啟下列對話方塊。

![設定伺服器探索](../../media/Manage-Resources-in-Multiple-Active-Directory-Forests/ipam_serverdiscovery.jpg)

>[!NOTE]
>如需 Active Directory 跨樹系案例的群組原則型布建 \- ，請確定您在 IPAM 伺服器上執行下列 Windows PowerShell Cmdlet，而不是在信任的網域 dc 上執行。 例如，如果您的 IPAM 伺服器已加入樹系 corp.contoso.com，而信任樹系是 fabrikam.com，您可以在 corp.contoso.com 中的 IPAM 伺服器上執行下列 Windows PowerShell Cmdlet，以在 fabrikam.com 樹系上以群組原則為基礎的布建 \- 。 若要執行此 Cmdlet，您必須是 fabrikam.com 樹系中 Domain Admins 群組的成員。

```powershell
    Invoke-IpamGpoProvisioning -Domain fabrikam.COM -GpoPrefixName IPAMSERVER -IpamServerFqdn IPAM.CORP.CONTOSO.COM
```

在 [ **設定伺服器探索** ] 對話方塊中，按一下 [ **選取樹** 系]，然後選擇您要使用 IPAM 管理的樹系。 也請選取您想要管理的網域，然後按一下 [ **新增**]。

在 **[選取要探索的伺服器角色**] 中，針對您要管理的每個網域，指定要探索的伺服器類型。 選項包括 [ **網域控制站**]、[ **DHCP 伺服器**] 和 [ **DNS 伺服器**]。

根據預設，系統會探索網域控制站、DHCP 伺服器和 DNS 伺服器，因此，如果您不想要探索其中一種類型的伺服器，請確定您取消選取該選項的核取方塊。

在上述的範例圖中，IPAM 伺服器會安裝在 contoso.com 樹系中，並新增 fabrikam.com 樹系的根域以進行 IPAM 管理。 選取的伺服器角色可讓 IPAM 探索及管理 fabrikam.com 根域和 contoso.com 根域中的網域控制站、DHCP 伺服器和 DNS 伺服器。

指定樹系、網域和伺服器角色之後，請按一下 **[確定]**。 IPAM 會執行探索，當探索完成時，您可以管理本機和遠端樹系中的資源。
