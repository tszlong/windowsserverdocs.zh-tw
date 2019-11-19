---
ms.assetid: 824005ae-c3c1-459b-9baa-1660158918ab
title: 建立同盟伺服器的時機
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 91c260dad1bd260a7dad7320fecd15e6472c50a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407888"
---
# <a name="when-to-create-a-federation-server"></a>建立同盟伺服器的時機

當您建立 Active Directory 同盟服務 \(AD FS\)的同盟 serverin 時，您會提供組織可使用的方法：  
  
-   在 Web 單一\-登入\-，與另一個組織進行 \(SSO\)型通訊，\(也至少有一部同盟伺服器\) 並在必要時，與您組織中的員工 \(需要透過網際網路\)存取。  
  
-   使用識別委派，啟用前端服務以模擬使用者執行基礎結構服務。 如需詳細資訊，請參閱 [When to Use Identity Delegation](When-to-Use-Identity-Delegation.md)。  
  
下列各節將說明決定何時及何時建立一部或多部同盟伺服器的一些重要決策。  
  
## <a name="determine-the-organizational-role-for-the-federation-server"></a>決定同盟伺服器的組織角色  
若要針對何時建立新的同盟伺服器做出明智的決策，您必須先判斷伺服器所在的組織。 同盟伺服器在組織中扮演的角色，取決於您是將同盟伺服器放置在帳戶夥伴組織中或資源夥伴組織中。  
  
當同盟伺服器放在帳戶夥伴的公司網路時，其角色就是驗證瀏覽器、Web 服務或身分識別選取器用戶端的使用者認證，並將安全性權杖傳送給用戶端。 如需詳細資訊，請參閱＜ [Review the Role of the Federation Server in the Account Partner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)＞。  
  
當同盟伺服器放在資源夥伴的公司網路時，其角色是根據資源夥伴組織中的同盟伺服器所發出的安全性權杖來驗證使用者，或其角色是重新導向權杖要求針對用戶端所屬的帳戶夥伴組織，設定 Web 應用程式或 Web 服務。 如需詳細資訊，請參閱 <<c0> [ 檢閱資源夥伴中的同盟伺服器角色](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)。  
  
## <a name="determine-which-ad-fs-design-to-deploy"></a>判斷要部署哪一種 AD FS 設計  
當您想要部署下列任何一項 AD FS 設計時，您可以在組織中建立同盟伺服器：  
  
-   [網頁 SSO 設計](Web-SSO-Design.md)  
  
-   [同盟網頁 SSO 設計](Federated-Web-SSO-Design.md)  
  
如有需要，部署同盟網頁 SSO 設計的組織可以設定單一同盟伺服器，使其同時在帳戶夥伴角色和資源夥伴角色中運作。 在此情況下，同盟伺服器可能會根據使用者帳戶所在的位置，產生安全性聲明標記語言 \(SAML\) 權杖，依據其組織中的使用者帳戶，或將權杖要求重新路由至組織。  
  
> [!NOTE]  
> 針對同盟網頁 SSO 設計，帳戶夥伴中至少要有一部同盟伺服器，而在資源夥伴中至少必須有一部同盟伺服器。  
  
## <a name="differences-between-a-federation-server-and-a-federation-server-proxy"></a>同盟伺服器與同盟伺服器 Proxy 之間的差異  
同盟伺服器可以用與同盟伺服器 proxy 相同的方式，在中提供登入\-、原則、驗證及探索的網頁。 同盟伺服器與同盟伺服器 proxy 之間的主要差異，在於同盟伺服器可執行哪些作業，同盟伺服器 proxy 無法執行。  
  
以下是只有同盟伺服器可以執行的作業：  
  
-   同盟伺服器會執行可產生權杖的密碼編譯作業。 雖然同盟伺服器 proxy 無法產生權杖，但它們可以用來將權杖路由或重新導向至用戶端，並在必要時回到同盟伺服器。 如需使用同盟伺服器的詳細資訊，請參閱[何時建立同盟伺服器 Proxy](When-to-Create-a-Federation-Server-Proxy.md)。  
  
-   同盟伺服器支援針對公司網路上的用戶端使用 Windows 整合式驗證;同盟伺服器 proxy 不是。 如需搭配使用 Windows 整合式驗證與同盟伺服器的詳細資訊，請參閱[何時建立同盟伺服器](When-to-Create-a-Federation-Server-Farm.md)陣列。  
  
> [!CAUTION]  
> 依預設，同盟伺服器和 SQL Server 組態資料庫、SQL Server 屬性存放區、網域控制站與 AD LDS 執行個體之間的通訊並不完整，或未被視為機密加以保護。 若要減輕此問題，請考慮使用 IPSEC 或在所有這些伺服器之間使用實體安全的連線，來保護這些伺服器之間的通道。 針對同盟伺服器和 SQL server 之間的通訊，請考慮在連接字串中使用 SSL 保護。 針對同盟伺服器與網域控制站之間的連接，請考慮開啟 Kerberos 簽署和加密。 針對 LDAP，AD LDS\/AD DS 不支援 LDAP\/S。  
  
## <a name="how-to-create-a-federation-server"></a>如何建立同盟伺服器  
您可以使用 AD FS 同盟伺服器設定向導 或 Fsconfig.exe 命令\-行工具來建立同盟伺服器。 使用上述任一工具時，您可以選擇下列任何選項來建立同盟伺服器。  
  
-   建立獨立的\-獨立同盟伺服器  
  
    如需如何設定獨立\-獨立同盟伺服器的詳細資訊，請參閱[建立獨立同盟伺服器](../../ad-fs/deployment/Create-a-Stand-Alone-Federation-Server.md)。  
  
-   在同盟伺服器陣列中建立第一部同盟伺服器  
  
    如需如何設定第一部同盟伺服器，或將同盟伺服器加入至伺服陣列的詳細資訊，請參閱＜ [Create the First Federation Server in a Federation Server Farm](../../ad-fs/deployment/Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)＞。  
  
-   新增同盟伺服器到同盟伺服器陣列  
  
    如需如何將同盟伺服器加入至伺服陣列的詳細資訊，請參閱＜ [Add a Federation Server to a Federation Server Farm](../../ad-fs/deployment/Add-a-Federation-Server-to-a-Federation-Server-Farm.md)＞。  
  
如需詳細的每一種選項之運作方式的相關資訊，請參閱[AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
如需如何設定所有必要的先決條件以部署同盟伺服器的詳細資訊，請參閱＜ [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)＞。  
  
## <a name="see-also"></a>請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

