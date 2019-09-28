---
ms.assetid: 3a840b63-78b7-4e62-af7b-497026bfdb93
title: 逐步解說指南-使用條件式存取控制管理風險
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: aefcd597a580de526a758c6d026c6c91d02d10c8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407466"
---
# <a name="walkthrough-guide-manage-risk-with-conditional-access-control"></a>逐步解說指南：使用條件式存取控制管理風險




## <a name="about-this-guide"></a>關於本指南
本逐步解說提供的指示可讓您透過 Windows Server 2012 R2 中的 Active Directory 同盟服務（AD FS）中的條件式存取控制機制，使用其中一個因素（使用者資料）來管理風險。 如需 Windows Server 2012 R2 AD FS 中的條件式存取控制和授權機制的詳細資訊，請參閱[使用條件式存取控制管理風險](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)。

本逐步解說包含下列各節：

-   [步驟 1：設定實驗室環境 @ no__t-0

-   [步驟 2：確認預設 AD FS 存取控制機制 @ no__t-0

-   [步驟 3：根據使用者資料設定條件式存取控制原則 @ no__t-0

-   [步驟 4：驗證條件式存取控制機制 @ no__t-0

## <a name="BKMK_1"></a>步驟1：設定實驗室環境
若要完成本逐步解說，您的環境必須包含下列元件：

-   具有測試使用者和群組帳戶的 Active Directory 網域，在 Windows Server 2008、Windows Server 2008 R2 或 Windows Server 2012 上執行，其架構已升級至 Windows server 2012 R2 或在 Windows Server 2012 R2 上執行的 Active Directory 網域

-   在 Windows Server 2012 R2 上執行的同盟伺服器

-   裝載範例應用程式的網頁伺服器

-   用來存取範例應用程式的用戶端電腦

> [!WARNING]
> 強烈建議 (在生產或測試環境中) 您不要使用相同的電腦作為同盟伺服器和網頁伺服器。

在這個環境中，同盟伺服器會發行所需的宣告，讓使用者能夠存取範例應用程式。 裝載範例應用程式的網頁伺服器會信任出示同盟伺服器發行之宣告的使用者。

如需有關如何設定此環境的指示，請參閱在[Windows Server 2012 R2 中設定 AD FS 的實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

## <a name="BKMK_2"></a>步驟2：驗證預設的 AD FS 存取控制機制
在這個步驟您要驗證預設的 AD FS 存取控制機制，使用者會被重新導向到 AD FS 登入頁面、提供有效的認證，然後授與應用程式的存取權。 您可以使用**Robert Hatley** AD 帳戶，以及您在[為 Windows Server 2012 R2 中的 AD FS 設定實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)中所設定的**claimapp**範例應用程式。

#### <a name="to-verify-the-default-ad-fs-access-control-mechanism"></a>驗證預設的 AD FS 存取控制機制

1.  在您的用戶端電腦上，開啟瀏覽器視窗，並流覽至您的範例應用程式： **https://webserv1.contoso.com/claimapp** 。

    這個動作會將要求自動重新導向到同盟伺服器，且會提示您以使用者名稱和密碼登入。

2.  輸入您在[為 Windows Server 2012 R2 中的 AD FS 設定實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)中所建立的**Robert Hatley** AD 帳號憑證。

    您將會獲得應用程式的存取權。

## <a name="BKMK_3"></a>步驟3：根據使用者資料設定條件式存取控制原則
在這個步驟中，您將根據使用者群組成員資格資料設定存取控制原則。 換句話說，您將為代表範例應用程式 ( **claimapp** ) 的信賴憑證者信任，在同盟伺服器上設定 [發行授權規則]。 根據此規則的邏輯， **Robert Hatley** AD 使用者將會發出存取此應用程式所需的宣告，因為他屬於**財務**群組。 您已在為[Windows Server 2012 R2 中的 AD FS 設定實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)中，將**Robert Hatley**帳戶新增至**財務**群組。

您可以使用 AD FS 管理主控台或透過 Windows PowerShell 完成此工作。

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-the-ad-fs-management-console"></a>透過 AD FS 管理主控台根據使用者資料來設定條件式存取控制原則

1.  在 AD FS 管理主控台中，瀏覽到 [信任關係]，然後到 [信賴憑證者信任]。

2.  選取代表您範例應用程式 (**claimapp**) 的信賴憑證者信任，然後在 [執行] 窗格或在此信賴憑證者信任按一下滑鼠右鍵，選取 [編輯宣告規則]。

3.  在 [編輯 claimapp 宣告規則] 視窗中，選取 [發行授權規則] 索引標籤，然後按一下 [新增規則]。

4.  在 [新增發行授權宣告規則精靈]中，選取 [選取規則範本]頁面上的 [根據連入宣告允許或拒絕使用者] 宣告規則範本，然後按 [下一步]。

5.  在 [設定規則] 頁面上，執行下列所有動作，然後按一下 [完成]：

    1.  輸入宣告規則的名稱，例如 **TestRule**。

    2.  選取 [群組 SID] 作為 [連入宣告類型]。

    3.  按一下 [瀏覽]，輸入 **Finance** 作為 AD 測試群組的名稱，然後針對 [連入宣告值] 欄位加以解析。

    4.  選取 [拒絕具有這個傳入宣告的使用者的存取] 選項。

6.  在 [編輯 claimapp 宣告規則] 視窗中，請務必刪除建立此信賴憑證者信任時，預設建立的 [允許所有使用者存取] 規則。

#### <a name="to-configure-conditional-access-control-policy-based-on-user-data-via-windows-powershell"></a>根據使用者資料透過 Windows PowerShell 設定條件式存取控制原則

1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗，並執行下列命令：


~~~
`$rp = Get-AdfsRelyingPartyTrust -Name claimapp`
~~~


2. 在相同的 Windows PowerShell 命令視窗中，執行下列命令：


~~~
`$GroupAuthzRule = '@RuleTemplate = "Authorization" @RuleName = "Foo" c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i)<group_SID>$"] =>issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");'
Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -IssuanceAuthorizationRules $GroupAuthzRule`
~~~

> [!NOTE]
> 請務必將 <group_SID> 取代為 AD **Finance** 群組的 SID 值。

## <a name="BKMK_4"></a>步驟4：驗證條件式存取控制機制
在這個步驟中，您將驗證上一個步驟中設定的條件式存取控制原則。 您可以使用下列程序來驗證 **Robert Hatley** AD 使用者可以存取您的範例應用程式，因為他屬於 **Finance** 群組，而不屬於 **Finance** 群組的 AD 使用者無法存取範例應用程式。

1.  在您的用戶端電腦上，開啟瀏覽器視窗，並流覽至您的範例應用程式： **https://webserv1.contoso.com/claimapp**

    這個動作會將要求自動重新導向到同盟伺服器，且會提示您以使用者名稱和密碼登入。

2.  輸入您在[為 Windows Server 2012 R2 中的 AD FS 設定實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)中所建立的**Robert Hatley** AD 帳號憑證。

    您將會獲得應用程式的存取權。

3.  輸入另一個不屬於 **Finance** 群組的 AD 使用者認證 （如需如何在 AD 中建立使用者帳戶的詳細資訊，請參閱[https://technet.microsoft.com/library/cc7833232.aspx](https://technet.microsoft.com/library/cc783323%28v=ws.10%29.aspx)。

    此時，因為您在上一個步驟中設定的存取控制原則，所以會針對不屬於**財務**群組的此 AD 使用者顯示「拒絕存取」訊息。 預設郵件內文為 @no__t 0You 未獲授權，無法存取此網站。按一下這裡登出並重新登入，或洽詢您的系統管理員以取得許可權。 ** 。不過，您可以完全自訂這段文字內容。 如需如何自訂登入體驗的詳細資訊，請參閱＜ [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx)＞。

## <a name="see-also"></a>另請參閱
[使用條件式存取控制管理風險](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)
[在 Windows Server 2012 R2 中設定 AD FS 的實驗室環境](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



