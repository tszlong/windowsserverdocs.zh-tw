---
title: 步驟 2 準備叢集伺服器
description: 本主題是部署在 Windows Server 2016 的叢集中的遠端存取快速入門的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35d68abb-6914-42e0-91e8-803933cf785e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 10a40b30acbf022ed34f454d753884cb8c5c97d4
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281201"
---
# <a name="step-2-prepare-cluster-servers"></a>步驟 2 準備叢集伺服器

>適用於：Windows Server （半年通道），Windows Server 2016

您可以設定叢集部署之前，您會準備要新增至叢集的其他伺服器。  
  
|工作|描述|  
|----|--------|  
|[2.1 設定遠端存取基礎結構](#BKMK_config)|在您想要新增到叢集中每個伺服器上，設定伺服器拓樸、 IP 位址、 路由和轉送。 如果您設定的虛擬機器的負載平衡的叢集，您必須設定虛擬機器，若要使用 MAC 位址詐騙。<br /><br />此外，每一部伺服器加入相同的網域，而且所有伺服器都連接到相同的子網路。|  
|[2.2 安裝遠端存取角色](#BKMK_Install)|每個其他伺服器上您想要新增到叢集，安裝遠端存取角色|  
|[2.3 Install NLB](#BKMK_NLB)|已部署的遠端存取伺服器上，每個您想要新增到叢集的其他伺服器上安裝 NLB 功能。 請注意，此步驟不需要時使用外部負載平衡器。|  
  
## <a name="BKMK_config"></a>2.1 設定遠端存取基礎結構  
若要設定遠端存取叢集，您必須設定伺服器拓樸、 IP 位址、 路由和轉送將構成叢集的一部分的每部伺服器上。  
  
### <a name="to-configure-the-remote-access-infrastructure"></a>若要設定遠端存取基礎結構  
  
1.  設定每個將會有相同的拓樸，第一部遠端存取伺服器叢集中的伺服器。  
  
2.  設定每個伺服器的適當 IP 位址、 路由和轉送的第一部遠端存取伺服器設定為基礎的叢集。 請注意，在叢集中的所有伺服器必須都連線到相同的子網路。  
  
3.  加入每個會位於相同的網域，第一部遠端存取伺服器叢集中的伺服器。  
  
## <a name="BKMK_Install"></a>2.2 安裝遠端存取角色  
若要設定遠端存取叢集，您必須將構成叢集的一部分的每部伺服器上安裝遠端存取角色。  
  
### <a name="to-install-the-remote-access-role-on-always-on-vpn-servers"></a>一律開啟 」 VPN 伺服器上安裝遠端存取角色  
  
1.  在 DirectAccess 伺服器上，在 [伺服器管理員] 主控台中，在**儀表板**，按一下**新增角色及功能**。  
  
2.  按 [下一步]  3 次，進入伺服器角色選擇畫面。  
  
3.  在 [**選取伺服器角色**對話方塊中，選取**遠端存取**，然後按一下**下一步]** 。  
  
4.  按一下 [**下一步]** 三次。  
  
5.  在 **選取角色服務**對話方塊中，選取**DirectAccess 和 VPN (RAS)** ，然後按一下 **新增功能**。  
  
6.  選取 **路由**，選取**Web 應用程式 Proxy**，按一下 **新增功能**，然後按一下**下一步**。  
  
7. 按 [下一步]  ，然後按一下 [安裝]  。  
  
8.  在 [安裝進度]  對話方塊中，確認安裝成功，然後按一下 [關閉]  。  
  
9.  重複此程序，您想要成為叢集成員的所有伺服器上。  
  
## <a name="BKMK_NLB"></a>2.3 安裝 NLB  
若要設定遠端存取叢集，您必須將構成叢集的一部分的每部伺服器上安裝網路負載平衡功能。  
  
> [!NOTE]  
> 如果使用外部負載平衡器，不需要此步驟。  
  
#### <a name="to-install-the-nlb-role"></a>若要安裝 NLB 角色  
  
1.  在 DirectAccess 伺服器上，在 [伺服器管理員] 主控台中，在**儀表板**，按一下**新增角色及功能**。  
  
2.  按一下 [**下一步]** 四次，以前往 [選取伺服器功能] 畫面。  
  
3.  在上**選取的功能**對話方塊中，選取**網路負載平衡**，按一下**新增功能**，按一下 [**下一步]** ，然後按一下**安裝**。  
  
4.  在 [安裝進度]  對話方塊中，確認安裝成功，然後按一下 [關閉]  。  
  
5.  重複此程序，您想要成為叢集成員的所有伺服器上。  
  
## <a name="BKMK_Links"></a>另請參閱  
  
-   [步驟 3：設定負載平衡叢集](Step-3-Configure-a-Load-Balanced-Cluster.md)  
  


