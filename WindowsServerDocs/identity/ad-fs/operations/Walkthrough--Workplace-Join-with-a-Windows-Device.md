---
ms.assetid: c17d143b-86b4-47c0-b76e-1862dda8f0bd
title: 逐步解說-使用 Windows 裝置加入工作地點
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8b3b2934e7aa177e873e19d77530b2d796ccd521
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188904"
---
# <a name="walkthrough-workplace-join-with-a-windows-device"></a>逐步解說：將 Windows 裝置加入工作地點網路

此主題示範如何使用「加入工作地點網路」將您的 Windows 裝置連線到您的工作地點網路，以及如何使用「單一登入」來存取 Web 應用程式。 您必須先完成中的步驟[適用於 Windows Server 2012 R2 中的 AD FS 設定實驗室環境](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)區段之前，您可以試試看本逐步解說。

## <a name="access-the-web-application-before-device-registration"></a>在執行裝置註冊之前存取 Web 應用程式
在此逐步解說中，您會在將裝置加入工作地點網路之前存取公司 Web 應用程式。 網頁會顯示您的安全性權杖中包含的宣告。 請注意，宣告清單不包含有關您裝置的任何資訊。 您可能也會發現您沒有「單一登入」。

#### <a name="to-access-the-web-application-before-you-use-workplace-join-on-your-device"></a>在於您的裝置上使用「加入工作地點網路」之前存取 Web 應用程式

1.  使用您的 Microsoft 帳戶登入 Client1。

2.  開啟 Internet Explorer 並瀏覽至您的一般宣告應用程式**https://webserv1.contoso.com/claimapp**。

3.  登入該網頁使用公司網域帳戶： **roberth@contoso.com**，密碼： **P@ssword**。

4.  網頁會列出您的安全性權杖中的所有宣告。 您的安全性權杖中只有使用者宣告。

5.  關閉 Internet Explorer。

6.  開啟 Internet Explorer 並瀏覽至相同的宣告應用程式**https://webserv1.contoso.com/claimapp**。

7.  請注意，系統會提示您再次輸入認證。 您並未使用「加入工作地點網路」功能連線到工作地點網路，因此沒有「單一登入」。

## <a name="join-your-device-with-workplace-join"></a>將您的裝置加入工作地點網路

> [!IMPORTANT]
> 工作地點加入成功，用戶端電腦 (Client1) 必須信任的 SSL 憑證，用來設定 Active Directory Federation Services (AD FS) 在[步驟 2:設定 Federation Server with Device Registration Service (ADFS1)](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4)。 它也必須能夠驗證憑證的撤銷資訊。 如果您在工作地點加入方面有任何問題，可以檢視 Client1 上的事件記錄檔。
> 
> 若要查看事件記錄檔，請開啟 [事件檢視器]，依序展開 [應用程式及服務記錄檔]  、[Microsoft]  、[Windows]  ，然後按一下 [工作地點加入]  。

#### <a name="to-join-your-device-with-workplace-join"></a>將您的裝置加入工作地點網路

1.  使用您的 Microsoft 帳戶登入 Client1。

2.  在 [開始]  畫面上，開啟 [常用鍵]  列，然後選取 [設定]  常用鍵。 選取 [變更電腦設定]  。

3.  在 [電腦設定]  頁面上，選取 [網路]  ，然後按一下 [工作地點]  。

4.  在 **輸入您的使用者識別碼，以取得工作地點存取權，或開啟裝置管理**方塊中，輸入**roberth@contoso.com**，然後按一下**加入**。

5.  當提示您輸入認證時時，請輸入**roberth@contoso.com**，和密碼： **P@ssword**。 按一下 [確定]  。

6.  您現在應該會看到下列訊息：「此裝置已加入您的工作地點網路。」

### <a name="access-the-web-application-after-joining-the-workplace"></a>在加入工作地點網路之後存取 Web 應用程式
在示範的此部分中，您會從已使用「加入工作地點網路」連線的裝置存取公司 Web 應用程式。 網頁會顯示您的安全性權杖中包含的宣告。 請注意，宣告清單包含裝置與使用者資訊。 您可能也會發現您現在有「單一登入」。

##### <a name="to-access-the-web-application-after-joining-the-workplace"></a>在加入工作地點網路之後存取 Web 應用程式

1.  使用您的 Microsoft 帳戶登入 **Client1** 。

2.  開啟 Internet Explorer 並瀏覽至您的一般宣告應用程式**https://webserv1.contoso.com/claimapp**。

3.  登入該網頁使用公司網域帳戶： **roberth@contoso.com**，密碼： **P@ssword**。

4.  網頁會列出您的安全性權杖中的宣告。 您的權杖包含使用者與裝置宣告。

5.  關閉 Internet Explorer。

6.  開啟 Internet Explorer 並瀏覽至相同的宣告應用程式**https://webserv1.contoso.com/claimapp**。

7.  請注意，系統 **不會** 提示您再次輸入認證。 您已使用「加入工作地點網路」功能連線，因此有「單一登入」。

## <a name="see-also"></a>另請參閱
[從任何裝置加入工作地點網路提供 SSO 和無縫式第二個因素 Authentication Across Company Applications](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[適用於 Windows Server 2012 R2 中的 AD FS 設定實驗室環境](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
 [逐步解說：將 iOS 裝置加入工作地點網路](Walkthrough--Workplace-Join-with-an-iOS-Device.md)



