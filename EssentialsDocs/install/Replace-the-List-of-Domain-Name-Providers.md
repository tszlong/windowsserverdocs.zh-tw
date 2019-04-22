---
title: 取代網域名稱提供者清單
description: 描述如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820019"
---
# <a name="replace-the-list-of-domain-name-providers"></a>取代網域名稱提供者清單

>適用於：Windows Server 2016 Essentials、 Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

您可以透過完成下列工作，取代設定網域名稱精靈中顯示的網域名稱提供者清單：  
  

-   [建立轉介服務檔案](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [在參照電腦上的登錄中新增項目](Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

-   [建立轉介服務檔案](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)  
  
-   [在參照電腦上的登錄中新增項目](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_AddRegistry)  

  
###  <a name="BKMK_ReferralFiles"></a> 建立轉介服務檔案  
 轉介服務管理工具會建立一組檔案，這些檔案用來定義設定網域名稱精靈中顯示的網域名稱提供者清單。 會針對全球每個地區各建立一個 XML 格式的檔案，檔案中會包含您在工具中指定的網域名稱提供者。 此工具建立的檔案必須位於可在網際網路上透過您所管理的安全連結 (HTTPS) 存取的資料夾中。  
  
##### <a name="to-create-the-referral-files"></a>若要建立轉介檔案  
  
1.  開啟 [轉介服務管理工具]。  
  
2.  按一下 **\[新增\]**。  
  
3.  在 [新增網域名稱提供者] 對話方塊中，輸入網域名稱提供者的名稱。  
  
4.  新增該網域名稱提供者所支援的頂層網域。 若要執行此動作，請按一下 **[新增]**，輸入頂層網域識別項，然後選取支援的地區。 您可以選取 **[所有地區]**。  
  
5.  輸入網域名稱提供者的描述。  
  
6.  新增所有與網域名稱提供者關聯之網站的 URL。  
  
7.  如果網域名稱提供者有可用的標誌，請按一下 **[變更標誌]** 以新增標誌。  
  
8.  按一下 [儲存] 。  
  
9. 針對每個要列在精靈中的網域名稱提供者重複步驟 2 到 8。  
  
10. 新增完所有的網域名稱提供者後，選擇要用於放置轉介檔案的資料夾。 選擇資料夾時，請記得必須能夠透過 HTTPS 連結來存取轉介檔案。  
  
11. 按一下 **[產生檔案至檔案系統]**。  
  
###  <a name="BKMK_AddRegistry"></a> 在參照電腦上的登錄中新增項目  
 您必須新增登錄項目，以指定作業系統可以找到轉介服務檔案的位置。  
  
##### <a name="to-add-a-key-to-the-registry"></a>若要新增登錄機碼  
  
1.  在參照電腦上，按一下**開始**，輸入**regedit**，然後按**Enter**。  
  
2.  在左窗格中，依序展開 **HKEY_LOCAL_MACHINE**、**SOFTWARE**、**Microsoft**、**Windows Server**、**Domain Managers** 和 **Providers**。  
  
3.  以滑鼠右鍵按一下 **E423C85D-6B1F-4583-95E0-449D8263BAC4** 機碼，然後按一下 **[字串值]**。  
  
4.  輸入 **ReferralServerHttpsUri** 作為字串名稱，然後按 **ENTER**。  
  
5.  在右窗格中，以滑鼠右鍵按一下新的 **ReferralServerHttpsUri** ，然後按一下 **[修改]**。  
  

6.  輸入用於存取您在 [Create the referral service files](Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)中建立之轉介檔案的 HTTPS URL，然後按一下 [確定] 。  

6.  輸入用於存取您在 [Create the referral service files](../install/Replace-the-List-of-Domain-Name-Providers.md#BKMK_ReferralFiles)中建立之轉介檔案的 HTTPS URL，然後按一下 [確定] 。  

  
    > [!IMPORTANT]
    >  URL 結尾必須有正斜線 (/)。  
  
###  <a name="BKMK_ReplaceDomainNameProviders"></a> 網域名稱狀態問題  
 如果合作夥伴新增網域名稱提供者，並使用 Windows Server Essentials SDK 中的應用程式開發介面 (API)，來設定憑證的 Unknown、 Failed 及 CertificateRequestNotSubmitted 狀態，客戶會收到不正確訊息及設定結果。 這是因為此情況是依例外狀況處理，而不是傳回狀態。  
  
 下列網域狀態為失敗，而且應該報告為錯誤：  
  
-   Failed  
  
-   PendingCustomerInterventionRequired  
  
-   PurchaseFailed  
  
-   DomainNotFound  
  
-   InRenewalCustomerInterventionRequired  
  
-   RenewalFailed  
  
 下列網域狀態為成功，而且應該報告為成功：  
  
-   就緒  
  
-   Pending  
  
-   InRenewal  
  
## <a name="see-also"></a>另請參閱  

 [建立和自訂映像](Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](Additional-Customizations.md)   
 [準備用於部署的映像](Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](Testing-the-Customer-Experience.md)

 [建立和自訂映像](../install/Creating-and-Customizing-the-Image.md)   
 [其他自訂項目](../install/Additional-Customizations.md)   
 [準備用於部署的映像](../install/Preparing-the-Image-for-Deployment.md)   
 [測試客戶經驗](../install/Testing-the-Customer-Experience.md)

