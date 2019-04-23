---
title: 使用 Azure AD 建立 VPN 驗證的根憑證
description: Azure AD 會使用 VPN 憑證來簽署簽發給 Windows 10 用戶端，VPN 連線能力的 Azure AD 進行驗證時的憑證。 標示為主要憑證是 Azure AD 使用的簽發者。
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 14ef17ab403cc4e7c9891f4ede48e41c25e8522d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851559"
---
# <a name="step-72-create-conditional-access-root-certificates-for-vpn-authentication-with-azure-ad"></a>步驟 7.2. 建立與 Azure AD 條件式存取的 VPN 驗證的根憑證

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows 10

&#171;  [**前一個：** 步驟 7.1.設定 EAP-TLS 要略過憑證撤銷清單 (CRL) 檢查](vpn-config-eap-tls-to-ignore-crl-checking.md)<br>
&#187;[ **下一步:** 步驟 7.3.設定條件式存取原則](vpn-config-conditional-access-policy.md)

在此步驟中，您可以設定 VPN 驗證的條件式存取的根憑證與 Azure AD 中，會自動建立稱為租用戶中的 VPN 伺服器的雲端應用程式。 若要設定 VPN 連線能力的條件式存取，您需要：

1. （您可以建立多個憑證） 在 Azure 入口網站中建立 VPN 憑證。
2. 下載 VPN 憑證。
2. 將憑證部署到您的 VPN 與 NPS 伺服器。

當使用者嘗試的 VPN 連線時，VPN 用戶端會在 Windows 10 用戶端會呼叫至 Web 帳戶管理員 (WAM)。 WAM 會對 VPN 伺服器的雲端應用程式呼叫。 當滿足條件和條件式存取原則中的控制項時，Azure AD 會發給 WAM 存留較短 （1 小時） 憑證的表單中的語彙基元。 WAM 將憑證放在使用者的憑證存放區，並關閉控制給 VPN 用戶端。  

VPN 用戶端再由 Azure AD，憑證問題傳送認證驗證 VPN。  Azure AD 會使用憑證標示為 **主要** 為簽發者 [VPN 連線] 刀鋒視窗中。 

在 Azure 入口網站中，您會建立兩種憑證以一個憑證即將到期時管理轉換。 當您建立憑證時，您會選擇它是否為主要憑證，在驗證期間用來簽署連線憑證。

**程序：**

1. 登入您[Azure 入口網站](https://portal.azure.com)身為全域管理員。

2. 在左側功能表中，按一下  **Azure Active Directory**。 

    ![選取 Azure Active Directory](../../media/Always-On-Vpn/01.png)

3. 在  **Azure Active Directory**頁面上，於**管理**區段中，按一下**條件式存取**。

    ![選取 條件式存取](../../media/Always-On-Vpn/02.png)

4. 在 **條件式存取**頁面上，於**管理**區段中，按一下**VPN 連線能力 （預覽）**。

    ![選取 VPN 連線能力](../../media/Always-On-Vpn/03.png)

5. 在  **VPN 連線能力**頁面上，按一下**新憑證**。

    ![選取新的憑證](../../media/Always-On-Vpn/04.png)

6. 在 **新增**頁面上，執行下列步驟：

    ![選取持續時間和主要](../../media/Always-On-Vpn/05.png)

    a. 針對**選取持續時間**，選取 1 或 2 年。 您可以新增最多兩個憑證，憑證即將到期時管理轉換。 您可以選擇哪一個是主要 （一個在驗證期間用來簽署連線憑證）。

    b. 針對**主要**，選取**是**。

    c.  按一下 [建立] 。

## <a name="next-step"></a>後續步驟
[7.3 的步驟。設定條件式存取原則](vpn-config-conditional-access-policy.md):在此步驟中，您可以設定 VPN 連線能力的條件式存取原則。 

---