---
title: 認證保護的新功能
description: Windows Server 安全性
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f297
author: gitmichiko
ms.author: michikos
manager: dongill
ms.date: 03/06/2017
ms.openlocfilehash: 521562b938b002e4c5fe5ffee5fcd7c677551f98
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995753"
---
# <a name="whats-new-in-credential-protection"></a>認證保護的新功能

## <a name="credential-guard-for-signed-in-user"></a>已登入使用者的 Credential Guard

從 Windows 10 版本1507、Kerberos 和 NTLM 開始，會使用虛擬化安全性來保護已登入之使用者登錄會話的 Kerberos & NTLM 密碼。

從 Windows 10 版本1511開始，認證管理員會使用虛擬化安全性來保護網域認證類型的已儲存認證。 登入認證和已儲存的網域認證將不會使用遠端桌面傳遞至遠端主機。 您可以在沒有 UEFI 鎖定的情況下啟用 Credential Guard。

從 Windows 10 版本1607開始，獨立的使用者模式會包含在 Hyper-v 中，因此不會再針對 Credential Guard 部署另行安裝。

[深入瞭解 Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard)。


## <a name="remote-credential-guard-for-signed-in-user"></a>已登入使用者的遠端認證防護

從 Windows 10 版本1607開始，遠端認證防護會在用戶端裝置上保護 Kerberos 和 NTLM 密碼，以在使用遠端桌面時保護已登入的使用者認證。 若要讓遠端主機以使用者的身分評估網路資源，驗證要求需要用戶端裝置才能使用密碼。

從 Windows 10 版本1703開始，遠端認證防護會在使用遠端桌面時，保護提供的使用者認證。

[深入瞭解遠端認證防護](/windows/security/identity-protection/remote-credential-guard)。

## <a name="domain-protections"></a>網域保護

網域保護需要 Active Directory 網域。

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>已加入網域的裝置支援使用公開金鑰進行驗證

從 Windows 10 版本1507和 Windows Server 2016 開始，如果已加入網域的裝置能夠在 Windows Server 2016 網域控制站 (DC) 中登錄其系結的公開金鑰，則裝置可以對 Windows Server 2016 DC 使用 Kerberos PKINIT 驗證，以公開金鑰進行驗證。

從 Windows Server 2016 開始，Kdc 支援使用 Kerberos 金鑰信任進行驗證。

[深入瞭解 & Kerberos 金鑰信任的已加入網域裝置的公開金鑰支援](../kerberos/whats-new-in-kerberos-authentication.md)。

### <a name="pkinit-freshness-extension-support"></a>PKINIT Freshness extension 支援

從 Windows 10 版本1507和 Windows Server 2016 開始，Kerberos 用戶端會針對公開金鑰型的登入嘗試 PKInit 有效期限延伸模組。

從 Windows Server 2016 開始，Kdc 可以支援 PKInit freshness extension。  根據預設，Kdc 不會提供 PKInit freshness extension。

[深入瞭解 PKINIT 時效性 extension 支援](../kerberos/whats-new-in-kerberos-authentication.md)。

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>僅滾動公開金鑰使用者的 NTLM 秘密

從 Windows Server 2016 網域功能等級開始 (DFL) ，Dc 可以支援僅限公開金鑰的使用者 NTLM 秘密。 這項功能在較低的 DFLs 中 unavailble。

> [!WARNING]
> 將網域控制站新增到已啟用輪替 NTLM 秘密的網域中，在 DC 更新至少為2016年11月8日之前，服務會執行 DC 損毀的風險。

設定：對於新的網域，此功能預設為啟用。 針對現有的網域，必須在 Active Directory 管理中心設定：

1. 從 Active Directory 的管理中心，以滑鼠右鍵按一下左窗格中的網域，然後選取 [**屬性**]。

    ![網域屬性](../media/Credentials-Protection-And-Management/domain-properties.png)

2. 選取 [在**登入期間，針對需要使用 Microsoft Passport 或智慧卡進行互動式登入的使用者，啟用正在進行過期 NTLM 秘密的回復**]。

    ![Autoroll 過期的 NTLM 秘密](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. 按一下 [確定]  。

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>當使用者限制為已加入網域的特定裝置時，允許網路 NTLM

從 Windows Server 2016 網域功能等級 (DFL) ，Dc 可以支援在使用者限制為已加入網域的特定裝置時允許網路 NTLM。 這項功能無法在較低的 DFLs 中使用。

設定：在 [驗證原則] 上，按一下 **[當使用者受限於選取的裝置時，允許 NTLM 網路驗證**]。

[深入瞭解驗證原則](./authentication-policies-and-authentication-policy-silos.md)。