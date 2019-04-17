---
title: 設定條件式存取原則
description: 在建立根憑證之後，VPN 連線觸發程序 'VPN 伺服器' 雲端應用程式的客戶租用戶中的建立。
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
ms.openlocfilehash: 8c00855c50de79efa1b48c7b8762e1b679db4a87
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067117"
---
# 步驟 7.3. 設定條件式存取原則

>適用於： Windows Server （半年度管道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #171; [**先前：** 7.2 步驟。使用 Azure AD 中建立 VPN 驗證的根憑證](vpn-create-root-cert-for-vpn-auth-azure-ad.md)<br>
& #187;[**下一步：** 7.4 步驟。將條件式存取根憑證部署到內部部署 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

在此步驟中，您可以設定 VPN 連線的條件式存取的原則。 在 'VPN 連線' 刀鋒視窗中建立第一個根憑證時，它會自動建立 'VPN 伺服器' 雲端應用程式的租用戶中。 

建立會指派給 VPN 使用者群組和範圍**VPN 伺服器**的**雲端應用程式**的條件式存取原則： 

- **使用者**： VPN 使用者
- **雲端應用程式**： VPN 伺服器
- **授與 （存取控制）**: '要求多重要素驗證'。 如有需要，可以使用其他控制項。

**程序：** 此步驟涵蓋建立最基本的條件式存取原則。如有需要，可以使用其他條件和控制項。


1. **條件式存取**在頁面上，在工具列上上方，按一下 \ [**加入**。

    ![選取 [新增條件式存取 \] 頁面上](../../media/Always-On-Vpn/07.png)

2. 在**新**頁面上，在 [**名稱**] 方塊中，輸入您的原則的名稱。 例如，輸入**VPN 原則**。

    ![條件式存取頁面上新增原則的名稱](../../media/Always-On-Vpn/08.png)

3. 在 [**指派**] 區段中，按一下 [**使用者和群組**。

    ![選取使用者和群組](../../media/Always-On-Vpn/09.png)

4. 在**使用者和群組**頁面上，執行下列步驟：

    ![選取的測試使用者](../../media/Always-On-Vpn/10.png)

    a. 按一下 [**選取使用者及群組**。

    b。 按一下 [**選取**。

    c. 在**選取**的頁面上，選取**VPN 使用者**群組，然後按一下**選取**。

    d. 在**使用者和群組**頁面上，按一下 [**完成**]。

5. 在**新**頁面上，執行下列步驟：

    ![選取雲端應用程式](../../media/Always-On-Vpn/11.png)

    a. 在 [**指派**] 區段中，按一下 [**雲端應用程式**。

    b。 在**雲端應用程式**頁面上，按一下 [**選取應用程式**。

    d. 選取**VPN 伺服器**。

13. 在**新**頁面上，若要開啟**授與**頁面上，**控制項**] 區段中，按一下**授與**。

    ![選取授與](../../media/Always-On-Vpn/13.png)

14. 在**授與**頁面上，執行下列步驟：

    ![選取 [需要多重要素驗證](../../media/Always-On-Vpn/14.png)

    a. 選取 [**需要多重要素驗證**。

    b。 按一下 [**選取**。

15. **在**新**頁面上下**啟用原則**，請, 按一下。**

    ![啟用原則](../../media/Always-On-Vpn/15.png)

16. 在**新**頁面上，按一下 [**建立**。


## 後續步驟
[步驟 7.4。將條件式存取根憑證部署到內部部署 AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)： 在此步驟中，您將部署條件式存取根憑證做為受信任的根憑證的 VPN 驗證到您內部部署 AD。

---