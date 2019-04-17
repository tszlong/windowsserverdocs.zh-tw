---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: "提供您的 Active Directory 使用者存取權的應用程式與其他公司的服務"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d50d26c5c654e5c91b82f6f209e21f257221c12d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>提供您的 Active Directory 使用者存取權的應用程式與其他公司的服務

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

這個 Active Directory 同盟服務 \(AD FS\) 部署目標組建上的目標[提供您 Active Directory 使用者存取您宣告感知應用程式與服務](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)。  
  
當您是系統管理員 account 合作夥伴組織和您有提供員工聯盟的存取部署目標裝載另一個組織中的資源：  
  
-   員工的公司網路 Active Directory domain 登入可用來存取多個 Web\ 為基礎的應用程式或服務、在另一家中的應用程式或服務時，所受到 AD FS，single\ sign\ 上 \(SSO\) 功能。 如需詳細資訊，請查看[的聯盟網路 SSO 設計](Federated-Web-SSO-Design.md)。  
  
    例如，Fabrikam 可能會想公司網路員工有聯盟裝載中 Contoso Web 服務的存取。  
  
-   Active Directory domain 登入遠端員工可從聯盟伺服器聯盟 AD FS – 保護 Web\ 型應用程式或其他組織裝載的服務存取您在組織中取得權杖 AD FS。  
  
    例如，Fabrikam 可能會想有同盟服務的存取 AD FS – 保護裝載中 Contoso，而不需要將 Fabrikam 公司網路上的 Fabrikam 員工其遠端員工。  
  
除了中所述的基礎元件[提供您 Active Directory 使用者存取您宣告感知應用程式與服務](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)，將會變暗下圖，下列元件所需的此部署目標：  
  
-   **考慮合作夥伴聯盟伺服器 proxy:**員工可從網際網路存取同盟的服務或應用程式可以使用此 AD FS 元件進行驗證。 根據預設，這元件執行表單驗證，但它也可以執行基本驗證。 您也可以設定此元件執行安全通訊端層 \(SSL\) client 驗證，如果在您的組織員工呈現的憑證。 如需詳細資訊，請查看[放置聯盟 Proxy 伺服器](Where-to-Place-a-Federation-Server-Proxy.md)。  
  
-   **周邊 DNS:**這個實作網域名稱系統 \(DNS\) 提供主機周邊網路的名稱。 如需有關如何周邊 DNS 聯盟 proxy 伺服器設定的詳細資訊，請查看[聯盟的 Proxy 伺服器的名稱解析需求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  
-   **遠端員工：**員工遠端存取 Web\ 為基礎的應用程式 \（透過支援 Web browser\) 或 Web\ 服務 \（透過 application\)，使用有效的憑證，從公司網路時，員工時離站使用網際網路。 在遠端位置員工的 client 電腦會直接與聯盟伺服器 proxy 產生預付碼和驗證的應用程式或服務通訊。  
  
檢視後連結主題中的資訊，就可以開始中的步驟來部署這個目標[檢查清單︰ 實作聯盟網路 SSO 設計](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)。  
  
下圖顯示每個此 AD FS 部署目標的必要元件。  
  
![存取您的應用程式](media/50af4837-31e0-451f-a942-e705c2300065.gif)  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
