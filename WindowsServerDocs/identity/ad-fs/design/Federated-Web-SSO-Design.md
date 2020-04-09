---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: 同盟網頁 SSO 設計
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9915a2942c9336d5aeb7776169d2e51491c22909
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853141"
---
# <a name="federated-web-sso-design"></a>同盟網頁 SSO 設計

在 Active Directory 同盟服務 \(AD FS\) 中，\(SSO\) 設計上的同盟 Web 單一\-登入\-牽涉到跨多個防火牆、周邊網路和名稱\-解析伺服器的安全通訊，以及整個網際網路路由基礎結構。  
  
一般而言，當兩個組織同意建立同盟信任關係，讓某個組織中的使用者 \(帳戶夥伴組織\) 存取由資源夥伴組織 \(之其他組織中的 Web\-應用程式或服務（由 AD FS 保護）時，就會使用此設計。\)  
  
換句話說，同盟信任關係是企業\-等級協定或兩個組織之間合作關係的體現。 如下圖所示，您可以建立兩個企業之間的同盟信任關係，這會導致結束\-\-的「結束同盟」案例。  
  
![同盟網頁 sso](media/adfs2_FederatedWebSSODesign.gif)  
  
圖例中的一個\-方式箭號表示同盟信任的方向，也就是 Windows 信任的方向，一律指向樹系的帳戶端。 這表示驗證流程會從帳戶夥伴組織進行至資源夥伴組織。  
  
在此同盟網頁 SSO 設計中，兩部同盟伺服器 \(Fabrikam 的其中一個，而 Contoso 中的另一個則\) 將來自 Fabrikam 使用者帳戶的驗證要求路由傳送至 Contoso 中以 Web\-為基礎的應用程式或服務。  
  
> [!NOTE]  
> 為了增加安全性，您可以使用同盟伺服器 proxy 將要求轉送至無法直接從網際網路存取的同盟伺服器。  
  
在此範例中，Fabrikam 是身分識別或帳戶提供者。 同盟網頁 SSO 設計的 Fabrikam 部分會使用下列 AD FS 部署目標：  
  
-   [為 Active Directory 使用者提供其他組織的應用程式與服務的存取權](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso 是資源提供者。 同盟網頁 SSO 設計的 Contoso 部分會達到下列 AD FS 部署目標：  
  
-   [為其他組織的使用者提供您的宣告感知應用程式與服務的存取權](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [為 Active Directory 使用者提供宣告感知應用程式與服務的存取權](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
如需可用來規劃和部署同盟網頁 SSO 設計的詳細工作清單，請參閱＜ [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)＞。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
