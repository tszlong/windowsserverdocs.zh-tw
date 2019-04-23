---
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: 為 Active Directory 使用者提供宣告感知應用程式與服務的存取權
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f6fb37c16c20915c0051e3a24cdb0c147ae92d9c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835869"
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>為 Active Directory 使用者提供宣告感知應用程式與服務的存取權

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

當您是系統管理員可以在帳戶夥伴組織中的 Active Directory Federation Services \(AD FS\)部署，而且您有部署目標是提供單一\-號\-上\(SSO\)員工在公司網路裝載的資源的存取權：  
  
-   登入公司網路中 Active Directory 樹系的員工可以使用 SSO 存取組織內周邊網路中的多個應用程式或服務。 這些應用程式和服務都受到 AD FS。  
  
    例如，Fabrikam 可能想要具有同盟存取 Web 權限的公司網路員工\-Fabrikam 周邊網路中裝載的應用程式。  
  
-   遠端登入 Active Directory 網域的員工可以從 AD fs 的同盟存取貴組織中的同盟伺服器取得 AD FS 權杖\-保護 Web\-基礎應用程式或服務也位於您的組織。  
  
-   Active Directory 屬性存放區中的資訊可以填入員工的 AD FS 權杖。  
  
此部署目標需要下列元件：  
  
-   **Active Directory 網域服務\(AD DS\):** AD DS 包含用來產生 AD FS 權杖的員工的使用者帳戶。 會將群組成員資格和屬性等資訊視為群組宣告和自訂宣告而填入 AD FS 權杖中。  
  
    > [!NOTE]  
    > 您也可以使用輕量型目錄存取通訊協定\(LDAP\)或結構化查詢語言\(SQL\)包含適用於 AD FS 身分識別權杖的產生。  
  
-   **公司 DNS：** 這項實作的網域名稱系統\(DNS\)包含簡單的主機\(A\)資源記錄，讓內部網路用戶端可以找到帳戶同盟伺服器。 這項 DNS 實作也可以裝載公司網路所需的其他 DNS 記錄。 如需詳細資訊，請參閱 [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md)。  
  
-   **帳戶夥伴同盟伺服器：** 此同盟伺服器加入帳戶夥伴樹系的網域。 它會驗證員工使用者帳戶，並產生 AD FS 權杖。 員工的用戶端電腦執行 Windows 整合式驗證，對這個同盟伺服器，來產生 AD FS 權杖。 如需詳細資訊，請參閱＜ [Review the Role of the Federation Server in the Account Partner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)＞。  
  
    帳戶夥伴同盟伺服器可以驗證下列使用者：  
  
    -   在這個網域中具有使用者帳戶的員工  
  
    -   在這個樹系中具有使用者帳戶的員工  
  
    -   在這個樹系信任樹系中的使用者帳戶的員工\(透過兩個\-Windows 信任的方式\)  
  
-   **員工：** 員工會存取 Web\-型服務\(透過應用程式\)或 Web\-架構的應用程式\(透過支援的網頁瀏覽器\)他或她會登入公司網路。 在公司網路員工的用戶端電腦會直接與驗證的同盟伺服器通訊。  
  
檢閱連結主題中的資訊之後，您可以開始部署此目標中的步驟[檢查清單：實作同盟的網頁 SSO 設計](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)。  
  
下圖顯示每個此 AD FS 部署目標所需的元件。  
  
![存取您的宣告](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
