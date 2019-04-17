---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: "部署聯盟伺服器"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f80425b6f062040c51357353038fd07ff0a79ae6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="interoperating-with-ad-fs-1x"></a>AD FS 進行相互操作 1.x

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Windows Server® 2012 Active Directory 同盟服務 \(AD FS\) 之間 AD FS 1 跨平台。*x*，完成一或多個下列工作，根據您的組織的需求：  
  
-   規劃跨平台與 Windows Server 2012 中的 AD FS 舊版 AD FS，並了解更多有關名稱 ID 宣告類型。 如需詳細資訊，請查看[規劃 AD FS 使用的跨平台 1.x](https://technet.microsoft.com/library/ff678040.aspx)。  
  
-   如果您將會傳送宣告從 Windows Server 2012，可由 AD FS 1 中 AD FS 同盟服務。*x*看到同盟服務，[檢查清單︰ 設定來傳送給 AD FS 以的宣告 AD FS 1.x 同盟服務](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md)。  
  
-   如果您將會傳送宣告的網頁伺服器執行 AD FS 1 裝載的應用程式可以使用的 Windows Server 2012 中 AD FS 同盟服務。*x* claims\ 感知網路代理程式中，查看[檢查清單︰ 設定來傳送給 AD FS 以的宣告 AD FS 1.x 宣告感知 Web 代理程式](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md)。  
  
-   如果您將會從 AD FS 1 傳送主張。*x*看到同盟服務由 Windows Server 2012，AD FS 同盟服務[檢查清單︰ 設定從 AD FS AD FS 使用宣告 1.x](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md)。  
  
## <a name="differences-between-federation-service-settings"></a>不同同盟服務設定  
雖然大部分的 AD FS 1。*x*同盟服務設定與設定 Windows Server 2012 中 AD FS 同盟服務類似的方式工作，已變更部分設定的名稱。 下表列出設定 AD FS 1 的名稱。*x*同盟服務與 Windows Server 2012 中 AD FS 同盟服務他們相同名稱。  
  
|AD FS 1.x 同盟服務設定|設定 Windows Server 2012 中對等 AD FS 同盟服務  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|Account 合作夥伴|宣告提供者信任  
|資源合作夥伴|可以廠商信任 
|應用程式|可以廠商信任  
|應用程式屬性|可以廠商信任屬性  
|應用程式 URL|可以廠商識別碼和 WS\-聯盟被動式端點 URL  
|聯盟服務 URI|聯盟服務識別碼  
|聯盟服務端點 URL|聯盟 WS\ 被動式端點 URL  
  
## <a name="see-also"></a>也了  
[AD FS 以及 AD FS 1.x 交互操作](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

