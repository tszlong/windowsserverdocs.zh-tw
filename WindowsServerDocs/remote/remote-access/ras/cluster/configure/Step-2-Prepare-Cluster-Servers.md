---
title: 步驟2準備叢集伺服器
description: 本主題是在 Windows Server 2016 的叢集中部署遠端存取指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35d68abb-6914-42e0-91e8-803933cf785e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 87983076ee8a7d5546a5ac491ed4ca88153798f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367415"
---
# <a name="step-2-prepare-cluster-servers"></a>步驟2準備叢集伺服器

>適用於：Windows Server (半年度管道)、Windows Server 2016

在您可以設定叢集部署之前，請先準備要新增至叢集的其他伺服器。  
  
|工作|描述|  
|----|--------|  
|[2.1 設定遠端存取基礎結構](#BKMK_config)|在您要新增到叢集的每部伺服器上，設定伺服器拓撲、IP 定址、路由和轉送。 如果您設定虛擬機器的負載平衡叢集，則必須將虛擬機器設定為使用 MAC 位址詐騙。<br /><br />此外，將每部伺服器聯結至相同的網域，並將所有伺服器連線到相同的子網。|  
|[2.2 安裝遠端存取角色](#BKMK_Install)|在您要新增到叢集的每部額外伺服器上，安裝遠端存取角色|  
|[2.3 安裝 NLB](#BKMK_NLB)|在部署的遠端存取服務器上，以及您要新增到叢集的每部額外伺服器上，安裝 NLB 功能。 請注意，使用外部 Load Balancer 時，不需要執行此步驟。|  
  
## <a name="BKMK_config"></a>2.1 設定遠端存取基礎結構  
若要設定遠端存取叢集，您必須在將形成叢集一部分的每部伺服器上，設定伺服器拓撲、IP 位址、路由和轉送。  
  
### <a name="to-configure-the-remote-access-infrastructure"></a>設定遠端存取基礎結構  
  
1.  使用與第一部遠端存取服務器相同的拓撲，設定將位於叢集中的每部伺服器。  
  
2.  根據第一部遠端存取服務器的設定，使用適當的 IP 位址、路由和轉送，設定每個將位於叢集中的伺服器。 請注意，叢集中的所有伺服器都必須連接到相同的子網。  
  
3.  將叢集中的每部伺服器聯結到與第一部遠端存取服務器相同的網域。  
  
## <a name="BKMK_Install"></a>2.2 安裝遠端存取角色  
若要設定遠端存取叢集，您必須在將形成叢集一部分的每一部伺服器上安裝遠端存取角色。  
  
### <a name="to-install-the-remote-access-role-on-always-on-vpn-servers"></a>在 Always On VPN 伺服器上安裝遠端存取角色  
  
1.  在 DirectAccess 伺服器上，在伺服器管理員主控台的 [**儀表板**] 中，按一下 [**新增角色及功能**]。  
  
2.  按 [下一步] 3 次，進入伺服器角色選擇畫面。  
  
3.  在 [**選取伺服器角色**] 對話方塊中，選取 [**遠端存取**]，然後按 **[下一步]** 。  
  
4.  按三次 **[下一步]** 。  
  
5.  在 [**選取角色服務**] 對話方塊中，選取 [ **DirectAccess 和 VPN （RAS）** ]，然後按一下 [**新增功能**]。  
  
6.  選取 [**路由**]，選取 [ **Web 應用程式 Proxy**]，按一下 [**新增功能**]，然後按 **[下一步]**  
  
7. 按 [下一步]，然後按一下 [安裝]。  
  
8.  在 [安裝進度] 對話方塊中，確認安裝成功，然後按一下 [關閉]。  
  
9.  在您要做為叢集成員的所有伺服器上重複此程式。  
  
## <a name="BKMK_NLB"></a>2.3 安裝 NLB  
若要設定遠端存取叢集，您必須在將形成叢集一部分的每一部伺服器上安裝網路負載平衡功能。  
  
> [!NOTE]  
> 如果使用外部負載平衡器，則不需要執行此步驟。  
  
#### <a name="to-install-the-nlb-role"></a>若要安裝 NLB 角色  
  
1.  在 DirectAccess 伺服器上，在伺服器管理員主控台的 [**儀表板**] 中，按一下 [**新增角色及功能**]。  
  
2.  按 **[下一步]** 四次，以進入伺服器功能選取畫面。  
  
3.  在 [**選取功能**] 對話方塊中，選取 [**網路負載平衡**]，按一下 [**新增功能**]，按 **[下一步]** ，然後按一下 [**安裝**]  
  
4.  在 [安裝進度] 對話方塊中，確認安裝成功，然後按一下 [關閉]。  
  
5.  在您要做為叢集成員的所有伺服器上重複此程式。  
  
## <a name="BKMK_Links"></a>另請參閱  
  
-   [步驟 3：設定負載平衡叢集 @ no__t-0  
  


