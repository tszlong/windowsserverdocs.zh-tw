---
title: 使用 Azure AD 建立 VPN 驗證的根憑證
description: Azure AD 會使用 VPN 憑證來簽署簽發給 Windows 10 用戶端，VPN 連線能力的 Azure AD 進行驗證時的憑證。 標示為主要憑證是 Azure AD 使用的簽發者。
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.workload: identity
ms.topic: article
ms.date: 06/28/2019
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 40403f6be65c84cc1e2506a222a009400fcdaacc
ms.sourcegitcommit: 34232723f15c7b4d6a32ca38b7348a417ba30ae0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2019
ms.locfileid: "67464953"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>步驟 7.2. 建立與 Azure AD 條件式存取的 VPN 驗證的根憑證

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows 10

- [**前一個：** 步驟 7.1.設定 EAP-TLS 以忽略憑證撤銷清單 (CRL) 檢查](vpn-config-eap-tls-to-ignore-crl-checking.md)
- [**下一步:** 步驟 7.3.設定條件式存取原則](vpn-config-conditional-access-policy.md)

在此步驟中，您可以設定 VPN 驗證的條件式存取的根憑證與 Azure AD 中，會自動建立稱為租用戶中的 VPN 伺服器的雲端應用程式。 若要設定 VPN 連線能力的條件式存取，您需要：

1. 在 Azure 入口網站中建立 VPN 憑證。
2. 下載 VPN 憑證。
3. 將憑證部署到您的 VPN 與 NPS 伺服器。

> [!IMPORTANT]
> 一旦在 Azure 入口網站中建立 VPN 憑證時，Azure AD 會開始立即使用它來短期存在的憑證發行給 VPN 用戶端。 請務必 VPN 憑證，可立即部署到 VPN 伺服器，以避免與認證驗證的 VPN 用戶端的任何問題。

當使用者嘗試的 VPN 連線時，VPN 用戶端會在 Windows 10 用戶端會呼叫至 Web 帳戶管理員 (WAM)。 WAM 會對 VPN 伺服器的雲端應用程式呼叫。 當滿足條件和條件式存取原則中的控制項時，Azure AD 會發給 WAM 存留較短 （1 小時） 憑證的表單中的語彙基元。 WAM 將憑證放在使用者的憑證存放區，並關閉控制給 VPN 用戶端。  

VPN 用戶端再由 Azure AD，憑證問題傳送認證驗證 VPN。  

> [!NOTE]
> Azure AD 會使用 VPN 連線 刀鋒視窗中最近建立的憑證，簽發者。

**程序：**

1. 登入您[Azure 入口網站](https://portal.azure.com)身為全域管理員。
2. 在左側功能表中，按一下  **Azure Active Directory**。
3. 在  **Azure Active Directory**頁面上，於**管理**區段中，按一下**條件式存取**。
4. 在 **條件式存取**頁面上，於**管理**區段中，按一下**VPN 連線能力 （預覽）** 。
5. 在  **VPN 連線能力**頁面上，按一下**新憑證**。
6. 在 **新增**頁面上，執行下列步驟：。 針對**選取持續時間**，選取 1、 2 或 3 年。
   b. 選取 [建立]  。

## <a name="next-steps"></a>後續步驟

[步驟 7.3.設定條件式存取原則](vpn-config-conditional-access-policy.md):在此步驟中，您可以設定 VPN 連線能力的條件式存取原則。
