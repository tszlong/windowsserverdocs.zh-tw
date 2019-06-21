---
title: 步驟 3 計劃 OTP 憑證部署
description: 本主題是 Windows Server 2016 中的 OTP 驗證部署遠端存取快速入門的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eca02eeb-d92d-463e-aae0-1f7038ba26fe
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4102fc4f7eacf0b407446717caa83e4e5f70ae57
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282373"
---
# <a name="step-3-plan-otp-certificate-deployment"></a>步驟 3 計劃 OTP 憑證部署

>適用於：Windows Server （半年通道），Windows Server 2016

規劃 RADIUS 伺服器之後, 您必須規劃憑證授權單位 (CA) 需求，包括單次密碼 (OTP) 憑證，OTP 憑證範本和使用遠端註冊授權單位憑證將發行 CA登入所有的 DirectAccess 用戶端 OTP 的憑證要求的存取伺服器。 這些憑證可用，如下所示：  
  
1.  DirectAccess 用戶端要求 OTP 的憑證，並遠端存取伺服器收到要求。  
  
2.  遠端存取伺服器會驗證 OTP 認證之後若有效的話，伺服器會做為登錄授權單位，並簽署 OTP 憑證註冊要求使用存留較短的簽章憑證。  
  
3.  遠端存取伺服器會傳回給 DirectAccess 用戶端來傳送簽署的憑證註冊要求  
  
4.  然後，用戶端會註冊使用由伺服器簽署憑證註冊要求的 CA 的 OTP 憑證。  
  
5.  CA 會驗證認證和要求。  
  
|工作|描述|  
|----|--------|  
|[3.1 規劃 OTP CA](#bkmk_3_1_CA)|規劃憑證授權單位 (CA)，若要使用 OTP 驗證的 DirectAccess 用戶端發出憑證。|  
|[3.2 規劃 OTP 憑證範本](#bkmk_3_2_OTP_Cert)|規劃 OTP 憑證範本。|
|[3.3 計劃註冊授權單位的憑證](#bkmk_33RACert)|計劃註冊授權單位的憑證簽署所有的 OTP 驗證憑證要求。|

## <a name="bkmk_3_1_CA"></a>3.1 規劃 OTP CA  
若要部署 DirectAccess 使用單次密碼驗證 (OTP)，您需要內部 CA 來簽發 OTP 驗證憑證給 DirectAccess 用戶端電腦。 基於此目的，您可以使用相同的內部 CA，您用來發出用於規則的 IPsec 電腦驗證的憑證。  
  
## <a name="bkmk_3_2_OTP_Cert"></a>3.2 規劃 OTP 憑證範本  
每個 DirectAccess 用戶端需要 OTP 驗證憑證，才能存取內部網路。 您必須在內部的 OTP 憑證的 CA 上設定範本。 設定 OTP 憑證範本時，請注意下列：  
  
-   所有需要執行 OTP 驗證的使用者必須具有讀取和註冊權限，才能使用此範本。  
  
-   主體名稱應從 Active Directory 資訊，以確保主體名稱符合的 OTP 使用者名稱，而不是執行憑證要求的遠端存取伺服器的名稱來建立。 主體名稱必須是完整的辨別名稱的格式，且主體替代名稱必須以 UPN 格式。 這可確保已註冊的 OTP 憑證適用於智慧卡 Kerberos 驗證。  
  
-   必須智慧卡登入憑證的用途。  
  
-   發行必須要求一個授權簽章。 簽章必須與登錄授權單位簽署的憑證範本中設定的預先定義 DirectAccess OTP 應用程式原則設定。  
  
-   有效期間應該設定為一小時。  
  
    > [!NOTE]  
    > 在 CA 伺服器所在的 Windows Server 2003 電腦的情況下，則必須設定範本不同的電腦上。 這是因為，設定**有效期**在小時是不可能執行 Windows 2008/Vista 之前的版本時。 如果您用來設定範本的電腦沒有安裝憑證服務角色，或它是用戶端電腦，您可能需要安裝憑證範本 嵌入式管理單元。 如需有關這個主題中，按一下[此處](https://technet.microsoft.com/library/cc732445.aspx)。  
  
-   更新期間應該設定為 0。  
  
-   （選擇性）憑證及要求不應儲存在 CA 資料庫。  
  
-   憑證增強金鑰使用方法參數必須設定正確，如下：  
  
    -   Directaccess 註冊簽署的憑證範本會使用金鑰 1.3.6.1.4.1.311.81.1.1。  
  
    -   OTP 驗證憑證範本使用的金鑰 1.3.6.1.4.1.311.20.2.2 金鑰。  
  
## <a name="bkmk_33RACert"></a>3.3 計劃註冊授權單位的憑證  
當 DirectAccess 用戶端要求的 OTP 憑證時，遠端存取伺服器會接收從用戶端的要求。 遠端存取伺服器會簽署所有的 OTP 憑證要求，使用登錄授權單位憑證的用戶端。 只有當要求由遠端存取伺服器上的登錄授權單位憑證簽署，在 CA 簽發憑證。 憑證必須由內部 CA 發行，無法自我簽署憑證。 它沒有由發行 OTP 憑證的 CA 發行，但發出 OTP 憑證的 CA 必須信任發出登錄授權單位簽署憑證的 CA。  
  
## <a name="BKMK_Links"></a>另請參閱  
  
-   [步驟 4：規劃遠端存取伺服器上的 OTP](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  


