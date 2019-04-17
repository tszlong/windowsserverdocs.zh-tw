---
ms.assetid: c81b8291-fba5-4b30-a43d-7feb2f4b66be
title: "在 Windows Server 2012 R2 的 AD FS 設計指南"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f618add4c4803142b3bd7278908834a412f30999
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="identify-your-ad-fs-deployment-goals"></a>找出您 AD FS 部署務目標

>適用於：Windows Server 2016、Windows Server 2012 R2

正確辨識您的 Active Directory 同盟服務 \(AD FS\) 部署目標務必 AD FS 設計專案的成功。 排定優先順序，可能，將您的部署目標結合，讓您可以設計和部署 AD FS 使用方法。 您可以利用現有、 文件，並預先定義的 AD FS 部署目標，AD FS 的設計帶有相關的開發工作順序遭遇問題。  
  
AD FS 舊版最常部署以達成下列動作：  
  
-   提供您的員工或針對 web\ 為基礎，SSO 體驗時存取您的企業 claims\ 型應用程式。  
  
-   為您的員工或針對提供任何聯盟合作夥伴組織存取資源 web\ 為基礎的 SSO 體驗。  
  
-   提供您的員工或針對 Web\ 為基礎，SSO 體驗時遠端存取內部裝載的網站或服務。  
  
-   時，提供您的員工或針對 web\ 型 SSO 經驗資源或的雲端服務的存取。  
  
除了這些項目，在 Windows Server® 2012 R2 AD FS 可可幫助您達到下列功能：  
  
-   適用於 SSO 和順暢的第二個裝置的工作地點加入因素驗證。 這可讓組織允許存取的使用者的個人裝置，並提供這個存取權時管理的風險。  
  
-   管理 multi\ 雙因素存取控制與風險。 AD FS 提供豐富的層級的授權，以控制哪些應用程式存取的人員。 這可以根據使用者屬性 \ (UPN 電子郵件、 安全性群組成員資格、 驗證越等 \)，裝置屬性 \ （無論是裝置的工作地點 joined\） 或要求屬性 \ （網路位置、 IP 位址，或是使用者 agent\）。  
  
-   管理其他 multi\ 雙因素驗證敏感的應用程式的風險。 AD FS 可讓您控制原則全球或每個應用程式為基礎，可能需要 multi\ 雙因素驗證。 此外，AD FS 提供任何 multi\ 雙因素廠商安全且順暢 multi\ 雙因素體驗深度整合終端使用者的擴充的功能。  
  
-   適用於存取受保護應用程式網路 proxy 的 web 資源的外部提供驗證與授權功能。  
  
若要簡言之，在 Windows Server 2012 R2 AD FS 可以達成下列目標您在組織中的部署：  
  
### <a name="enable-your-users-to-access-resources-on-their-personal-devices-from-anywhere"></a>讓使用者從任何地方存取他們個人的裝置上的資源  
  
-   可讓使用者公司 Active Directory 中加入他們的個人裝置，並時存取公司資源從這些裝置，如此一來取得存取和順暢的體驗的工作地點加入。  
  
-   Pre\ 驗證的企業網路受 proxy Web 應用程式和從網際網路存取資源。  
  
-   變更密碼，讓使用者可以變更密碼的地點任何加入裝置時過期使其可以存取資源繼續他們的密碼。  
  
### <a name="enhance-your-access-control-risk-management-tools"></a>美化存取控制風險管理工具  
管理風險是重要的控管與每個 IT 在組織中的規範。 有許多存取控制風險管理調節中 AD FS 在 Windows Server® 2012 R2，其中包括：  
  
-   根據網路位置，以管理使用者存取 AD FS\ 保護的應用程式的驗證方式彈性的控制項。  
  
-   若要判斷使用者是否需要執行 multi\ 雙因素驗證使用者的資料、 裝置資料，以及網路位置為基礎的彈性原則。  
  
-   Per\ 應用程式控制項略過 SSO 和使用者提供的認證每次存取敏感的應用程式。  
  
-   根據使用者資料、 裝置的資料或網路位置彈性 per\ 應用程式存取原則。  
  
-   AD FS 外部鎖定，讓系統管理員 Active Directory 帳號防止暴力來自網際網路的攻擊。  
  
-   適用於任何地點存取撤銷加入裝置已停用或在 Active Directory。  
  
### <a name="use-ad-fs-to-enhance-the-sign-in-experience"></a>請使用 AD FS 美化 sign\ 中的體驗  
以下是新增 AD FS 功能在 Windows Server® 2012 R2，來自訂和愉快 sign\ 中的系統管理員：  
  
-   整合 AD FS 服務的變更做了一次，而會自動傳送給 AD FS 聯盟伺服器在指定的其餘的自訂項目。  
  
-   更新的 sign\ 中的網頁現代化的外觀，並自動迎合不同尺寸規格。  
  
-   自動後援 forms\ 驗證適用於未加入網域企業，但仍使用裝置的支援產生中的企業網路 \(intranet\) 來自存取要求。  
  
-   自訂公司商標圖示的影像、 適用於 IT 的支援，首頁，隱私權，標準連結簡單的控制項。  
  
-   Sign\ 在頁面中的描述簡訊的自訂項目。  
  
-   Web 主題的自訂項目。  
  
-   家用領域探索 \(HRD\) 根據組織尾碼美化的合作夥伴公司的隱私權的使用者。  
  
-   HRD 的篩選 per\ 應用程式為基礎，自動挑選領域根據應用程式。  
  
-   按一下 One\ 的錯誤報告變得更容易 IT 進行疑難排解。  
  
-   自訂的錯誤訊息。  
  
-   使用者驗證選項時使用多個驗證提供者。  
  
## <a name="see-also"></a>也了  
[在 Windows Server 2012 R2 的 AD FS 設計指南](../../ad-fs/design/AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

