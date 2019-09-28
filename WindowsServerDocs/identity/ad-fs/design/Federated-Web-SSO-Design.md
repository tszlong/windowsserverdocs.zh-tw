---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: 同盟網頁 SSO 設計
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6a3e7eb6c42c8190da799c88c1e947e6aef1c29f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408108"
---
# <a name="federated-web-sso-design"></a>同盟網頁 SSO 設計

同盟 Web Single @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3 Active Directory 同盟服務 \(AD FS @ no__t-5 的設計牽涉到跨多個防火牆、周邊網路和名稱 @ no__t-6resolution 的安全通訊。伺服器—除了整個網際網路路由基礎結構外。  
  
一般而言，當兩個組織同意建立同盟信任關係，以允許某個組織中的使用者 @no__t 0the 帳戶夥伴組織 @ no__t-1 來存取 Web @ no__t 2based 應用程式或服務（受到下列保護）時，會使用此設計AD FS，在其他組織中 @no__t 3the 資源夥伴組織 @ no__t-4。  
  
換句話說，同盟信任關係是商務 @ no__t-0level 合約的體現，或是兩個組織之間的合作關係。 如下圖所示，您可以建立兩個企業之間的同盟信任關係，這會產生 end @ no__t-0to @ no__t-1end 同盟案例。  
  
![同盟網頁 sso](media/adfs2_FederatedWebSSODesign.gif)  
  
圖例中的一個 @ no__t-0way 箭號表示同盟信任的方向，也就是 Windows 信任的方向，一律指向樹系的帳戶端。 這表示從帳戶夥伴組織到資源夥伴組織的驗證流程。  
  
在此同盟網頁 SSO 設計中，Fabrikam 中 @no__t 0one 兩部同盟伺服器，另一個在 Contoso @ no__t-1 會將驗證要求從 Fabrikam 中的使用者帳戶路由至 Web @ no__t-2based 應用程式或 Contoso 中的服務。  
  
> [!NOTE]  
> 為了增加安全性，您可以使用同盟伺服器 proxy 將要求轉送至無法直接從網際網路存取的同盟伺服器。  
  
在此範例中，Fabrikam 是身分識別或帳戶提供者。 同盟網頁 SSO 設計的 Fabrikam 部分會使用下列 AD FS 部署目標：  
  
-   [為 Active Directory 使用者提供其他組織的應用程式與服務的存取權](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso 是資源提供者。 同盟網頁 SSO 設計的 Contoso 部分會達到下列 AD FS 部署目標：  
  
-   [為其他組織的使用者提供您的宣告感知應用程式與服務的存取權](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [為 Active Directory 使用者提供宣告感知應用程式與服務的存取權](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
如需您可以用來規劃及部署同盟網頁 SSO 設計的詳細工作清單，請參閱 @no__t 0Checklist：執行同盟網頁 SSO 設計 @ no__t-0。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
