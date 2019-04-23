---
title: 步驟 2 計劃叢集伺服器
description: 本主題是部署在 Windows Server 2016 的叢集中的遠端存取快速入門的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 673c5bfb-b590-4932-8e54-ca0a466d90cc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1f0ab7c1d888466ed575ab7cc2ed204db9a02542
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854409"
---
# <a name="step-2-plan-cluster-servers"></a>步驟 2 計劃叢集伺服器

>適用於：Windows Server （半年通道），Windows Server 2016

部署單一遠端存取伺服器之後, 打算在叢集中加入其他伺服器。  
  
|工作|描述|  
|----|--------|  
|[2.1 安裝角色和功能](#BKMK_Install)。|每個伺服器，將會新增至叢集，計劃安裝遠端存取角色以及 Windows NLB 功能 （如有需要），規劃拓樸、 IP 位址、 路由和轉送。|  
|[2.2 設定伺服器設定](#BKMK_Config)|設定會新增至叢集的每一部伺服器的設定。 請注意，您可以設定負載平衡叢集中使用虛擬機器的伺服器。 為了讓路由與連線才能正常運作，您必須設定虛擬機器，若要使用 MAC 位址詐騙。|  
  
## <a name="BKMK_Install"></a>2.1 安裝角色和功能  
每個伺服器，您想要加入叢集，計劃安裝遠端存取角色。 除了計劃安裝網路負載平衡 (NLB) 功能，如果您想要使用 Windows NLB 叢集平衡流量負載。 如需詳細資訊，請參閱[網路負載平衡](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing)。  
  
## <a name="BKMK_Config"></a>2.2 設定伺服器設定  
每個伺服器，將會新增至叢集，規劃 IP 位址和網域設定。 請注意下列事項：  
  
1.  在叢集中的伺服器必須全部屬於相同的網域。  
  
2.  在叢集中的伺服器必須位於相同的子網路。  
  
3.  在叢集中的每一部伺服器為 DirectAccess 部署的使用中應有相同數目的網路介面卡。  
  
當您進行負載平衡使用 Windows NLB 叢集會套用以下的 Windows NLB 設定：  
  
1.  作業模式單點傳播。 這可以變更為使用 NLB 管理員的多點傳送。 在 遠端存取管理主控台中，無法修改此設定。  
  
2.  負載的權數因素定義為相等的其中所有叢集伺服器都有相等的負載。  
  
3.  篩選模式流量將會是負載平衡跨多部主機。  
  
4.  親和性單一親和性會定義。  
  
5.  這通訊協定  

