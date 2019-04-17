---
title: "移轉 AD FS 2.0 聯盟 proxy 伺服器"
description: "AD FS proxy 伺服器移轉到 Windows Server 2012 R2 提供相關資訊。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 33ab29fd5efdb0bdd1fe25580e3f4434071e1c7d
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/12/2017
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>Active Directory 同盟服務 Proxy 伺服器移轉到 Windows Server 2012 R2

在 Active Directory 同盟 Services (AD FS) 在 Windows Server 2012 R2，聯盟 proxy 伺服器的角色被處理呼叫 Web 應用程式 Proxy 新遠端存取的角色服務。 在 Windows Server 2012 R2，可讓您的企業網路，以外的協助工具的 AD FS 您可以部署一或多個 Web 應用程式 Proxy。 不過，您無法移轉聯盟伺服器 proxy 至 Web 應用程式 Proxy Windows Server 2012 R2 上執行 Windows Server 2008 R2 或 Windows Server 2012 上執行。  
  
> [!IMPORTANT]
>  不支援執行 Windows Server 2008、Windows Server 2008 R2 或 Windows Server 2012 上執行 Windows Server 2012 R2 Web 應用程式 Proxy 聯盟伺服器 proxy 移轉。  
  
如果您想要在 Windows Server 2012 R2 移轉發電廠外部網路存取設定 AD FS，您必須執行全新的一或多個 Web 應用程式 Proxy 電腦部署 AD FS 基礎結構的一部分。  
  
若要規劃 Web 應用程式 Proxy 部署，您可以檢視的下列主題中的資訊：  
  
-   [Web 應用程式 Proxy 基礎結構計劃](https://technet.microsoft.com/en-us/library/dn383648.aspx)  
  
-   [計畫網站的應用程式的 Proxy 伺服器](https://technet.microsoft.com/en-us/library/dn383647.aspx)  
  
 若要部署的應用程式網路 proxy，您可以依照下列主題中的程序：  
  
-   [設定應用程式網路 Proxy 架構](https://technet.microsoft.com/en-us/library/dn383644.aspx)  
  
-   [安裝並將 Web 應用程式的 Proxy 伺服器設定](https://technet.microsoft.com/en-us/library/dn383662.aspx)  
  
## <a name="next-steps"></a>後續步驟
 [Active Directory 同盟服務角色服務移轉到 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [正在準備移轉 AD FS 聯盟伺服器](prepare-migrate-ad-fs-server-r2.md)   
 [移轉 AD FS 聯盟伺服器](migrate-ad-fs-fed-server-r2.md)    
 [檢查 AD FS 移轉到 Windows Server 2012 R2](verify-ad-fs-migration.md)

