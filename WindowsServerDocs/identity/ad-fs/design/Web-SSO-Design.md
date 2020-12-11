---
description: 深入瞭解：網頁 SSO 設計
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: 網頁 SSO 設計
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: e61402f12af51036c3eea739288ddd94e1644e5e
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049336"
---
# <a name="web-sso-design"></a>網頁 SSO 設計

在 Active Directory 同盟服務 AD FS 的 Web 單一 \- 登錄 \- \( SSO \) 設計中 \( \) ，使用者只需驗證一次，即可存取多個 AD FS \- 安全的應用程式或服務。 在此設計中，所有使用者都是外部的，且因為沒有夥伴組織而沒有同盟信任存在。 一般來說，當您想要透過網際網路提供個別取用者或客戶存取一或多個 AD FS 保護的服務或應用程式時，您會部署這項設計，如下圖所示。

![網頁 sso 設計](media/adfs2_WebSSODesign.gif)

使用網頁 SSO 設計時，通常 \- 會在周邊網路中裝載 AD FS 安全的應用程式或服務的組織，可以在周邊網路中維護不同的客戶帳戶存放區，讓您更輕鬆地將客戶帳戶與員工帳戶隔離。

您可以使用 Active Directory Domain Services \( AD DS \) 、SQL Server 或自訂屬性存放區，管理周邊網路中客戶的本機帳戶。

此設計符合 [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)中的部署目標。

如需可用來規劃和部署網頁 SSO 設計的詳細工作清單，請參閱＜ [Checklist: Implementing a Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md)＞。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
