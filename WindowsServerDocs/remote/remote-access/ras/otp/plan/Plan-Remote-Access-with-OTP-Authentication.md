---
title: 使用 OTP 驗證規劃遠端存取
description: 本主題是在 Windows Server 2016 中使用 OTP 驗證部署遠端存取指南的一部分。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 762bc463-eead-46ac-8b90-32355743c27c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c268b52adddc7aa36c855e7bffa265eaa8dbe2c8
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86964340"
---
# <a name="plan-remote-access-with-otp-authentication"></a>使用 OTP 驗證規劃遠端存取

>適用於：Windows Server (半年度管道)、Windows Server 2016

 Windows Server 2016 和 Windows Server 2012 將 DirectAccess 與路由及遠端存取服務（RRAS） VPN 合併成一個遠端存取角色。 本總覽提供部署單一 Windows Server 2016 或 Windows Server 2012 遠端存取多網站部署所需之設定步驟的簡介。  
  
  
-  步驟1：[使用 [高級設定] 部署單一 DirectAccess 伺服器](../../../directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings.md)。 此步驟包含規劃部署單一伺服器所需的基礎結構。 其中包含規劃網路和伺服器設定、憑證需求、DNS 設定、網路位置伺服器部署、DirectAccess 管理伺服器、Active Directory 設定，以及群組原則物件（Gpo）。  
  
-   [步驟2：規劃 RADIUS 伺服器部署](Step-2-Plan-the-RADIUS-Server-Deployment.md)  
  
-   [步驟3：規劃 OTP 憑證部署](Step-3-Plan-OTP-Certificate-Deployment.md)  
  
-   [步驟4：在遠端存取服務器上規劃 OTP](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  
完成這些規劃步驟之後，請參閱[使用 OTP 驗證設定遠端存取](../configure/configure-ra-with-otp-authentication.md)。 如需在實驗室環境中將多網站部署設定為概念證明的詳細資訊，請參閱[測試實驗室指南：示範使用 OTP 驗證和 RSA SecurID 的 DirectAccess](../../../directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid.md)。  
  
