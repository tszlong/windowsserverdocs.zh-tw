---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: 網頁 SSO 設計
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1b8344594c9fc477ed8424c716ec8d7f7fd91ef3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852799"
---
# <a name="web-sso-design"></a>網頁 SSO 設計

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在 Web 單一\-登\-上\(SSO\) Active Directory Federation Services 中的設計\(AD FS\)，使用者必須一次驗證以存取多個 AD FS\-受保護的應用程式或服務。 在此設計中，所有使用者都是外部的，且因為沒有夥伴組織而沒有同盟信任存在。 一般而言，您部署此設計，當您要透過網際網路提供個別取用者或客戶存取其中一個或多個 AD FS 保護的服務或應用程式，如下圖所示。  
  
![網頁 sso 設計](media/adfs2_WebSSODesign.gif)  
  
透過網頁 SSO 設計中，組織通常裝載 AD FS\-受保護的應用程式或服務位於周邊網路中的可以維護客戶帳戶，在周邊網路中，可讓您更輕鬆地找出客戶的個別存放區帳戶和員工帳戶。  
  
您也可以使用 Active Directory 網域服務的周邊網路中客戶管理的本機帳戶\(AD DS\)，SQL Server 或自訂屬性存放區。  
  
此設計符合 [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)中的部署目標。  
  
如需您可以使用計劃和部署網頁 SSO 設計的詳細工作的清單，請參閱[檢查清單：實作網頁 SSO 設計](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
