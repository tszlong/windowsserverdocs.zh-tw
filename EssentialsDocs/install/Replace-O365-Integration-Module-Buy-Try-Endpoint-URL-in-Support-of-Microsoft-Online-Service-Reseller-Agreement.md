---
title: "取代 O365 整合模組購買-Try 端點 URL 支援 Microsoft Online Service 經銷商合約"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9860a6b9-baea-4bf0-9a9f-6f1a288f996e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b690cedd2f692cc6d11af6e05dd0cd4b4ea5a1d6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="replace-o365-integration-module-buy-try-endpoint-url-in-support-of-microsoft-online-service-reseller-agreement"></a>取代 O365 整合模組購買-Try 端點 URL 支援 Microsoft Online Service 經銷商合約

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

##  <a name="BKMK_O365"></a>   
 如果您的 Microsoft Online Service 經銷商合約 (MOSRA) 合作夥伴，以確保客戶註冊交易處理透過您的入口網站，您必須更換端點使用 Windows Server Essentials 的 Office 365 整合模組 Url。  
  
 整合模組使用下列四個端點 Url:  
  
1.  Office 365 企業版裝機費購買結束點。  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   輸入 = REG-SZ  
  
    -   按鍵名稱 = MOSRASTDBUY  
  
    -   值 = *wintab-xxxxx*，其中 wintab-xxxxx 是您的企業裝機費購買 URL。 例如，值 = http://syndicatepartner.office365.com/enterprisebuy.html  
  
2.  Office 365 企業版裝機費試用端點。  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   輸入 = REG-SZ  
  
    -   按鍵名稱 = MOSRASTDTRY  
  
    -   值 = *wintab-xxxxx*，其中 wintab-xxxxx 是您的企業裝機費購買 URL。 例如，值 = http://syndicatepartner.office365.com/enterprisetry.html  
  
3.  Office 365 小型企業 Premium 裝機費購買結束點。  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   輸入 = REG-SZ  
  
    -   按鍵名稱 = MOSRALITEBUY  
  
    -   值 = *wintab-xxxxx*，其中 wintab-xxxxx 是您的企業裝機費購買 URL。 例如，值 = http://syndicatepartner.office365.com/smallbizbuy.html  
  
4.  Office 365 小型企業 Premium 裝機費試用端點。  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   輸入 = REG-SZ  
  
    -   按鍵名稱 = MOSRALITETRY  
  
    -   值 = *wintab-xxxxx*，其中 wintab-xxxxx 是您的企業裝機費購買 URL。 例如，值 = http://syndicatepartner.office365.com/smallbiztry.html  
  
#### <a name="to-add-an-endpoint-url-key-to-the-registry"></a>若要新增端點 URL 鍵登錄  
  
1.  參考在電腦上，按一下 [ **[開始]**，輸入**regedit**，然後按 ENTER 鍵。  
  
2.  在左窗格中，展開**跳**，展開 [**軟體**，展開 [ **Microsoft**，展開 [ **Windows Server**，然後展開 [ **MSO**。  
  
3.  如果 MSO 不存在，以滑鼠右鍵按一下**Windows Server**，指向 [**新**，按一下 [**鍵**，然後輸入**MSO**按鍵的名稱。  
  
4.  MSO，以滑鼠右鍵按一下，然後按一下**字串值**。 輸入下列端點字串名稱名稱字串的其中一項：  
  
    -   MOSRASTDBUY  
  
    -   MOSRASTDTRY  
  
    -   MOSRALITEBUY  
  
    -   MOSRALITETRY  
  
5.  以滑鼠右鍵按一下右窗格中的新字串，然後按一下**修改]**。  
  
6.  輸入您在新的端點 URL**數值資料**文字方塊，然後再按一下**[確定]**。  
  
7.  重複步驟 4 6 列出執行「步驟 4 在每個字串的名稱。  
  
## <a name="see-also"></a>也了  

 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)[建立和自訂映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](../install/Additional-Customizations.md)   
 [準備部署映像](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](../install/Testing-the-Customer-Experience.md)

