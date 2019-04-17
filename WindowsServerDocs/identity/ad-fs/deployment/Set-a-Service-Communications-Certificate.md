---
ms.assetid: 638c89bd-87e6-484b-9d2e-8ae2a74227e5
title: "設定服務通訊憑證"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0d630372d82cf1519da82397a9c90d6a28903ed0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="set-a-service-communications-certificate"></a>設定服務通訊憑證

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Active Directory 同盟服務 \(AD FS\) 聯盟伺服器使用憑證服務通訊保護 Web 服務的資料傳輸與 Web 戶端或聯盟伺服器 proxy 安全通訊端層 \(SSL\) 通訊。 這是聯盟伺服器 SSL 憑證網際網路資訊服務 \(IIS\) 中使用相同憑證。  
  
若要變更服務通訊憑證 AD FS 管理 snap\ 中，您可以使用下列程序。  
  
> [!NOTE]  
> AD FS 管理 snap\ 中稱為伺服器驗證憑證的聯盟伺服器服務通訊的憑證。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)\ (go.microsoft.com\ fwlink\ 方式 http:///\/ # / 嗎？LinkId\ = 83477\)。   
  
### <a name="to-set-a-service-communications-certificate"></a>若要設定的服務通訊的憑證  
  
1.  在**[開始]**畫面中，輸入**AD FS 管理**，然後按 ENTER 鍵。  
  
2.  主控台中 double\ 按一下**服務**，然後按一下 [**的憑證**。  
  
3.  在**動作**窗格中，按**設定服務通訊憑證**連結。  
  
4.  在**選取服務通訊的憑證**對話方塊中，瀏覽到您想要為服務通訊憑證設定、 選取憑證檔案，然後按一下 [的憑證檔案**開放**。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單︰ 設定聯盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[聯盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)  
  

