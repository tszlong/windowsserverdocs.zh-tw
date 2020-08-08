---
title: 設定條件式存取原則
description: 建立根憑證之後，[VPN 連線能力] 會觸發在客戶的租使用者中建立「VPN 伺服器」雲端應用程式。
ms.topic: article
ms.date: 05/25/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 7535de9f11a8daf38ad1aab2fe95a620f9682025
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946701"
---
# <a name="step-73-configure-the-conditional-access-policy"></a>步驟 7.3. 設定條件式存取原則

>適用于： Windows Server (半年通道) 、Windows Server 2016、Windows Server 2012 R2、Windows 10

- [**上一步：** 步驟7.2。使用 Azure AD 建立根憑證以進行 VPN 驗證](vpn-create-root-cert-for-vpn-auth-azure-ad.md)
- [**下一步：** 步驟7.4。將條件式存取根憑證部署到內部部署 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

在此步驟中，您會設定 VPN 連線能力的條件式存取原則。 當第一個根憑證建立于 [VPN 連線能力] 分頁時，它會自動在租使用者中建立「VPN 伺服器」雲端應用程式。

建立指派給 VPN 使用者群組的條件式存取原則，並將**雲端應用程式**的範圍設為**vpn 伺服器**：

- **使用者**： VPN 使用者
- **雲端應用程式**： VPN 伺服器
- **授與 (存取控制) ： [** 需要多重要素驗證]。 如有需要，可以使用其他控制項。

程式 **：** 此步驟涵蓋建立最基本的條件式存取原則。如有需要，可以使用其他條件和控制項。


1. 在 [**條件式存取**] 頁面頂端的工具列中，選取 [**新增**]。

    ![在條件式存取頁面上選取新增](../../media/Always-On-Vpn/07.png)

2. 在 [**新增**] 頁面的 [**名稱**] 方塊中，輸入原則的名稱。 例如，輸入**VPN 原則**。

    ![在條件式存取頁面上新增名稱](../../media/Always-On-Vpn/08.png)

3. 在 [**指派**] 區段中，選取 [**使用者和群組**]。

    ![選取使用者和群組](../../media/Always-On-Vpn/09.png)

4. 在 [使用者和群組]**** 頁面上，執行下列步驟︰

    ![選取測試使用者](../../media/Always-On-Vpn/10.png)

    a. 選取 [**選取使用者和群組**]。

    b. 選取 [選取]  。

    c. 在 [**選取**] 頁面上，選取 [ **VPN 使用者**] 群組，然後選取 [**選取**]。

    d. 在 [**使用者和群組**] 頁面上，選取 [**完成**]。

5. 在 [新增]**** 頁面上，執行下列步驟：

    ![選取雲端應用程式](../../media/Always-On-Vpn/11.png)

    a. 在 [**指派**] 區段中，選取 [**雲端應用程式**]。

    b. 在 [**雲端應用程式**] 頁面上，選取 [**選取應用程式**]。

    d. 選取 [VPN 伺服器]****。

6.  在 [**新增**] 頁面上，若要開啟**授**與頁面，請在 [**控制項**] 區段中選取 **[授**與]。

    ![選取授與](../../media/Always-On-Vpn/13.png)

7.  在 [授與]**** 頁面上，執行下列步驟︰

    ![選取需要多重要素驗證](../../media/Always-On-Vpn/14.png)

    a. 選取 [需要多重要素驗證]  。

    b. 選取 [選取]  。

8.  在 [**新增**] 頁面的 [**啟用原則**] 底下，選取 [**開啟**]。

    ![啟用原則](../../media/Always-On-Vpn/15.png)

9.  在 [**新增**] 頁面上，選取 [**建立**]。


## <a name="next-steps"></a>後續步驟
[步驟7.4。將條件式存取根憑證部署到內部部署 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)：在此步驟中，您會將條件式存取根憑證部署為受信任的根憑證，以進行 VPN 驗證至您的內部部署 ad。
