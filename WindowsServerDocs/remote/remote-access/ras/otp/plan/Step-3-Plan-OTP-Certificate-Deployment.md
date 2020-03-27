---
title: 步驟3規劃 OTP 憑證部署
description: 本主題是在 Windows Server 2016 中使用 OTP 驗證部署遠端存取指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eca02eeb-d92d-463e-aae0-1f7038ba26fe
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d34630b4faa8012eee73967a99bc0541f1305a09
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313525"
---
# <a name="step-3-plan-otp-certificate-deployment"></a>步驟3規劃 OTP 憑證部署

>適用於：Windows Server (半年通道)、Windows Server 2016

在規劃 RADIUS 伺服器之後，您必須規劃憑證授權單位單位（CA）需求，包括將會發出單次密碼（OTP）憑證的 CA、OTP 憑證範本，以及遠端所使用的登錄授權單位憑證。存取伺服器以簽署所有 DirectAccess 用戶端 OTP 憑證要求。 這些憑證的使用方式如下：  
  
1.  DirectAccess 用戶端會要求 OTP 憑證，而遠端存取服務器會收到要求。  
  
2.  遠端存取服務器會驗證 OTP 認證，如果有效，伺服器會做為登錄授權單位，並使用短期的簽署憑證來簽署 OTP 憑證註冊要求。  
  
3.  遠端存取服務器會將已簽署的憑證註冊要求傳回 DirectAccess 用戶端  
  
4.  接著，用戶端會使用伺服器所簽署的憑證註冊要求，從 CA 註冊 OTP 憑證。  
  
5.  CA 會驗證認證和要求。  
  
|工作|描述|  
|----|--------|  
|[3.1 規劃 OTP CA](#bkmk_3_1_CA)|規劃憑證授權單位單位（CA），以用來頒發證書給 DirectAccess 用戶端進行 OTP 驗證。|  
|[3.2 規劃 OTP 憑證範本](#bkmk_3_2_OTP_Cert)|規劃 OTP 憑證範本。|
|[3.3 規劃登錄授權單位憑證](#bkmk_33RACert)|規劃登錄授權單位憑證來簽署所有的 OTP 驗證憑證要求。|

## <a name="31-plan-the-otp-ca"></a><a name="bkmk_3_1_CA"></a>3.1 規劃 OTP CA  
若要使用單次密碼驗證（OTP）部署 DirectAccess，您需要內部 CA 將 OTP 驗證憑證發行到 DirectAccess 用戶端電腦。 基於此目的，您可以使用您用來發行用於定期 IPsec 電腦驗證之憑證的相同內部 CA。  
  
## <a name="32-plan-the-otp-certificate-template"></a><a name="bkmk_3_2_OTP_Cert"></a>3.2 規劃 OTP 憑證範本  
每個 DirectAccess 用戶端都需要 OTP 驗證憑證，才能取得內部網路的存取權。 您必須在您的內部 CA 上設定 OTP 憑證的範本。 設定 OTP 憑證範本時，請注意下列事項：  
  
-   所有需要執行 OTP 驗證的使用者，都必須擁有此範本的 [讀取] 和 [註冊] 許可權。  
  
-   主體名稱應從 Active Directory 資訊建立，以確保主體名稱符合 OTP 使用者名稱，而不是執行憑證要求的遠端存取服務器名稱。 [主體名稱] 必須是 [完整辨別名稱] 格式，而 [主體替代名稱] 必須是 UPN 格式。 這可確保已註冊的 OTP 憑證對智慧卡 Kerberos 驗證有效。  
  
-   憑證的用途必須是智慧卡登入  
  
-   發行必須要求一個授權簽章。 簽章必須以登錄授權單位簽署憑證範本中所設定的預先定義 DirectAccess OTP 應用程式原則來設定。  
  
-   有效期間應設定為一小時。  
  
    > [!NOTE]  
    > 在 CA 伺服器是 Windows Server 2003 電腦的情況下，您必須在不同的電腦上設定範本。 這是因為在執行 2008/Vista 之前的 Windows 版本時，不可能設定以小時為單位的**有效期間**。 如果您用來設定範本的電腦未安裝憑證服務角色，或者它是用戶端電腦，則您可能需要安裝 [憑證範本] 嵌入式管理單元。 如需有關此主題的詳細資訊，請按一下[這裡](https://technet.microsoft.com/library/cc732445.aspx)。  
  
-   更新期間應設定為0。  
  
-   選擇性憑證和要求不應儲存在 CA 資料庫中。  
  
-   憑證的增強金鑰使用方法參數必須正確設定，如下所示：  
  
    -   針對 DirectAccess 註冊簽署憑證範本，請使用金鑰1.3.6.1.4.1.311.81.1.1。  
  
    -   針對 OTP 驗證憑證範本，請使用金鑰1.3.6.1.4.1.311.20.2.2 金鑰。  
  
## <a name="33-plan-the-registration-authority-certificate"></a><a name="bkmk_33RACert"></a>3.3 規劃登錄授權單位憑證  
當 DirectAccess 用戶端要求 OTP 憑證時，遠端存取服務器會接收來自用戶端的要求。 遠端存取服務器會使用註冊授權憑證來簽署用戶端的所有 OTP 憑證要求。 只有在遠端存取服務器上的登錄授權單位憑證簽署要求時，CA 才會頒發證書。 憑證必須由內部 CA 發行，憑證無法自我簽署。 簽發 OTP 憑證的 CA 並不需要發行，但發出 OTP 憑證的 CA 必須信任頒發憑證授權單位單位簽署憑證的 ca。  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>另請參閱  
  
-   [步驟4：為遠端存取服務器規劃 OTP](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  


