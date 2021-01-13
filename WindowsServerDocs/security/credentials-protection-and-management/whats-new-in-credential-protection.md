---
title: 認證保護的新功能
description: 瞭解已登入使用者的 Credential Guard、已登入使用者的遠端認證防護，以及網域保護。
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f297
author: gitmichiko
ms.author: michikos
manager: dongill
ms.date: 03/06/2017
ms.openlocfilehash: 8f546884ef8400da397a6f2508b9e9ee2e5422e8
ms.sourcegitcommit: decb6c8caf4851b13af271d926c650d010a6b9e9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/13/2021
ms.locfileid: "98177307"
---
# <a name="whats-new-in-credential-protection"></a>認證保護的新功能

## <a name="credential-guard-for-signed-in-user"></a>已登入使用者的 Credential Guard

從 Windows 10 開始，版本1507、Kerberos 和 NTLM 都會使用虛擬化型安全性，來保護已登入使用者登入會話的 Kerberos & NTLM 秘密。

從 Windows 10 1511 版開始，認證管理員會使用虛擬化型安全性來保護網域認證類型的已儲存認證。 使用遠端桌面時，登入認證和儲存的網域認證不會傳遞到遠端主機。 您可以在沒有 UEFI 鎖定的情況下啟用 Credential Guard。

從 Windows 10 開始，版本1607，隔離的使用者模式會隨附于 Hyper-v，因此不會再針對 Credential Guard 部署另外安裝。

[深入瞭解 Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard)。


## <a name="remote-credential-guard-for-signed-in-user"></a>已登入使用者的遠端認證防護

從 1607 Windows 10 開始，當您使用遠端桌面保護用戶端裝置上的 Kerberos 和 NTLM 秘密時，遠端 Credential Guard 會保護登入的使用者認證，以保護已登入的使用者認證。 若要讓遠端主機評估使用者的網路資源，驗證要求會要求用戶端裝置使用秘密。

從 Windows 10，1703版起，Remote Credential Guard 會在使用遠端桌面時，保護提供的使用者認證。

[深入瞭解遠端 credential guard](/windows/security/identity-protection/remote-credential-guard)。

## <a name="domain-protections"></a>網域保護

網域保護需要 Active Directory 網域。

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>已加入網域的裝置支援使用公開金鑰進行驗證

從 Windows 10 1507 版和 Windows Server 2016 開始，如果已加入網域的裝置能夠向 Windows Server 2016 網域控制站 (DC) 註冊其系結的公開金鑰，則裝置可以使用 Kerberos PKINIT 驗證對 Windows Server 2016 DC 進行驗證，以使用公開金鑰進行驗證。

從 Windows Server 2016 開始，Kdc 支援使用 Kerberos 金鑰信任進行驗證。

[深入瞭解 & Kerberos 金鑰信任的已加入網域裝置的公開金鑰支援](../kerberos/whats-new-in-kerberos-authentication.md)。

### <a name="pkinit-freshness-extension-support"></a>PKINIT 有效期限延伸模組支援

從 Windows 10、1507版和 Windows Server 2016 開始，Kerberos 用戶端會嘗試以 public key 為基礎的登入的 PKInit 有效期限延伸模組。

從 Windows Server 2016 開始，Kdc 可支援 PKInit 有效期限延伸模組。  根據預設，Kdc 不會提供 PKInit 有效期限延伸模組。

[深入瞭解 PKINIT 時效性 extension 支援](../kerberos/whats-new-in-kerberos-authentication.md)。

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>僅限滾動公開金鑰使用者的 NTLM 秘密

從 Windows Server 2016 網域功能等級開始 (DFL) ，Dc 可以支援僅限公開金鑰使用者的 NTLM 秘密。 這項功能在較低的 DFLs 中無法使用。

> [!WARNING]
> 將網域控制站新增至網域，並在已更新 DC 之前啟用的復原 NTLM 秘密2016，且至少有11月8日的服務會執行 DC 損毀的風險。

設定：針對新網域，預設會啟用此功能。 針對現有的網域，必須在 Active Directory 管理中心設定：

1. 從 Active Directory 管理中心，以滑鼠右鍵按一下左窗格中的網域，然後選取 [ **屬性**]。

    ![定義域屬性](../media/Credentials-Protection-And-Management/domain-properties.png)

2. 選取 [在 **登入期間啟用即將過期的 NTLM 秘密]，針對需要使用 Microsoft Passport 或智慧卡進行互動式登入的使用者**。

    ![Autoroll 過期的 NTLM 秘密](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. 按一下 [確定]  。

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>當使用者僅限特定已加入網域的裝置時，允許網路 NTLM

從 Windows Server 2016 網域功能等級開始 (DFL) ，Dc 可以支援在使用者僅限特定的已加入網域裝置時允許網路 NTLM。 這項功能在較低的 DFLs 中無法使用。

設定：在 [驗證原則] 上，按一下 **[當使用者僅限選取的裝置時，允許 NTLM 網路驗證**]。

[深入瞭解驗證原則](./authentication-policies-and-authentication-policy-silos.md)。