---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: "在 Windows Server 2012 R2 的 AD FS 設計指南"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 498b399818fb8c9e463f9990fa13c87648c0a33d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-design-guide-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 的 AD FS 設計指南

>適用於：Windows Server 2016、Windows Server 2012 R2

Active Directory 同盟服務 \(AD FS\) 想要存取應用程式中廣告 FS\ 保護企業版聯盟合作夥伴，或在雲端中的使用者提供簡化的受保護的身分聯盟和 Web 單一 sign\ 上 \(SSO\) 功能。  
  
在 Windows Server® 2012 R2，AD FS 包含做為身分提供者同盟服務角色服務 \（驗證使用者提供信任 AD FS\ 的應用程式的安全性權杖）或聯盟提供者 \（消耗權杖從其他身分提供者，並提供信任 AD FS\ 的應用程式的安全性權杖）。  
  
提供應用程式和服務，在 Windows Server 2012 R2 AD FS 受到外部網路存取的功能現在稱為 Web 應用程式 Proxy 新遠端存取的角色服務執行。 這是從先前的 Windows Server 此功能由 AD FS 聯盟伺服器 proxy 版本不同的。 Web 應用程式 Proxy 是設計用來提供廣告 FS\ 相關外部案例和其他外部案例存取伺服器角色。 適用於 Web 應用程式 Proxy 詳細資訊，請查看[Web 應用程式 Proxy 逐步解說指南](https://technet.microsoft.com/library/dn280944.aspx)。  
  
## <a name="about-this-guide"></a>有關本指南  
本指南提供建議以協助您計畫新部署的 AD FS，根據您的組織的需求。 本指南被針對使用的基礎結構專員或系統架構。 它會反白顯示您的主要決策點為您計劃 AD FS 部署。 本指南朗讀時之前，您應該會有深入了解 AD FS 上功能的層級的運作方式。 如需詳細資訊，請查看[了解主要 AD FS 概念](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)。  
  
## <a name="in-this-guide"></a>本指南  
  
-   [找出您 AD FS 部署務目標](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [AD FS 部署拓撲計劃](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [AD FS 需求](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>也了  
[AD FS 設計](../../ad-fs/AD-FS-Design.md)  
  

