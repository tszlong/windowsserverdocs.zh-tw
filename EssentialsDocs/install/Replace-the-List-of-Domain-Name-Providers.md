---
title: "更換網域名稱提供者清單"
description: "告訴您如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 104d0412-2d77-4cd4-99f7-65a885522850
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ebaef0f88456f61fa229c9a18ee8014987fe7fa7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="replace-the-list-of-domain-name-providers"></a>更換網域名稱提供者清單

>適用於：Windows Server 2016 Essentials 程式集 Windows Server 2012 R2、Windows Server 2012 程式集

您可以取代網域名稱提供者清單，請按照下列工作顯示設定網域名稱精靈中：  
  

-   [建立推薦服務檔案](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [新增登錄參考電腦上的項目](Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

-   [建立推薦服務檔案](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [新增登錄參考電腦上的項目](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

  
###  <a name="BKMK_ReferralFiles"></a>建立推薦服務檔案  
 推薦服務管理工具建立檔案，用來定義清單的顯示設定網域名稱精靈中的網域名稱提供者的的設定。 建立的每個全球地區格式化的 XML 檔案，並包含在工具中指定的網域名稱提供者的資訊。 工具所建立的檔案必須位在透過網際網路，您管理的安全連結 (HTTPS) 資料夾。  
  
##### <a name="to-create-the-referral-files"></a>若要建立推薦檔案  
  
1.  打開推薦服務管理工具。  
  
2.  按一下**新增**。  
  
3.  在 [加入網域名稱提供者對話方塊中，輸入名稱的網域名稱提供者。  
  
4.  加入的網域名稱提供者所支援的最上層網域。 您只要按一下**新增**，輸入的最上層網域識別碼，然後選取 [支援的地區。 您可以選取**所有地區都能**。  
  
5.  輸入的網域名稱提供者的描述。  
  
6.  加入的網域名稱提供者相關聯的所有網站的 Url。  
  
7.  如果商標可用的網域名稱提供者，新增商標按一下**變更商標**。  
  
8.  按一下**儲存**。  
  
9. 重複步驟 2 到 8 每個您想要精靈中列出的網域名稱提供者。  
  
10. 您加入的網域名稱提供者的所有之後，請選擇 [會位於推薦檔案的資料夾。 請記住，當選擇資料夾，推薦檔案必須透過 HTTPS 連結來存取。  
  
11. 按一下**產生檔案，檔案系統以**。  
  
###  <a name="BKMK_AddRegistry"></a>新增登錄參考電腦上的項目  
 必須指定作業系統可以在何處推薦服務檔案新增登錄項目。  
  
##### <a name="to-add-a-key-to-the-registry"></a>若要新增按鍵登錄  
  
1.  參考在電腦上，按一下 [ **[開始]**，輸入**regedit**，然後按**輸入**。  
  
2.  在左窗格中，展開**跳**，展開**軟體**，展開 [ **Microsoft**，展開 [ **Windows Server**，展開 [**網域經理**，然後展開**提供者**。  
  
3.  以滑鼠右鍵按一下**E423C85D-6B1F-4583-95E0-449D8263BAC4**鍵，並再按**字串值**。  
  
4.  輸入**ReferralServerHttpsUri**字串，然後按下的名稱為**輸入**。  
  
5.  以滑鼠右鍵按一下新的**ReferralServerHttpsUri**字串右窗格中，然後按一下 [**修改]**。  
  

6.  輸入 HTTPS URL 用來存取您在建立推薦檔案[建立推薦服務檔案](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)，然後按一下 [ **[確定]**。  

6.  輸入 HTTPS URL 用來存取您在建立推薦檔案[建立推薦服務檔案](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)，然後按一下 [ **[確定]**。  

  
    > [!IMPORTANT]
    >  需要 URL 的結尾斜線（/）。  
  
###  <a name="BKMK_ReplaceDomainNameProviders"></a>網域名稱狀態的問題  
 如果合作夥伴加入網域名稱提供者，並使用 Windows Server Essentials sdk 的應用程式介面 (API) 來設定憑證不明、失敗和 CertificateRequestNotSubmitted 狀態，客戶收到錯誤訊息和設定的結果。 這是因為案例由例外除了退貨狀態。  
  
 下列網域狀態會失敗並應該錯誤與報告：  
  
-   失敗  
  
-   PendingCustomerInterventionRequired  
  
-   PurchaseFailed  
  
-   DomainNotFound  
  
-   InRenewalCustomerInterventionRequired  
  
-   RenewalFailed  
  
 下列網域狀態成功與應該報告的成功為：  
  
-   準備好  
  
-   擱置中  
  
-   InRenewal  
  
## <a name="see-also"></a>也了  

 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](Additional-Customizations.md)   
 [準備部署映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](Testing-the-Customer-Experience.md)

 [建立和自訂映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他的自訂項目](../install/Additional-Customizations.md)   
 [準備部署映像](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶體驗](../install/Testing-the-Customer-Experience.md)

