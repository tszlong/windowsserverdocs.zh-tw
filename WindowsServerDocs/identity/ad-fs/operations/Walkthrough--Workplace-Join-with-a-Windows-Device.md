---
ms.assetid: c17d143b-86b4-47c0-b76e-1862dda8f0bd
title: "逐步解說-與 Windows 裝置的工作地點加入"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fd222eb47982591e051594e8a572443b65c0357f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="walkthrough-workplace-join-with-a-windows-device"></a>Windows 裝置的逐步解說： 地點加入

>適用於：Windows Server 2016、Windows Server 2012 R2

本主題示範如何使用工作地點加入來連接您的 Windows 裝置使用您的工作地點，以及如何使用單一登入來存取 web 應用程式。 您必須先完成中的步驟執行[在 Windows Server 2012 R2 AD FS 設定實驗室](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)區段之前，您可以嘗試本節。

## <a name="access-the-web-application-before-device-registration"></a>存取裝置的登記之前 web 應用程式
在本節中，您會存取公司 web 應用程式之前，您的工作地點到加入您的裝置。 網頁將會顯示在您的安全性權杖中已包含宣告。 請注意，索賠項目清單中的不包含任何裝置的相關資訊。 您也可能會觀察到，您不需要單一登入。

#### <a name="to-access-the-web-application-before-you-use-workplace-join-on-your-device"></a>若要之前您使用工作地點加入您的裝置上存取 web 應用程式

1.  使用您的 Microsoft account 登入 Client1。

2.  左 Internet Explorer，瀏覽至您的一般宣告應用程式， **https://webserv1.contoso.com/claimapp**。

3.  登入使用公司核對網頁： ** roberth@contoso.com**的密碼： ** P@ssword **。

4.  網頁列出您的安全性權杖中所有宣告。 僅限使用者宣告會出現在您的安全性權杖。

5.  關閉 Internet Explorer。

6.  打開 Internet Explorer 和相同宣告應用程式中，瀏覽**https://webserv1.contoso.com/claimapp**。

7.  請注意，提示您重新輸入認證。 您不使用加入的工作地點裝置連接到地點，因此不需要單一登入。

## <a name="join-your-device-with-workplace-join"></a>使用工作地點加入加入您的裝置

> [!IMPORTANT]
> 工作地點加入成功，client 電腦 (Client1) 必須信任 SSL 憑證用來設定 「 Active Directory 同盟 Services (AD FS)[步驟 2： 設定聯盟伺服器裝置登記服務 (ADFS1) 以](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)。 它也必須驗證憑證的撤銷資訊。 如果您使用工作地點加入的任何問題，您可以在 Client1 檢視事件登入。
> 
> 事件登入，請打開事件檢視器，展開**應用程式與服務登**，展開**Microsoft**，展開**Windows**，然後按一下 [**的工作地點加入**。

#### <a name="to-join-your-device-with-workplace-join"></a>若要加入的工作地點加入您的裝置

1.  使用您的 Microsoft account 登入 Client1。

2.  在**[開始]**畫面左**常用**列，]，然後選取**設定**常用鍵。 選取 [ **[變更電腦設定**。

3.  在**電腦設定**頁面上，選取**網路**，然後按一下 [**的工作地點**。

4.  在**請輸入您的工作地點的存取，或管理的裝置上關閉身份**方塊中，輸入** roberth@contoso.com **，然後按一下 [**加入**。

5.  當系統提示您輸入認證時，輸入** roberth@contoso.com **，並密碼： ** P@ssword **。 按一下**[確定]**。

6.  您現在應該會看到此訊息: 「 這個裝置已加入您的工作場所網路 」。

### <a name="access-the-web-application-after-joining-the-workplace"></a>存取加入工作場所之後 web 應用程式
示範的這一角，您會從裝置連接的工作地點加入存取公司 web 應用程式。 網頁將會顯示在您的安全性權杖中已包含宣告。 請注意索賠項目清單，包括裝置和使用者資訊。 您可能也會看到您現在具有單一登入。

##### <a name="to-access-the-web-application-after-joining-the-workplace"></a>存取加入工作場所之後 web 應用程式

1.  登入**Client1**使用您的 Microsoft account。

2.  左 Internet Explorer，瀏覽至您的一般宣告應用程式， **https://webserv1.contoso.com/claimapp**。

3.  登入使用公司核對網頁： ** roberth@contoso.com**的密碼： ** P@ssword **。

4.  網頁列出您的安全性權杖中主張。 您權杖包含宣告使用者及裝置。

5.  關閉 Internet Explorer。

6.  打開 Internet Explorer 和相同宣告應用程式中，瀏覽**https://webserv1.contoso.com/claimapp**。

7.  請注意，您的**未**提示您重新輸入認證。 您的工作地點加入裝置連接，因此單一登入。

## <a name="see-also"></a>也了
[加入的任何裝置 SSO 和順暢第二個因數驗證在公司應用程式的工作地點](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[設定實驗室 AD FS 在 Windows Server 2012 R2 的](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
[逐步解說： 的工作地點裝置 iOS 加入](Walkthrough--Workplace-Join-with-an-iOS-Device.md)



