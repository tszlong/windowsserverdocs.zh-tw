---
title: 設定多站台部署
description: 本主題是本指南的一部分部署多部遠端存取伺服器在 Windows Server 2016 中的多站台部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb84920e-7cf5-4266-b071-d09e3d5e1f10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b602855db271348ac48ee0a5691424a7321c7370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849749"
---
# <a name="configure-a-multisite-deployment"></a>設定多站台部署

>適用於：Windows Server （半年通道），Windows Server 2016

 Windows Server 2016 會將 DirectAccess 和遠端存取服務 (RAS) VPN 合併成一個遠端存取角色。 這個概觀提供部署單一的 Windows Server 2016 或 Windows Server 2012 遠端存取多站台部署所需的設定步驟的簡介。  
  
-   步驟 1：[部署單一 DirectAccess 伺服器使用進階設定](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings)。 安裝和設定單一遠端存取伺服器。 多站台的部署需要您設定站台部署前請先安裝在單一伺服器。  
  
-   [步驟 2：設定多站台的基礎結構](Step-2-Configure-the-Multisite-Infrastructure.md)。 多站台部署中，您必須設定其他 Active Directory 站台和網域控制站。 其他安全性的群組和群組原則物件 (Gpo) 也需要。 如果您不想要使用自動設定的 Gpo  
  
-   [步驟 3：設定多站台部署](Step-3-Configure-the-Multisite-Deployment.md)-額外的遠端存取伺服器上安裝遠端存取角色，啟用多站台部署中，與設定額外的伺服器做為部署的進入點。  
  
-   [步驟 4：請確認多站台部署](Step-4-Verify-the-Multisite-Deployment.md) 
  


