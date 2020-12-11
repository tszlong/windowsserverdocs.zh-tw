---
description: 深入瞭解： Windows Server 中的 AD FS 設計指南
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Windows Server 2012 R2 中的 AD FS 設計指南
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 0fb36e6a2a19ef5e946aaa1125a85850bf670c4f
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048446"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Windows Server 中的 AD FS 設計指南

Active Directory 同盟服務 \( AD FS \) \- \( \) 為想要存取 AD FS \- 安全企業、同盟夥伴組織或雲端中的應用程式的使用者，提供簡化、安全的身分識別同盟和 Web 單一登入 SSO 功能。

在 Windows Server &reg; 2012 R2 中，AD FS 包含作為身分識別提供者的 federation service 角色服務， \( 可驗證使用者是否提供安全性權杖給信任 AD FS 的應用程式， \) 或作為同盟提供者 \( 使用來自其他身分識別提供者的權杖，然後將安全性權杖提供給信任 AD FS 的應用程式 \) 。

提供外部網路存取權給受到 Windows Server 2012 R2 中 AD FS 保護的應用程式和服務的功能，現在由稱為 Web 應用程式 Proxy 的新遠端存取角色服務執行。 這是從舊版的 Windows Server 出發，其中此功能是由 AD FS 同盟伺服器 Proxy 處理。 Web 應用程式 Proxy 是一種伺服器角色，其設計目的是要為 AD FS \- 相關的外部網路案例和其他外部網路案例提供存取權。 如需 Web 應用程式 Proxy 的詳細資訊，請參閱 [Web 應用程式 Proxy 逐步解說指南](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11))。

## <a name="about-this-guide"></a>關於本指南
本指南提供的建議可協助您根據組織的需求規劃新的 AD FS 部署。 本指南的使用對象為基礎結構專家或系統架構設計人員。 當您規劃 AD FS 部署時，它會強調您的主要決策點。 閱讀本指南之前，您應該先充分瞭解 AD FS 在功能等級上的運作方式。 如需詳細資訊，請參閱 [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md)。

## <a name="in-this-guide"></a>本指南內容

-   [識別您的 AD FS 部署目標](Identify-Your-AD-FS-Deployment-Goals.md)

-   [規劃您的 AD FS 部署拓撲](Plan-Your-AD-FS-Deployment-Topology.md)

-   [AD FS 需求](AD-FS-Requirements.md)


## <a name="see-also"></a>另請參閱
[AD FS 設計](../../ad-fs/AD-FS-Design.md)

