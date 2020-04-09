---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: 網頁 SSO 設計
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 47c946ac617cc64c224c1bc3153fcaf55c2d069c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858511"
---
# <a name="web-sso-design"></a>網頁 SSO 設計

在 Web Single\-登入\-在 Active Directory 同盟服務 \(AD FS\)的 \(SSO\) 設計中，使用者只需驗證一次，即可存取多個 AD FS\-安全的應用程式或服務。 在此設計中，所有使用者都是外部的，且因為沒有夥伴組織而沒有同盟信任存在。 一般而言，當您想要透過網際網路提供個別取用者或客戶存取一或多個 AD FS 保護的服務或應用程式時，您會部署這項設計，如下圖所示。  
  
![網頁 sso 設計](media/adfs2_WebSSODesign.gif)  
  
透過網頁 SSO 設計，通常會在周邊網路中裝載 AD FS\-安全應用程式或服務的組織，可以在周邊網路中維護不同的客戶帳戶存放區，這樣可讓您更輕鬆地隔離客戶帳戶與員工帳戶。  
  
您可以使用 Active Directory Domain Services \(AD DS\)、SQL Server 或自訂屬性存放區，在周邊網路中管理客戶的本機帳戶。  
  
此設計符合 [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)中的部署目標。  
  
如需可用來規劃和部署網頁 SSO 設計的詳細工作清單，請參閱＜ [Checklist: Implementing a Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md)＞。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
