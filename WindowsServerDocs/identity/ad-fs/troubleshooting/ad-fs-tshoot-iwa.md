---
title: AD FS 疑難排解-整合式 Windows 驗證
description: 本檔說明如何針對整合式 windows 驗證進行疑難排解
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.openlocfilehash: f1f3adbb7b2e692d2db18c6891694761a3205f08
ms.sourcegitcommit: 6717decb5839aa340c81811d6fde020aabaddb3b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/26/2021
ms.locfileid: "98781875"
---
# <a name="ad-fs-troubleshooting---integrated-windows-authentication"></a>AD FS 疑難排解-整合式 Windows 驗證
整合式 Windows 驗證可讓使用者使用其 Windows 認證進行登入，並使用 Kerberos 或 NTLM 體驗單一登入 (SSO) 。

## <a name="reason-integrated-windows-authentication-fails"></a>整合式 windows 驗證失敗的原因
整合式 windows 驗證會失敗的主要原因有三個。 這些包括：
    - 服務主體名稱 (SPN) 設定錯誤
    - 通道系結 Token
    - Internet Explorer 設定

## <a name="spn-misconfiguration"></a>SPN 設定錯誤
服務主體名稱 (SPN) 是服務實例的唯一識別碼。 Kerberos 驗證會使用 Spn，將服務實例與服務登入帳戶建立關聯。 這可讓用戶端應用程式要求服務驗證帳戶，即使用戶端沒有帳戶名稱也一樣。

使用 SPN 搭配 AD FS 的範例如下所示：
1. Web 瀏覽器查詢 Active Directory，以判斷正在執行的服務帳戶 sts.contoso.com
2. Active Directory 會告知瀏覽器是 AD FS 服務帳戶。
3. 瀏覽器會取得 AD FS 服務帳戶的 Kerberos 票證。

如果 AD FS 服務帳戶的設定不正確或 SPN 錯誤，則可能會造成問題。  查看網路追蹤，您可能會看到 KRB 錯誤： KRB5KDC_ERR_S_PRINCIPAL_UNKNOWN 等錯誤。

使用網路追蹤 (例如 Wireshark) 您可以判斷瀏覽器嘗試解析的 SPN，然後使用 setspn-Q 命令列工具，來 <spn> 查閱該 spn。  可能找不到它，或可能指派給 AD FS 服務帳戶以外的其他帳戶。

![命令提示字元視窗的螢幕擷取畫面。](media/ad-fs-tshoot-iwa/iwa3.png)

您可以查看 AD FS 服務帳戶的屬性，以驗證 SPN。

![[D F S 服務帳戶屬性] 對話方塊之 [屬性編輯器] 索引標籤的螢幕擷取畫面，其中顯示已呼叫的服務主體名稱值。 ](media/ad-fs-tshoot-iwa/iwa1.png)

## <a name="channel-binding-token"></a>通道系結 Token
就目前而言，當用戶端應用程式使用 Kerberos、Digest 或 NTLM 透過 HTTPS 向伺服器驗證自身時，首先會建立傳輸層安全性 (TLS) 通道，並且使用此通道進行驗證。

通道系結權杖是受 TLS 保護的外部通道的屬性，可用來將外部通道系結至用戶端驗證內部通道上的交談。

如果發生「中間人」攻擊，而它們正在解密並重新加密 SSL 流量，則金鑰將不會相符。  AD FS 會決定 web 流覽 r 和本身之間是否有一些東西。  這會導致 Kerberos 驗證失敗，而且系統會提示使用者輸入401對話方塊，而不是使用 SSO 體驗。

這可能是由下列原因所造成：
 - 在瀏覽器與 AD FS 之間的任何東西
 - Fiddler
 - 反向 proxy 執行 SSL 橋接

根據預設，AD FS 會將此設定為 [允許]。  您可以使用 PowerShell Cmdlet 來變更這種設定 `Set-ADFSProperties -ExtendProtectionTokenCheck`

如需詳細資訊，請參閱 [AD FS 安全規劃與部署的最佳做法](../../ad-fs/design/best-practices-for-secure-planning-and-deployment-of-ad-fs.md)。

## <a name="internet-explorer-configuration"></a>Internet Explorer 設定

依預設，Internet explorer 會有下列方法：

1. Internet explorer 將會在標頭中，從 AD FS 收到401的回應。
2. 這會指示網頁瀏覽器取得要傳回給 AD FS 的 Kerberos 或 NTLM 票證。
3. 根據預設，如果在標頭中有一組協商，則 IE 會嘗試 (SPNEGO) ，而不需要使用者互動。  它只適用于內部網路網站。

有2個主要專案可以防止這種情況發生。

- 啟用整合式 Windows 驗證不會在 IE 的屬性中進行檢查。  這位於 [網際網路選項]-> Advanced-> 安全性。

   ![[網際網路選項] 對話方塊之 [Advanced] 索引標籤的螢幕擷取畫面，其中已呼叫 [啟用整合式 Windows 驗證] 選項。](media/ad-fs-tshoot-iwa/iwa4.png)

- 未正確設定安全性區域

  - Fqdn 不在內部網路區域中

  - AD FS 的 URL 不在內部網路區域中。

    ![[網際網路選項] 對話方塊前方 [近端內部網路] 視窗的螢幕擷取畫面。](media/ad-fs-tshoot-iwa/iwa5.png)

## <a name="next-steps"></a>後續步驟

- [AD FS 疑難排解](ad-fs-tshoot-overview.md)
