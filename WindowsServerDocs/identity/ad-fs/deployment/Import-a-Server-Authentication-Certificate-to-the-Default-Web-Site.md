---
ms.assetid: e1f2ce2d-b24f-4ccd-8add-9e69419fc6c1
title: "匯入伺服器驗證憑證的預設網站"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 7bc890c744de5cd86d4e8b0418e75512518f656c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="import-a-server-authentication-certificate-to-the-default-web-site"></a>匯入伺服器驗證憑證的預設網站

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

取得伺服器驗證憑證的憑證授權單位之後 \(CA\)，您必須手動安裝該憑證預設的網站上的每個聯盟伺服器或聯盟伺服器 proxy 伺服器陣列中的。  
  
Web 伺服器，您必須手動安裝伺服器驗證憑證 virtual directory 同盟的應用程式的所在位置的適當的網站上。  
  
如果您的設定發電廠，請務必相同執行這個程序，使用相同的設定 — 每個您伺服器上。  
  
> [!NOTE]  
> AD FS 管理 snap\ 中稱為伺服器驗證憑證的聯盟伺服器服務通訊的憑證。  
  
資格在**系統管理員**，或相當於、在本機電腦上的最低需求完成此程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
### <a name="to-import-a-server-authentication-certificate-to-the-default-web-site"></a>若要匯入伺服器驗證憑證來預設的網站  
  
1.  在**[開始]**畫面中，輸入**\(IIS\) 管理員]**，然後按 ENTER 鍵。  
  
2.  主控台中，按一下 [**電腦名稱**。  
  
3.  在中央窗格中，按一下 double\**伺服器的憑證**。  
  
4.  在**動作**窗格中，按**匯入**。  
  
5.  在**憑證匯入**對話方塊中，按**...** 按鈕。  
  
6.  瀏覽至 pfx 憑證檔案的位置，該反白，然後按一下**開放**。  
  
7.  輸入認證，密碼，然後按一下**[確定]**。  
  
## <a name="additional-references"></a>其他參考資料  
[檢查清單︰ 設定聯盟伺服器](Checklist--Setting-Up-a-Federation-Server.md)  
  
[檢查清單︰ 聯盟 Proxy 伺服器設定](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  
[聯盟伺服器的憑證需求](https://technet.microsoft.com/library/dd807040.aspx)  
  
[聯盟的 Proxy 伺服器的憑證需求](https://technet.microsoft.com/library/dd807054.aspx)  
   
  

