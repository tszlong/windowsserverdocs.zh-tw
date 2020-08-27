---
title: 使用 Azure AD 建立 VPN 驗證的根憑證
description: Azure AD 會使用 VPN 憑證，簽署在向 Azure AD 驗證 VPN 連線能力時簽發給 Windows 10 用戶端的憑證。 標示為「主要」的憑證是 Azure AD 使用的簽發者。
ms.topic: article
ms.date: 06/28/2019
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: dfcdea3b719cee685222fdf5919ce5e674ca1e64
ms.sourcegitcommit: 52a8d5d7e969eaa07fd3a45ed6d3cb5a5173b6d1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970635"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>步驟 7.2. 使用 Azure AD 建立 VPN 驗證的條件式存取根憑證

> 適用于： Windows Server (半年通道) 、Windows Server 2016、Windows Server 2012 R2、Windows 10

- [**上一步：** 步驟7.1。設定 EAP-TLS，以忽略 (CRL) 檢查的憑證撤銷清單](vpn-config-eap-tls-to-ignore-crl-checking.md)
- [**下一步：** 步驟7.3。設定條件式存取原則](vpn-config-conditional-access-policy.md)

在此步驟中，您會設定使用 Azure AD 的 VPN 驗證條件式存取根憑證，這會自動在租使用者中建立一個名為 VPN 伺服器的雲端應用程式。 若要設定 VPN 連線能力的條件式存取，您需要執行下列動作：

1. 在 Azure 入口網站中建立 VPN 憑證。
2. 下載 VPN 憑證。
3. 將憑證部署到您的 VPN 和 NPS 伺服器。

> [!IMPORTANT]
> 一旦在 Azure 入口網站中建立 VPN 憑證之後，Azure AD 將立即開始使用它來發出短期的憑證給 VPN 用戶端。 VPN 憑證必須立即部署到 VPN 伺服器，以避免 VPN 用戶端的認證驗證發生任何問題，這是很重要的。

當使用者嘗試使用 VPN 連線時，VPN 用戶端會呼叫 Windows 10 用戶端上的 Web 帳戶管理員 (WAM) 。 WAM 會呼叫 VPN 伺服器雲端應用程式。 滿足條件式存取原則中的條件和控制項時，Azure AD 會以存留期 (1 小時) 憑證的格式來發出權杖給 WAM。 WAM 會將憑證放在使用者的憑證存放區，並將控制權交給 VPN 用戶端。 

然後 VPN 用戶端會將 Azure AD 所簽發的憑證傳送給 VPN 進行認證驗證。 

> [!NOTE]
> Azure AD 使用 VPN 連線能力分頁中最近建立的憑證做為簽發者。

**程式：**

1. 以全域管理員身分登入 [Azure 入口網站](https://portal.azure.com)。
2. 在左側功能表上，按一下 [Azure Active Directory]****。
3. 在 [ **Azure Active Directory** ] 頁面的 [ **管理** ] 區段中，按一下 [ **安全性**]。
4. 在 [ **安全性** ] 頁面的 [ **保護** ] 區段中，按一下 [ **條件式存取**]。
5. 在 **條件式存取 |原則** ] 頁面的 [ **管理** ] 區段中，按一下 [ **VPN 連線能力**]。
5. 在 [VPN 連線能力]**** 頁面上，按一下 [新增憑證]****。
6. 在 **新** 的頁面上，執行下列步驟： a。 針對 [ **選取持續時間**]，選取1、2或3年。
    b. 選取 [建立]  。

## <a name="next-steps"></a>後續步驟

[步驟7.3。設定條件式存取原則](vpn-config-conditional-access-policy.md)：在此步驟中，您會設定 VPN 連線能力的條件式存取原則。
