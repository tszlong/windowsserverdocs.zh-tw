---
description: 深入瞭解：同盟網頁 SSO 設計
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: 同盟網頁 SSO 設計
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 3551ab3d14b479750d5338580aa1c43660da1623
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047716"
---
# <a name="federated-web-sso-design"></a>同盟網頁 SSO 設計

Active Directory 同盟服務 AD FS 中的同盟 Web 單一 \- 登錄 \- \( SSO \) 設計 \( \) 牽涉到跨多個防火牆、周邊網路和名稱解析伺服器的安全通訊，以及 \- 整個網際網路路由基礎結構。

一般來說，當兩個組織同意建立同盟信任關係時，會使用這項設計，讓某個組織中的使用者可以使用 \( 帳戶夥伴組織 \) 來存取 \- 其他組織中的 Web 應用程式或服務，這些應用程式或服務是由 \( 資源夥伴組織的其他組織中的 AD FS 保護 \) 。

換句話說，同盟信任關係是業務 \- 等級協定的 embodiment 或兩個組織之間的合作關係。 如下圖所示，您可以建立兩個企業之間的同盟信任關係，以產生端 \- 對 \- 端同盟案例。

![同盟網頁 sso](media/adfs2_FederatedWebSSODesign.gif)

\-圖中的單向箭號表示同盟信任的方向（如同 Windows 信任的方向）一律指向樹系的帳戶端。 這表示從帳戶夥伴組織到資源夥伴組織的驗證流程。

在此同盟網頁 SSO 設計中，Fabrikam 中的兩部同盟伺服器，以及 Contoso 中的另一部同盟伺服器，會 \( \) 將來自 fabrikam 的使用者帳戶的驗證要求路由至 \- Contoso 中的 Web 應用程式或服務。

> [!NOTE]
> 為了提高安全性，您可以使用同盟伺服器 proxy 將要求轉送到無法從網際網路直接存取的同盟伺服器。

在此範例中，Fabrikam 是身分識別或帳戶提供者。 聯合網頁 SSO 設計的 Fabrikam 部分使用下列 AD FS 部署目標：

-   [為 Active Directory 使用者提供其他組織的應用程式與服務的存取權](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)

Contoso 是資源提供者。 同盟網頁 SSO 設計的 Contoso 部分可達成下列 AD FS 部署目標：

-   [為其他組織的使用者提供您的宣告感知應用程式與服務的存取權](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)

-   [為 Active Directory 使用者提供宣告感知應用程式與服務的存取權](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)

如需可用來規劃和部署同盟網頁 SSO 設計的詳細工作清單，請參閱＜ [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)＞。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
