---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: 建立同盟伺服器 Proxy 的時機
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b41c2194940c85e39e5a3724f747dd12c2544259
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190642"
---
# <a name="when-to-create-a-federation-server-proxy"></a>建立同盟伺服器 Proxy 的時機

建立您組織中的同盟伺服器 proxy 將額外的安全性層級加入至您的 Active Directory Federation Services \(AD FS\)部署。 請考慮部署同盟伺服器 proxy 在貴組織的周邊網路中，當您想要：  
  
-   防止外部用戶端電腦直接存取您的同盟伺服器。 藉由部署同盟伺服器 proxy 在周邊網路，可以有效隔離您的同盟伺服器，以便僅供登入同盟伺服器 proxy，代表透過公司網路的用戶端電腦可以存取它們外部的用戶端電腦。 同盟伺服器 Proxy 無法存取用來產生權杖的私密金鑰。 如需詳細資訊，請參閱 [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md)。  
  
-   提供便利的方式來區分登\-來自網際網路，相對於來自您公司的網路使用 Windows 整合式驗證的使用者的使用者體驗中。 同盟伺服器 proxy 的認證或主領域的詳細資料從網際網路用戶端電腦透過探索收集的登入、 登出和身分識別提供者\(homerealmdiscovery.aspx\)會儲存在同盟網頁，伺服器 proxy。  
  
    相反地，來自公司網路發生不同的體驗，用戶端電腦為基礎的同盟伺服器設定。 公司網路同盟伺服器通常已針對 Windows 整合式驗證，可提供無縫式登\-在公司網路上的使用者體驗。  
  
同盟伺服器 proxy 在貴組織中所扮演的角色，取決於是否您放置同盟伺服器 proxy 帳戶夥伴組織中或資源夥伴組織中。 比方說，在帳戶夥伴的周邊網路中放置同盟伺服器 proxy，其角色是從瀏覽器用戶端收集使用者認證資訊。 資源夥伴的周邊網路中放置同盟伺服器 proxy，它會轉送安全性權杖要求資源同盟伺服器，並產生組織的安全性權杖中所提供的安全性權杖的回應其帳戶夥伴。  
  
如需詳細資訊，請參閱 [Review the Role of the Federation Server Proxy in the Account Partner](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md) 和 [Review the Role of the Federation Server Proxy in the Resource Partner](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
## <a name="how-to-create-a-federation-server-proxy"></a>如何建立同盟伺服器 Proxy  
您可以建立同盟伺服器 proxy 使用 AD FS 同盟伺服器 Proxy 設定精靈或 Fsconfig.exe 命令\-列工具。 如需有關如何執行這項操作的指示，請參閱 [Configure a Computer for the Federation Server Proxy Role](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。  
  
如需如何設定所有必要條件部署同盟伺服器 proxy 所需的一般資訊，請參閱[檢查清單：設定同盟伺服器 Proxy](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
