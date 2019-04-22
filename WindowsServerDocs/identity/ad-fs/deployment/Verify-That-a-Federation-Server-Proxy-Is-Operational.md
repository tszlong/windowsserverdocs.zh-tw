---
ms.assetid: d555041a-709e-42c7-920a-8dfd2c7e0488
title: 驗證同盟伺服器 Proxy 運作正常
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 4900d8621b94a514a07bba55b2f7f3df5dd36353
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814619"
---
# <a name="verify-that-a-federation-server-proxy-is-operational"></a>驗證同盟伺服器 Proxy 運作正常

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您可以使用下列程序來確認同盟伺服器 proxy 可以與 Active Directory Federation Services 中的 Federation Service 通訊\(AD FS\)。 執行之後，執行此程序**AD FS 同盟伺服器 Proxy 設定精靈**設定電腦以同盟伺服器 proxy 角色中執行。 如需如何執行此精靈的詳細資訊，請參閱[同盟伺服器 Proxy 角色設定的電腦](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)。  
  
> [!IMPORTANT]  
> 此測試的結果是在同盟伺服器 Proxy 電腦上的事件檢視器中成功產生特定事件。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
### <a name="to-verify-that-a-federation-server-proxy-is-operational"></a>若要驗證同盟伺服器 proxy 可以運作  
  
1.  同盟伺服器 proxy，以系統管理員身分登入。  
  
2.  在 **開始**畫面上，輸入**事件檢視器**，然後按 ENTER 鍵。  
  
3.  在詳細資料窗格中，按兩下\-按一下  **Applications and Services Logs**、 雙精度浮點\-按一下**AD FS 事件**，然後按一下 **管理員**。  
  
4.  在 **[事件識別碼]** 欄中，尋找事件識別碼 198。  
  
    如果同盟伺服器 proxy 設定正確，您會看到新的事件，事件檢視器的事件識別碼為 198 的應用程式記錄檔中。 這個事件會確認同盟伺服器 proxy 服務已成功啟動，且已上線。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

