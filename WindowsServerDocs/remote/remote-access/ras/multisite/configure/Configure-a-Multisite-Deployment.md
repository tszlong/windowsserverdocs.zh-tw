---
title: 設定多站台部署
description: 瞭解部署單一 Windows Server 2016 或 Windows Server 2012 遠端存取多網站部署所需的設定步驟。
manager: brianlic
ms.topic: article
ms.assetid: cb84920e-7cf5-4266-b071-d09e3d5e1f10
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 20a475b7dd48a20f216173ee5a6b4e07dad6d3c3
ms.sourcegitcommit: 7674bbe49517bbfe0e2c00160e08240b60329fd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/20/2021
ms.locfileid: "98603384"
---
# <a name="configure-a-multisite-deployment"></a>設定多站台部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

 Windows Server 2016 將 DirectAccess 與遠端存取服務 (RAS) VPN 合併為單一遠端存取角色。 本總覽提供部署單一 Windows Server 2016 或 Windows Server 2012 遠端存取多網站部署所需的設定步驟簡介。

-   步驟1： [部署具有 Advanced Settings 的單一 DirectAccess 伺服器](../../../directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings.md)。 安裝和設定單一遠端存取服務器。 多網站部署需要您先安裝單一伺服器，才能設定多網站部署。

-   [步驟2：設定多網站基礎結構](Step-2-Configure-the-Multisite-Infrastructure.md)。 針對多網站部署，您必須設定額外的 Active Directory 網站和網域控制站。 如果您未使用自動設定的 Gpo，也需要其他安全性群組和群組原則物件 (Gpo) 。

-   [步驟3：設定多網站部署](Step-3-Configure-the-Multisite-Deployment.md)-在其他遠端存取服務器上安裝遠端存取角色、啟用多網站部署，以及將額外的伺服器設定為部署的進入點。

-   [步驟4：驗證多網站部署](Step-4-Verify-the-Multisite-Deployment.md)

