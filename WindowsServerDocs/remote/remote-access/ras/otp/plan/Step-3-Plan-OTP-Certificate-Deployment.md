---
title: 步驟3規劃 OTP 憑證部署
description: 本主題是在 Windows Server 2016 中使用 OTP 驗證部署「遠端存取」指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: eca02eeb-d92d-463e-aae0-1f7038ba26fe
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 8c6898b6447e5d8c3a72653435028a7413fbb9c9
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97950444"
---
# <a name="step-3-plan-otp-certificate-deployment"></a>步驟3規劃 OTP 憑證部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

規劃 RADIUS 伺服器之後，您必須為憑證授權單位單位 (CA) 需求進行規劃，包括將發出單次密碼 (OTP) 憑證、OTP 憑證範本，以及遠端存取服務器用來簽署所有 DirectAccess 用戶端 OTP 憑證要求的登錄授權單位憑證的 CA。 這些憑證的使用方式如下：

1.  DirectAccess 用戶端會要求 OTP 憑證，而遠端存取服務器會收到要求。

2.  遠端存取服務器會驗證 OTP 認證，如果有效，則伺服器會作為註冊授權單位，並使用短期簽署憑證來簽署 OTP 憑證註冊要求。

3.  遠端存取服務器會將已簽署的憑證註冊要求傳回給 DirectAccess 用戶端

4.  然後，用戶端會使用伺服器所簽署的憑證註冊要求，從 CA 註冊 OTP 憑證。

5.  CA 會驗證認證和要求。

|Task|描述|
|----|--------|
|[3.1 規劃 OTP CA](#bkmk_3_1_CA)|規劃憑證授權單位單位 (CA) 用來將憑證發行給 DirectAccess 用戶端以進行 OTP 驗證。|
|[3.2 規劃 OTP 憑證範本](#bkmk_3_2_OTP_Cert)|規劃 OTP 憑證範本。|
|[3.3 規劃註冊授權單位憑證](#bkmk_33RACert)|規劃註冊授權單位憑證來簽署所有的 OTP 驗證憑證要求。|

## <a name="31-plan-the-otp-ca"></a><a name="bkmk_3_1_CA"></a>3.1 規劃 OTP CA
若要使用單次密碼驗證部署 DirectAccess (OTP) ，您需要內部 CA 將 OTP 驗證憑證發行至 DirectAccess 用戶端電腦。 基於此目的，您可以使用您用來發出一般 IPsec 電腦驗證所用憑證的相同內部 CA。

## <a name="32-plan-the-otp-certificate-template"></a><a name="bkmk_3_2_OTP_Cert"></a>3.2 規劃 OTP 憑證範本
每個 DirectAccess 用戶端都需要 OTP 驗證憑證，才能取得內部網路的存取權。 您必須在內部 CA 上為 OTP 憑證設定範本。 設定 OTP 憑證範本時，請注意下列事項：

-   需要執行 OTP 驗證的所有使用者都必須擁有此範本的 [讀取] 和 [註冊] 許可權。

-   主體名稱應從 Active Directory 資訊建立，以確保主體名稱與 OTP 使用者名稱相符，而不是執行憑證要求的遠端存取服務器名稱。 [主體名稱] 必須是 [完整分辨名稱] 格式，而 [主體別名] 必須是 [UPN 格式]。 這可確保註冊的 OTP 憑證對智慧卡 Kerberos 驗證有效。

-   憑證的用途必須是智慧卡登入

-   發行必須要求一個授權簽章。 簽章必須使用註冊授權單位簽署憑證範本中所設定的預先定義 DirectAccess OTP 應用程式原則進行設定。

-   有效期間應設定為一小時。

    > [!NOTE]
    > 在 CA 伺服器是 Windows Server 2003 電腦的情況下，您必須在不同的電腦上設定範本。 這是因為在執行 2008/Vista 之前的 Windows 版本時，不可能以小時為單位來設定 **有效期間** 。 如果您用來設定範本的電腦未安裝憑證服務角色，或它是用戶端電腦，則您可能需要安裝憑證範本嵌入式管理單元。 如需此主題的詳細資訊，請按一下 [這裡](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732445(v=ws.11))。

-   續約期間應設定為0。

-    (選用) 憑證和要求不應儲存在 CA 資料庫中。

-   憑證增強金鑰使用方法參數必須正確設定，如下所示：

    -   針對 DirectAccess 註冊簽署憑證範本，請使用金鑰1.3.6.1.4.1.311.81.1.1。

    -   若為 OTP 驗證憑證範本，請使用金鑰1.3.6.1.4.1.311.20.2.2 金鑰。

## <a name="33-plan-the-registration-authority-certificate"></a><a name="bkmk_33RACert"></a>3.3 規劃註冊授權單位憑證
當 DirectAccess 用戶端要求 OTP 憑證時，遠端存取服務器會接收來自用戶端的要求。 遠端存取服務器會使用登錄授權單位憑證來簽署用戶端的所有 OTP 憑證要求。 只有在遠端存取服務器上的註冊授權單位憑證簽署要求時，CA 才會發出憑證。 憑證必須由內部 CA 發出，憑證無法自我簽署。 它不需要由發行 OTP 憑證的 CA 發出，但是發行 OTP 憑證的 CA 必須信任簽發註冊授權單位簽署憑證的 CA。

## <a name="see-also"></a><a name="BKMK_Links"></a>請參閱

-   [步驟4：規劃遠端存取服務器的 OTP](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)

