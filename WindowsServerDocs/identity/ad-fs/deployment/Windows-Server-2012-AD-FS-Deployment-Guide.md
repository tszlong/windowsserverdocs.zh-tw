---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: Windows Server 2012 AD FS 部署指南
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1597230fcfde4fe8a9767f0f14c634bc6fabdceb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408258"
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Windows Server 2012 AD FS 部署指南


您可以使用 Active Directory® Federation Services \(AD FS\)搭配 Windows Server®2012作業系統來建立同盟身分識別管理解決方案，以擴充分散式識別、驗證和跨組織與平臺\-界限對 Web 應用程式的授權服務。 藉由部署 AD FS，您可以將組織現有的身分識別管理功能延伸至網際網路。  
  
部署 AD FS 可以：  
  
-   當您的員工或客戶需要遠端\-訪問內部主控的\-網站或\)服務時，請提供以網頁為基礎的單一\-登錄\(體驗。  
  
-   當您的員工或客戶從您\-網路的防火牆存取跨\-組織網站或服務時，請為他們提供以網頁為基礎的 SSO 體驗。  
  
-   讓您的員工或客戶可以順暢地存取\-網際網路上任何同盟夥伴組織中的 Web 資源，而不需要員工或客戶登入一次以上。  
  
-   在不使用其他\-登入提供者\(Windows Live ID、自由聯盟和其他人\)的情況下，保有員工或客戶身分識別的完整控制權。  
  
## <a name="about-this-guide"></a>關於本指南  
此指南適用於系統管理員與系統工程師。 它提供詳細的指引，供您部署已由您或組織中的基礎結構專家或系統架構設計人員預先選取的 AD FS 設計。  
  
如果尚未選取設計，建議您先依照本指南中的指示進行，直到您已經複習過[Windows Server 2012 中的 AD FS 設計指南](https://technet.microsoft.com/library/dd807036.aspx)中的設計選項，並為您選取了最適當的設計。結構. 如需有關在已選取的設計中使用本指南的詳細資訊，請參閱[執行您的 AD FS 設計計畫](Implementing-Your-AD-FS-Design-Plan.md)。  
  
從設計指南中選取您的設計並收集有關宣告、權杖類型、屬性存放區和其他專案的必要資訊之後，您可以使用本指南，在生產環境中部署您的 AD FS 設計。 本指南提供部署下列其中一個主要 AD FS 設計的步驟：  
  
-   網頁 SSO  
  
-   同盟網頁 SSO  
  
使用[執行 AD FS 設計計畫](Implementing-Your-AD-FS-Design-Plan.md)中的檢查清單來判斷如何使用本指南中的指示來部署您的特定設計。 如需部署 AD FS 之硬體和軟體需求的相關資訊， [請參閱附錄 A：查看 AD FS 設計](https://technet.microsoft.com/library/ff678034.aspx)指南中的 AD FS 需求。  
  
### <a name="what-this-guide-does-not-provide"></a>本指南中未提供的內容  
本指南中未提供的內容：  
  
-   有關在現有網路基礎結構中放置同盟伺服器、同盟伺服器 proxy 或網頁伺服器的時機和位置的指導方針。 如需這項資訊，請參閱 AD FS 設計指南中的[規劃同盟伺服器位置](https://technet.microsoft.com/library/dd807069.aspx)和[規劃同盟伺服器 Proxy 位置](https://technet.microsoft.com/library/dd807130.aspx)。  
  
-   使用憑證授權單位\(單位 ca\)設定 AD FS 的指引  
  
-   安裝或設定特定 Web\-應用程式的指引  
  
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
