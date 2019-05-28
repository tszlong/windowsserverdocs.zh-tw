---
ms.assetid: 10d6723e-c857-43da-9d2d-acb5641d3da8
title: 將電腦加入網域
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 02df9659ee3a1121c0cee3f7c5fa21b91c36b87c
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192049"
---
# <a name="join-a-computer-to-a-domain"></a>將電腦加入網域

Active Directory Federation services \(AD FS\)正常運作，每個函式的同盟伺服器必須加入網域的電腦。 同盟伺服器 proxy 可能加入網域，但這不是需求。  
  
您就不必在 Web 伺服器加入網域，如果 Web 伺服器裝載宣告\-感知應用程式。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
### <a name="to-join-a-computer-to-a-domain"></a>若要將電腦加入網域  
  
1.  在 **啟動**畫面上，輸入**控制台**，然後按 ENTER 鍵。  
  
2.  瀏覽至**系統及安全性**，然後按一下**系統**。  
  
3.  在 [電腦名稱、網域及工作群組設定]  底下，按一下 [變更設定]  。  
  
4.  在 [電腦名稱]  索引標籤中，按一下 [變更]  。  
  
5.  底下**隸屬**，按一下**網域**，輸入您想要以聯結，然後按一下 這台電腦的網域名稱**確定**。  
  
6.  按一下 **[確定]** ，然後重新啟動電腦。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[檢查清單：設定同盟伺服器 Proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

