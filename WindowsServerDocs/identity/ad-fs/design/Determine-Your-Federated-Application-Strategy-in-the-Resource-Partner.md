---
ms.assetid: 9eab8c43-a0f2-4d19-a5a4-e1399f0d5f25
title: 決定資源夥伴的同盟應用程式策略
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aca47658cc5a20f63dbd59a26ebe135dd04def92
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811929"
---
# <a name="determine-your-federated-application-strategy-in-the-resource-partner"></a>決定資源夥伴的同盟應用程式策略

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

設計新的 Active Directory Federation Services 很重要的一部分\(AD FS\)資源夥伴組織中的基礎結構會判斷您一組完整的應用程式和服務，將用來參與同盟與哪一個帳戶夥伴將那些資源的收件者。 在設計同盟應用程式和服務策略之前，請考量下列問題：  
  
-   您會啟用並部署 ASP.NET 應用程式或 Windows Communication Foundation \(WCF\)同盟服務嗎？  
  
-   您公司網路上的使用者需要透過 Windows 整合式驗證要求同盟應用程式或服務的存取權限嗎？  
  
-   您周邊網路的使用者會使用同盟應用程式或服務嗎？ 如果會，是否需要 Windows 整合式驗證？  
  
-   裝載同盟應用程式執行的是 Windows Server 作業系統和 Internet Information Services 網頁伺服器的所有\(IIS\)嗎？  
  
-   誰是同盟應用程式或服務提供資源的對象？  
  
回答這些問題，可協助您規劃堅實的 AD FS 設計。 它也可協助您建立符合成本效益和資源效率的同盟應用程式和服務策略。 如需有關設計最適合組織使用的同盟應用程式和服務策略等詳細資訊，請參閱本指南中的下列主題：  
  
-   [提供您的 Active Directory 使用者存取您的宣告感知應用程式和服務](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [您 Active Directory 使用者提供存取權的應用程式和其他組織的服務](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
-   [提供使用者在另一個組織存取您宣告感知應用程式和服務](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
如需有關如何建立宣告\-感知的 ASP.NET 應用程式或 WCF 服務，請參閱 < [Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266)。  
  
## <a name="see-also"></a>另請參閱
[Windows Server 2012 中 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

