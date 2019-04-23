---
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: Windows Server 2012 R2 中的 AD FS 設計指南
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f618add4c4803142b3bd7278908834a412f30999
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862569"
---
# <a name="identify-your-ad-fs-deployment-goals"></a>識別您的 AD FS 部署目標

>適用於：Windows Server 2016, Windows Server 2012 R2

正確識別您的 Active Directory Federation Services \(AD FS\)部署目標是在 AD FS 設計專案成功不可或缺。 設定優先順序，並且可能是合併部署目標，使您可以設計和部署 AD FS 使用互動的方法。 您可以利用現有的、 記錄、 且預先定義的 AD FS 部署目標，與 AD FS 設計和開發有效的解決方案，為您的情況。  
  
舊版 AD FS 是最常部署來達到下列目的：  
  
-   您的員工或顧客提供 web\-為基礎，SSO 的體驗時存取宣告\-架構您的企業內的應用程式。  
  
-   您的員工或顧客提供 web\-為基礎，在任何同盟夥伴組織中存取資源的 SSO 體驗。  
  
-   您的員工或顧客提供 Web\-當遠端存取內部主控的網站或服務的基礎，SSO 體驗。  
  
-   您的員工或顧客提供 web\-存取資源或雲端中的服務時的基礎，SSO 體驗。  
  
除了這些之外，Windows Server® 2012 R2 中的 AD FS 會新增功能，可協助您達到下列目的：  
  
-   針對 SSO 加入裝置工作地點和無縫式次要因素驗證。 這可讓組織允許從使用者的個人裝置的存取，並提供此存取權時管理風險。  
  
-   管理具有多個風險\-因素存取控制。 AD FS 提供豐富層級的授權，控制誰可以存取哪些應用程式。 這可以根據使用者屬性\(UPN、 電子郵件、 安全性群組成員資格、 驗證強度等等\)，裝置屬性\(裝置是否已加入工作場所\)或要求屬性\(網路位置、 IP 位址或使用者代理程式\)。  
  
-   使用其他多重管理風險\-authentication 為機密的應用程式。 AD FS 可讓您控制原則可能需要多\-因素全域方式或以每個應用程式為基礎的驗證。 此外，AD FS 會提供擴充點的任何多\-深度整合安全且順暢的多因素廠商\-因素使用者體驗。  
  
-   提供用於存取來自外部網路的網路資源所保護的 Web 應用程式 Proxy 的驗證和授權功能。  
  
總結來說，Windows Server 2012 R2 中的 AD FS 可以部署以達成貴組織中的下列目標：  
  
### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>讓使用者從任何地方存取其個人裝置上的資源  
  
-   工作地點加入可以讓使用者將其個人裝置加入公司 Active Directory，因而在從這些裝置存取公司資源時獲得存取權和順暢的體驗。  
  
-   Pre\-驗證的受保護的 Web 應用程式 proxy 並從網際網路存取公司網路內部的資源。  
  
-   密碼變更可以讓使用者在密碼到期時從已加入工作地點的任何裝置變更其密碼，以便能夠繼續存取資源。  
  
### <a name="enhance-your-access-control-risk-management-tools"></a>加強存取控制風險管理工具  
管理風險是每個 IT 組織中監管和法務遵循的重要層面。 有提供許多存取控制風險管理增強功能在 Windows Server® 2012 R2 中 AD FS 中的包括下列：  
  
-   根據網路位置管理使用者存取 AD FS 的驗證方式的彈性化控制項\-保護的應用程式。  
  
-   彈性的原則，以判斷使用者是否需要執行多重\-authentication 根據使用者的資料、 裝置資料，以及網路位置。  
  
-   每個\-忽略 SSO 並強制使用者提供認證，每次存取機密的應用程式的應用程式控制項。  
  
-   每個彈性\-應用程式存取原則會根據使用者資料、 裝置資料或網路位置。  
  
-   可讓系統管理員保護 Active Directory 帳戶避免受到來自網際網路之暴力密碼破解攻擊的 AD FS 外部網路鎖定。  
  
-   Active Directory 中已停用或已刪除之已加入工作地點裝置的存取權撤銷。  
  
### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>使用 AD FS 增強登\-體驗  
以下是新 AD FS 功能在 Windows Server® 2012 R2 中的，可讓系統管理員，來自訂及增強登\-經驗：  
  
-   整合 AD FS 服務的自訂，只要進行一次變更，然後就會自動傳播到指定伺服器陣列中其餘的 AD FS 同盟伺服器。  
  
-   更新登\-的外觀呈現現代化並且自動符合不同尺寸的頁面。  
  
-   支援表單的自動後援\-的裝置未加入公司網域，但仍使用會產生來自公司網路內的存取要求為基礎的驗證\(內部網路\)。  
  
-   簡單的控制項，以自訂公司標誌、繪圖影像、IT 支援、首頁、隱私權的標準連結等等。  
  
-   描述自訂訊息的登\-頁面中。  
  
-   Web 佈景主題的自訂。  
  
-   首頁領域探索\(HRD\)根據使用者提供的增強隱私權之公司的夥伴組織後置詞。  
  
-   HRD 篩選每個\-自動挑選領域的個別應用程式為基礎的應用程式。  
  
-   一個\-按一下 錯誤報告更容易 IT 疑難排解。  
  
-   可自訂錯誤訊息。  
  
-   有一個以上的驗證提供者可用時的使用者驗證選項。  
  
## <a name="see-also"></a>另請參閱  
[Windows Server 2012 R2 中 AD FS 設計指南](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

