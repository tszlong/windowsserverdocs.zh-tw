---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: "加入權杖簽署的憑證"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8a94e0724d6fd2a04e2fbfc22b3054b49d87f440
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="add-a-token-signing-certificate"></a>加入權杖簽署的憑證

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Active Directory 同盟服務 \(AD FS\) 聯盟伺服器需要 token\ 簽署的憑證，以避免攻擊者變更或仿冒的安全性權杖嘗試聯盟資源未經授權的存取。 每個 token\ 簽署的憑證包含密碼編譯私人和公開鍵是用來簽署 \（藉由私人 key\) 的安全性權杖。 之後，合作夥伴聯盟伺服器收到這些按鍵之後，他們驗證真確性 \（藉由公用 key\) 加密的安全性權杖。  
  
> [!CAUTION]  
> 用於 token\ 簽署的憑證的重大同盟服務的穩定性。 因為遺失或計畫的移除之任何設定為這個項目的的憑證可能會服務中斷，您應該備份設定為這個項目的任何憑證。  
  
信任的根同盟服務中應該鏈結 token\ 簽署的憑證。 您可以使用下列程序，AD FS 管理 snap\ 中將檔案，您將匯出 token\ 簽署的憑證。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)\ (go.microsoft.com\ fwlink\ 方式 http:///\/ # / 嗎？LinkId\ = 83477\)。   
  
### <a name="to-add-a-token-signing-certificate"></a>若要新增的 token\ 簽署的憑證  
  
1.  在**[開始]**畫面中，輸入**AD FS 管理**，然後按 ENTER 鍵。  
  
2.  主控台中 double\ 按一下**服務**，然後按一下 [**的憑證**。  
  
3.  在**動作**窗格中，按**新增 Token\ 簽署的憑證**連結。  
  
4.  在**瀏覽憑證檔案**對話方塊中，瀏覽至您想要新增、選取憑證檔案，然後再按憑證檔案**開放**。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單︰ 設定聯盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[聯盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)  
  

