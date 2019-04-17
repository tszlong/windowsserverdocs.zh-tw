---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: "確認 Proxy 伺服器，聯盟操作"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4900d8621b94a514a07bba55b2f7f3df5dd36353
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>確認 Proxy 伺服器，聯盟操作

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用下列程序，以確認聯盟 proxy 伺服器可以在 Active Directory 同盟服務 \(AD FS\) 同盟服務通訊。 執行此程序後執行**AD FS 聯盟伺服器 Proxy 設定精靈**來設定電腦，以在聯盟 proxy 伺服器的角色執行。 如需有關如何執行這個精靈的詳細資訊，請查看[設定電腦聯盟 Proxy 角色](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。  
  
> [!IMPORTANT]  
> 這項測試的結果是成功代聯盟伺服器 proxy 電腦上的特定事件事件檢視器中。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>若要確認操作聯盟 proxy 伺服器  
  
1.  聯盟伺服器 proxy 以系統管理員身分登入。  
  
2.  在**[開始]**畫面中，輸入**事件檢視器]**，然後按 ENTER 鍵。  
  
3.  在詳細資料窗格中，double\ 按一下**應用程式與服務登**，double\ 按**AD FS 事件**，然後按一下 [**系統管理員**。  
  
4.  在**263**欄中，尋找事件 ID 198。  
  
    如果聯盟 proxy 伺服器設定正確，您會看到新的事件的事件檢視器的事件編號 198 應用程式木頭中。 事件此驗證聯盟 proxy 伺服器成功開始使用現在已 online。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單︰ 聯盟 Proxy 伺服器設定](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

