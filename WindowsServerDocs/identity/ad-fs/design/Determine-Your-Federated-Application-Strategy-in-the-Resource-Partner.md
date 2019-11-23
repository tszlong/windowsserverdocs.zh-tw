---
ms.assetid: 9eab8c43-a0f2-4d19-a5a4-e1399f0d5f25
title: 決定資源夥伴的同盟應用程式策略
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0cc7a9202813cd3f8d45a72305d13ad197f5b04d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359158"
---
# <a name="determine-your-federated-application-strategy-in-the-resource-partner"></a>決定資源夥伴的同盟應用程式策略

在資源夥伴組織中設計新的 Active Directory 同盟服務 \(AD FS\) 基礎結構的重要部分，是決定將用來參與同盟的完整應用程式和服務，以及哪些帳戶夥伴將是這些資源的收件者。 在設計同盟應用程式和服務策略之前，請考量下列問題：  
  
-   您是否要啟用和部署同盟的 ASP.NET 應用程式或 Windows Communication Foundation \(WCF\) 服務？  
  
-   您公司網路上的使用者需要透過 Windows 整合式驗證要求同盟應用程式或服務的存取權限嗎？  
  
-   您周邊網路的使用者會使用同盟應用程式或服務嗎？ 如果會，是否需要 Windows 整合式驗證？  
  
-   裝載同盟應用程式的所有 Web 服務器都是執行 Windows Server 作業系統，而 Internet Information Services \(IIS\)嗎？  
  
-   誰是同盟應用程式或服務提供資源的對象？  
  
回答這些問題可協助您規劃穩固的 AD FS 設計。 它也可協助您建立符合成本效益和資源效率的同盟應用程式和服務策略。 如需有關設計最適合組織使用的同盟應用程式和服務策略等詳細資訊，請參閱本指南中的下列主題：  
  
-   [為 Active Directory 使用者提供宣告感知應用程式與服務的存取權](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [為 Active Directory 使用者提供其他組織的應用程式與服務的存取權](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
-   [為其他組織的使用者提供您的宣告感知應用程式與服務的存取權](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
如需如何建立宣告\-感知 ASP.NET 應用程式或 WCF 服務的詳細資訊，請參閱[Windows Identity FOUNDATION SDK](https://go.microsoft.com/fwlink/?LinkId=122266)。  
  
## <a name="see-also"></a>請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

