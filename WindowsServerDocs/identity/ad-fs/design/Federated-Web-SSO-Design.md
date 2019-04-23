---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: 同盟網頁 SSO 設計
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b85f49ac0556bf9b3542a23514d7fcbf82d2d88e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865139"
---
# <a name="federated-web-sso-design"></a>同盟網頁 SSO 設計

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

同盟網頁單一\-登\-上\(SSO\) Active Directory Federation Services 中的設計\(AD FS\)牽涉到安全的通訊，跨越多個防火牆、 周邊網路和名稱\-解析伺服器 — 除了整個網際網路路由基礎結構。  
  
一般而言，這種設計可在兩個組織同意建立同盟信任關係，以允許在一個組織中的使用者\(帳戶夥伴組織\)來存取 Web\-基礎應用程式或服務其中受保護的其他組織中的 AD FS\(資源夥伴組織\)。  
  
換句話說，同盟信任關係是企業的體現\-層級協議或兩個組織之間的合作關係。 下圖所示，您可以建立兩家公司，這會產生結束之間的同盟信任關係\-至\-端同盟案例。  
  
![同盟的網頁 sso](media/adfs2_FederatedWebSSODesign.gif)  
  
一個\-向箭號，在圖中表示的同盟信任 」，這 — 像是 Windows 信任方向 — 一律指向樹系的帳戶端。 這表示從帳戶夥伴組織到資源夥伴組織的驗證流程。  
  
在此同盟網頁 SSO 設計中，兩部同盟伺服器\(Fabrikam 和 Contoso 中另一個\)路由驗證要求，從使用者帳戶的 Fabrikam web\-基礎應用程式或 Contoso 中的服務。  
  
> [!NOTE]  
> 為了增加安全性，您可以使用同盟伺服器 proxy 將要求轉送至無法直接從網際網路存取的同盟伺服器。  
  
在此範例中，Fabrikam 是身分識別或帳戶提供者。 同盟網頁 SSO 設計的 Fabrikam 部分會使用下列的 AD FS 部署目標：  
  
-   [您 Active Directory 使用者提供存取權的應用程式和其他組織的服務](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso 是資源提供者。 同盟網頁 SSO 設計的 Contoso 部分會達成下列的 AD FS 部署目標：  
  
-   [提供使用者在另一個組織存取您宣告感知應用程式和服務](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [提供您的 Active Directory 使用者存取您的宣告感知應用程式和服務](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
如需您可用來規劃和部署同盟網頁 SSO 設計的詳細工作的清單，請參閱[檢查清單：實作同盟的網頁 SSO 設計](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
