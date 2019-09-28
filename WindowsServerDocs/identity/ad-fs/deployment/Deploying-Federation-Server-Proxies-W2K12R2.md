---
ms.assetid: 222e9f93-7c41-4527-8a98-8f7fbc7a58af
title: 部署同盟伺服器 Proxy
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 34d2a15d5ad4f2563beffbce6ae5e729cf72c3ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359682"
---
# <a name="deploying-federation-server-proxies"></a>部署同盟伺服器 Proxy

在 Active Directory 同盟服務 @no__t-Windows Server 2012 R2 中的 0AD FS @ no__t-1 中，同盟伺服器 proxy 的角色是由新的遠端存取角色服務（稱為 Web 應用程式 Proxy）所處理。 若要讓您的 AD FS 可從公司網路外部存取（這是在舊版 AD FS 中部署同盟伺服器 proxy 的目的，例如 Windows Server 2012 中的 AD FS 2.0 和 AD FS，您可以部署一或多個 web 應用程式 proxyWindows Server 2012 R2 中的 D FS。  
  
在 AD FS 的內容中，Web 應用程式 Proxy 會當做 AD FS 的同盟伺服器 proxy。 此外，Web 應用程式 Proxy 為您公司網路內部的 Web 應用程式提供反向 Proxy 功能，以讓任何裝置上的使用者能從公司網路外部存取這些應用程式。 如需 Web 應用程式 Proxy 的詳細資訊，請參閱 Web 應用程式 Proxy 概觀。  
  
若要規劃 Web 應用程式 Proxy 的部署，您可以檢閱下列主題的資訊：  
  
-   [規劃 Web 應用程式 Proxy 基礎結構（WAP）](https://technet.microsoft.com/library/dn383648.aspx)  
  
-   [規劃 Web 應用程式 Proxy 伺服器](https://technet.microsoft.com/library/dn383647.aspx)  
  
若要部署 Web Application Proxy，您可以依照下列主題的程序：  
  
-   [設定 Web 應用程式 Proxy 基礎結構](https://technet.microsoft.com/library/dn383644.aspx)  
  
-   [安裝和設定 Web 應用程式 Proxy 伺服器](https://technet.microsoft.com/library/dn383662.aspx)  
  
 
## <a name="see-also"></a>另請參閱 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署同盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

