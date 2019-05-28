---
ms.assetid: 8c3536b7-d091-4ee6-ad04-24713f070862
title: 在帳戶夥伴組織中部署 AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 93f61bc7fd147b2e0220178bcd163b6ca56279cf
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191575"
---
# <a name="deploying-ad-fs-in-the-account-partner-organization"></a>在帳戶夥伴組織中部署 AD FS

在 Active Directory 同盟服務帳戶夥伴\(AD FS\)代表實際儲存在支援的屬性存放區中的 使用者帳戶的同盟信任關係中的組織。 支援的屬性存放區的詳細資訊，請參閱[角色的屬性儲存](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)。  
  
帳戶夥伴組織中的同盟伺服器會驗證本機使用者，並建立安全性權杖，可供進行授權決策的資源夥伴。 信賴憑證者的合作對象，例如網站和 Web 服務便能輕鬆地自行註冊與同盟伺服器，並將進行驗證和存取控制發出的權杖。  
  
在您要提供多個同盟應用程式或服務的存取權給使用者的情況下 — 每個應用程式或服務不同的組織所裝載時 — 您可以設定帳戶夥伴同盟伺服器，以便您可以部署多個信賴憑證者合作對象。  
  
如需如何安裝和設定帳戶夥伴組織的詳細資訊，請參閱[檢查清單：設定帳戶夥伴組織](../../ad-fs/deployment/Checklist--Configuring-the-Account-Partner-Organization.md)。  
  
## <a name="in-this-section"></a>本節內容  
  
-   [檢閱帳戶夥伴中的同盟伺服器角色](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md)  
  
-   [檢閱帳戶夥伴中的同盟伺服器 Proxy 的角色](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)  
  
-   [準備帳戶夥伴中的用戶端電腦](Prepare-Client-Computers-in-the-Account-Partner.md)  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
