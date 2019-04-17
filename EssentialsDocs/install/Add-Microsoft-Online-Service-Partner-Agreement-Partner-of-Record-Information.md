---
title: "加入 Microsoft Online Service 合作夥伴合約合作夥伴資料錄資訊"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9bd191d6-ecc5-4230-a88e-f3fc281cb956
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 39ce43228cd7392bcc86de4a410c52676ce15047
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="add-microsoft-online-service-partner-agreement-partner-of-record-information"></a>加入 Microsoft Online Service 合作夥伴合約合作夥伴資料錄資訊

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

##  <a name="BKMK_3rdLevelDomanNames"></a>   
 如果您的 Office 365 Microsoft Online Service 合作夥伴合約 (MOSPA) 合作夥伴，以確保您的正確補償裝機費要求來自 Windows Server Essentials 透過 Office 365 整合模組，當您需要建立登錄按鍵包含您的合作夥伴資料錄驗證 (埠 ID)。 下列是讀取和通過註冊 Url Office 365 服務提供者。  
  
-   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO  
  
-   輸入 = 字串值。  
  
-   按鍵名稱 = 合作夥伴  
  
-   值 = 的 wintab-xxxxx，其中 wintab-xxxxx 是您埠 ID  
  
#### <a name="to-add-the-por-id-key-to-the-registry"></a>若要新增埠 ID 按鍵登錄  
  
1.  參考在電腦上，按一下 [ **[開始]**，輸入**regedit**，然後按 ENTER 鍵。  
  
2.  在左窗格中，展開**跳**，展開 [**軟體**，展開 [ **Microsoft**，然後展開 [ **Windows Server**。  
  
3.  以滑鼠右鍵按一下**Windows Server**，指向 [**新**，然後按一下 [**鍵**。  
  
4.  輸入**MSO**鍵的名稱。  
  
5.  以滑鼠右鍵按一下您剛鍵建立，，然後按一下**字串值**。  
  
6.  輸入**合作夥伴**字串，然後按下 ENTER 的名稱。  
  
7.  以滑鼠右鍵按一下新的**合作夥伴**字串右窗格中，然後按一下 [**修改]**。  
  
8.  輸入您的埠 ID 在**數值資料**文字方塊，然後再按一下**[確定]**。  
  
## <a name="see-also"></a>也了  

 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)

 [建立和自訂映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](../install/Additional-Customizations.md)   
 [準備部署映像](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](../install/Testing-the-Customer-Experience.md)

