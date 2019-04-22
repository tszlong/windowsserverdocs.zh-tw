---
title: AD FS 疑難排解-整合式 Windows 驗證
description: 本文件說明如何針對整合式的 windows 驗證進行疑難排解
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 91f252f5b0eca0f4c44e0b1a4564037298bf023c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814059"
---
# <a name="ad-fs-troubleshooting---integrated-windows-authentication"></a>AD FS 疑難排解-整合式 Windows 驗證
整合式的 Windows 驗證可讓使用者登入其 Windows 認證，並體驗單一登入 (SSO)，使用 Kerberos 或 NTLM。

## <a name="reason-integrated-windows-authentication-fails"></a>原因整合式的 windows 驗證將會失敗
有三個整合式的 windows 驗證會失敗的主要原因。 其中包括：
    - 服務主體名稱 （spn） 設定不正確
    - 通道繫結語彙基元
    - Internet Explorer 設定

## <a name="spn-misonfiguration"></a>SPN misonfiguration
服務主要名稱 (SPN) 是服務執行個體的唯一識別碼。 Kerberos 驗證會使用 Spn 來服務登入帳戶相關聯的服務執行個體。 這可讓用戶端應用程式要求的服務會驗證帳戶，即使在用戶端並沒有帳戶名稱。

相關範例學習 SPN 會搭配 AD FS 如下所示：
1. 網頁瀏覽器會查詢 Active Directory 來判斷哪一個服務帳戶執行 sts.contoso.com
2. Active Directory 會告知瀏覽器，它是 AD FS 服務帳戶。
3. 瀏覽器將會收到的 AD FS 服務帳戶的 Kerberos 票證。

如果 AD FS 服務帳戶具有設定不正確或錯誤的 SPN，這會造成問題。  查看網路追蹤，您可能會看到錯誤，例如 KRB 錯誤：KRB5KDC_ERR_S_PRINCIPAL_UNKNOWN.

使用網路追蹤 （例如 Wireshark) 您可以決定哪些瀏覽器嘗試解析的 SPN，然後使用命令列工具，setspn-Q <spn>，您可以執行該 SPN 的查閱。  它可能會找不到，或可能將它指派給 AD FS 服務帳戶以外的另一個帳戶。

![整合](media/ad-fs-tshoot-iwa/iwa3.png)

您可以查看 AD FS 服務帳戶的屬性，以確認 SPN。

![整合](media/ad-fs-tshoot-iwa/iwa1.png)

## <a name="channel-binding-token"></a>通道繫結語彙基元
目前，當用戶端應用程式將自己驗證為使用 Kerberos、 摘要或 NTLM 透過 HTTPS 的伺服器，首先會建立傳輸層安全性 (TLS) 通道，並驗證會使用這個通道。 

通道繫結語彙基元是受 TLS 保護外部通道的屬性，用於將外部通道繫結至交談用戶端驗證型內部通道上。

如果沒有"--攔截 」 攻擊的發生，而且它們 de 加密和重新加密的 SSL 流量，則不會符合索引鍵。  AD FS 將會發生 servicebus 位於中間的 web 瀏覽 r 和本身之間決定的。  這會造成 Kerberos 驗證失敗，並會提示使用者 401 的對話方塊，而不是 SSO 體驗。

這可能是由的原因：
 - 瀏覽器和 AD FS 之間的任何項目
 - Fiddler
 - 執行 SSL 橋接的反向 proxy

根據預設，AD FS 會有此設定為 [允許]。  您可以變更此設定，使用 PowerShell cmdlt `Set-ADFSProperties -ExtendProtectionTokenCheck`

如需有關這個，請參閱[安全規劃與部署 AD fs 的最佳作法](../../ad-fs/design/best-practices-for-secure-planning-and-deployment-of-ad-fs.md)。

## <a name="internet-explorer-configuration"></a>Internet Explorer 設定
根據預設，Internet explorer 將會有以下列方式：

1. Internet explorer 會從字交涉標頭中的 AD FS 收到 401 回應。
2. 這會告訴網頁瀏覽器，以取得 Kerberos 或 NTLM 的票證，以將傳送回 AD FS。
3. 根據預設 IE 嘗試執行此 (SPNEGO) 而不需要使用者互動，如果單字是標頭中的交涉。  它只適用於內部網路網站。

有 2 個主要的東西，可以防止這 happeing。
   - 啟用整合式的 Windows 驗證不會檢查內容中的 IE。  這位於 網際網路選項-> 進階-> 安全性。
![integrated](media/ad-fs-tshoot-iwa/iwa4.png)
   
   - 未正確設定安全性區域
       - Fqdn 不在內部網路區域
       - AD FS URL 不是在內部網路區域。

![整合](media/ad-fs-tshoot-iwa/iwa5.png)
## <a name="next-steps"></a>後續步驟

- [疑難排解 AD FS](ad-fs-tshoot-overview.md)