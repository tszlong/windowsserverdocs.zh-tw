---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: 為其他組織的使用者提供您的宣告感知應用程式與服務的存取權
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4a13332cd7cf6361824f05ead4568a45211cc70a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191024"
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>為其他組織的使用者提供您的宣告感知應用程式與服務的存取權


在 Active Directory Federation Services 資源夥伴組織中的系統管理員時\(AD FS\) ，而且您有部署目標是另一個組織中的使用者提供同盟的存取\(帳戶夥伴組織\)宣告\-感知應用程式或 Web\-型服務，它位於您的組織\(資源夥伴組織\):  
  
-   同盟組織中和在組織中人員已設定同盟信任您組織的使用者\(帳戶夥伴組織\)可以存取 AD FS 保護應用程式或服務所裝載的程式組織。 如需詳細資訊，請參閱 [Federated Web SSO Design](Federated-Web-SSO-Design.md)。  
  
    例如，Fabrikam 可能想要其公司網路員工對於裝載於 Contoso 的 Web 服務具有同盟存取權。  
  
-   同盟與受信任的組織沒有直接關聯的使用者\(例如個別客戶\)，裝載在您的周邊網路中的屬性存放區登入可以存取多個 AD FS\-受保護應用程式，也裝載在您的周邊網路中的登入一次從用戶端電腦位於網際網路上。 換句話說，當您裝載客戶帳戶以存取周邊網路中的應用程式或服務時，您在屬性存放區中裝載的客戶只要登入一次，就可以存取周邊網路中的一或多個應用程式或服務。 如需詳細資訊，請參閱 [Web SSO Design](Web-SSO-Design.md)。  
  
    例如，Fabrikam 可能想要客戶利用單一\-號\-上\(SSO\)存取多個應用程式或在其周邊網路中裝載的服務。  
  
此部署目標需要下列元件：  
  
-   **Active Directory 網域服務\(AD DS\):** 資源夥伴同盟伺服器必須加入 Active Directory 網域。  
  
-   **周邊網路 DNS：** 網域名稱系統\(DNS\)應該包含簡單的主機\(A\)資源記錄，以便用戶端電腦可以找到資源夥伴同盟伺服器和 Web 伺服器。 DNS 伺服器可以裝載周邊網路也需要的其他 DNS 記錄。 如需詳細資訊，請參閱 [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md)。  
  
-   **資源夥伴同盟伺服器：** 資源夥伴同盟伺服器會驗證帳戶夥伴傳送的 AD FS 權杖。 帳戶夥伴探索是透過此同盟伺服器執行。 如需詳細資訊，請參閱 [Review the Role of the Federation Server in the Resource Partner](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)。  
  
-   **Web 伺服器：** Web 伺服器可以裝載 Web 應用程式或 Web 服務。 Web 伺服器會先確認它從同盟使用者接收有效的 AD FS 權杖，才允許存取受保護的 Web 應用程式或 Web 服務。  
  
    使用 Windows Identity Foundation \(WIF\)，您可以開發 Web 應用程式或服務，使其接受同盟使用者登入要求的任何標準登入方法，例如使用者名稱和密碼。  
  
檢閱連結主題中的資訊之後，您可以開始部署此目標中的步驟[檢查清單：實作同盟的網頁 SSO 設計](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)和[檢查清單：實作網頁 SSO 設計](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md)。  
  
下圖顯示每個此 AD FS 部署目標所需的元件。  
  
![存取您的宣告](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
