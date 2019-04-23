---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: Windows Server 2012 AD FS 部署指南
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3e555d1003878e12320cb8557bd205ac24e1bbb3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882439"
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Windows Server 2012 AD FS 部署指南

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您可以使用 Active Directory® Federation Services \(AD FS\)與 Windows Server® 2012年作業系統，若要建立的同盟身分識別管理解決方案延伸至分散式的識別驗證，以及授權服務，以 Web\-基礎跨組織及平台界限的應用程式。 透過部署 AD FS，您就可以將組織現有的識別身分管理功能延伸至網際網路。  
  
部署 AD FS 可以：  
  
-   您的員工或顧客提供 Web\-為基礎，單一\-號\-上\(SSO\)體驗時所需的遠端存取內部主控的網站或服務。  
  
-   您的員工或顧客提供 Web\-為基礎，SSO 的體驗存取跨時\-組織的網站或從您的網路防火牆內的服務。  
  
-   您的員工或客戶，提供給 Web 不間斷地存取\-架構在網際網路上任何同盟夥伴組織中的資源，而不需要登入一次以上的員工或客戶。  
  
-   保留完整控制您的員工或客戶的身分識別，而不需使用其他符號\-提供者\(Windows Live ID、 Liberty Alliance，與其他人\)。  
  
## <a name="about-this-guide"></a>關於本指南  
此指南適用於系統管理員與系統工程師。 它提供部署由您或貴組織基礎結構專家或系統架構設計師預先選用的 AD FS 設計詳細的指導方針。  
  
如果尚未選取設計，我們建議您遵循的指示，直到本指南中，檢閱中的設計選項[Windows Server 2012 中 AD FS 設計指南](https://technet.microsoft.com/library/dd807036.aspx)，而且最多選取您的組織的有適當的設計。 如需使用本指南搭配已選取的設計的詳細資訊，請參閱[實作您 AD FS 設計計畫](Implementing-Your-AD-FS-Design-Plan.md)。  
  
從設計指南中選取您的設計，並收集有關宣告、 語彙基元型別、 屬性存放區和其他項目的必要的資訊之後，您可以使用本指南在生產環境中部署您的 AD FS 設計。 本指南提供部署下列的主要 AD FS 設計其中一項步驟：  
  
-   網頁 SSO  
  
-   同盟網頁 SSO  
  
使用中的檢查清單[實作您 AD FS 設計計畫](Implementing-Your-AD-FS-Design-Plan.md)來判斷如何使用本指南中的指示，來部署特定設計。 部署 AD FS 的硬體和軟體需求的相關資訊，請參閱[附錄 a:檢閱 AD FS 需求](https://technet.microsoft.com/library/ff678034.aspx)中 AD FS 設計指南。  
  
### <a name="what-this-guide-does-not-provide"></a>本指南中未提供的內容  
本指南中未提供的內容：  
  
-   取得有關何時及如何將同盟伺服器、 聯邦伺服器 proxy 或 Web 伺服器放在您現有的網路基礎結構中的指示。 這項資訊，請參閱[規劃同盟伺服器放置地點](https://technet.microsoft.com/library/dd807069.aspx)並[規劃同盟伺服器 Proxy 放置](https://technet.microsoft.com/library/dd807130.aspx)中 AD FS 設計指南。  
  
-   使用憑證授權單位的指導方針\(CAs\)設定 AD FS  
  
-   設定或設定特定網站的指導方針\-架構的應用程式  
  
-   設定測試實驗室環境的特定安裝指示。  
  
-   如何自訂同盟登入畫面、web.config 檔或設定資料庫的相關資訊。  
  
## <a name="in-this-guide"></a>本指南內容  
  
-   [規劃部署 AD FS](Planning-to-Deploy-AD-FS.md)  
  
-   [實作 AD FS 設計計畫](Implementing-Your-AD-FS-Design-Plan.md)  
  
-   [檢查清單：實作網頁 SSO 設計](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [檢查清單：實作同盟的網頁 SSO 設計](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
  
-   [設定夥伴組織](Configuring-Partner-Organizations.md)  
  
-   [設定宣告規則](Configuring-Claim-Rules.md)  
  
-   [部署同盟伺服器](Deploying-Federation-Servers.md)  
  
-   [部署同盟伺服器 Proxy](Deploying-Federation-Server-Proxies.md)  
  
-   [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md)  
