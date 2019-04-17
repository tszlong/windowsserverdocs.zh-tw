---
ms.assetid: f88238ea-d851-4129-8b4e-a3a62b813614
title: "檢視資源合作夥伴聯盟伺服器的角色"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6d4c7763d7204dd2340d10a234e48f47c96788dc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="review-the-role-of-the-federation-server-in-the-resource-partner"></a>檢視資源合作夥伴聯盟伺服器的角色

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

聯盟伺服器資源合作夥伴組織攔截收到安全性權杖中所傳送 account 聯盟伺服器的驗證簽署，並再問題自己的目的地 Web\ 型應用程式的安全性權杖。  
  
> [!NOTE]  
> 當聯盟的使用者存取 Web\ 為基礎的應用程式使用網頁瀏覽器時，聯盟伺服器資源合作夥伴組織中的組建將新的驗證 cookie，並將它寫入瀏覽器。 此 cookie 可讓 single\ sign\ 上 \(SSO\) 功能，因此，不需要重新登入聯盟伺服器 account 合作夥伴中，當使用者嘗試存取不同 Web\ 型應用程式資源合作夥伴使用者。  
  
在 [網站 SSO 設計，必須安裝至少一個聯盟伺服器周邊網路中。 聯盟網路 SSO 設計，有必須安裝 account 合作夥伴公司的企業網路至少一個聯盟伺服器並安裝在公司網路資源合作夥伴公司的至少一個聯盟伺服器。  
  
> [!NOTE]  
> 您可以設定資源合作夥伴組織中聯盟伺服器電腦之前，您必須先將電腦加入資源合作夥伴組織中的任何 Active Directory domain。 如需詳細資訊，請查看[檢查清單︰ 設定好聯盟伺服器](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md)。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

