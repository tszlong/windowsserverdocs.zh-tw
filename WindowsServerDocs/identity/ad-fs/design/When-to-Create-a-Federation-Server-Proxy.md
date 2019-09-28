---
ms.assetid: aa20c8b3-7f01-4165-8b73-92642bff9676
title: 建立同盟伺服器 Proxy 的時機
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f3325efe7acf8b0b0469489e8d9a42614a5af54a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358855"
---
# <a name="when-to-create-a-federation-server-proxy"></a>建立同盟伺服器 Proxy 的時機

在您的組織中建立同盟伺服器 proxy，會在您的 Active Directory 同盟服務 @no__t 0AD FS @ no__t-1 部署中新增額外的安全性階層。 當您想要執行下列動作時，請考慮在組織的周邊網路中部署同盟伺服器 proxy：  
  
-   防止外部用戶端電腦直接存取您的同盟伺服器。 藉由在周邊網路中部署同盟伺服器 proxy，您可以有效地隔離您的同盟伺服器，使其只能由透過同盟伺服器 proxy 登入公司網路的用戶端電腦存取，這會代表使用者採取行動的外部用戶端電腦。 同盟伺服器 Proxy 無法存取用來產生權杖的私密金鑰。 如需詳細資訊，請參閱 [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md)。  
  
-   為來自網際網路的使用者，提供便利的方式來區分正負號 @ no__t-0in 體驗，而不是使用 Windows 整合式驗證來自公司網路的使用者。 同盟伺服器 proxy 會使用登入、登出和識別提供者探索 \(homerealmdiscovery 儲存在同盟伺服器 proxy 上的 no__t-1 頁面，從網際網路用戶端電腦收集認證或主領域詳細資料。  
  
    相較之下，來自公司網路的用戶端電腦會根據同盟伺服器的設定，遇到不同的體驗。 公司網路同盟伺服器通常設定為使用 Windows 整合式驗證，為公司網路上的使用者提供順暢的 @ no__t 0in 體驗。  
  
同盟伺服器 proxy 在組織中扮演的角色，取決於您是將同盟伺服器 proxy 放在帳戶夥伴組織中或資源夥伴組織中。 例如，當同盟伺服器 proxy 放在帳戶夥伴的周邊網路時，其角色就是從瀏覽器用戶端收集使用者認證資訊。 當同盟伺服器 proxy 放在資源夥伴的周邊網路時，它會將安全性權杖要求轉送到資源同盟伺服器，並產生組織的安全性權杖以回應其所提供的安全性權杖帳戶夥伴。  
  
如需詳細資訊，請參閱 <<c0> [ 檢閱帳戶夥伴中的同盟伺服器 proxy 角色](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Account-Partner.md)和[檢閱資源夥伴中的同盟伺服器 proxy 角色](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
## <a name="how-to-create-a-federation-server-proxy"></a>如何建立同盟伺服器 Proxy  
您可以使用 AD FS 同盟伺服器 Proxy 設定 Wizard 或 Fsconfig.exe 命令 @ no__t-0line 工具來建立同盟伺服器 proxy。 如需有關如何執行這項操作的指示，請參閱[同盟伺服器 Proxy 角色設定的電腦](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。  
  
如需如何設定部署同盟伺服器 proxy 所需之所有必要條件的一般資訊，請參閱 @no__t 0Checklist：設定同盟伺服器 Proxy @ no__t-0。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
