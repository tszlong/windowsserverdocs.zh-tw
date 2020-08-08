---
ms.assetid: bb16e39d-566d-436c-b957-394c06d556db
title: Windows Server 2012 中的 AD FS 設計指南
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c9089a3eac4d0ba6f4961f97fcb87b4e9a1a8c91
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87949912"
---
# <a name="ad-fs-legacy-design-guide-in-windows-server"></a>Windows Server 中的 AD FS 舊版設計指南



> [!NOTE]
> 如需有關如何在 Windows Server 2012 R2 中部署 AD FS 的詳細資訊，請參閱[Windows server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)。

您可以 &reg; \( \) 在同盟服務提供者角色中，使用與 Windows Server 2012 作業系統 AD FS 的 Active Directory 同盟服務 &reg; ，讓您的使用者順暢地向 \- 位於資源夥伴組織中的任何 Web 服務或應用程式進行驗證，而不需要系統管理員在兩個組織的網路之間建立或維護外部信任或樹系信任，而不需要使用者再次登入。 在存取另一個網路中的資源時，對某個網路進行驗證的程式（不需要使用者重複登入動作的負擔）稱為單一登入 \- \( SSO \) 。

## <a name="about-this-guide"></a>關於本指南
本指南提供的建議可協助您根據組織的需求，規劃新的 AD FS 部署， \( 此指南也稱為部署目標 \) 和您想要建立的特定設計。 本指南的使用對象為基礎結構專家或系統架構設計人員。 它會在您規劃 AD FS 部署時，反白顯示您的主要決策點。 閱讀本指南之前，您應該先充分瞭解 AD FS 在功能層級上的運作方式。 您也應該充分瞭解將會反映在 AD FS 設計中的組織需求。

本指南描述以三個主要 AD FS 設計為基礎的一組部署目標，並協助您決定最適合您環境的設計。 您可以使用這些部署目標來形成下列其中一項完整的 AD FS 設計，或符合您環境需求的自訂設計：

-   同盟網頁 SSO，可支援企業 \- 對 \- 企業 \( B2B \) 案例，並支援與獨立樹系的業務單位之間的共同作業

-   網頁 SSO，可支援客戶存取企業中的應用程式對取用 \- \- 者 \( B2C \) 案例

針對每一個設計，您將發現適用於蒐集與您的環境有關之必要資料的指導方針。 接著，您可以使用這些指導方針來規劃和設計您的 AD FS 部署。 閱讀本指南並完成收集、記載及對應貴組織的需求之後，您將擁有使用《 [Windows Server 2012 AD FS 部署指南》](../../ad-fs/deployment/Windows-Server-2012-AD-FS-Deployment-Guide.md)中的指導方針開始部署 AD FS 所需的資訊。

## <a name="in-this-guide"></a>本指南內容

-   [識別您的 AD FS 部署目標](Identifying-Your-AD-FS-Deployment-Goals.md)

-   [將您的部署目標對應至 AD FS 設計](Mapping-Your-Deployment-Goals-to-an-AD-FS-Design.md)

-   [決定您的 AD FS 部署拓撲](Determine-Your-AD-FS-Deployment-Topology.md)

-   [規劃您的部署](Planning-Your-Deployment.md)

-   [規劃同盟伺服器的位置](Planning-Federation-Server-Placement.md)

-   [規劃同盟伺服器 Proxy 的位置](Planning-Federation-Server-Proxy-Placement.md)

-   [規劃 AD FS 伺服器容量](Planning-for-AD-FS-Server-Capacity.md)

-   [附錄 A：檢閱 AD FS 需求](Appendix-A--Reviewing-AD-FS-Requirements.md)


