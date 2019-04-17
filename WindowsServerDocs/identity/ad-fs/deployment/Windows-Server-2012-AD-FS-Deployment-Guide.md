---
ms.assetid: e2ad9e80-a036-4bac-a4fb-afa83756aa1f
title: "Windows Server 2012 AD FS 部署指南"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3e555d1003878e12320cb8557bd205ac24e1bbb3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="windows-server-2012-ad-fs-deployment-guide"></a>Windows Server 2012 AD FS 部署指南

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以與 Windows Server® 2012 年作業系統使用 Active Directory（）同盟服務 \(AD FS\) 建置聯盟的身分管理方案分散式的驗證，驗證和延伸服務 Web\ 為基礎的應用程式授權組織和邊界平台。 部署 AD FS，您可以網際網路延伸您組織的現有的身分管理功能。  
  
您可以部署至 AD FS:  
  
-   提供您的員工或針對 Web\ 為基礎，single\ sign\ 上 \(SSO\) 體驗時所需遠端內部裝載的網站或服務的存取。  
  
-   提供您的員工或針對 Web\ 型 SSO 體驗當他們存取 cross\ 組織的網站或中防火牆網路的服務。  
  
-   提供您的員工或針對順暢地存取網際網路上的任何聯盟合作夥伴組織中的 Web\ 資源而不需要員工或針對超過一次登入。  
  
-   不使用其他 sign\ 上提供者會保留完全掌控您的員工或客戶身分 \（Windows Live ID、自由 Alliance 和 others\）。  
  
## <a name="about-this-guide"></a>有關本指南  
本指南被針對使用系統管理員或系統的工程師。 提供的已您或您在組織中的基礎結構專員或系統設計師預先選擇 AD FS 設計用來部署詳細指導方針。  
  
如果尚未選取設計，我們建議您等候之後您已經檢視設計選項，請依照下列直到本文中的指示，[在 Windows Server 2012 中 AD FS 程式設計指南](https://technet.microsoft.com/library/dd807036.aspx)，而且您的組織選取最適合的設計。 如需有關本指南使用的設計，已選取的詳細資訊，請[實作您 AD FS 設計計劃](Implementing-Your-AD-FS-Design-Plan.md)。  
  
設計節目表中選取您的設計並收集關於宣告、權杖類型、屬性儲存及其他項目的必要的資訊後，您可以使用此快速入門，production 環境中部署 AD FS 設計。 本指南提供部署主要 AD FS 設計下列其中一項步驟：  
  
-   Web SSO  
  
-   聯盟的網路 SSO  
  
使用中的檢查清單[實作您 AD FS 設計計劃](Implementing-Your-AD-FS-Design-Plan.md)以最佳方式判斷部署特定設計本指南使用的指示操作。 部署 AD FS 硬體與軟體需求的相關資訊，請查看[附錄 a：審查 AD FS 需求](https://technet.microsoft.com/library/ff678034.aspx)中的 AD FS 設計。  
  
### <a name="what-this-guide-does-not-provide"></a>未提供哪些本指南  
本指南不提供：  
  
-   相關的時機，以及聯盟伺服器、聯盟伺服器 proxy 或網頁伺服器置於現有的網路基礎結構指導方針。 這項資訊，請查看[聯盟計畫伺服器位置](https://technet.microsoft.com/library/dd807069.aspx)和[規劃聯盟伺服器 Proxy 位置](https://technet.microsoft.com/library/dd807130.aspx)中 AD FS 設計。  
  
-   設定 AD FS 使用憑證授權單位 \(CAs\) 指導方針  
  
-   適用於設定或設定特定 Web\ 為基礎的應用程式的指導方針  
  
-   安裝指示的特定設定實驗室測試。  
  
-   如何自訂聯盟登入畫面、web.config 檔案或設定資料庫的相關資訊。  
  
## <a name="in-this-guide"></a>本指南  
  
-   [部署 AD FS 計劃](Planning-to-Deploy-AD-FS.md)  
  
-   [實作您 AD FS 設計計畫](Implementing-Your-AD-FS-Design-Plan.md)  
  
-   [檢查清單︰ 實作 Web SSO 設計](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [檢查清單︰ 實作的聯盟的網路 SSO 設計](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
  
-   [設定合作夥伴公司](Configuring-Partner-Organizations.md)  
  
-   [設定理賠要求規則](Configuring-Claim-Rules.md)  
  
-   [部署聯盟伺服器](Deploying-Federation-Servers.md)  
  
-   [部署聯盟的 Proxy 伺服器](Deploying-Federation-Server-Proxies.md)  
  
-   [AD FS 進行相互操作 1.x](Interoperating-with-AD-FS-1.x.md)  
