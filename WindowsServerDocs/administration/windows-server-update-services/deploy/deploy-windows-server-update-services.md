---
title: 部署 Windows Server Update Services
description: Windows Server Update Service (WSUS) 主題的連結來完成這項工作的四個步驟的部署程序概觀
ms.prod: windows-server-threshold
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: get-started-article
ms.assetid: 2708f6b2-4252-4b8f-9b7e-84c9b4222075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51972ad352f6530c8ee2aa84aec57b62784da728
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873179"
---
# <a name="deploy-windows-server-update-services"></a>部署 Windows Server Update Services

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

Windows Server Update Services (WSUS) 能夠讓資訊技術管理員部署最新的 Microsoft 產品更新。 WSUS 是 Windows Server 伺服器角色，可以安裝以管理和散佈更新。 WSUS 伺服器也可以是組織內其他 WSUS 伺服器的更新來源。 做為更新來源的 WSUS 伺服器稱為「上游伺服器」。  

在 WSUS 實作中，網路中至少要有一部 WSUS 伺服器必須連線到 Microsoft Update，以取得可用的更新資訊。 您可以判斷，根據網路安全性和設定，多少其他伺服器直接連線到 Microsoft Update。  

本指南會提供概念性資訊，規劃和部署 Windows Server Update Service。  

-   [規劃 WSUS 部署](../plan/plan-your-wsus-deployment.md)  

-   [步驟 1：安裝 WSUS 伺服器角色](1-install-the-wsus-server-role.md)  

-   [步驟 2：將 WSUS 設定](2-configure-wsus.md)  

-   [步驟 3：核准和部署 WSUS 更新](3-approve-and-deploy-updates-in-wsus.md)  

-   [步驟 4：設定自動更新的群組原則設定](4-configure-group-policy-settings-for-automatic-updates.md)  
