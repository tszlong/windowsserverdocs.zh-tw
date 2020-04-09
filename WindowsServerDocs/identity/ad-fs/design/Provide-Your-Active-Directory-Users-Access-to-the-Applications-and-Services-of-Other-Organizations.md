---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: 為 Active Directory 使用者提供其他組織的應用程式與服務的存取權
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 13b8c3650442ac85b8b248b58372448543c8ba02
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858581"
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>為 Active Directory 使用者提供其他組織的應用程式與服務的存取權

這個 Active Directory 同盟服務 \(AD FS\) 部署目標，是為了[讓您的 Active Directory 使用者能夠存取您的宣告感知應用程式和服務](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)。  
  
當您在帳戶夥伴組織中身為系統管理員，且部署目標是為員工提供同盟存取，以存取其他組織內裝載的資源時：  
  
-   當應用程式或服務位於不同的組織時，登入公司網路中 Active Directory 網域的員工可以使用 \(SSO\) 功能上的單一\-號\-來存取多個 Web\-應用程式或服務（由 AD FS 保護）。 如需詳細資訊，請參閱 [Federated Web SSO Design](Federated-Web-SSO-Design.md)。  
  
    例如，Fabrikam 可能想要公司網路員工對於裝載於 Contoso 的 Web 服務具有同盟存取權。  
  
-   登入 Active Directory 網域的遠端員工可以從您組織中的同盟伺服器取得 AD FS 權杖，以針對裝載在另一個組織中的 AD FS 保護 Web\-應用程式或服務，取得同盟存取權。  
  
    例如，Fabrikam 可能會想要讓遠端員工具備同盟存取裝載于 Contoso 的 AD FS 保護服務，而不需要 Fabrikam 員工在 Fabrikam 公司網路上。  
  
除了 [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md) 所述的基本元件 (它們在下圖中加上陰影來表示) 之外，這項部署目標也需要下列元件：  
  
-   **帳戶夥伴同盟伺服器 proxy：** 從網際網路存取同盟服務或應用程式的員工可以使用此 AD FS 元件來執行驗證。 根據預設，此元件會執行表單驗證，但它也可以執行基本驗證。 您也可以將此元件設定為執行安全通訊端層 \(SSL\) 用戶端驗證（如果您組織中的員工具有要呈現的憑證）。 如需詳細資訊，請參閱 [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md)。  
  
-   **周邊 DNS：** 此網域名稱系統 \(DNS\) 的執行會提供周邊網路的主機名稱。 如需有關如何為同盟伺服器 proxy 設定周邊 DNS 的詳細資訊，請參閱[同盟伺服器 proxy 的名稱解析需求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  
-   **遠端員工：** 遠端員工透過支援的網頁瀏覽器\) 或 Web\-型服務 \(透過應用程式\)（使用公司網路中的有效認證）來存取 Web\-應用 \(程式，而員工則使用網際網路在異地。 員工在遠端位置的用戶端電腦會直接與同盟伺服器 proxy 通訊，以產生權杖並向應用程式或服務進行驗證。  
  
檢閱連結主題中的資訊之後，您可以遵循＜ [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)＞中的步驟開始部署此目標。  
  
下圖顯示此 AD FS 部署目標的每個必要元件。  
  
![存取您的應用程式](media/50af4837-31e0-451f-a942-e705c2300065.gif)  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
