---
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: "提供您 Active Directory 的使用者存取您的宣告感知應用程式與服務"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f6fb37c16c20915c0051e3a24cdb0c147ae92d9c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>提供您 Active Directory 的使用者存取您的宣告感知應用程式與服務

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

當您在 Active Directory 同盟服務 \(AD FS\) 部署 account 合作夥伴公司的系統管理員與您有提供您裝載資源公司網路上的員工 \(SSO\) 存取 single\ sign\ 上部署目標：  
  
-   員工登入的 Active Directory 森林中的企業網路存取多個應用程式或服務周邊網路在組織中的使用 SSO。 這些應用程式和服務會受到 AD FS。  
  
    例如，Fabrikam 可能會想公司網路員工有聯盟 Web\ 為基礎的應用程式的 Fabrikam 裝載周邊網路存取。  
  
-   Active Directory domain 登入遠端員工可從聯盟伺服器聯盟廣告 FS\ 保護 Web\ 為基礎的應用程式或服務的存放在您的組織存取您在組織中取得權杖 AD FS。  
  
-   Active Directory 屬性存放區中的資訊可擴展到員工 AD FS 發行。  
  
下列元件所需此部署目標：  
  
-   **Active Directory Domain Services \(AD DS\):** AD DS 包含員工使用建立 AD FS 權杖帳號。 填入 AD FS 權杖群組宣告及自訂宣告到資訊，例如群組成員資格和屬性。  
  
    > [!NOTE]  
    > 您也可以使用輕量型 Directory 存取通訊協定 \(LDAP\) 或結構化查詢語言 \(SQL\) 包含 AD FS 權杖代身分。  
  
-   **企業 DNS:**這個實作網域名稱系統 \(DNS\) 包含簡單主機 \(A\) 資源記錄，使內部戶端可以找出 account 聯盟伺服器。 此實作 DNS 也可能會主機其他公司網路中所需的 DNS 記錄。 如需詳細資訊，請查看[聯盟伺服器的名稱解析需求](Name-Resolution-Requirements-for-Federation-Servers.md)。  
  
-   **Account 合作夥伴聯盟 server:**這個聯盟伺服器加入網域 account 合作夥伴森林中。 它驗證員工帳號，並會產生權杖 AD FS。 員工 client 電腦執行 Windows 的整合式驗證產生 AD FS 權杖此聯盟伺服器。 如需詳細資訊，請查看[檢視聯盟伺服器 Account 合作夥伴中的角色](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)。  
  
    Account 合作夥伴聯盟伺服器可以進行下列使用者驗證：  
  
    -   員工帳號，在這個網域中  
  
    -   員工帳號此森林中的任何位置點一下  
  
    -   員工的任何位置的 forests 的帳號信任的樹系 \（透過 two\ 向 Windows trust\)  
  
-   **員工：**員工存取 Web\ 服務 \(through an application\) 或 Web\ 型應用程式 \（透過支援 Web browser\) 時他登入公司網路。 員工的公司網路上的 client 電腦會直接與驗證的聯盟伺服器通訊。  
  
檢視後連結主題中的資訊，就可以開始中的步驟來部署這個目標[檢查清單︰ 實作聯盟網路 SSO 設計](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)。  
  
下圖顯示每個此 AD FS 部署目標的必要元件。  
  
![存取您的宣告](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
