---
title: 規劃遠端存取叢集部署
description: 本主題是在 Windows Server 2016 的叢集中部署遠端存取指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: 88ffd598-2fde-402c-bd12-be790f84dc96
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 38769a52823932bfa9fc8bd8f84c7bc0a05b915d
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947764"
---
# <a name="plan-a-remote-access-cluster-deployment"></a>規劃遠端存取叢集部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

 Windows Server 2016 和 Windows Server 2012 將 DirectAccess 與遠端存取服務 (RAS) VPN 合併為單一遠端存取角色。 本總覽提供部署 Windows Server 2016 或 Windows Server 2012 遠端存取服務器叢集所需之規劃步驟的簡介。

-   [規劃 Advanced DirectAccess 部署](../../../directaccess/single-server-advanced/Plan-an-Advanced-DirectAccess-Deployment.md)。 此步驟包含部署單一伺服器所需的基礎結構規劃。 其中包括規劃網路和伺服器設定、憑證需求、DNS 設定、網路位置伺服器部署、DirectAccess 管理伺服器、Active Directory 設定，以及 (Gpo) 的群組原則物件。

-   [步驟2：規劃叢集伺服器](Step-2-Plan-Cluster-Servers.md) 。

-   [步驟3：規劃 Load-Balanced 叢集部署](Step-3-Plan-a-Load-Balanced-Cluster-Deployment.md)。

-   步驟4：記錄遠端存取 advanced deployment 的規劃決策。 這個記錄可做為參與完成部署步驟的每個人的工作輔助。

完成這些規劃步驟之後，請參閱 [設定遠端存取](../configure/Configure-a-Remote-Access-Cluster.md)叢集。

如需在實驗室環境中將叢集部署設定為概念證明的指示，請參閱 [測試實驗室指南-在具有 WINDOWS NLB](../../../directaccess/tlg-cluster-nlb/Test-Lab-Guide-Demonstrate-DirectAccess-in-a-Cluster-with-Windows-NLB.md)的叢集中示範 DirectAccess。



