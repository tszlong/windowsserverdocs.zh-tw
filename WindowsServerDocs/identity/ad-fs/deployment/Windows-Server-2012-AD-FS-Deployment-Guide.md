---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: Windows Server 2012 AD FS 部署指南
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: aeb1d9042cea6be77ea15f6c753d720d7f814fe6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855821"
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Windows Server 2012 AD FS 部署指南


您可以使用 Active Directory&reg; 同盟服務 \(AD FS\) 與 Windows Server&reg; 2012 作業系統，建立可將分散式識別、驗證和授權服務延伸至跨組織與平臺界限之 Web\-應用程式的同盟識別身分管理解決方案。 藉由部署 AD FS，您可以將組織現有的身分識別管理功能延伸至網際網路。  
  
部署 AD FS 可以：  
  
-   當您的員工或客戶需要遠端存取內部主控的網站或服務時，就能以 Web\-為基礎、單一\-號\-在 \(SSO\) 體驗上。  
  
-   當您的員工或客戶從您網路的防火牆存取跨\-組織網站或服務時，請提供以 Web\-為基礎的 SSO 體驗。  
  
-   讓您的員工或客戶可以順暢地存取網際網路上任何同盟夥伴組織中的 Web\-資源，而不需要員工或客戶登入一次以上。  
  
-   不需在提供者 \(Windows Live ID、自由聯盟和其他\)上使用其他登入\-，即可保有員工或客戶身分識別的完整控制權。  
  
## <a name="about-this-guide"></a>關於本指南  
本指南主要提供給系統管理員與系統工程師使用。 它提供詳細的指引，供您部署已由您或組織中的基礎結構專家或系統架構設計人員預先選取的 AD FS 設計。  
  
如果尚未選取設計，建議您先依照本指南中的指示進行，直到您已經複習過[Windows Server 2012 中 AD FS 設計指南](https://technet.microsoft.com/library/dd807036.aspx)中的設計選項，並為您的組織選擇最適合的設計。 如需有關在已選取的設計中使用本指南的詳細資訊，請參閱[執行您的 AD FS 設計計畫](Implementing-Your-AD-FS-Design-Plan.md)。  
  
從設計指南中選取您的設計並收集有關宣告、權杖類型、屬性存放區和其他專案的必要資訊之後，您可以使用本指南，在生產環境中部署您的 AD FS 設計。 本指南提供部署下列其中一個主要 AD FS 設計的步驟：  
  
-   網頁 SSO  
  
-   同盟網頁 SSO  
  
使用[執行 AD FS 設計計畫](Implementing-Your-AD-FS-Design-Plan.md)中的檢查清單來判斷如何使用本指南中的指示來部署您的特定設計。 如需部署 AD FS 之硬體和軟體需求的相關資訊，請參閱 AD FS 設計指南中的[附錄 A：審查 AD FS 需求](https://technet.microsoft.com/library/ff678034.aspx)。  
  
### <a name="what-this-guide-does-not-provide"></a>本指南中未提供的內容  
本指南中未提供的內容：  
  
-   有關在現有網路基礎結構中放置同盟伺服器、同盟伺服器 proxy 或網頁伺服器的時機和位置的指導方針。 如需這項資訊，請參閱 AD FS 設計指南中的[規劃同盟伺服器位置](https://technet.microsoft.com/library/dd807069.aspx)和[規劃同盟伺服器 Proxy 位置](https://technet.microsoft.com/library/dd807130.aspx)。  
  
-   使用憑證授權單位單位 \(Ca\) 設定 AD FS 的指引  
  
-   安裝或設定特定 Web\-應用程式的指引  
  
-   設定測試實驗室環境的特定安裝指示。  
  
-   如何自訂同盟登入畫面、web.config 檔或設定資料庫的相關資訊。  
  
## <a name="in-this-guide"></a>在本指南中  
  
-   [計畫部署 AD FS](Planning-to-Deploy-AD-FS.md)  
  
-   [實作您的 AD FS 設計計畫](Implementing-Your-AD-FS-Design-Plan.md)  
  
-   [檢查清單：執行網頁 SSO 設計](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [檢查清單：執行同盟網頁 SSO 設計](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
  
-   [設定夥伴組織](Configuring-Partner-Organizations.md)  
  
-   [設定宣告規則](Configuring-Claim-Rules.md)  
  
-   [部署同盟伺服器](Deploying-Federation-Servers.md)  
  
-   [部署同盟伺服器 Proxy](Deploying-Federation-Server-Proxies.md)  
  
-   [與 AD FS 1.x 互通](Interoperating-with-AD-FS-1.x.md)  
