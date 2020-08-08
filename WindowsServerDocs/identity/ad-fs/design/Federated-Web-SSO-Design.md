---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: 同盟網頁 SSO 設計
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: f3f34aca7f7a316ff714b88209e3bf81de420ecb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87942762"
---
# <a name="federated-web-sso-design"></a>同盟網頁 SSO 設計

Active Directory 同盟服務 AD FS 中的同盟 Web 單一 \- 登錄 \- \( SSO \) 設計 \( \) 牽涉到跨多個防火牆、周邊網路和名稱解析伺服器的安全通訊，以及 \- 整個網際網路路由基礎結構。

一般而言，當兩個組織同意建立同盟信任關係，以允許某個組織中 \( 的使用者帳戶夥伴組織 \) 存取 \- 其他組織資源夥伴組織的 Web 應用程式或服務（由 AD FS 保護）時，就會使用此設計 \( \) 。

換句話說，同盟信任關係是企業 \- 層級協定或兩個組織之間合作關係的體現。 如下圖所示，您可以建立兩個企業之間的同盟信任關係，這會導致端 \- 對 \- 端同盟案例。

![同盟網頁 sso](media/adfs2_FederatedWebSSODesign.gif)

\-下圖中的單向箭號表示同盟信任的方向，這就像 Windows 信任的方向，一律指向樹系的帳戶端。 這表示從帳戶夥伴組織到資源夥伴組織的驗證流程。

在此同盟網頁 SSO 設計中，Fabrikam 中有兩部同盟伺服器， \( 而另一個在 contoso 中，將 \) 驗證要求從 fabrikam 中的使用者帳戶路由傳送至以 Web 為 \- 基礎的應用程式或 contoso 中的服務。

> [!NOTE]
> 為了增加安全性，您可以使用同盟伺服器 proxy 將要求轉送至無法直接從網際網路存取的同盟伺服器。

在此範例中，Fabrikam 是身分識別或帳戶提供者。 同盟網頁 SSO 設計的 Fabrikam 部分會使用下列 AD FS 部署目標：

-   [為 Active Directory 使用者提供其他組織的應用程式與服務的存取權](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)

Contoso 是資源提供者。 同盟網頁 SSO 設計的 Contoso 部分會達到下列 AD FS 部署目標：

-   [為其他組織的使用者提供您的宣告感知應用程式與服務的存取權](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)

-   [為 Active Directory 使用者提供宣告感知應用程式與服務的存取權](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)

如需可用來規劃和部署同盟網頁 SSO 設計的詳細工作清單，請參閱＜ [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md)＞。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
