---
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: Windows Server 2012 R2 中的 AD FS 設計指南
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d79bfa571f420b23b1716cd1bceca33fdc96e038
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359109"
---
# <a name="identify-your-ad-fs-deployment-goals"></a>識別您的 AD FS 部署目標

正確地識別您\(的\) Active Directory 同盟服務 AD FS 部署目標對於您的 AD FS 設計專案是否成功而言，是不可或缺的。 將您的部署目標排定優先順序，並可讓您使用反復的方法來設計和部署 AD FS。 您可以利用與 AD FS 設計相關的現有、已記載和預先定義的 AD FS 部署目標，並為您的情況開發可行的解決方案。  
  
先前的 AD FS 版本最常部署，以達成下列目標：  
  
-   當您的員工或客戶在企業\-記憶體取宣告\-式應用程式時，為其提供以網頁為基礎的 SSO 體驗。  
  
-   為您的員工或客戶提供以\-網頁為基礎的 SSO 體驗，以存取任何同盟夥伴組織中的資源。  
  
-   在遠端存取內部裝載的網站或\-服務時，為您的員工或客戶提供網頁型 SSO 體驗。  
  
-   在存取雲端中的資源或服務\-時，為您的員工或客戶提供以網頁為基礎的 SSO 體驗。  
  
除此之外，Windows Server® 2012 R2 中的 AD FS 新增了可協助您達成下列目標的功能：  
  
-   針對 SSO 加入裝置工作地點和無縫式次要因素驗證。 這可讓組織允許從使用者的個人裝置存取，並在提供此存取權時管理風險。  
  
-   使用多重\-要素存取控制來管理風險。 AD FS 提供豐富層級的授權，控制誰可以存取哪些應用程式。 這\(可以根據使用者屬性 UPN、電子郵件、安全性群組成員資格、驗證強度\)等，裝置屬性\(是裝置是否已加入\)工作地點或要求屬性\(網路位置、IP 位址或使用者代理程式\)。  
  
-   透過其他多\-因素驗證管理機密應用程式的風險。 AD FS 可讓您控制可能需要全域或每\-個應用程式進行多重要素驗證的原則。 此外，AD FS 會為任何多\-因素廠商提供擴充點，以深入整合以提供安全且順暢的多\-因素體驗給終端使用者。  
  
-   提供驗證和授權功能，以從 Web 應用程式 Proxy 所保護的外部網路存取 web 資源。  
  
總而言之，您可以部署 Windows Server 2012 R2 中的 AD FS，以達成組織的下列目標：  
  
### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>讓您的使用者可以從任何地方存取其個人裝置上的資源  
  
-   工作地點加入可以讓使用者將其個人裝置加入公司 Active Directory，因而在從這些裝置存取公司資源時獲得存取權和順暢的體驗。  
  
-   預先\-驗證公司網路內受 Web 應用程式 proxy 保護並從網際網路存取的資源。  
  
-   密碼變更可以讓使用者在密碼到期時從已加入工作地點的任何裝置變更其密碼，以便能夠繼續存取資源。  
  
### <a name="enhance-your-access-control-risk-management-tools"></a>增強您的存取控制風險管理工具  
管理風險是每個 IT 組織中監管和法務遵循的重要層面。 Windows Server® 2012 R2 中的 AD FS 有許多存取控制風險管理增強功能，包括下列各項：  
  
-   以網路位置為基礎的彈性控制項，用來管理使用者驗證以存取\-AD FS 安全應用程式的方式。  
  
-   彈性的原則，用來判斷使用者是否需要根據\-使用者的資料、裝置資料和網路位置來執行多重要素驗證。  
  
-   每\-個應用程式控制都會忽略 SSO，並強制使用者在每次存取敏感性應用程式時提供認證。  
  
-   以使用者\-資料、裝置資料或網路位置為基礎的每個應用程式存取原則彈性。  
  
-   可讓系統管理員保護 Active Directory 帳戶避免受到來自網際網路之暴力密碼破解攻擊的 AD FS 外部網路鎖定。  
  
-   Active Directory 中已停用或已刪除之已加入工作地點裝置的存取權撤銷。  
  
### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>使用 AD FS 增強\-登入體驗  
以下是 Windows Server® 2012 R2 中的新 AD FS 功能，可讓系統管理員自訂及增強\-登入體驗：  
  
-   整合 AD FS 服務的自訂，只要進行一次變更，然後就會自動傳播到指定伺服器陣列中其餘的 AD FS 同盟伺服器。  
  
-   更新的登入頁面，看起來現代化，並自動滿足不同的外型規格。\-  
  
-   針對未加入公司網域但\-仍會使用的裝置，支援自動回復至以表單為基礎的驗證，會從公司網路內部\)網路\(內部產生存取要求。  
  
-   簡單的控制項，以自訂公司標誌、繪圖影像、IT 支援、首頁、隱私權的標準連結等等。  
  
-   在登\-入頁面中自訂描述訊息。  
  
-   Web 佈景主題的自訂。  
  
-   主領域探索\(HRD\)是以使用者的組織尾碼為基礎，以增強公司合作夥伴的隱私權。  
  
-   針對每個\-應用程式 HRD 篩選，以根據應用程式自動挑選領域。  
  
-   \-按一下 [錯誤報表] 以輕鬆進行疑難排解。  
  
-   可自訂錯誤訊息。  
  
-   有一個以上的驗證提供者可用時的使用者驗證選項。  
  
## <a name="see-also"></a>另請參閱  
[Windows Server 2012 R2 中的 AD FS 設計指南](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

