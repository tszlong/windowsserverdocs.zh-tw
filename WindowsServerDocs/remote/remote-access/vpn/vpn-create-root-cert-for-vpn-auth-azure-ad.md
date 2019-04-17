---
title: 使用 Azure AD 建立 VPN 驗證的根憑證
description: Azure AD 會使用 VPN 憑證來簽署的 VPN 連線的 Azure ad 驗證時發行至 Windows 10 用戶端憑證。 標示為主要的憑證是使用 Azure AD 簽發者。
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
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067114"
---
# 步驟 7.2. 建立與 Azure AD 的條件式存取 VPN 驗證的根憑證

>適用於： Windows Server （半年度管道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #171; [**先前：** 7.1 步驟。設定 EAP-TLS 忽略檢查憑證撤銷清單 (CRL)](vpn-config-eap-tls-to-ignore-crl-checking.md)<br>
& #187;[**下一步：** 7.3 步驟。設定條件式存取原則](vpn-config-conditional-access-policy.md)

在此步驟中，您可以設定 VPN 驗證的條件式存取根憑證和租用戶中稱為 VPN 伺服器的雲端應用程式會自動建立的 Azure AD。 若要設定的 VPN 連線的條件式存取，您需要：

1. 建立 VPN 憑證 （您可以建立多個憑證） 在 Azure 入口網站。
2. 下載 VPN 憑證。
2. 將憑證部署至您 VPN 和 NPS 伺服器。

當使用者嘗試 VPN 連線時，VPN 用戶端會在 Windows 10 用戶端上會呼叫到 Web 帳戶管理員 (WAM)。 WAM 會呼叫到 VPN 伺服器雲端應用程式。 當條件和條件式存取原則中的控制項都已符合時，Azure AD 發出 WAM 短期 （1 小時） 憑證的表單中的語彙基元。 WAM 將憑證放在使用者的憑證存放區，並將關閉控制傳遞給 VPN 用戶端。  

VPN 用戶端則會傳送憑證問題的 Azure AD 認證驗證為 VPN。Azure AD 使用會標示的憑證做為**主要**做為發行者的 VPN 連線刀鋒視窗中。 

在 Azure 入口網站，您必須建立兩個以管理轉換時的一個憑證即將到期的憑證。 當您建立一個憑證時，您可以選擇是否它是主要的憑證，以在驗證期間用來簽署憑證的連線。

**程序：**

1. 全域系統管理員身分登入您[的 Azure 入口網站](https://portal.azure.com)。

2. 在左功能表中，按一下**Azure Active Directory**。 

    ![選取 [Azure Active Directory](../../media/Always-On-Vpn/01.png)

3. 在**Azure Active Directory**頁面上，在 [**管理**] 區段中，按一下 [**條件式存取**。

    ![選取 [條件式存取](../../media/Always-On-Vpn/02.png)

4. **條件式存取**在頁面上，在 [**管理**] 區段中，按一下 [ **VPN 連線 （預覽）**。

    ![選取 VPN 連線](../../media/Always-On-Vpn/03.png)

5. **VPN 連線**在頁面上，按一下 [**新的憑證**。

    ![選取新的憑證](../../media/Always-On-Vpn/04.png)

6. 在**新**頁面上，執行下列步驟：

    ![選取的持續時間和主要](../../media/Always-On-Vpn/05.png)

    a. 對於**選取的持續時間**，選取 1 或 2 年。 您可以新增最多 2 個憑證，以管理轉換何時即將到期的憑證。 您可以選擇哪一個是主要 （一個在驗證期間用來簽署憑證的連線）。

    b。 針對**主要**，選取 [**是**]。

    c. 按一下 \[建立\]****。

## 後續步驟
[步驟 7.3。設定條件式存取原則](vpn-config-conditional-access-policy.md)： 在此步驟中，您可以設定的 VPN 連線的條件式存取原則。 

---