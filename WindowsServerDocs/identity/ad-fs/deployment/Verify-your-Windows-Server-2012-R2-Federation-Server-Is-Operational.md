---
ms.assetid: 1115d276-00f6-4c23-9278-eedcc31295d8
title: 確認您 Windows Server 2012 R2 同盟伺服器作業
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2df8a00a953196d7ca19ea0d164abbbf6eefd829
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840739"
---
# <a name="verify-your-windows-server-2012-r2-federation-server-is-operational"></a>確認您 Windows Server 2012 R2 同盟伺服器作業

>適用於：Windows Server 2016, Windows Server 2012 R2

您可以使用下列程序來確認同盟伺服器可以運作，也就是說，相同網路上的所有用戶端都可以連線到新的同盟伺服器。  
  
本機電腦上 **Users**、**Backup Operators**、**Power Users**、**Administrators** 或具有同等權限的成員資格，至少需要完成此程序。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
### <a name="procedure-1-to-verify-that-a-federation-server-is-operational"></a>程序 1：確認該同盟伺服器運作正常  
  
1.  若要確認 Internet Information Services \(IIS\)在同盟伺服器上，登入用戶端電腦位於相同的樹系的同盟伺服器已正確設定。  
  
2.  開啟瀏覽器視窗，在網址列中輸入同盟伺服器的 DNS 主機名稱，然後附加\/adfs\/fs\/為 federationserverservice.asmx 它適用於新的同盟伺服器，例如：  
  
    **https:\/\/fs1.fabrikam.com\/adfs\/fs\/federationserverservice.asmx**  
  
3.  按 ENTER 鍵，然後在同盟伺服器電腦上完成下一個程序。 如果您看到 **[此網站的安全性憑證有問題]** 訊息，按一下 **[繼續瀏覽此網站]**。  
  
    預期的輸出是顯示 XML 以及服務描述文件。 如果出現此頁面，表示同盟伺服器上的 IIS 可以運作，而且成功提供頁面服務。  
  
若要完成此程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
### <a name="procedure-2-to-verify-that-a-federation-server-is-operational"></a>程序 2:確認該同盟伺服器運作正常  
  
1.  以系統管理員身分登入新的同盟伺服器。  
  
2.  在 **開始**畫面上，輸入**事件檢視器**，然後按 ENTER 鍵。  
  
3.  在詳細資料窗格中，按兩下\-按一下  **Applications and Services Logs**、 雙精度浮點\-按一下**AD FS 事件**，然後按一下 **管理員**。  
  
4.  在 [**事件識別碼**] 欄中，尋找事件識別碼為 100。 如果同盟伺服器的設定正確，您會看到新的事件-事件檢視器的應用程式記錄檔中，與事件識別碼為 100。 這個事件會確認同盟伺服器能夠成功與 Federation Service 進行通訊。  
  
## <a name="see-also"></a>另請參閱 

[AD FS 部署](../../ad-fs/AD-FS-Deployment.md)  

[Windows Server 2012 R2 AD FS 部署指南](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[部署同盟伺服器陣列](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
   
  

