---
title: AD FS 疑難排解-Idp-Initiated 登入
description: 本檔說明如何疑難排解 AD FS 登入頁面。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.openlocfilehash: 8ab9497a2c6711c638010c66b9a7c7dd8066876f
ms.sourcegitcommit: 03048411c07c1a1d0c8bb0b2a60c1c17c9987314
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/09/2020
ms.locfileid: "96938968"
---
# <a name="ad-fs-troubleshooting---idp-initiated-sign-on"></a>AD FS 疑難排解-Idp-Initiated 登入
您可以使用 AD FS 登入頁面來測試驗證是否正常運作。  這是藉由流覽至頁面並登入來完成。  此外，我們也可以使用登入頁面來確認是否列出所有的 SAML 2.0 信賴憑證者。

## <a name="enable-the-idp-initiated-sign-on-page"></a>啟用 Idp-Initiated 登入頁面
根據預設，Windows 2016 中的 AD FS 沒有啟用 [登入] 頁面。  為了啟用此功能，您可以使用 PowerShell 命令集-Set-adfsproperties。  使用下列程式啟用頁面：

1.  開啟 Windows PowerShell
2.  輸入：  `Get-AdfsProperties` 並按 enter 鍵
3.  確認 **EnableIdpInitiatedSignonPage** 設定為 false ![ false](media/ad-fs-tshoot-initiatedsignon/idp2.png)
4.  在 PowerShell 中，輸入：  `Set-AdfsProperties -EnableIdpInitiatedSignonPage $true`
5.  您將不會看到確認，請再次輸入 Get-AdfsProperties 並確認 **EnableIdpInitatedSignonPage** 已設定為 true。
![True](media/ad-fs-tshoot-initiatedsignon/idp4.png)

## <a name="test-authentication"></a>測試驗證
使用下列程式，以 Idp-Initiated 登入頁面測試 AD FS 驗證。

1.  開啟網頁瀏覽器並流覽至 Idp 登入頁面。  範例：https://sts.contoso.com/adfs/ls/idpinitiatedsignon.aspx
2.  系統應該會提示您登入。  輸入認證。
![登入](media/ad-fs-tshoot-initiatedsignon/idp5.png)
3.  如果成功，您應該已登入。


## <a name="test-authentication-using-a-seamless-logon-experience"></a>使用順暢的登入體驗來測試驗證
您可以藉由確認 AD FS 伺服器的 URL 已新增至網際網路選項的近端內部網路區域，來測試無縫登入體驗。  請使用下列程序：

1.  在 Windows 10 用戶端上，按一下 [開始] 並輸入 [網際網路選項]，然後選取 [網際網路選項]。
2.   按一下 [安全性] 索引標籤，按一下 [近端內部網路]，然後按一下 [網站] 按鈕。
![無縫](media/ad-fs-tshoot-initiatedsignon/idp8.png)
1.  按一下 [進階]。
2.  輸入您的 url，然後按一下 [新增]。  按一下 [關閉]。
![新增 url](media/ad-fs-tshoot-initiatedsignon/idp9.png)
1.  按一下 [確定]。  按一下 [確定]。  這應該會關閉網際網路選項。
2.  開啟網頁瀏覽器並流覽至 Idp 登入頁面。  範例：https://sts.contoso.com/adfs/ls/idpinitiatedsignon.aspx
3.  按一下 [登入] 按鈕。  您應該會自動登入，而不會提示您輸入認證。
![無縫](media/ad-fs-tshoot-initiatedsignon/idp6.png)

## <a name="next-steps"></a>後續步驟

- [AD FS 疑難排解](ad-fs-tshoot-overview.md)
