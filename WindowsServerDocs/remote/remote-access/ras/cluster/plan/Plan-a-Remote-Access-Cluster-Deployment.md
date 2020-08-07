---
title: 規劃遠端存取叢集部署
description: 本主題是在 Windows Server 2016 的叢集中部署遠端存取指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: 88ffd598-2fde-402c-bd12-be790f84dc96
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d3765ce86bcb5552188b135c293e5eea24b0547e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963844"
---
# <a name="plan-a-remote-access-cluster-deployment"></a>規劃遠端存取叢集部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

 Windows Server 2016 和 Windows Server 2012 將 DirectAccess 與遠端存取服務 (RAS) VPN 合併成一個遠端存取角色。 本總覽提供部署 Windows Server 2016 或 Windows Server 2012 遠端存取服務器叢集所需之規劃步驟的簡介。

-   [規劃先進的 DirectAccess 部署](../../../directaccess/single-server-advanced/Plan-an-Advanced-DirectAccess-Deployment.md)。 此步驟包含規劃部署單一伺服器所需的基礎結構。 其中包括規劃網路和伺服器設定、憑證需求、DNS 設定、網路位置伺服器部署、DirectAccess 管理伺服器、Active Directory 設定，以及 (Gpo) 群組原則物件。

-   [步驟2：規劃叢集伺服器](Step-2-Plan-Cluster-Servers.md)。

-   [步驟3：規劃負載平衡的叢集部署](Step-3-Plan-a-Load-Balanced-Cluster-Deployment.md)。

-   步驟4：記錄遠端存取先進部署的規劃決策。 這個記錄可做為參與完成部署步驟的每個人的工作輔助。

完成這些規劃步驟之後，請參閱[設定遠端存取](../configure/Configure-a-Remote-Access-Cluster.md)叢集。

如需在實驗室環境中將叢集部署設定為概念證明的指示，請參閱[測試實驗室指南-示範使用 WINDOWS NLB 的](../../../directaccess/tlg-cluster-nlb/Test-Lab-Guide-Demonstrate-DirectAccess-in-a-Cluster-with-Windows-NLB.md)叢集中的 DirectAccess。



