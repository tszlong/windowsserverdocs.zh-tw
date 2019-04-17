---
ms.assetid: d0ba3c0d-869f-4e24-89d7-499da7576f22
title: "檢視中 Account 合作夥伴聯盟伺服器角色"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0914d32e8f24d5e7db0a25c733342c1bde3e0329
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-in-the-account-partner"></a>檢視中 Account 合作夥伴聯盟伺服器角色

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Active Directory 同盟服務 \(AD FS\) 聯盟伺服器的安全性權杖發行者功能。 聯盟伺服器產生宣告型群組清單儲存在本機屬性的值，並，讓使用者可以順暢地進行存取 Web\ browser\ 型應用程式到安全性權杖套件它們 \（使用單一 sign\ 上 \(SSO\)\) 裝載資源合作夥伴組織中。  
  
> [!NOTE]  
> 當您的使用者存取聯盟應用程式使用網頁瀏覽器時，聯盟伺服器會自動維護他們登入的狀態該 Web\ browser\ 型應用程式的使用者問題 cookie。 這些 cookie 包含宣告使用者。 Cookie 可以讓 SSO 功能，讓使用者不必輸入認證每次瀏覽不同的 Web\ browser\ 型應用程式，在資源合作夥伴。  
  
在 [網站 SSO 設計，周邊網路與網際網路存取應用程式的使用者想要的組織必須安裝聯盟伺服器 proxy 周邊網路中。 聯盟網路 SSO 設計，有必須安裝 account 合作夥伴公司的企業網路至少一個聯盟伺服器並安裝在公司網路資源合作夥伴公司的至少一個聯盟伺服器。  
  
> [!NOTE]  
> 您可以設定 account 合作夥伴組織聯盟伺服器電腦之前，您必須第一次將電腦加入任何位置使用聯盟伺服器來驗證使用者的樹系的 Active Directory 森林中的網域。 如需詳細資訊，請查看[檢查清單︰ 設定好聯盟伺服器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
