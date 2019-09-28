---
ms.assetid: ad61c586-ba8a-4534-8824-b45994d60c6b
title: 驗證同盟伺服器運作正常
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6d27c8d69affe001630d8deaa2c21f334f8f86ad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408315"
---
# <a name="verify-that-a-federation-server-is-operational"></a>驗證同盟伺服器運作正常


您可以使用下列程序來確認同盟伺服器可以運作，也就是說，相同網路上的所有用戶端都可以連線到新的同盟伺服器。  
  
本機電腦上 **Users**、**Backup Operators**、**Power Users**、**Administrators** 或具有同等權限的成員資格，至少需要完成此程序。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>程序 1：確認該同盟伺服器運作正常  
  
1.  若要確認已\(在\)同盟伺服器上正確設定 Internet Information Services IIS，請登入與同盟伺服器位於相同樹系中的用戶端電腦。  
  
2.  開啟瀏覽器視窗，在網址列中輸入同盟伺服器的 DNS 主機名稱，然後為新的同盟伺服器附加/adfs/fs/federationserverservice.asmx，例如：  
  
    **https://fs1.fabrikam.com/adfs/fs/federationserverservice.asmx**  
  
3.  按 ENTER 鍵，然後在同盟伺服器電腦上完成下一個程序。 如果您看到 [**此網站的安全性憑證有問題**] 訊息，請按一下 [**繼續流覽此網站**]。  
  
    預期的輸出是顯示 XML 以及服務描述文件。 如果出現此頁面，表示同盟伺服器上的 IIS 可以運作，而且成功提供頁面服務。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>程式2：確認該同盟伺服器運作正常  
  
1.  以系統管理員身分登入新的同盟伺服器。  
  
2.  在 [**開始**] 畫面上，輸入**事件檢視器**，然後按 enter。  
  
3.  在詳細資料窗格中，\-按兩下 [**應用程式及服務記錄**檔]，再按兩下\-[ **AD FS 事件**]，然後按一下 [系統**管理員**]。  
  
4.  在 [**事件識別碼**] 欄中，尋找事件識別碼100。 如果同盟伺服器已正確設定，您會在事件檢視器的應用程式記錄檔中看到一個新事件，其事件識別碼為100。 這個事件會確認同盟伺服器是否能夠順利與同盟服務通訊。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單：設定同盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  

