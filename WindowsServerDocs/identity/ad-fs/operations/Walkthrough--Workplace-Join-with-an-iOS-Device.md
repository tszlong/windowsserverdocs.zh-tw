---
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: 逐步解說-iOS 裝置的工作地點加入
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0a8643bab913dfec07c6bbea0c068e1240f16c5b
ms.sourcegitcommit: a2260c96b0e49519d180c7382b921ce8ddb053fe
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>逐步解說： IOS 裝置加入的工作地點

>適用於： Windows Server 2012 R2

本主題示範地點加入 iOS 裝置上。 您必須先完成中的步驟執行[在 Windows Server 2012 R2 AD FS 設定實驗室](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)區段之前，您可以嘗試本節。 您可以使用裝置存取相同的公司 web 應用程式存取您的[逐步解說： 地點加入 Windows 裝置的](Walkthrough--Workplace-Join-with-a-Windows-Device.md)。

## <a name="join-an-ios-device-with-workplace-join"></a>使用工作地點加入加入 iOS 裝置

> [!IMPORTANT]
> IOS 裝置上場所 DRS 設定時，必須在信任安全通訊端層 (SSL) 憑證用來設定 「 Active Directory 同盟 Services (AD FS)[步驟 2： 設定聯盟伺服器 (ADFS1) 裝置登記服務的](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)的工作地點加入成功。
> 
> -   如果 AD FS SSL 憑證從測試憑證授權單位，您必須安裝 iOS 裝置上的憑證授權單位憑證。
> -   如果憑證憑證授權單位發行在網站上，您可以從您的 iOS 裝置瀏覽的網站，並安裝憑證。

在此範例中，您可以加入裝置到地點。

#### <a name="to-join-an-ios-device-to-a-workplace"></a>若要加入 iOS 裝置到工作地點

1.  -   **Azure Active Directory 裝置登記服務時設定的 DRS:**開放 Apple Safari 並瀏覽至 Azure Active Directory 裝置登記服務 Over-the-Air 設定檔端點適用於 iOS 的裝置，<`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` > 位置 <`yourdomainname`> 是您所使用 Azure Active Directory 設定的網域名稱。 例如，如果您的網域名稱 contoso.com，URL 為： `https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

    -   **先 DRS 時設定的 DRS**： 開放 Apple Safari 並瀏覽至裝置登記服務 (DRS) Over-the-Air 設定檔端點適用於 iOS 的裝置`https://adf1s.contoso.com/enrollmentserver/otaprofile`

    有許多您的使用者此 URL 的通訊方式。 一個的建議的方式是發行這個 URL 自訂應用程式存取拒絕 AD FS 的訊息。 這即將一節中所涵蓋：[建立應用程式存取原則和自訂拒絕存取的訊息](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message)

2.  登入使用公司核對網頁： ** roberth@contoso.com **和密碼： ** P@ssword **。

3.  系統提示您安裝的設定檔。 在**安裝設定檔**畫面中，按**安裝**。

4.  當系統提示 [確認安裝的個人檔案，按一下**立即安裝]**。

5.  如果您的裝置需要 PIN 來解除鎖定裝置時，會提示您輸入 pin 碼。

6.  設定檔安裝完成時，請**安裝設定檔**畫面。 按一下**完成**。

    返回 Safari。 您可以關閉或離開 Safari 訊息會通知您。

> [!TIP]
> 若要檢視或移除的工作地點加入個人檔案，請瀏覽至**設定**，按一下 [**一般**，然後按一下 [**設定檔**iOS 裝置上。

## <a name="see-also"></a>也了


- [加入的任何裝置 SSO 和順暢的第二個工作地點因數驗證跨公司應用程式](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [在 Windows Server 2012 R2 AD FS 設定實驗室](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [Windows 裝置的逐步解說： 地點加入](Walkthrough--Workplace-Join-with-a-Windows-Device.md)



