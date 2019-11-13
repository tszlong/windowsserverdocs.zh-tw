---
title: 設定多站台部署
description: 本主題是在 Windows Server 2016 的多網站部署中部署多部遠端存取服務器指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb84920e-7cf5-4266-b071-d09e3d5e1f10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 25c0ce5d62268f64113ebc39345b2d50867bebf7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367117"
---
# <a name="configure-a-multisite-deployment"></a>設定多站台部署

>適用於：Windows Server (半年通道)、Windows Server 2016

 Windows Server 2016 將 DirectAccess 與遠端存取服務（RAS） VPN 結合成一個遠端存取角色。 本總覽提供部署單一 Windows Server 2016 或 Windows Server 2012 遠端存取多網站部署所需之設定步驟的簡介。  
  
-   步驟1：[使用 [高級設定] 部署單一 DirectAccess 伺服器](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings)。 安裝和設定單一遠端存取服務器。 多網站部署會要求您先安裝單一伺服器，再設定多網站部署。  
  
-   [步驟2：設定多網站基礎結構](Step-2-Configure-the-Multisite-Infrastructure.md)。 針對多網站部署，您必須設定額外的 Active Directory 網站和網域控制站。 如果您未使用自動設定的 Gpo，則也需要額外的安全性群組和群組原則物件（Gpo）。  
  
-   [步驟3：設定多網站部署](Step-3-Configure-the-Multisite-Deployment.md)-在其他遠端存取服務器上安裝遠端存取角色，啟用多網站部署，並將其他伺服器設定為部署的進入點。  
  
-   [步驟4：驗證多網站部署](Step-4-Verify-the-Multisite-Deployment.md) 
  


