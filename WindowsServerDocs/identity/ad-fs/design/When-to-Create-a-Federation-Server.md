---
ms.assetid: 824005ae-c3c1-459b-9baa-1660158918ab
title: "建立聯盟伺服器的時機"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8013764b88a1061cfcaa3a507466c111bfd59aad
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="when-to-create-a-federation-server"></a>建立聯盟伺服器的時機

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

當您建立聯盟 serverin Active Directory 同盟服務 \(AD FS\) 時，您提供一種方法可以您的組織：  
  
-   交戰 single\ sign\ 在網頁中 \ (SSO\) – 根據通訊與其他公司 \ （也有一個以上的聯盟 server\） 和必要時，使用您的組織中的員工 \ （人員將需要透過 Internet\ 存取）。  
  
-   讓模擬基礎結構服務使用的身分委派使用者前端服務。 如需詳細資訊，請查看[何時要使用的身分委派](When-to-Use-Identity-Delegation.md)。  
  
下列章節描述一些重要決策判斷時，並建立一個或多個聯盟伺服器的位置。  
  
## <a name="determine-the-organizational-role-for-the-federation-server"></a>判斷組織聯盟伺服器角色  
要充分有關何時建立新的聯盟伺服器，您必須先判斷組織的伺服器會位於。 聯盟伺服器播放是在組織中的角色是否放置聯盟伺服器 account 合作夥伴公司或資源合作夥伴組織中而定。  
  
當聯盟伺服器位於 account 合作夥伴的企業網路時，其的角色是驗證的瀏覽器、 Web 服務或身分選取器戶端使用者認證，並傳送安全性權杖給戶端。 如需詳細資訊，請查看[檢視聯盟伺服器 Account 合作夥伴中的角色](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)。  
  
聯盟伺服器置於企業網路資源協力廠商，當的角色是驗證使用者，根據發出聯盟伺服器資源合作夥伴組織中的安全性權杖或的角色是重新導向權杖要求 account 合作夥伴公司 client 屬於設定的 Web 應用程式或 Web 服務。 如需詳細資訊，請查看[檢視的資源合作夥伴聯盟伺服器角色](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)。  
  
## <a name="determine-which-ad-fs-design-to-deploy"></a>判斷 AD FS 設計部署  
建立聯盟伺服器您在組織中每當您想要部署的下列 AD FS 設計的任何：  
  
-   [Web SSO 設計](Web-SSO-Design.md)  
  
-   [聯盟的網路 SSO 設計](Federated-Web-SSO-Design.md)  
  
如有需要，部署的聯盟網路 SSO 設計組織可以設定單一聯盟伺服器，使其做 account 合作夥伴角色與資源合作夥伴角色中。 在這種情形下，聯盟伺服器可能會根據其組織中帳號安全性判斷提示標記語言 \(SAML\) 權杖，或變更路徑權杖要求組織，根據使用者帳號所在的位置。  
  
> [!NOTE]  
> 聯盟網路 SSO 設計，必須在 account 合作夥伴至少一個聯盟伺服器和資源合作夥伴至少一個聯盟伺服器。  
  
## <a name="differences-between-a-federation-server-and-a-federation-server-proxy"></a>聯盟伺服器與聯盟 proxy 伺服器不同  
聯盟伺服器可以查看網頁 sign\ 中原則、 驗證和探索做聯盟 proxy 伺服器會相同的方式。 聯盟伺服器及聯盟 proxy 伺服器主要不同可以執行的作業聯盟伺服器可以執行的聯盟 proxy 伺服器無法執行。  
  
以下是聯盟伺服器可以執行的作業：  
  
-   聯盟伺服器執行權杖密碼編譯作業。 雖然聯盟的 proxy 伺服器無法產生權杖，他們可以用於路由或重新導向權杖給戶端，必要時，回聯盟伺服器。 如需有關使用聯盟伺服器的資訊，請查看[當建立聯盟 Proxy 伺服器](When-to-Create-a-Federation-Server-Proxy.md)。  
  
-   聯盟伺服器支援使用 Windows 整合驗證的企業網路; 戶端聯盟伺服器 proxy 不執行動作。 如需關於 Windows 的整合式驗證使用聯盟伺服器的資訊，請查看[當建立聯盟伺服器陣列](When-to-Create-a-Federation-Server-Farm.md)。  
  
> [!CAUTION]  
> 聯盟伺服器及 SQL Server 設定資料庫、 SQL Server 屬性存放區，網域控制站與廣告 LDS 執行個體之間的通訊不完整性或機密性預設保護。 若要減少此問題，請考慮保護這些伺服器使用 IPSEC 或使用這些伺服器的所有之間的實體安全連接間通訊通道。 聯盟伺服器 SQL 伺服器間通訊，請考慮使用 SSL 保護連接字串。 網域控制站伺服器聯盟之間的連接，請考慮將在 Kerberos 簽署及加密。 適用於 LDAP，LDAP\ S/不支援的廣告 LDS\ 日 AD DS。  
  
## <a name="how-to-create-a-federation-server"></a>如何建立聯盟伺服器  
您可以建立聯盟伺服器使用 AD FS 聯盟伺服器設定精靈或 Fsconfig.exe command\ 列工具。 當您使用這些工具時，您可以選取下列其中一個選項來建立聯盟伺服器任何。  
  
-   建立 stand\ 只聯盟伺服器  
  
    如需了解如何設定 stand\ 只聯盟伺服器的資訊，請查看[建立獨立聯盟伺服器](../../ad-fs/deployment/Create-a-Stand-Alone-Federation-Server.md)。  
  
-   建立的第一個聯盟伺服器聯盟伺服器陣列  
  
    如需了解如何設定的第一個聯盟伺服器，或新增至陣列聯盟伺服器的資訊，請查看[第一個聯盟伺服器建立聯盟伺服器陣列](../../ad-fs/deployment/Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)。  
  
-   新增至聯盟伺服器陣列聯盟伺服器  
  
    如需了解如何新增至陣列聯盟伺服器的資訊，請查看[新增聯盟伺服器聯盟伺服器陣列到](../../ad-fs/deployment/Add-a-Federation-Server-to-a-Federation-Server-Farm.md)。  
  
如需詳細資訊每個選項的工作方式時，請查看[的角色 AD FS 設定資料庫的](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
如需了解如何設定所有必要條件部署聯盟伺服器所需的詳細資訊，請查看[檢查清單︰ 設定好聯盟伺服器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

