---
title: "Credential 保護中的新功能"
description: "Windows Server 安全性"
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
ms.openlocfilehash: 0556c606b987a69eae663b0196467f532d5a307a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="whats-new-in-credential-protection"></a>Credential 保護中的新功能

## <a name="credential-guard-for-signed-in-user"></a>登入的使用者的 credential Guard

開始使用 Windows 10 版本 1507，Kerberos 和 NTLM 使用模擬為基礎的安全性保護工作階段登入的使用者登入 Kerberos 與 NTLM 機密資訊。 

開始使用 Windows 10，版本 1511，認證管理員會使用保護的網域 credential 類型的已儲存的認證模擬為基礎的安全性。 登入認證和網域已儲存的認證將不會被傳送至遠端使用遠端桌面主機。 可以將 UEFI 鎖定不支援 credential Guard。

開始使用 Windows 10，版本 1607，隔離的使用者模式已隨附 HYPER-V，不會再安裝分開 Credential Guard 部署。

[深入了解 Credential Guard](https://technet.microsoft.com/en-us/itpro/windows/keep-secure/credential-guard)。


## <a name="remote-credential-guard-for-signed-in-user"></a>使用者登入遠端 Credential Guard

開始使用 Windows 10，版本 1607，遠端 Credential Guard 保護登入的使用者的認證，當您使用遠端桌面保護 Kerberos 和 NTLM 秘密 client 裝置上。 遠端存取網路資源，為使用者主機，驗證要求需要 client 裝置使用密碼。

開始使用 Windows 10，版本 1703，遠端 Credential Guard 保護提供的使用者的認證，當您使用遠端桌面。

[深入了解遠端 credential guard](https://technet.microsoft.com/en-us/itpro/windows/keep-secure/remote-credential-guard)。

## <a name="domain-protections"></a>網域保護

網域保護需要 Active Directory domain。

### <a name="domain-joined-device-support-for-authentication-using-public-key"></a>使用公用鍵驗證加入網域的裝置支援

從 Windows 10 版本 1507年與 Windows Server 2016 加入網域的裝置是否可以與 Windows Server 2016 網域控制站 DC 登記其結合公用鍵開始，然後裝置可以驗證使用 Windows Server 2016 DC PKINIT Kerberos 驗證公開金鑰。

開始使用 Windows Server 2016、 Kdc 支援使用 Kerberos 按鍵信任驗證。  

[深入了解加入網域的裝置與 Kerberos 按鍵信任公開主要支援](https://technet.microsoft.com/en-us/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication)。

### <a name="pkinit-freshness-extension-support"></a>PKINIT 有效期限延伸支援

開始使用 Windows 10、 1507 版和 Windows Server 2016、 Kerberos 戶端將嘗試登入附加公開金鑰根據的 PKInit 有效期限擴充功能。 

開始使用 Windows Server 2016、 Kdc 可支援 PKInit 有效期限擴充功能。  根據預設，Kdc 將不提供 PKInit 有效期限擴充功能。 

[深入了解 PKINIT 有效期限延伸支援](https://technet.microsoft.com/en-us/windows-server-docs/security/kerberos/whats-new-in-kerberos-authentication)。

### <a name="rolling-public-key-only-users-ntlm-secrets"></a>正在公開按鍵只使用者 NTLM 密碼

開始使用 Windows Server 2016 網域功能等級 (DFL)，Dc 可支援循環公用按鍵只使用者的 NTLM 的機密資訊。 找不到較低 DFLs 中的，則此功能。

> [!WARNING] 
> 網域控制站加入網域的循環 NTLM 可支援之前 DC 已更新的至少 2016 年 11 月 8 日維護執行俠當機的風險。 

設定： 新的網域，此功能預設。 現有的網域它必須設定 Active Directory 系統管理員中心] 中： 

1. 從 「 Active Directory 系統管理員中心 」，以滑鼠右鍵按一下網域在左窗格中，然後選取 [**屬性**。

    ![網域屬性](../media/Credentials-Protection-And-Management/domain-properties.png)
    
2. 選取 [**讓循環過期 NTLM 密碼的登入期間，使用者必須使用 Microsoft Passport 或智慧卡互動式登入**。

    ![Autoroll 過期 NTLM 密碼](../media/Credentials-Protection-And-Management/autoroll-ntlm.png)

3. 按一下**[確定]**。 

### <a name="allowing-network-ntlm-when-user-is-restricted-to-specific-domain-joined-devices"></a>允許網路 NTLM 時限制使用者特定加入網域的裝置

開頭和 Windows Server 2016 網域功能層級 (DFL)，Dc 可支援允許網路 NTLM 使用者時限於特定加入網域的裝置。 在下方 DFLs 無法使用這項功能。

設定： 驗證原則中，按一下 [**允許 NTLM 網路驗證使用者限制時選取裝置**。 

[深入了解驗證原則](https://technet.microsoft.com/en-us/windows-server-docs/security/credentials-protection-and-management/authentication-policies-and-authentication-policy-silos)。
