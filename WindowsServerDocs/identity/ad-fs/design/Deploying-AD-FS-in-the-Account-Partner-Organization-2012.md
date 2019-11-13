---
ms.assetid: 9aaca9c5-ce44-495c-aad6-61aede87a83f
title: 在帳戶夥伴組織中部署 AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 62d7549f124b96cd7addf7e54cc5d0c1d9897098
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408121"
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>在帳戶夥伴組織中部署 AD FS

Active Directory 同盟服務 \(AD FS\) 中的帳戶夥伴代表在支援的屬性存放區中實際儲存使用者帳戶的同盟信任關係中的組織。 如需有關支援哪些屬性存放區的詳細資訊，請參閱[屬性存放區的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)。  
  
帳戶夥伴組織中的同盟伺服器會驗證本機使用者，並建立資源夥伴用來進行授權決策的安全性權杖。 Web sites 和 Web 服務之類的信賴憑證者就能夠輕鬆地自行向同盟伺服器註冊，並使用發行的權杖進行驗證和存取控制。  
  
在您需要為使用者提供多個同盟應用程式或服務的存取權的情況下（當每個應用程式或服務由不同的組織所裝載）時，您可以設定帳戶夥伴同盟伺服器，讓您可以部署多個信賴憑證者。  
  
如需有關如何安裝和設定帳戶夥伴組織的詳細資訊，請參閱 [Checklist: Configuring the Account Partner Organization](../../ad-fs/deployment/Checklist--Configuring-the-Account-Partner-Organization.md)。  
  
## <a name="in-this-section"></a>本節內容  
  
-   [檢閱帳戶夥伴中的同盟伺服器角色](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)  
  
-   [檢閱帳戶夥伴中的同盟伺服器 Proxy 的角色](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)  
  
-   [在帳戶夥伴中準備用戶端電腦](Prepare-Client-Computers-in-the-Account-Partner.md)  
  
## <a name="see-also"></a>請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
