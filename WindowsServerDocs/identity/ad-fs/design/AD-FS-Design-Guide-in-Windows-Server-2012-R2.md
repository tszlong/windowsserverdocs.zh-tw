---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Windows Server 2012 R2 中的 AD FS 設計指南
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 498b399818fb8c9e463f9990fa13c87648c0a33d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822149"
---
# <a name="ad-fs-design-guide-in-windows-server-2012-r2"></a>Windows Server 2012 R2 中的 AD FS 設計指南

>適用於：Windows Server 2016, Windows Server 2012 R2

Active Directory Federation Services \(AD FS\)提供簡化、 安全的身分識別同盟和網頁單一登\-上\(SSO\)終端使用者想要存取應用程式的功能在 AD FS\-保護企業、 同盟協力廠商組織，或在雲端中。  
  
在 Windows Server® 2012 R2 AD FS 包含做為身分識別提供者的同盟服務角色服務\(驗證使用者以提供安全性權杖給信任 AD FS 的應用程式\)或做為同盟提供者\(會取用來自其他身分識別提供者的權杖，並提供安全性權杖給信任 AD FS 的應用程式\)。  
  
提供外部網路存取權給受到 Windows Server 2012 R2 中 AD FS 保護的應用程式和服務的功能，現在由稱為 Web 應用程式 Proxy 的新遠端存取角色服務執行。 這是從舊版的 Windows Server 出發，其中此功能是由 AD FS 同盟伺服器 proxy 處理。 Web 應用程式 Proxy 是設計來提供存取適用於 AD FS 伺服器角色\-相關外部網路狀況和其他外部網路案例。 如需有關 Web 應用程式 Proxy 的詳細資訊，請參閱 < [Web 應用程式 Proxy 逐步解說指南](https://technet.microsoft.com/library/dn280944.aspx)。  
  
## <a name="about-this-guide"></a>關於本指南  
本指南提供建議來協助您規劃新部署的 AD FS 中，根據貴組織的需求。 本指南的使用對象為基礎結構專家或系統架構設計人員。 它反白顯示您的主要決策點，當您在規劃 AD FS 部署。 在閱讀本指南之前，您應該充分了解 AD FS 功能等級上運作的方式。 如需詳細資訊，請參閱 [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)。  
  
## <a name="in-this-guide"></a>本指南內容  
  
-   [識別您的 AD FS 部署目標](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [規劃您的 AD FS 部署拓撲](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [AD FS 需求](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>另請參閱  
[AD FS 設計](../../ad-fs/AD-FS-Design.md)  
  

