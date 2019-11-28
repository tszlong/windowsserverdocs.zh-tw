---
title: 部署 Windows Server Update Services
description: Windows Server Update Service (WSUS) 主題 - 部署程序的概觀，其中包含完成四步驟的連結
ms.prod: windows-server
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: get-started-article
ms.assetid: 2708f6b2-4252-4b8f-9b7e-84c9b4222075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e3e6bcd5f90d1a7df2a35dda45b4bf8951940815
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361684"
---
# <a name="deploy-windows-server-update-services"></a>部署 Windows Server Update Services

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Windows Server Update Services (WSUS) 能夠讓資訊技術管理員部署最新的 Microsoft 產品更新。 WSUS 是 Windows Server 伺服器角色，可以安裝以管理和散佈更新。 WSUS 伺服器也可以是組織內其他 WSUS 伺服器的更新來源。 做為更新來源的 WSUS 伺服器稱為「上游伺服器」。  

在 WSUS 實作中，網路中至少要有一部 WSUS 伺服器必須連線到 Microsoft Update，以取得可用的更新資訊。 您可以根據網路安全性和設定，決定有多少其他伺服器可以與 Microsoft Update 直接連線。  

本指南提供了在規劃和部署 Windows Server Update Service 的概念資訊。  

-   [規劃 WSUS 部署](../plan/plan-your-wsus-deployment.md)  

-   [步驟 1：安裝 WSUS 伺服器角色](1-install-the-wsus-server-role.md)  

-   [步驟 2：設定 WSUS](2-configure-wsus.md)  

-   [步驟 3：在 WSUS 中核准和部署更新](3-approve-and-deploy-updates-in-wsus.md)  

-   [步驟 4：設定自動更新的群組原則設定](4-configure-group-policy-settings-for-automatic-updates.md)  
