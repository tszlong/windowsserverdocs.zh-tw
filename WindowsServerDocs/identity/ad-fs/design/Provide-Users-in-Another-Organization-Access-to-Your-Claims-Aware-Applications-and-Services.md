---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: 為其他組織的使用者提供您的宣告感知應用程式與服務的存取權
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2e47197a980c9bcb576d6634a0031a8ae13afbfd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858601"
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>為其他組織的使用者提供您的宣告感知應用程式與服務的存取權


當您是 Active Directory 同盟服務 \(\) AD FS 的資源夥伴組織中的系統管理員，而且您的部署目標是要為另一個組織中的使用者提供同盟存取權 \(帳戶夥伴組織\) 到您組織中的宣告\-感知應用程式或 Web\-服務（\(資源夥伴組織\)）：  
  
-   組織中和已設定同盟信任給組織的組織 \(帳戶夥伴組織\) 可以存取組織所裝載的 AD FS 保護應用程式或服務。 如需詳細資訊，請參閱 [Federated Web SSO Design](Federated-Web-SSO-Design.md)。  
  
    例如，Fabrikam 可能想要其公司網路員工對於裝載於 Contoso 的 Web 服務具有同盟存取權。  
  
-   與受信任的組織沒有直接關聯的同盟使用者 \(例如登入您周邊網路中裝載之屬性存放區的個別客戶\)，可以透過從位於網際網路的用戶端電腦登入一次，存取多個 AD FS\-受保護的應用程式（也裝載在您的周邊網路中）。 換句話說，當您裝載客戶帳戶以存取周邊網路中的應用程式或服務時，您在屬性存放區中裝載的客戶只要登入一次，就可以存取周邊網路中的一或多個應用程式或服務。 如需詳細資訊，請參閱 [Web SSO Design](Web-SSO-Design.md)。  
  
    例如，Fabrikam 可能會想要讓客戶在 \(SSO 上擁有單一\-登\-，\) 存取裝載在其周邊網路中的多個應用程式或服務。  
  
此部署目標需要下列元件：  
  
-   **Active Directory Domain Services \(AD DS\)：** 資源夥伴同盟伺服器必須加入 Active Directory 網域。  
  
-   **周邊 DNS：** 網域名稱系統 \(DNS\) 應該包含簡單的主機 \(\) 資源記錄，讓用戶端電腦可以找到資源夥伴同盟伺服器和 Web 服務器。 DNS 伺服器可以裝載周邊網路也需要的其他 DNS 記錄。 如需詳細資訊，請參閱[同盟伺服器的名稱解析需求](Name-Resolution-Requirements-for-Federation-Servers.md)。  
  
-   **資源夥伴同盟伺服器：** 資源夥伴同盟伺服器會驗證帳戶夥伴所傳送 AD FS 權杖。 帳戶夥伴探索是透過此同盟伺服器執行。 如需詳細資訊，請參閱 [Review the Role of the Federation Server in the Resource Partner](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)。  
  
-   **Web 伺服器：** Web 伺服器可以裝載 Web 應用程式或 Web 服務。 Web 伺服器會先確認它從同盟使用者接收有效的 AD FS 權杖，才允許存取受保護的 Web 應用程式或 Web 服務。  
  
    藉由使用 Windows Identity Foundation \(WIF\)，您可以開發 Web 應用程式或服務，讓它接受使用任何標準登入方法（例如使用者名稱和密碼）所提出的同盟使用者登入要求。  
  
在查閱連結主題中的資訊之後，您可以遵循[檢查清單：執行同盟網頁 Sso 設計](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)和[檢查清單：執行網頁 sso 設計](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md)中的步驟來開始部署此目標。  
  
下圖顯示此 AD FS 部署目標的每個必要元件。  
  
![存取您的宣告](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
