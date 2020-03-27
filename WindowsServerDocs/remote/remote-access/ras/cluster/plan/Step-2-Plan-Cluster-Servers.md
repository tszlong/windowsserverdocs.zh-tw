---
title: 步驟2規劃叢集伺服器
description: 本主題是在 Windows Server 2016 的叢集中部署遠端存取指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 673c5bfb-b590-4932-8e54-ca0a466d90cc
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 608bc4b2805639e2638ac12f74b712c812ce165f
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308240"
---
# <a name="step-2-plan-cluster-servers"></a>步驟2規劃叢集伺服器

>適用於：Windows Server (半年通道)、Windows Server 2016

部署單一遠端存取服務器之後，請規劃新增其他伺服器到叢集。  
  
|工作|描述|  
|----|--------|  
|[2.1 安裝角色和功能](#BKMK_Install)。|針對要新增到叢集的每部伺服器，規劃遠端存取角色和 Windows NLB 功能的安裝（如有需要），規劃拓撲、IP 位址、路由和轉送。|  
|[2.2 設定伺服器設定](#BKMK_Config)|針對將新增到叢集的每部伺服器進行設定。 請注意，您可以使用虛擬機器設定伺服器的負載平衡叢集。 為了讓路由和連線正常運作，您必須將虛擬機器設定為使用 MAC 位址詐騙。|  
  
## <a name="21-installing-roles-and-features"></a><a name="BKMK_Install"></a>2.1 安裝角色和功能  
針對您想要加入叢集的每部伺服器，規劃安裝遠端存取角色。 此外，如果您想要使用 Windows NLB 將流量負載平衡至叢集，請規劃安裝網路負載平衡（NLB）功能。 如需詳細資訊，請參閱[網路負載平衡](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing)。  
  
## <a name="22-configure-server-settings"></a><a name="BKMK_Config"></a>2.2 設定伺服器設定  
針對將新增到叢集的每部伺服器，規劃 IP 位址和網域設定。 請注意下列事項：  
  
1.  叢集中的伺服器必須全都屬於相同的網域。  
  
2.  叢集中的伺服器必須位於相同的子網上。  
  
3.  叢集中的每部伺服器都應該有相同數目的網路介面卡，以用於 DirectAccess 部署。  
  
當您使用 Windows NLB 對叢集進行負載平衡時，會套用下列 Windows NLB 設定：  
  
1.  作業模式-單播。 您可以使用 NLB 管理員將此變更為多播。 此設定無法在 [遠端存取管理] 主控台中修改。  
  
2.  負載權數因數-定義為 [等於]，其中所有的叢集伺服器都有相等的負載。  
  
3.  篩選模式-流量會在多部主機間進行負載平衡。  
  
4.  親和性-已定義單一親和性。  
  
5.  通訊協定-兩者  

