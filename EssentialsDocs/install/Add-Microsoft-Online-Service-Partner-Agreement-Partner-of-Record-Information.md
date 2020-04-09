---
title: 新增 Microsoft Online Service 合作夥伴合約列名的合作夥伴資訊
description: 說明如何使用 Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 9bd191d6-ecc5-4230-a88e-f3fc281cb956
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7a297ed077f4c1457bd1e59fc0ea22feedd5de0d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817583"
---
# <a name="add-microsoft-online-service-partner-agreement-partner-of-record-information"></a>新增 Microsoft Online Service 合作夥伴合約列名的合作夥伴資訊

>適用于： Windows Server 2016 Essentials、Windows Server 2012 R2 Essentials、Windows Server 2012 Essentials

##  <a name="BKMK_3rdLevelDomanNames"></a>   
 如果您是 Office 365 的 Microsoft Online Service 合作夥伴合約（MOSPA）合作夥伴，為了確保您在透過 Office 365 整合模組從 Windows Server Essentials 產生訂用帳戶要求時，可以正確補償，您必須建立包含記錄夥伴識別碼（POR ID）的登錄機碼。 會讀取下列資訊，並透過 Office 365 註冊 URL 將資訊傳遞給服務提供者。  
  
-   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO  
  
-   類型 = 字串值  
  
-   機碼名稱 = Partner  
  
-   值 = xxxxx，其中 xxxxx 是 POR ID  
  
#### <a name="to-add-the-por-id-key-to-the-registry"></a>若要將 POR ID 機碼新增至登錄  
  
1.  在參照電腦上，按一下 [開始]，輸入 **regedit**，然後按 ENTER。  
  
2.  在左窗格中，依序展開 **HKEY_LOCAL_MACHINE**、**SOFTWARE**、**Microsoft**，然後展開 **Windows Server**。  
  
3.  以滑鼠右鍵按一下 **Windows Server**，指向 **[新增]** ，然後按一下 **[機碼]** 。  
  
4.  輸入 **MSO** 做為機碼的名稱。  
  
5.  以滑鼠右鍵按一下您剛才建立的機碼，然後按一下 **[字串值]** 。  
  
6.  輸入 **Partner** 作為字串名稱，然後按 ENTER。  
  
7.  在右窗格中，以滑鼠右鍵按一下新的 **Partner** 字串，然後按一下 **[修改]** 。  
  
8.  在 **[數值資料]** 文字方塊中輸入您的 POR ID，然後按一下 **[確定]** 。  
  
## <a name="see-also"></a>另請參閱  

 [建立和自訂映射](Creating-and-Customizing-the-Image.md)   
 [其他自訂](Additional-Customizations.md)   
 [準備映射以進行部署](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)

 [建立和自訂映射](../install/Creating-and-Customizing-the-Image.md)   
 [其他自訂](../install/Additional-Customizations.md)   
 [準備映射以進行部署](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](../install/Testing-the-Customer-Experience.md)

