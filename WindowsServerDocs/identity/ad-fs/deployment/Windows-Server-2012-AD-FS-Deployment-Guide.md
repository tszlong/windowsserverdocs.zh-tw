---
description: 深入瞭解： Windows Server 2012 AD FS 部署指南
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: Windows Server 2012 AD FS 部署指南
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a31e2225574ec43b3a5328963de36833a4b85c67
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049016"
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Windows Server 2012 AD FS 部署指南


您可以使用 &reg; \( AD FS \) 與 Windows Server 2012 作業系統 Active Directory Federation services &reg; 來建立同盟身分識別管理解決方案，以將分散式識別、驗證和授權服務延伸到 \- 跨組織及平臺界限的 Web 應用程式。 藉由部署 AD FS，您可以將組織現有的身分識別管理功能延伸到網際網路。

部署 AD FS 可以：

-   當您的員工或客戶 \- \- \- \( \) 需要從遠端存取內部裝載的網站或服務時，請提供單一登入 SSO 體驗給您的員工或客戶。

-   當您的員工或客戶 \- \- 從您網路的防火牆存取跨組織網站或服務時，請提供其 SSO 體驗給您的員工或客戶。

-   讓您的員工或客戶能夠順暢地存取 \- 網際網路上任何同盟夥伴組織中的 Web 資源，而不需要員工或客戶多次登入。

-   無須使用其他登入 \- 提供者 \( 的 Windows Live ID、自由聯盟及其他登入提供者，就能保有員工或客戶身分識別的完整控制權 \) 。

## <a name="about-this-guide"></a>關於本指南
此指南適用於系統管理員與系統工程師。 它會提供詳細的指引，說明如何部署由您或貴組織中的基礎結構專家或系統架構設計師預先選取的 AD FS 設計。

如果尚未選取設計，我們建議您等到遵循本指南中的指示，直到您已經複習過 [Windows Server 2012 中的 AD FS 設計指南](../design/ad-fs-design-guide-in-windows-server-2012.md) 中的設計選項，並且已為您的組織選取最適當的設計。 如需有關如何搭配已選取的設計使用本指南的詳細資訊，請參閱 [實施您的 AD FS 設計計畫](Implementing-Your-AD-FS-Design-Plan.md)。

從設計指南中選取您的設計，並收集有關宣告、權杖類型、屬性存放區和其他專案的必要資訊之後，您可以使用本指南，在生產環境中部署 AD FS 的設計。 本指南提供部署下列其中一項主要 AD FS 設計的步驟：

-   網頁 SSO

-   同盟網頁 SSO

您可以使用 [執行 AD FS 設計計畫](Implementing-Your-AD-FS-Design-Plan.md) 中的檢查清單，判斷如何使用本指南中的指示來部署特定設計。 如需有關部署 AD FS 的硬體和軟體需求的詳細資訊，請參閱 AD FS 設計指南中的 [附錄 A：審查 AD FS 需求](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ff678034(v=ws.11)) 。

### <a name="what-this-guide-does-not-provide"></a>本指南中未提供的內容
本指南中未提供的內容：

-   有關在現有的網路基礎結構中放置同盟伺服器、同盟伺服器 proxy 或網頁伺服器之時機和位置的指引。 如需相關資訊，請參閱 AD FS 設計指南中的 [規劃同盟伺服器放置](../design/planning-federation-server-placement.md) 和 [規劃同盟伺服器 Proxy 位置](../design/planning-federation-server-proxy-placement.md) 。

-   使用憑證授權單位單位 \( ca \) 設定 AD FS 的指導方針

-   設定或設定特定 Web \- 應用程式的指引

-   設定測試實驗室環境的特定安裝指示。

-   如何自訂同盟登入畫面、web.config 檔或設定資料庫的相關資訊。

## <a name="in-this-guide"></a>本指南內容

-   [計畫部署 AD FS](Planning-to-Deploy-AD-FS.md)

-   [實作您的 AD FS 設計計畫](Implementing-Your-AD-FS-Design-Plan.md)

-   [檢查清單：實作網頁 SSO 設計](Checklist--Implementing-a-Web-SSO-Design.md)

-   [檢查清單：實作同盟網頁 SSO 設計](Checklist--Implementing-a-Federated-Web-SSO-Design.md)

-   [設定夥伴組織](Configuring-Partner-Organizations.md)

-   [設定宣告規則](Configuring-Claim-Rules.md)

-   [部署同盟伺服器](Deploying-Federation-Servers.md)

-   [部署同盟伺服器 Proxy](Deploying-Federation-Server-Proxies.md)

-   [與 AD FS 1.x 互通](Interoperating-with-AD-FS-1.x.md)
