---
description: 深入瞭解：逐步解說：使用 iOS 裝置 Workplace Join
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: 逐步解說-使用 iOS 裝置 Workplace Join
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2018
ms.topic: article
ms.openlocfilehash: d77a004dbf9b2010b1bed6c300ad1ebe90c0099c
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039526"
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>逐步解說：將 iOS 裝置加入工作地點網路


> [!IMPORTANT]
> 此方法僅適用于完全內部部署的客戶。 混合式或僅限雲端的客戶不得使用這個方法來註冊其 iOS 裝置。 當內部部署客戶決定移至雲端時，此方法並不相容。 裝置必須已取消註冊並向雲端註冊。

此主題示範如何將 iOS 裝置加入工作地點網路。 您必須先完成「 [在 Windows Server 2012 R2 中設定 AD FS 的實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) 」一節中的步驟，才能嘗試執行此逐步解說。 您可以使用裝置來存取您在 [逐步解說： Workplace Join 與 Windows 裝置](Walkthrough--Workplace-Join-with-a-Windows-Device.md)所存取的相同公司 web 應用程式。


## <a name="join-an-ios-device-with-workplace-join"></a>將 iOS 裝置加入工作地點網路

> [!IMPORTANT]
> 如果已設定內部部署 DRS，iOS 裝置必須信任在 [Step 2: Configure the federation server (ADFS1) with Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)中用來設定 Active Directory 同盟服務 (AD FS) 的安全通訊端層 (SSL) 憑證，才能成功執行「加入工作地點網路」。
>
> -   若該 AD FS SSL 憑證是從測試憑證授權單位 (CA) 簽發，您必須在您的 iOS 裝置上安裝憑證授權單位憑證。
> -   若您的憑證授權單位憑證是發佈在網站上，您可以從您的 iOS 裝置瀏覽該網站並安裝該憑證。

在此示範中，您會將裝置加入到工作地點網路。

#### <a name="to-join-an-ios-device-to-a-workplace"></a>將 iOS 裝置加入工作地點網路

1. -   **當 Azure Active Directory 裝置註冊服務是設定的 DRS 時：** 開啟 Apple Safari，然後流覽至 iOS 裝置的 Azure Active Directory 裝置註冊 service 無線設定檔端點，<`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` > 其中 <`yourdomainname`> 是您使用 Azure Active Directory 設定的功能變數名稱。 例如，如果您的網域名稱是 contoso.com，URL 將是：`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

   -   **當內部部署 drs 是設定的 Drs 時**：開啟 Apple Safari 並流覽至裝置註冊服務 (DRS) iOS 裝置的無線設定檔端點。 `https://adf1s.contoso.com/enrollmentserver/otaprofile`

   有很多種方式可將此 URL 提供給您的使用者。 其中一個建議方式是在 AD FS 中的自訂應用程式拒絕存取訊息發佈此 URL。 這涵蓋于下一節： [建立應用程式存取原則和自訂拒絕存取訊息](/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message)

2. 使用公司網域帳戶登入網頁：  <strong>roberth@contoso.com</strong> 和密碼： <strong>P@ssword</strong> 。

3. 系統會提示您安裝設定檔。 在 [安裝設定檔] 畫面上，按一下 [安裝]。

4. 當系統提示您確認安裝設定檔時，請按一下 [立即安裝]。

5. 若您的裝置要求您輸入 PIN 以解除鎖定裝置，系統會提示您輸入 PIN。

6. 當您看到 [設定檔已安裝] 畫面時，表示設定檔已安裝完成。 按一下 [完成]。

   返回到 Safari。 會顯示一個訊息通知您您可以關閉或離開 Safari。

> [!TIP]
> 若要檢視或移除「加入工作地點網路」設定檔，請在您的 iOS 裝置上瀏覽到 [設定]，按一下 [一般]，然後按一下 [設定檔]。

## <a name="see-also"></a>另請參閱


- [從任何裝置加入工作地點網路，並在公司的各個應用程式提供 SSO 和無縫式的次要因素驗證](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [在 Windows Server 2012 R2 設定 AD FS 實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [逐步解說：將 Windows 裝置加入工作地點網路](Walkthrough--Workplace-Join-with-a-Windows-Device.md)
