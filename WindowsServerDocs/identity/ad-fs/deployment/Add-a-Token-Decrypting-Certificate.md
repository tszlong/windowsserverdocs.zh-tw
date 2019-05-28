---
ms.assetid: 27e1e299-0beb-4e86-8143-1ba031dc3502
title: 新增權杖解密憑證
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: cf89972120f3f0effa3eb1cf0fee6d29dbc8ed4e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192479"
---
# <a name="add-a-token-decrypting-certificate"></a>新增權杖解密憑證

同盟伺服器會使用語彙基元\-當信賴憑證者的合作對象同盟伺服器必須設定新的憑證做為主要解密憑證之後，使用較舊的憑證所簽發的權杖解密的解密憑證。 Active Directory Federation Services \(AD FS\)會使用 Secure Sockets Layer \(SSL\) Internet Information Services 的憑證\(IIS\)作為預設解密憑證。  
  
> [!CAUTION]  
> 憑證用於語彙基元\-解密至關重要的 Federation Service 的穩定性。 因為遺失或意外的移除基於此目的設定的任何憑證的中斷服務，您應該備份任何針對此目的設定的憑證。  
  
您可以使用下列程序來新增語彙基元\-解密憑證至 AD FS 管理嵌入式管理單元\-中從您已匯出的檔案。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  檢閱詳細使用適當帳戶和群組成員[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/嗎？LinkId\=83477\)。   
  
### <a name="to-add-a-token-decrypting-certificate"></a>若要新增權杖\-解密憑證  
  
1.  在 **開始**畫面上，輸入**AD FS 管理**，然後按 ENTER 鍵。  
  
2.  在主控台樹狀目錄中，按兩下\-按一下 **服務**，然後按一下**憑證**。  
  
3.  在 **動作**窗格中，按一下**新增權杖\-解密憑證**連結。  
  
4.  在 **瀏覽憑證檔案**對話方塊方塊中，瀏覽至您想要新增選取的憑證檔案，然後按一下 憑證檔案**開啟**。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[同盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)  
  

