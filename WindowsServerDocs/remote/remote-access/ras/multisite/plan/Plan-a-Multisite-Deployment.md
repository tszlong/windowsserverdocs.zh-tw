---
title: 規劃多站台部署
description: 深入瞭解在多網站設定中部署 Windows Server 2016 或 Windows Server 2012 遠端存取所需的規劃步驟。
manager: brianlic
ms.topic: article
ms.assetid: 8387eabe-7363-4367-b5b1-03c67baa2933
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: a4a56e430b1b40627aaf0e387c8df8ec122b94fb
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97941634"
---
# <a name="plan-a-multisite-deployment"></a>規劃多站台部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

 Windows Server 2016、Windows Server 2012 將 DirectAccess 與路由及遠端存取服務 (RRAS) VPN 合併為單一遠端存取角色。 本總覽提供在多網站設定中部署 Windows Server 2016 或 Windows Server 2012 遠端存取所需之規劃步驟的簡介。

1.  [使用 Advanced Settings 部署單一 DirectAccess 伺服器](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831436(v=ws.11))。 此步驟包含部署單一伺服器所需的基礎結構規劃。 其中包括規劃網路和伺服器設定、憑證需求、DNS 設定、網路位置伺服器部署、DirectAccess 管理伺服器、Active Directory 設定，以及 (Gpo) 的群組原則物件。

2.  [步驟2：規劃多網站基礎結構](Step-2-Plan-the-Multisite-Infrastructure.md)。 此步驟包含 Active Directory 和 GPO 規劃，以及 DNS 設定。

3.  [步驟3：規劃多網站部署](Step-3-Plan-the-Multisite-Deployment.md)。 此步驟包含規劃憑證設定、網路位置伺服器設定、用戶端進入點設定、IPv6 首碼設定，以及選擇性的全域負載平衡設定。

> [!NOTE]
> 記錄遠端存取 advanced deployment 的規劃決策。 這個記錄可做為參與完成部署步驟的每個人的工作輔助。

完成這些規劃步驟之後，請參閱設定 [多網站部署](../configure/Configure-a-Multisite-Deployment.md)。

