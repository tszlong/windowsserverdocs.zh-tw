---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Windows Server 2012 R2 中的 AD FS 設計指南
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 614bc2b4571dd8a1b35c075ae1dec6934e77e148
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408167"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Windows Server 中的 AD FS 設計指南 

Active Directory 同盟服務 \(AD FS\) 針對想要存取\-\(安全企業、同盟夥伴組織或雲端中應用程式的終端使用者，提供簡化、安全的身分識別同盟和 Web 單一登入\) AD FS SSO\-功能。  
  
在 Windows Server® 2012 R2 中，AD FS 包含做為身分識別提供者的同盟服務角色服務 \(會驗證使用者，以提供安全性權杖給信任 AD FS\) 的應用程式，或做為同盟提供者 \(取用來自其他身分識別提供者的權杖，然後提供安全性權杖給信任 AD FS\)的應用程式。  
  
提供外部網路存取權給受到 Windows Server 2012 R2 中 AD FS 保護的應用程式和服務的功能，現在由稱為 Web 應用程式 Proxy 的新遠端存取角色服務執行。 這是從舊版的 Windows Server 出發，其中此功能是由 AD FS 同盟伺服器 proxy 處理。 Web 應用程式 Proxy 是一種伺服器角色，設計用來提供 AD FS\-相關外部網路案例和其他外部網路案例的存取權。 如需 Web 應用程式 Proxy 的詳細資訊，請參閱[Web 應用程式 Proxy 逐步解說指南](https://technet.microsoft.com/library/dn280944.aspx)。  
  
## <a name="about-this-guide"></a>關於本指南  
本指南提供的建議可協助您根據組織的需求，規劃新的 AD FS 部署。 本指南的使用對象為基礎結構專家或系統架構設計人員。 它會在您規劃 AD FS 部署時，反白顯示您的主要決策點。 閱讀本指南之前，您應該先充分瞭解 AD FS 在功能層級上的運作方式。 如需詳細資訊，請參閱 [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)。  
  
## <a name="in-this-guide"></a>此指南內容  
  
-   [識別您的 AD FS 部署目標](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [規劃您的 AD FS 部署拓撲](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [AD FS 需求](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>請參閱  
[AD FS 設計](../../ad-fs/AD-FS-Design.md)  
  

