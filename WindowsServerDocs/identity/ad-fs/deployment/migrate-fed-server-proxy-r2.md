---
title: 移轉 AD FS 2.0 同盟 proxy 伺服器
description: 提供移轉到 Windows Server 2012 R2 的 AD FS proxy 伺服器的相關資訊。
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a2eb6c670e704564bed49486b8950dab96da8a80
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444576"
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>移轉到 Windows Server 2012 R2 的 Active Directory Federation Services Proxy 伺服器

在 Active Directory Federation Services (AD FS) 在 Windows Server 2012 R2 中，同盟伺服器 proxy 的角色會處理由稱為 Web Application Proxy 的新遠端存取角色服務。 在 Windows Server 2012 R2，若要啟用從公司網路外部存取 AD FS 您可以部署一或多個 Web 應用程式 Proxy。 不過，您無法移轉 Windows Server 2008 R2 或 Windows Server 2012 上執行 Windows Server 2012 R2 上執行的 Web 應用程式 proxy 的同盟伺服器 proxy。  
  
> [!IMPORTANT]
>  不支援執行 Windows Server 2008、 Windows Server 2008 R2 或 Windows Server 2012 上執行 Windows Server 2012 R2 上的 Web 應用程式 Proxy 的同盟伺服器 proxy 移轉。  
  
如果您想要設定 AD FS 外部網路存取的 Windows Server 2012 R2 移轉伺服器陣列中，您必須執行一或多個 Web Application Proxy 電腦重新部署，為您的 AD FS 基礎結構的一部分。  
  
若要規劃 Web Application Proxy部署，您可以檢閱下列主題的資訊：  
  
- [規劃 Web Application Proxy 基礎結構](https://technet.microsoft.com/library/dn383648.aspx)  
  
- [規劃 Web Application Proxy 伺服器](https://technet.microsoft.com/library/dn383647.aspx)  
  
  若要部署 Web Application Proxy，您可以依照下列主題的程序：  
  
- [設定 Web Application Proxy 基礎結構](https://technet.microsoft.com/library/dn383644.aspx)  
  
- [安裝和設定 Web Application Proxy 伺服器](https://technet.microsoft.com/library/dn383662.aspx)  
  
## <a name="next-steps"></a>後續步驟
 [將 Active Directory Federation Services 角色服務移轉到 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [準備移轉 AD FS 同盟伺服器](prepare-migrate-ad-fs-server-r2.md)   
 [移轉 AD FS 同盟伺服器](migrate-ad-fs-fed-server-r2.md)    
 [驗證 AD FS 移轉到 Windows Server 2012 R2](verify-ad-fs-migration.md)

