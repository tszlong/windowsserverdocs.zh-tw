---
title: 在 Microsoft Online Service 經銷商合約的支援下，取代 O365 整合模組並購買/嘗試端點 URL
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833099"
---
# <a name="replace-o365-integration-module-buy-try-endpoint-url-in-support-of-microsoft-online-service-reseller-agreement"></a>在 Microsoft Online Service 經銷商合約的支援下，取代 O365 整合模組並購買/嘗試端點 URL

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

##  <a name="BKMK_O365"></a>   
 如果您是 Microsoft Online Service 經銷商合約 (MOSRA) 合作夥伴，以確保客戶註冊交易，會處理透過入口網站中，您必須取代 Windows Server Essentials 的 Office 365 整合模組所使用的端點 Url。  
  
 整合模組使用下列四個端點 URL：  
  
1.  Office 365 Enterprise 訂閱購買端點。  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   類型 = REG-SZ  
  
    -   機碼名稱 = MOSRASTDBUY  
  
    -   值 = *xxxxx*，其中 xxxxx 是您的企業訂閱購買 URL。 例如，值 = http://syndicatepartner.office365.com/enterprisebuy.html  
  
2.  Office 365 Enterprise 訂閱試用端點。  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   類型 = REG-SZ  
  
    -   機碼名稱 = MOSRASTDTRY  
  
    -   值 = *xxxxx*，其中 xxxxx 是您的企業訂閱購買 URL。 例如，值 = http://syndicatepartner.office365.com/enterprisetry.html  
  
3.  Office 365 Small Business Premium 訂閱購買端點。  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   類型 = REG-SZ  
  
    -   機碼名稱 = MOSRALITEBUY  
  
    -   值 = *xxxxx*，其中 xxxxx 是您的企業訂閱購買 URL。 例如，值 = http://syndicatepartner.office365.com/smallbizbuy.html  
  
4.  Office 365 Small Business Premium 訂用帳戶試用端點。  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   類型 = REG-SZ  
  
    -   機碼名稱 = MOSRALITETRY  
  
    -   值 = *xxxxx*，其中 xxxxx 是您的企業訂閱購買 URL。 例如，值 = http://syndicatepartner.office365.com/smallbiztry.html  
  
#### <a name="to-add-an-endpoint-url-key-to-the-registry"></a>將端點 URL 機碼新增到登錄  
  
1.  在參照電腦上，按一下 [開始]，輸入 **regedit**，然後按 ENTER。  
  
2.  在左窗格中，依序展開 **HKEY_LOCAL_MACHINE**、**SOFTWARE**、**Microsoft**、**Windows Server** 及 **MSO**。  
  
3.  如果 MSO 不存在，請在 [Windows Server] 上按一下滑鼠右鍵，指向 [新增]，按一下 [機碼]，然後輸入 **MSO** 作為機碼的名稱。  
  
4.  以滑鼠右鍵按一下 MSO，然後按一下 **[字串值]**。 輸入下列其中一個端點字串名稱，當做字串的名稱：  
  
    -   MOSRASTDBUY  
  
    -   MOSRASTDTRY  
  
    -   MOSRALITEBUY  
  
    -   MOSRALITETRY  
  
5.  在右窗格中，以滑鼠右鍵按一下新的字串，然後按一下 **[修改]**。  
  
6.  在 **[數值資料]** 文字方塊中輸入您的新端點 URL，然後按一下 **[確定]**。  
  
7.  針對在步驟 4 中列出的每個字串名稱重複步驟 4-6。  
  
## <a name="see-also"></a>另請參閱  

 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](Additional-Customizations.md)   
 [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)[建立和自訂映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](../install/Additional-Customizations.md)   
 [準備用於部署的映像](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](../install/Testing-the-Customer-Experience.md)

