---
title: 步驟2規劃叢集伺服器
description: 瞭解如何規劃將額外的伺服器新增至叢集中。
manager: brianlic
ms.topic: article
ms.assetid: 673c5bfb-b590-4932-8e54-ca0a466d90cc
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: c0ec55b17f810964408d1a033a9c45bc99a71b45
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965680"
---
# <a name="step-2-plan-cluster-servers"></a>步驟2規劃叢集伺服器

>適用於：Windows Server (半年度管道)、Windows Server 2016

部署單一遠端存取服務器之後，請規劃將額外的伺服器新增至叢集中。

|Task|描述|
|----|--------|
|[2.1 安裝角色和功能](#BKMK_Install)。|針對要新增至叢集的每部伺服器，規劃遠端存取角色的安裝和 Windows NLB 功能 (如有需要) ，請規劃拓撲、IP 定址、路由和轉送。|
|[2.2 設定伺服器設定](#BKMK_Config)|針對將新增至叢集的每部伺服器設定設定。 請注意，您可以使用虛擬機器來設定伺服器的負載平衡叢集。 為了讓路由和連接正常運作，您必須將虛擬機器設定為使用 MAC 位址詐騙。|

## <a name="21-installing-roles-and-features"></a><a name="BKMK_Install"></a>2.1 安裝角色和功能
針對您想要加入叢集的每部伺服器，規劃安裝遠端存取角色。 此外，如果您想要使用 Windows NLB 將流量負載平衡至叢集，請安裝網路負載平衡 (NLB) 功能。 如需詳細資訊，請參閱 [網路負載平衡](../../../../../networking/technologies/network-load-balancing.md)。

## <a name="22-configure-server-settings"></a><a name="BKMK_Config"></a>2.2 設定伺服器設定
針對將新增至叢集的每部伺服器，規劃 IP 位址和網域設定。 請注意：

1.  叢集中的伺服器必須全都屬於相同的網域。

2.  叢集中的伺服器必須位於相同的子網上。

3.  叢集中的每部伺服器都應使用相同數目的網路介面卡來進行 DirectAccess 部署。

當您使用 Windows NLB 對叢集進行負載平衡時，會套用下列 Windows NLB 設定：

1.  操作模式-單播。 您可以使用 NLB 管理員將此變更為多播。 無法在遠端存取管理主控台中修改此設定。

2.  負載加權因數定義為相等，其中所有的叢集伺服器都有相等的負載。

3.  篩選模式-流量會在多部主機之間進行負載平衡。

4.  已定義 Affinity-Single 親和性。

5.  Protocols-Both
