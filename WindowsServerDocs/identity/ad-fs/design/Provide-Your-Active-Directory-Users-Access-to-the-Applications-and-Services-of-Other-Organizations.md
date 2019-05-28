---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: 為 Active Directory 使用者提供其他組織的應用程式與服務的存取權
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fc0cbb461a48d04ddaa677d4de2369ef58fd5390
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190965"
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>為 Active Directory 使用者提供其他組織的應用程式與服務的存取權

這個 Active Directory Federation Services \(AD FS\)中的目標為基礎的部署目標[提供 Your Active Directory Users Access to Your Claims-aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)。  
  
當您在帳戶夥伴組織中身為系統管理員，且部署目標是為員工提供同盟存取，以存取其他組織內裝載的資源時：  
  
-   登入公司網路中 Active Directory 網域的員工可以使用單一\-號\-上\(SSO\)功能，可存取多個 Web\-基礎應用程式或服務，其中受保護的 AD FS 中，當應用程式或服務位於不同的組織。 如需詳細資訊，請參閱 [Federated Web SSO Design](Federated-Web-SSO-Design.md)。  
  
    例如，Fabrikam 可能想要公司網路員工對於裝載於 Contoso 的 Web 服務具有同盟存取權。  
  
-   遠端登入 Active Directory 網域的員工可以從您的組織同盟 AD FS 保護的 Web 存取中的同盟伺服器取得 AD FS 權杖\-基礎應用程式或服務所裝載的另一個組織。  
  
    例如，Fabrikam 可能想要遠端員工具有同盟存取 AD FS 保護的服務裝載於 Contoso，而不要求 Fabrikam 員工位於 Fabrikam 公司網路。  
  
除了 [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md) 所述的基本元件 (它們在下圖中加上陰影來表示) 之外，這項部署目標也需要下列元件：  
  
-   **帳戶夥伴同盟伺服器 proxy:** 從網際網路存取同盟的服務或應用程式的員工可以使用此 AD FS 元件來執行驗證。 根據預設，此元件會執行表單驗證，但它也可以執行基本驗證。 您也可以設定此元件以執行安全通訊端層\(SSL\)用戶端驗證，如果貴組織的員工有憑證可呈現的話。 如需詳細資訊，請參閱 [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md)。  
  
-   **周邊網路 DNS：** 這項實作的網域名稱系統\(DNS\)提供周邊網路的主機名稱。 如需如何設定同盟伺服器 proxy 的周邊網路 DNS 的詳細資訊，請參閱[同盟伺服器 Proxy 的名稱解析需求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  
-   **遠端員工：** 遠端員工會存取 Web\-架構的應用程式\(透過支援的網頁瀏覽器\)或 Web\-型服務\(透過應用程式\)，使用有效的認證，從公司網路，而員工是公司時則使用網際網路。 在遠端位置中的員工的用戶端電腦會直接與同盟伺服器 proxy，來產生權杖，並向應用程式或服務通訊。  
  
檢閱連結主題中的資訊之後，您可以開始部署此目標中的步驟[檢查清單：實作同盟的網頁 SSO 設計](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)。  
  
下圖顯示每個此 AD FS 部署目標所需的元件。  
  
![存取您的應用程式](media/50af4837-31e0-451f-a942-e705c2300065.gif)  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
