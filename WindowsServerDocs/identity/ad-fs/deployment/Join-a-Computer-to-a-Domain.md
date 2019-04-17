---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: "加入網域的電腦"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 641a1541143206d06973a6a0f11c689390abea21
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="join-a-computer-to-a-domain"></a>加入網域的電腦

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory 同盟服務 \(AD FS\) 運作的每一部電腦，作為聯盟伺服器必須加入網域。 聯盟的 proxy 伺服器可能會加入網域，但不一定。  
  
您尚未加入網域網頁伺服器，如果網頁伺服器裝載只 claims\ 感知應用程式。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="to-join-a-computer-to-a-domain"></a>若要加入網域的電腦  
  
1.  在**[開始]**畫面中，輸入**[控制台]**，然後按 ENTER 鍵。  
  
2.  瀏覽至**系統及安全性**，然後按**系統**。  
  
3.  在**電腦名稱、網域及工作群組設定**，按一下 [**變更設定**。  
  
4.  在**電腦名稱**索引標籤上，按**變更**。  
  
5.  在**的成員**，按一下 [**網域**，輸入名稱的網域，這台電腦將會加入，然後按一下 [ **[確定]**。  
  
6.  按一下**[確定]**，然後重新開機和。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單︰ 設定聯盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[檢查清單︰ 聯盟 Proxy 伺服器設定](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

