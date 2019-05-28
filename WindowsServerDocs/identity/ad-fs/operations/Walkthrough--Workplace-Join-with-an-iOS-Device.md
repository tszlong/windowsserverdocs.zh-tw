---
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: 逐步解說-ios 裝置加入工作地點
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 42b71667758f392d641c5262e34322f8b21cfad9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188909"
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>逐步解說：將 iOS 裝置加入工作地點網路


> [!IMPORTANT] 
> 這個方法會只完全在內部部署客戶與相關項目。 混合式或僅限雲端的客戶必須使用這個方法來註冊其 iOS 裝置。 而且這個方法不相容時的內部客戶決定將移至雲端。 必須取消註冊裝置，並將它向雲端中。 

此主題示範如何將 iOS 裝置加入工作地點網路。 您必須先完成中的步驟[適用於 Windows Server 2012 R2 中的 AD FS 設定實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)區段之前，您可以試試看本逐步解說。 您可以使用裝置來存取相同公司 web 應用程式存取的[逐步解說：Windows 裝置加入工作地點](Walkthrough--Workplace-Join-with-a-Windows-Device.md)。


## <a name="join-an-ios-device-with-workplace-join"></a>將 iOS 裝置加入工作地點網路

> [!IMPORTANT]
> 已設定內部部署 DRS，iOS 裝置必須信任用來設定 Active Directory Federation Services (AD FS) 的安全通訊端層 (SSL) 憑證中[步驟 2:設定同盟伺服器 (ADFS1) with Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)，工作地點加入成功。
> 
> -   若該 AD FS SSL 憑證是從測試憑證授權單位 (CA) 簽發，您必須在您的 iOS 裝置上安裝憑證授權單位憑證。
> -   若您的憑證授權單位憑證是發佈在網站上，您可以從您的 iOS 裝置瀏覽該網站並安裝該憑證。

在此示範中，您會將裝置加入到工作地點網路。

#### <a name="to-join-an-ios-device-to-a-workplace"></a>將 iOS 裝置加入工作地點網路

1.  -   **當 Azure Active Directory 裝置註冊服務時設定的 DRS 時：** 開啟 Apple Safari 並瀏覽至 Azure Active Directory 裝置註冊服務 Over-the-Air 設定檔端點，針對 iOS 裝置，<`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` >，<`yourdomainname`> 是您已設定與 Azure Active Directory 的網域名稱。 例如，如果您的網域名稱是 contoso.com，URL 將是：`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

    -   **當內部部署 DRS 是設定的 DRS 時**：開啟 Apple Safari 並瀏覽到 iOS 裝置，裝置註冊服務 (DRS) Over-the-Air 設定檔端點 `https://adf1s.contoso.com/enrollmentserver/otaprofile`

    有很多種方式可將此 URL 提供給您的使用者。 建議的方式之一，是在 AD FS 的自訂應用程式存取拒絕訊息中發佈此 URL。 這點會在後續章節中說明：[建立應用程式存取原則和自訂存取拒絕訊息](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message)

2.  登入該網頁使用公司網域帳戶： **roberth@contoso.com**和 密碼： **P@ssword**。

3.  系統會提示您安裝設定檔。 在 [安裝設定檔]  畫面上，按一下 [安裝]  。

4.  當系統提示您確認安裝設定檔時，請按一下 [立即安裝]  。

5.  若您的裝置要求您輸入 PIN 以解除鎖定裝置，系統會提示您輸入 PIN。

6.  當您看到 [設定檔已安裝]  畫面時，表示設定檔已安裝完成。 按一下 [完成]  。

    返回到 Safari。 會顯示一個訊息通知您您可以關閉或離開 Safari。

> [!TIP]
> 若要檢視或移除「加入工作地點網路」設定檔，請在您的 iOS 裝置上瀏覽到 [設定]  ，按一下 [一般]  ，然後按一下 [設定檔]  。

## <a name="see-also"></a>另請參閱


- [從任何裝置加入工作地點網路提供 SSO 和無縫式次要因素驗證，跨公司應用程式](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [在 Windows Server 2012 R2 設定 AD FS 實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [逐步解說：將 Windows 裝置加入工作地點網路](Walkthrough--Workplace-Join-with-a-Windows-Device.md)



