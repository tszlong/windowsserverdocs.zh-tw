---
ms.assetid: ad61c586-ba8a-4534-8824-b45994d60c6b
title: "請確認聯盟伺服器操作"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2034b4c35061879a64004486395d0887c59087b2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="verify-that-a-federation-server-is-operational"></a>請確認聯盟伺服器操作

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以使用下列程序來確認聯盟伺服器操作。是的在相同網路的任何 client 可以瑞曲之戰新聯盟伺服器。  
  
成員資格**使用者**，**備份電信業者**、**進階使用者**、**系統管理員**或相當於、 在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>程序 1︰ 驗證操作聯盟伺服器  
  
1.  要驗證網際網路資訊服務 \(IIS\) 正確設定聯盟伺服器上 client 的電腦位於聯盟伺服器相同樹系登入。  
  
2.  打開瀏覽器視窗，在網址列輸入聯盟 DNS 伺服器的主機名稱，然後附加 /adfs/fs/federationserverservice.asmx 新聯盟伺服器，例如：  
  
    **https://fs1.fabrikam.com/adfs/fs/federationserverservice.asmx**  
  
3.  按下 ENTER，並完成聯盟伺服器電腦上的下一步程序。 如果您看見 [**這個網站的安全性憑證有問題**，按一下 [**瀏覽此網站繼續**。  
  
    輸出預期會顯示的 XML 文件服務描述與。 如果出現此頁面，IIS 聯盟伺服器上的為成功正常運作且波頁面。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>程序 2︰ 驗證操作聯盟伺服器  
  
1.  以系統管理員身分登入新的聯盟伺服器。  
  
2.  在**[開始]**畫面中，輸入**事件檢視器]**，然後按 ENTER 鍵。  
  
3.  在詳細資料窗格中，double\ 按一下**應用程式與服務登**，double\ 按**AD FS 事件**，然後按一下 [**系統管理員**。  
  
4.  在**263**欄中，尋找事件 ID 100。 如果聯盟伺服器設定正確，您會看到新的事件-中的應用程式登入的事件檢視器 — 的事件編號 100。 事件此驗證聯盟伺服器是能順利與同盟服務通訊。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單︰ 設定聯盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  

