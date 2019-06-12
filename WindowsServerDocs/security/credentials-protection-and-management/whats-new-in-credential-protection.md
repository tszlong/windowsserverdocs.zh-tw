---
title: 認證保護中最新消息
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b0b5180-f65a-43ac-8ef3-66014116f297
author: gitmichiko
ms.author: michikos
manager: dongill
ms.date: 03/06/2017
ms.openlocfilehash: 475b6a0b24b811008ee213c1604d98d9aa9eb092
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447031"
---
# <a name="whats-new-in-credential-protection"></a>認證保護中最新消息

## <a name="credential-guard-for-signed-in-user"></a>Credential Guard，登入的使用者

從 Windows 10 版本 1507、 Kerberos 和 NTLM 使用虛擬化型安全性來保護 Kerberos 與 NTLM 登入的使用者登入工作階段的機密資訊。 

從 Windows 10 版本 1511，認證管理員可以使用虛擬化型安全性來保護網域認證類型的已儲存的認證。 登入的認證和已儲存的網域認證將不會傳遞至使用遠端桌面的遠端主機。 不與 UEFI 鎖定，您可以啟用 Credential Guard。

從 Windows 10 版本 1607 版，請隔離使用者模式是隨附於 HYPER-V 讓它不再分開安裝 Credential Guard 部署。

[深入了解 Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard)。


## <a name="remote-credential-guard-for-signed-in-user"></a>遠端登入使用者的 Credential Guard

使用遠端桌面來保護用戶端裝置上的 Kerberos 和 NTLM 密碼時，開頭為 Windows 10 1607年版中，遠端 Credential Guard 保護登入的使用者認證。 評估網路資源，以使用者身分遠端主機的驗證要求會要求用戶端裝置使用祕密。

使用遠端桌面時，開頭為 Windows 10 版本 1703年遠端 Credential Guard 保護提供的使用者認證。

[深入了解遠端 credential guard](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard)。

## <a name="domain-protections"></a>網域保護

網域保護需要 Active Directory 網域。

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>使用公開金鑰進行驗證的已加入網域的裝置支援

從 Windows 10 1507年版與 Windows Server 2016 中，如果已加入網域的裝置可註冊其繫結的公開金鑰與 Windows Server 2016 網域控制站 (DC)，然後裝置可以向使用 Kerberos PKINIT 的公用金鑰Windows Server 2016 DC 驗證。

從 Windows Server 2016 開始，Kdc 會支援使用 Kerberos 信任驗證。  

[深入了解公用金鑰的支援，針對已加入網域的裝置和 Kerberos 信任](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication)。

### <a name="pkinit-freshness-extension-support"></a>PKINIT 有效期限延伸模組支援

從 Windows 10，1507年版和 Windows Server 2016 開始，Kerberos 用戶端會嘗試 PKInit 有效期限延伸模組的公用金鑰型登入。 

從 Windows Server 2016 開始，Kdc 可以支援 PKInit 有效期限延伸模組。  根據預設，Kdc 將不會提供 PKInit 有效期限延伸模組。 

[深入了解 PKINIT 有效期限延伸模組支援](https://technet.microsoft.com/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication)。

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>輪流公用金鑰唯一使用者的 NTLM 密碼

從 Windows Server 2016 網域功能等級 (DFL)，網域控制站可以支援輪流公用金鑰唯一使用者的 NTLM 祕密。 這項功能是在較低的 DFLs 中找不到。

> [!WARNING] 
> 將網域控制站加入網域，以循環 DC 已更新至少 2016 年 11 月 8 日服務執行之前啟用的 NTLM 祕密的 DC 的損毀的風險。 

設定：對於新的網域，預設會啟用這項功能。 對於現有的網域，它必須在 Active Directory Administrative center 中將其設定： 

1. 從 Active Directory 管理中心，以滑鼠右鍵按一下左窗格上的網域，然後選取**屬性**。

    ![網域屬性](../media/Credentials-Protection-And-Management/domain-properties.png)

2. 選取 **啟用輪流即將過期的 NTLM 祕密在註冊時，才能使用 Microsoft Passport 或智慧卡進行互動式登入的使用者**。

    ![Autoroll 過期 NTLM 祕密](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. 按一下 [確定]  。 

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>當使用者限制為特定的已加入網域的裝置時，允許網路 NTLM

從 Windows Server 2016 網域功能等級 (DFL)，網域控制站可支援允許網路 NTLM，當使用者以限制為特定網域的裝置。 這項功能是在較低的 DFLs 中無法使用。

設定：在 驗證原則中，按一下 **選取的裝置，允許 NTLM 網路驗證時使用者僅限於**。 

[深入了解驗證原則](https://technet.microsoft.com/windows-server-docs/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos)。
