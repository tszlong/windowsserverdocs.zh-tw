---
description: 深入瞭解：為您的 Active Directory 使用者提供宣告感知應用程式和服務的存取權
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: 為 Active Directory 使用者提供宣告感知應用程式與服務的存取權
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a37147b46dc0ed8a6aac7700ad3549052c90c4bb
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049416"
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>為 Active Directory 使用者提供宣告感知應用程式與服務的存取權

當您是 Active Directory 同盟服務 AD FS 部署中帳戶夥伴組織的系統管理員 \( \) ，而且您的部署目標是要為 \- \- 公司網路上的員工提供單一登入 \( SSO 存取權 \) 給您的託管資源：

-   登入公司網路中 Active Directory 樹系的員工可以使用 SSO 存取組織內周邊網路中的多個應用程式或服務。 這些應用程式和服務會受到 AD FS 的保護。

    例如，Fabrikam 可能希望公司網路員工擁有 \- 在 Fabrikam 的周邊網路中裝載的 Web 應用程式同盟存取權。

-   登入 Active Directory 網域的遠端員工可以從您組織中的同盟伺服器取得 AD FS 權杖，以取得與 \- \- 您組織中的 AD FS 安全的 Web 應用程式或服務的同盟存取權。

-   Active Directory 屬性存放區中的資訊可以填入員工的 AD FS 權杖。

此部署目標需要下列元件：

-   **Active Directory Domain Services \(AD DS \) ：** AD DS 包含用來產生 AD FS 權杖的員工使用者帳戶。 會將群組成員資格和屬性等資訊視為群組宣告和自訂宣告而填入 AD FS 權杖中。

    > [!NOTE]
    > 您也可以使用輕量型目錄存取協定 \( LDAP \) 或結構化查詢語言 (SQL) \( SQL \) 來包含 AD FS 權杖產生的身分識別。

-   **公司 DNS：** 此網域名稱系統 DNS 的執行 \( \) 包含簡單主機 \( \) 資源記錄，讓內部網路用戶端可以找到帳戶同盟伺服器。 這項 DNS 實作也可以裝載公司網路所需的其他 DNS 記錄。 如需詳細資訊，請參閱[同盟伺服器的名稱解析需求](Name-Resolution-Requirements-for-Federation-Servers.md)。

-   **帳戶夥伴同盟伺服器：** 此同盟伺服器會加入帳戶夥伴樹系中的網域。 它會驗證員工使用者帳戶，並產生 AD FS 權杖。 員工的用戶端電腦會對此同盟伺服器執行 Windows 整合式驗證，以產生 AD FS 的權杖。 如需詳細資訊，請參閱＜ [Review the Role of the Federation Server in the Account Partner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)＞。

    帳戶夥伴同盟伺服器可以驗證下列使用者：

    -   在這個網域中具有使用者帳戶的員工

    -   在這個樹系中具有使用者帳戶的員工

    -   在樹系中任何位於此樹系受此樹系信任之樹系中的員工， \( 透過兩 \- 種方式進行 Windows 信任\)

-   **員工：** 員工在 \- \( \) \- \( \) 登入公司網路時，會透過受支援的網頁瀏覽器，透過應用程式或 web 應用程式存取 web 型服務。 公司網路上的員工用戶端電腦會直接與同盟伺服器通訊以進行驗證。

檢閱連結主題中的資訊之後，您可以遵循＜ [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)＞中的步驟開始部署此目標。

下圖顯示此 AD FS 部署目標的每個必要元件。

![存取您的宣告](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
