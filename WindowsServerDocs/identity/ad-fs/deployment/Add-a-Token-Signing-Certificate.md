---
ms.assetid: bbb84ea6-7e31-4442-85ab-a9447e7c19e8
title: 新增權杖簽署憑證
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ac9f9b95ad6226a8e3b7012e317899f1d48c60c9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192472"
---
# <a name="add-a-token-signing-certificate"></a>新增權杖簽署憑證


在 Active Directory Federation Services 中的同盟伺服器\(AD FS\)要求權杖\-簽署憑證讓攻擊者無法改變或盜用安全性權杖，以嘗試取得未經授權的存取為同盟的資源。 每個語彙基元\-簽署憑證包含私密金鑰的密碼編譯金鑰和用來數位簽章的公開金鑰\(透過私密金鑰\)安全性權杖。 稍後，夥伴同盟伺服器會收到這些機碼之後，他們驗證真實性\(透過公開金鑰\)加密的安全性權杖。  
  
> [!CAUTION]  
> 憑證用於語彙基元\-簽署至關重要的 Federation Service 的穩定性。 因為遺失或意外的移除基於此目的設定的任何憑證的中斷服務，您應該備份任何針對此目的設定的憑證。  
  
語彙基元\-簽署憑證應該鏈結到 Federation Service 中受信任的根。 您可以使用下列程序來新增語彙基元\-簽章憑證至 AD FS 管理嵌入式管理單元\-中從您已匯出的檔案。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  檢閱詳細使用適當帳戶和群組成員[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/嗎？LinkId\=83477\)。   
  
### <a name="to-add-a-token-signing-certificate"></a>若要新增權杖\-簽署憑證  
  
1.  在 **開始**畫面上，輸入**AD FS 管理**，然後按 ENTER 鍵。  
  
2.  在主控台樹狀目錄中，按兩下\-按一下 **服務**，然後按一下**憑證**。  
  
3.  在 **動作**窗格中，按一下**新增權杖\-簽署憑證**連結。  
  
4.  在 **瀏覽憑證檔案**對話方塊方塊中，瀏覽至您想要新增選取的憑證檔案，然後按一下 憑證檔案**開啟**。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[同盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)  
  

