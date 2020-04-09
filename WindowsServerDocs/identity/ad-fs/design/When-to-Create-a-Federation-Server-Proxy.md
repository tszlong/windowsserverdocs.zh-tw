---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: 建立同盟伺服器 Proxy 的時機
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 13ca7d49e8044273c9f40e58c06e705e758c68bf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858481"
---
# <a name="when-to-create-a-federation-server-proxy"></a>建立同盟伺服器 Proxy 的時機

在您的組織中建立同盟伺服器 proxy，會在您的 Active Directory 同盟服務 \(AD FS\) 部署中新增額外的安全性層級。 當您想要執行下列動作時，請考慮在組織的周邊網路中部署同盟伺服器 proxy：  
  
-   防止外部用戶端電腦直接存取您的同盟伺服器。 藉由在周邊網路中部署同盟伺服器 proxy，您可以有效地隔離您的同盟伺服器，使其只能由透過同盟伺服器 proxy 登入公司網路的用戶端電腦存取，這會代表外部用戶端電腦執行動作。 同盟伺服器 Proxy 無法存取用來產生權杖的私密金鑰。 如需詳細資訊，請參閱 [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md)。  
  
-   提供便利的方式來區分來自網際網路\-的使用者體驗，而不是使用 Windows 整合式驗證來自公司網路的使用者。 同盟伺服器 proxy 會使用登入、登出和識別提供者探索 \(homerealmdiscovery 儲存在同盟伺服器 proxy 上的\) 頁面，從網際網路用戶端電腦收集認證或主領域詳細資料。  
  
    相較之下，來自公司網路的用戶端電腦會根據同盟伺服器的設定，遇到不同的體驗。 公司網路同盟伺服器通常是針對 Windows 整合式驗證而設定的，可為公司網路上的使用者提供順暢的登入\-。  
  
同盟伺服器 proxy 在組織中扮演的角色，取決於您是將同盟伺服器 proxy 放在帳戶夥伴組織中或資源夥伴組織中。 例如，當同盟伺服器 proxy 放在帳戶夥伴的周邊網路時，其角色就是從瀏覽器用戶端收集使用者認證資訊。 當同盟伺服器 proxy 放在資源夥伴的周邊網路時，它會將安全性權杖要求轉送到資源同盟伺服器，並產生組織的安全性權杖以回應其帳戶夥伴所提供的安全性權杖。  
  
如需詳細資訊，請參閱 [Review the Role of the Federation Server Proxy in the Account Partner](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md) 和 [Review the Role of the Federation Server Proxy in the Resource Partner](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
## <a name="how-to-create-a-federation-server-proxy"></a>如何建立同盟伺服器 Proxy  
您可以使用 AD FS 同盟伺服器 Proxy 設定向導 或 Fsconfig.exe 命令\-行工具來建立同盟伺服器 proxy。 如需有關如何執行這項操作的指示，請參閱 [Configure a Computer for the Federation Server Proxy Role](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。  
  
如需如何設定部署同盟伺服器 Proxy 所需之所有必要項目的一般資訊，請參閱 [Checklist: Setting Up a Federation Server Proxy](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
