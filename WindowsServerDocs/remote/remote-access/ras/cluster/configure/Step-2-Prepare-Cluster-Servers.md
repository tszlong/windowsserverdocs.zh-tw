---
title: 步驟2準備叢集伺服器
description: 瞭解如何準備要新增至叢集的其他伺服器。
manager: brianlic
ms.topic: article
ms.assetid: 35d68abb-6914-42e0-91e8-803933cf785e
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 09c2a1a05d6800a3497bc6b8b15536eb73466770
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947384"
---
# <a name="step-2-prepare-cluster-servers"></a>步驟2準備叢集伺服器

>適用於：Windows Server (半年度管道)、Windows Server 2016

在您可以設定叢集部署之前，請先準備要新增至叢集的其他伺服器。

|Task|描述|
|----|--------|
|[2.1 設定遠端存取基礎結構](#BKMK_config)|在您想要新增到叢集的每部伺服器上，設定伺服器拓撲、IP 定址、路由和轉送。 如果您設定虛擬機器的負載平衡叢集，則必須將虛擬機器設定為使用 MAC 位址詐騙。<p>此外，請將每部伺服器聯結至相同的網域，並將所有伺服器連接到相同的子網。|
|[2.2 安裝遠端存取角色](#BKMK_Install)|在您想要新增到叢集的每部其他伺服器上，安裝遠端存取角色|
|[2.3 安裝 NLB](#BKMK_NLB)|在已部署的遠端存取服務器上，以及您想要新增至叢集的每部其他伺服器上，安裝 NLB 功能。 請注意，使用外部 Load Balancer 時，不需要執行此步驟。|

## <a name="21-configure-the-remote-access-infrastructure"></a><a name="BKMK_config"></a>2.1 設定遠端存取基礎結構
若要設定遠端存取叢集，您必須在將構成叢集一部分的每部伺服器上設定伺服器拓撲、IP 定址、路由和轉送。

### <a name="to-configure-the-remote-access-infrastructure"></a>設定遠端存取基礎結構

1.  使用與第一部遠端存取服務器相同的拓撲，設定將在叢集中的每部伺服器。

2.  根據第一部遠端存取服務器的設定，設定將在叢集中的每部伺服器，並具有適當的 IP 位址、路由和轉送。 請注意，叢集中的所有伺服器都必須連接到相同的子網。

3.  將叢集中的每部伺服器聯結到與第一個遠端存取服務器相同的網域。

## <a name="22-install-the-remote-access-role"></a><a name="BKMK_Install"></a>2.2 安裝遠端存取角色
若要設定遠端存取叢集，您必須在將構成叢集一部分的每部伺服器上安裝遠端存取角色。

### <a name="to-install-the-remote-access-role-on-always-on-vpn-servers"></a>在 Always On VPN 伺服器上安裝遠端存取角色

1.  在 DirectAccess 伺服器的伺服器管理員主控台中，按一下 [ **儀表板**] 中的 [ **新增角色及功能**]。

2.  按 [下一步] 3 次，進入伺服器角色選擇畫面。

3.  在 [ **選取伺服器角色** ] 對話方塊中，選取 [ **遠端存取**]，然後按 **[下一步]**。

4.  按三次 **[下一步]** 。

5.  在 [ **選取角色服務** ] 對話方塊中，選取 [ **DirectAccess 和 VPN] (RAS)** 然後按一下 [ **新增功能**]。

6.  依序選取 [ **路由**]、[ **Web 應用程式 Proxy**]、[ **新增功能** **] 和 [下一步]**。

7. 按 [下一步]，然後按一下 [安裝]。

8.  在 [安裝進度] 對話方塊中，確認安裝成功，然後按一下 [關閉]。

9.  在您想要成為叢集成員的所有伺服器上重複此程式。

## <a name="23-install-nlb"></a><a name="BKMK_NLB"></a>2.3 安裝 NLB
若要設定遠端存取叢集，您必須在將構成叢集一部分的每部伺服器上安裝網路負載平衡功能。

> [!NOTE]
> 如果使用外部負載平衡器，則不需要此步驟。

#### <a name="to-install-the-nlb-role"></a>若要安裝 NLB 角色

1.  在 DirectAccess 伺服器的伺服器管理員主控台中，按一下 [ **儀表板**] 中的 [ **新增角色及功能**]。

2.  按 **[下一步]** 四次以進入伺服器功能選取畫面。

3.  在 [**選取功能**] 對話方塊中，依序選取 [**網路負載平衡**]、[**新增功能**]、 **[下一步] 及 [****安裝**]。

4.  在 [安裝進度] 對話方塊中，確認安裝成功，然後按一下 [關閉]。

5.  在您想要成為叢集成員的所有伺服器上重複此程式。

## <a name="see-also"></a><a name="BKMK_Links"></a>請參閱

-   [步驟3：設定負載平衡叢集](Step-3-Configure-a-Load-Balanced-Cluster.md)



