---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: "在另一部組織存取的使用者提供您宣告感知應用程式與服務"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b4ec08182e2523b0fcb16088ec9c1d094a5923fe
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>在另一部組織存取的使用者提供您宣告感知應用程式與服務

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

當系統管理員身分在 Active Directory 同盟服務 \(AD FS\) 資源合作夥伴組織中的與您有提供其他組織中的使用者存取聯盟的部署目標 \ (account 合作夥伴 organization\) claims\ 感知應用程式，或位於您在組織中的 Web\ 服務 \ (資源合作夥伴 organization\):  
  
-   聯盟的使用者在組織中與在組織中設定聯盟人到您的組織信任 \ (account 合作夥伴 organizations\) 可以存取 AD FS 受保護的應用程式或服務是由您的組織裝載。 如需詳細資訊，請查看[的聯盟網路 SSO 設計](Federated-Web-SSO-Design.md)。  
  
    例如，Fabrikam 可能會想其公司網路員工有聯盟裝載中 Contoso Web 服務的存取。  
  
-   聯盟直接遵守受信任的組織使用者 \（例如個人 customers\) 人員登入的屬性存放區位於周邊網路，可以存取多 AD FS\ 保護的應用程式，也會在周邊網路上裝載藉由從網際網路上的 client 電腦一次登入。 亦即，當您裝載客戶帳號，可讓應用程式或服務周邊網路存取權，針對您的主機屬性市集中可以存取一或多個應用程式或服務的周邊網路只要一次登入。 如需詳細資訊，請查看[網站 SSO 設計](Web-SSO-Design.md)。  
  
    例如，Fabrikam 可能會想上市 single\ sign\ 上 \(SSO\) 存取多個應用程式或其周邊網路的服務。  
  
下列元件所需此部署目標：  
  
-   **Active Directory Domain Services \(AD DS\):** Active Directory domain 必須加入資源合作夥伴聯盟伺服器。  
  
-   **周邊 DNS:**網域名稱系統 \(DNS\) 應該資料簡單主機 \(A\) 資源，以便資源合作夥伴聯盟伺服器與 Web 伺服器，可以找出 client 的電腦。 DNS 伺服器可能主機其他 DNS 記錄也所需的周邊網路。 如需詳細資訊，請查看[聯盟伺服器的名稱解析需求](Name-Resolution-Requirements-for-Federation-Servers.md)。  
  
-   **資源合作夥伴聯盟 server:**資源合作夥伴聯盟伺服器驗證 account 合作夥伴傳送給 AD FS 權杖。 透過此聯盟伺服器執行 account 合作夥伴探索。 如需詳細資訊，請查看[檢視的資源合作夥伴聯盟伺服器角色](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)。  
  
-   **Web server:**的網頁伺服器可裝載 Web 應用程式或 Web 服務。 Web 伺服器確認它收到來自聯盟使用者有效 AD FS 發行之前就可讓存取受保護的 Web 應用程式或 Web 服務。  
  
    使用 Windows 的身分基本知識 \(WIF\)，您可以開發 Web 應用程式或服務，讓它接受聯盟的使用者登入要求所做的任何標準登入方式，例如使用者名稱和密碼。  
  
檢視後連結主題中的資訊，就可以開始中的步驟來部署這個目標[檢查清單：實作聯盟網路 SSO 設計](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)和[檢查清單︰ 實作 Web SSO 設計](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md)。  
  
下圖顯示每個此 AD FS 部署目標的必要元件。  
  
![存取您的宣告](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
