---
title: AD FS 疑難排解-Idp 起始的登入
description: 本檔說明如何針對 AD FS 登入頁面進行疑難排解。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 89ee45bd0387cf728bc126529169b6b1ca045d6f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366186"
---
# <a name="ad-fs-troubleshooting---idp-initiated-sign-on"></a>AD FS 疑難排解-Idp 起始的登入
AD FS 登入 頁面可用來測試驗證是否正常運作。  這是藉由流覽至頁面並登入來完成。  此外，我們也可以使用登入頁面來確認是否列出所有 SAML 2.0 信賴憑證者。

## <a name="enable-the-idp-initiated-sign-on-page"></a>啟用 Idp 起始的登入頁面
根據預設，Windows 2016 中的 AD FS 不會啟用 [登入] 頁面。  若要啟用它，您可以使用 PowerShell 命令集-Set-adfsproperties。  使用下列程式啟用頁面：

1.  開啟 Windows PowerShell
2.  輸入： `Get-AdfsProperties` 並按 enter 鍵
3.  確認**EnableIdpInitiatedSignonPage**設定為 False ![false](media/ad-fs-tshoot-initiatedsignon/idp2.png)
4.  在 PowerShell 中，輸入： `Set-AdfsProperties -EnableIdpInitiatedSignonPage $true`
5.  您將不會看到確認，因此請再次輸入 Set-adfsproperties，並確認**EnableIdpInitatedSignonPage**已設定為 true。
![True](media/ad-fs-tshoot-initiatedsignon/idp4.png)

## <a name="test-authentication"></a>測試驗證
使用下列程式，透過 Idp 起始的登入頁面來測試 AD FS 驗證。

1.  開啟網頁瀏覽器，並流覽至 [Idp 登入] 頁面。  範例： https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
2.  系統應該會提示您登入。  輸入您的認證。
![登入](media/ad-fs-tshoot-initiatedsignon/idp5.png)
3.  如果這項作業成功，您應該已登入。


## <a name="test-authentication-using-a-seamless-logon-experience"></a>使用順暢的登入體驗來測試驗證
您可以藉由確定 AD FS 伺服器的 URL 已新增網際網路選項的 [近端內部網路] 區域，來測試順暢的登入體驗。  請使用下列程序：

1.  在 Windows 10 用戶端上，按一下 [開始] 並輸入 [網際網路選項]，然後選取 [網際網路選項]。
2.   按一下 [安全性] 索引標籤、按一下 [近端內部網路]，然後按一下 [網站] 按鈕。
![順暢的](media/ad-fs-tshoot-initiatedsignon/idp8.png)
1.  按一下 [進階]。
2.  輸入您的 url，然後按一下 [新增]。  按一下 [關閉]。
![新增 url](media/ad-fs-tshoot-initiatedsignon/idp9.png)
1.  按一下 [確定]。  按一下 [確定]。  這應該會關閉 [網際網路選項]。
2.  開啟網頁瀏覽器，並流覽至 [Idp 登入] 頁面。  範例： https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
3.  按一下 [登入] 按鈕。  您應該會自動登入，而不會提示您輸入認證。
![順暢的](media/ad-fs-tshoot-initiatedsignon/idp6.png)

## <a name="next-steps"></a>後續步驟

- [AD FS 疑難排解](ad-fs-tshoot-overview.md)
