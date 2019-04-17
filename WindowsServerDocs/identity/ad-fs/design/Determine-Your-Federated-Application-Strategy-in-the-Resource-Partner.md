---
ms.assetid: 9eab8c43-a0f2-4d19-a5a4-e1399f0d5f25
title: "判斷您聯盟應用程式中的策略資源合作夥伴"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aca47658cc5a20f63dbd59a26ebe135dd04def92
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="determine-your-federated-application-strategy-in-the-resource-partner"></a>判斷您聯盟應用程式中的策略資源合作夥伴

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

重要的部分新 Active Directory 同盟服務 \(AD FS\) 基礎結構資源合作夥伴組織中的設計判斷您的完整的應用程式和服務，可用於參與聯盟和的 account 合作夥伴將這些資源的收件者。 設計的聯盟應用程式和服務策略之前，請考慮下列問題：  
  
-   將會讓和部署 ASP.NET 應用程式或 Windows 通訊基本知識 \(WCF\) 服務聯盟嗎？  
  
-   將您企業網路上的使用者要求聯盟應用程式或透過 Windows 整合式驗證服務的存取權？  
  
-   聯盟應用程式或服務會使用您周邊網路中的使用者嗎？ 若是如此，請將 Windows 整合式驗證必須嗎？  
  
-   所有的網頁伺服器會執行 Windows Server 作業系統和 \(IIS\) 網際網路資訊服務主機聯盟應用程式？  
  
-   誰會聯盟應用程式或服務提供的資源，取得？  
  
這些問題的回答會協助您計畫 AD FS 設計。 它也會協助您建立聯盟應用程式與服務策略成本效益的和資源有效率。 如需關於設計的最適合聯盟應用程式與服務策略組織的詳細資訊，查看下列主題本指南：  
  
-   [提供您 Active Directory 的使用者存取您的宣告感知應用程式與服務](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [提供您的 Active Directory 使用者存取權的應用程式與其他公司的服務](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
-   [在另一部組織存取的使用者提供您宣告感知應用程式與服務](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
如需如何建立 ASP.NET claims\ 感知應用程式或服務 WCF 的詳細資訊，請查看[Windows 身分基本知識 SDK](https://go.microsoft.com/fwlink/?LinkId=122266)。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

