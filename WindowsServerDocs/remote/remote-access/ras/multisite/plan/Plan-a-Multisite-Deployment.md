---
title: 規劃多站台部署
description: 本主題是本指南的一部分部署多部遠端存取伺服器在 Windows Server 2016 中的多站台部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8387eabe-7363-4367-b5b1-03c67baa2933
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ba813fc5da53e9635e9ef1363f1a559a29419312
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282596"
---
# <a name="plan-a-multisite-deployment"></a>規劃多站台部署

>適用於：Windows Server （半年通道），Windows Server 2016

 Windows Server 2016、 Windows Server 2012 結合成單一的遠端存取角色 DirectAccess 與路由及遠端存取服務 (RRAS) VPN。 本概觀介紹部署 Windows Server 2016 或 Windows Server 2012 遠端存取多站台設定中所需的規劃步驟。  
  
1.  [部署單一 DirectAccess 伺服器使用進階設定](https://technet.microsoft.com/library/hh831436(v=ws.11).aspx)。 這個步驟包含規劃部署單一伺服器所需的基礎結構。 它包含規劃網路和伺服器設定、 憑證需求、 DNS 設定、 網路位置伺服器部署、 DirectAccess 管理伺服器、 Active Directory 設定，以及群組原則物件 (Gpo)。  
  
2.  [步驟 2 中規劃多站台的基礎結構](Step-2-Plan-the-Multisite-Infrastructure.md)。 這個步驟包括 Active Directory 和 GPO 的規劃，以及 DNS 設定。  
  
3.  [步驟 3 中規劃多站台部署](Step-3-Plan-the-Multisite-Deployment.md)。 這個步驟包含規劃憑證設定、 網路位置伺服器組態、 用戶端的進入點設定、 IPv6 首碼的設定及選擇性地全域負載平衡設定。  
  
> [!NOTE]  
> 記錄遠端存取進階部署規劃決策。 這個記錄可做為參與完成部署步驟的每個人的工作輔助。  
  
您已完成這些規劃步驟之後，請參閱[設定多站台部署](../configure/Configure-a-Multisite-Deployment.md)。  
  


