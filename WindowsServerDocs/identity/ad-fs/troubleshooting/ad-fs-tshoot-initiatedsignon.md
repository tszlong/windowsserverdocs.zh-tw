---
title: AD FS 疑難排解-Idp 起始登入
description: 本文件說明如何疑難排解 AD FS 登入頁面。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 61e9adc708e95a6ab4a82550280737b2f4bad0a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844939"
---
# <a name="ad-fs-troubleshooting---idp-initiated-sign-on"></a>AD FS 疑難排解-Idp 起始登入
AD FS 登入頁面可用來測試驗證運作。  這是瀏覽至頁面，然後登入。  此外，我們可以使用登入頁面，來確認已列出所有 SAML 2.0 信賴憑證者的合作對象。

## <a name="enable-the-idp-intiated-sign-on-page"></a>啟用 Idp 起始登入頁面
根據預設，Windows 2016 中的 AD FS 沒有符號在已啟用 頁面上。  若要啟用它，您可以使用 PowerShell 命令 Set-adfsproperties。  您可以使用下列程序來啟用頁面：

1.  開啟 Windows PowerShell
2.  請輸入：`Get-AdfsProperties`然後按 enter 鍵
3.  確認**EnableIdpInitiatedSignonPage**設定為 false ![False](media/ad-fs-tshoot-initiatedsignon/idp2.png)
4.  在 PowerShell 中，輸入：  `Set-AdfsProperties -EnableIdpInitiatedSignonPage $true`
5.  您將不會看到確認訊息，因此輸入 Get-adfsproperties 一次，並確認**EnableIdpInitatedSignonPage**設為 true。
![True](media/ad-fs-tshoot-initiatedsignon/idp4.png)

## <a name="test-authentication"></a>測試驗證
您可以使用下列程序來測試 AD FS 驗證搭配 Idp-Initiated 登入頁面。

1.  開啟網頁瀏覽器並瀏覽至 Idp 登入頁面。  範例：  https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
2.  您應該會登入提示。  輸入您的認證。
![登入](media/ad-fs-tshoot-initiatedsignon/idp5.png)
3.  如果這是成功，您應該登入。


## <a name="test-authentication-using-a-seamless-logon-experience"></a>測試使用無縫式登入體驗的驗證
您可以藉由確認 AD FS 伺服器的 URL 會新增您的網際網路選項的本機內部網路區域來測試順暢的登入經驗。  請使用下列程序：

1.  在 Windows 10 用戶端中，按一下 開始 和輸入網際網路選項，選取 網際網路選項。
2.   按一下 [安全性] 索引標籤、 按一下在近端內部網路，然後按一下 [網站] 按鈕。
![無縫式](media/ad-fs-tshoot-initiatedsignon/idp8.png)
1.  按一下 [進階]。
2.  輸入您的 url，然後按一下 [新增]。  按一下 [關閉]。
![新增 url](media/ad-fs-tshoot-initiatedsignon/idp9.png)
1.  按一下 [確定]。  按一下 [確定]。  這應該在關閉 網際網路選項。
2.  開啟網頁瀏覽器並瀏覽至 Idp 登入頁面。  範例：  https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
3.  按一下登入 按鈕。  您應該會自動登入，並不會提示輸入認證。
![無縫式](media/ad-fs-tshoot-initiatedsignon/idp6.png)

## <a name="next-steps"></a>後續步驟

- [疑難排解 AD FS](ad-fs-tshoot-overview.md)