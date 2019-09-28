---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: 網頁 SSO 設計
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d7f52cd36588a1e5de4536a760c38c147dd1e003
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407837"
---
# <a name="web-sso-design"></a>網頁 SSO 設計

在 Web Single @ no__t-0Sign @ no__t-1On 中 \(SSO @ no__t-3 設計 Active Directory 同盟服務 \(AD FS @ no__t-5，使用者只需驗證一次，即可存取多個 AD FS @ no__t-6secured 應用程式或服務。 在此設計中，所有使用者都是外部的，且因為沒有夥伴組織而沒有同盟信任存在。 一般而言，當您想要透過網際網路提供個別取用者或客戶存取一或多個 AD FS 保護的服務或應用程式時，您會部署這項設計，如下圖所示。  
  
![網頁 sso 設計](media/adfs2_WebSSODesign.gif)  
  
透過網頁 SSO 設計，通常會在周邊網路中裝載 AD FS @ no__t 0secured 應用程式或服務的組織，可以在周邊網路中維護不同的客戶帳戶存放區，讓客戶帳戶更容易隔離。員工帳戶。  
  
您可以使用 Active Directory Domain Services \(AD DS @ no__t-1、SQL Server 或自訂屬性存放區，來管理周邊網路中客戶的本機帳戶。  
  
此設計符合 [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)中的部署目標。  
  
如需您可以用來規劃及部署網頁 SSO 設計的詳細工作清單，請參閱 @no__t 0Checklist：執行網頁 SSO 設計 @ no__t-0。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
