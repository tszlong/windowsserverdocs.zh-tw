---
title: AD FS 疑難排解-整合式 Windows 驗證
description: 本檔說明如何針對整合式 windows 驗證進行疑難排解
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.openlocfilehash: 4c1823e1cbfc58e50c7231293b846c31480e01a6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954154"
---
# <a name="ad-fs-troubleshooting---integrated-windows-authentication"></a>AD FS 疑難排解-整合式 Windows 驗證
整合式 Windows 驗證可讓使用者使用其 Windows 認證進行登入，並透過 Kerberos 或 NTLM 體驗 (SSO) 單一登入。

## <a name="reason-integrated-windows-authentication-fails"></a>整合式 windows 驗證失敗的原因
整合式 windows 驗證會失敗的主要原因有三個。 其中包括：
    -  (SPN 的服務主體名稱) 設定錯誤
    - 通道系結 Token
    - Internet Explorer 設定

## <a name="spn-misconfiguration"></a>SPN 設定錯誤
 (SPN) 的服務主體名稱是服務實例的唯一識別碼。 Kerberos 驗證會使用 Spn，將服務實例與服務登入帳戶產生關聯。 這可讓用戶端應用程式要求服務驗證帳戶，即使用戶端沒有帳戶名稱也一樣。

如何搭配 AD FS 使用 SPN 的範例如下所示：
1. Web 瀏覽器查詢 Active Directory 以判斷正在執行 sts.contoso.com 的服務帳戶
2. Active Directory 會告訴瀏覽器它是 AD FS 服務帳戶。
3. 瀏覽器將會取得 AD FS 服務帳戶的 Kerberos 票證。

如果 AD FS 服務帳戶的設定不正確或 SPN 錯誤，這可能會造成問題。  查看網路追蹤時，您可能會看到類似 KRB 錯誤的錯誤： KRB5KDC_ERR_S_PRINCIPAL_UNKNOWN。

使用網路追蹤 (例如 Wireshark) 您可以判斷瀏覽器嘗試解析的 SPN，然後使用命令列工具 setspn-Q <spn> ，您可以在該 spn 上進行查閱。  可能找不到該檔案，或將它指派給 AD FS 服務帳戶以外的其他帳戶。

![整合式](media/ad-fs-tshoot-iwa/iwa3.png)

您可以藉由查看 AD FS 服務帳戶的屬性來驗證 SPN。

![整合式](media/ad-fs-tshoot-iwa/iwa1.png)

## <a name="channel-binding-token"></a>通道系結 Token
就目前而言，當用戶端應用程式使用 Kerberos、Digest 或 NTLM 透過 HTTPS 向伺服器驗證自身時，首先會建立傳輸層安全性 (TLS) 通道，並且使用此通道進行驗證。

通道系結 Token 是受 TLS 保護之外部通道的屬性，用來透過用戶端驗證的內部通道，將外部通道系結至交談。

如果發生「攔截式」攻擊，而他們已取消加密並重新加密 SSL 流量，則金鑰將不會相符。  AD FS 會判斷 web 流覽 r 和其本身中間有一些東西。  這會造成 Kerberos 驗證失敗，而使用者會收到401對話方塊，而不是 SSO 體驗。

這可能是由下列原因所造成：
 - 位於瀏覽器和 AD FS 之間的任何專案
 - Fiddler
 - 執行 SSL 橋接的反向 proxy

根據預設，AD FS 會將此設為 "allow"。  您可以使用 PowerShell cmdlt 來變更此設定`Set-ADFSProperties -ExtendProtectionTokenCheck`

如需詳細資訊，請參閱[安全規劃和部署 AD FS 的最佳做法](../../ad-fs/design/best-practices-for-secure-planning-and-deployment-of-ad-fs.md)。

## <a name="internet-explorer-configuration"></a>Internet Explorer 設定
根據預設，Internet explorer 會有下列方式：

1. Internet explorer 將會收到來自 AD FS 的401回應，並在標頭中使用「NEGOTIATE」一詞。
2. 這會告訴網頁瀏覽器取得 Kerberos 或 NTLM 票證以傳回 AD FS。
3. 根據預設，IE 會嘗試執行這項操作 (如果在標頭中有一詞，則不需要使用者互動即可進行 SPNEGO) 。  僅適用于內部網路網站。

有2個主要專案可防止 happeing。
   - [啟用整合式 Windows 驗證] 不會在 IE 的屬性中檢查。  這位於 [網際網路選項] 底下-> Advanced-> Security]。

   ![整合式](media/ad-fs-tshoot-iwa/iwa4.png)

   - 未正確設定安全性區域
       - Fqdn 不在內部網路區域中
       - AD FS URL 不在內部網路區域中。

      ![整合式](media/ad-fs-tshoot-iwa/iwa5.png)
## <a name="next-steps"></a>後續步驟

- [AD FS 疑難排解](ad-fs-tshoot-overview.md)
