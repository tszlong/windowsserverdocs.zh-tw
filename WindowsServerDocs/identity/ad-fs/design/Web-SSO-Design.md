---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: "Web SSO 設計"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1b8344594c9fc477ed8424c716ec8d7f7fd91ef3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="web-sso-design"></a>Web SSO 設計

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在網頁中 Active Directory 同盟服務 \(AD FS\) Single\ Sign\ 上 \(SSO\) 設計，使用者必須一次多 AD FS\ 保護的應用程式或服務存取驗證。 在這種設計所有使用者都的外部，並不存在同盟信任因為都不有任何合作夥伴。 一般而言，當您想要提供一或多個 AD FS – 保護服務或應用程式個人消費者或客戶存取網際網路，如下所示部署這個設計。  
  
![web sso 設計](media/adfs2_WebSSODesign.gif)  
  
Web SSO 設計，通常主控 AD FS\ 保護的應用程式的組織或周邊網路的服務，可以維持不同的周邊網路，讓它更容易找出員工帳號客戶帳號客戶帳號存放區。  
  
您可以管理本機帳號周邊網路中針對使用 Active Directory Domain Services \(AD DS\)、 SQL Server 或自訂屬性存放區。  
  
這種設計相合部署目標中使用[提供您 Active Directory 使用者存取您宣告感知應用程式與服務](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)。  
  
如需詳細的工作，您可以使用計劃和部署網頁 SSO 設計的清單，請查看[檢查清單︰ 實作 Web SSO 設計](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md)。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
