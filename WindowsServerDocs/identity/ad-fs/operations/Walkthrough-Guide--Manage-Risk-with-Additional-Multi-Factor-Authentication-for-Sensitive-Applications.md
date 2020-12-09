---
ms.assetid: 5fd4063d-34dc-4b15-9a88-cc6c1fff455a
title: 逐步解說指南-使用機密應用程式的額外 Multi-Factor Authentication 管理風險
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 1dc0b0c278577e7318ead6b4e3ebba04b21c8a9c
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866278"
---
# <a name="walkthrough-guide-manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>逐步解說指南：透過其他多因素驗證管理機密應用程式的風險




## <a name="about-this-guide"></a>關於本指南
本逐步解說提供的指示說明如何根據使用者的群組成員資格資料，在 Windows Server 2012 R2 中的 Active Directory 同盟服務 (AD FS) 中設定多重要素驗證 (MFA) 。

如需有關 AD FS 中的 MFA 和驗證機制的詳細資訊，請參閱 [使用敏感性應用程式的其他 Multi-Factor Authentication 管理風險](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。

本逐步解說包含下列各節：

-   [步驟 1：設定實驗室環境](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [步驟 2：驗證預設的 AD FS 驗證機制](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)

-   [步驟 3：在同盟伺服器上設定 MFA](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_3)

-   [步驟 4：驗證 MFA 機制](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_4)

## <a name="step-1-setting-up-the-lab-environment"></a><a name="BKMK_1"></a>步驟 1：設定實驗室環境
若要完成本逐步解說，您的環境必須包含下列元件：

-   具有測試使用者和群組帳戶的 Active Directory 網域，在 Windows Server 2012 R2 上執行，或是在 Windows server 2008、Windows Server 2008 R2 或 Windows Server 2012 上執行的 Active Directory 網域，且其架構已升級為 Windows Server 2012 R2

-   在 Windows Server 2012 R2 上執行的同盟伺服器

-   裝載範例應用程式的網頁伺服器

-   用來存取範例應用程式的用戶端電腦

> [!WARNING]
> 強烈建議 (在生產和測試環境中) 您不要使用相同的電腦作為同盟伺服器和網頁伺服器。

在這個環境中，同盟伺服器會發行所需的宣告，讓使用者能夠存取範例應用程式。 裝載範例應用程式的網頁伺服器會信任出示同盟伺服器發行之宣告的使用者。

如需有關如何設定此環境的指示，請參閱 [在 Windows Server 2012 R2 中設定 AD FS 的實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

## <a name="step-2-verify-the-default-ad-fs-authentication-mechanism"></a><a name="BKMK_2"></a>步驟 2：驗證預設的 AD FS 驗證機制
在這個步驟您要驗證預設的 AD FS 存取控制機制 (外部網路為 [表單驗證]，內部網路為 [Windows 驗證])，使用者會被重新導向到 AD FS 登入頁面、提供有效的認證，然後授與應用程式的存取權。 您可以使用 **Robert Hatley** AD 帳戶，以及您在 [設定實驗室環境中為 Windows Server 2012 R2 中的 AD FS 所設定](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)的 **claimapp** 範例應用程式。

1.  在您的用戶端電腦上，開啟瀏覽器視窗，然後流覽至您的範例應用程式： **https://webserv1.contoso.com/claimapp** 。

    這個動作會將要求自動重新導向到同盟伺服器，且會提示您以使用者名稱和密碼登入。

2.  輸入您在 [Windows Server 2012 R2 中為 AD FS 設定實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)中所建立的 **Robert Hatley** AD 帳號憑證。

    您將會獲得應用程式的存取權。

## <a name="step-3-configure-mfa-on-your-federation-server"></a><a name="BKMK_3"></a>步驟 3：在同盟伺服器上設定 MFA
在 Windows Server 2012 R2 的 AD FS 中，有兩個部分可設定 MFA：

-   [選取其他驗證方法](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_5)

-   [設定 MFA 原則](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_6)

### <a name="select-an-additional-authentication-method"></a><a name="BKMK_5"></a>選取其他驗證方法
若要設定 MFA，您必須選取其他驗證方法。 在這個逐步解說中，若要設定其他驗證方法，您可以選擇下列選項：

-   依預設，您可以在 Windows Server 2012 R2 的 AD FS 中選取可用的 [憑證驗證](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_7) 方法

-   設定並選取 [Windows Azure Multi-Factor Authentication](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_8)

#### <a name="certificate-authentication"></a><a name="BKMK_7"></a>憑證驗證
完成下列其中一個程序，以選取憑證驗證作為其他驗證方法：

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-the-ad-fs-management-console"></a>透過 AD FS 管理主控台將憑證驗證設為其他驗證方法

1.  在同盟伺服器的 AD FS 管理主控台中，瀏覽到 [驗證原則] 節點，然後在 [Multi-factor Authentication] 區段按一下 [通用設定] 子區段旁的 [編輯] 連結。

2.  在 [編輯通用驗證原則] 視窗中，選取 [憑證驗證] 作為其他驗證方法，然後按一下 [確定]。

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-windows-powershell"></a>透過 Windows PowerShell 將憑證驗證設為其他驗證方法

1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗，並執行下列命令：

    ```
    Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider CertificateAuthentication

    ```

    > [!WARNING]
    > 若要確認是否已順利執行此命令，可以執行 `Get-AdfsGlobalAuthenticationPolicy` 命令。

#### <a name="windows-azure-multi-factor-authentication"></a><a name="BKMK_8"></a>Windows Azure Multi-Factor Authentication
完成下列其中一個程序，以下載並設定和選取 [Windows Azure Multi-Factor Authentication] 作為同盟伺服器上的其他驗證：

1.  [透過 Windows Azure 入口網站建立多因素驗證提供者](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_a)

2.  [下載 Windows Azure Multi-Factor Authentication Server](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_b)

3.  [在同盟伺服器上安裝 Windows Azure Multi-Factor Authentication Server](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_c)

4.  [設定 Windows Azure Multi-Factor Authentication 作為其他驗證方法](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_d)

##### <a name="create-a-multi-factor-authentication-provider-via-the-windows-azure-portal"></a><a name="BKMK_a"></a>透過 Windows Azure 入口網站建立多因素驗證提供者

1.  以系統管理員身分登入 Windows Azure 入口網站。

2.  在左側選取 [Active Directory]。

3.  在 [Active Directory] 頁面的上方選取 [多因素驗證提供者]。  接著按一下底部的 [新增]。

4.  在 [應用程式服務->Active Directory] 下，選取 [多因素驗證提供者]，再選取 [快速建立]。

5.  在 [應用程式服務] 下方，選取 [Active Auth 提供者]，然後選取 [快速建立]。

6.  填寫下列欄位，然後選取 [建立]。

    1.  **名稱** -多因素驗證提供者的名稱。

    2.  **使用量模型** -Multi-Factor Authentication 提供者的使用模式。

        -   依每次驗證收費的 **每次驗證** 購買模型。 通常用於消費者導向應用程式中使用 Windows Azure Multi-Factor Authentication 的案例。

        -   **每** 個啟用的使用者購買模型（依已啟用的使用者收費）。  通常用於員工導向案例，例如 Office 365。

        如需使用模式的詳細資訊，請參閱 [Windows Azure 定價詳細資料](https://www.windowsazure.com/pricing/details/active-directory/)。

    3.  **目錄** -與 Multi-Factor Authentication 提供者相關聯的 Windows Azure Active Directory 租使用者。 這是選擇性的欄位，因為在保護內部部署應用程式安全性時，提供者不一定要與 Windows Azure Active Directory 連結。

7.  按下建立之後，就會建立多因素驗證提供者，且應該會看到下列訊息：已成功建立多因素驗證提供者。  按一下 [確定]  。

接著，您必須下載 Windows Azure Multi-Factor Authentication Server。 您可以從 Windows Azure 入口網站啟動 Windows Azure Multi-Factor Authentication 入口網站來完成此動作。

##### <a name="download-the-windows-azure-multi-factor-authentication-server"></a><a name="BKMK_b"></a>下載 Windows Azure Multi-Factor Authentication Server

1.  以系統管理員身分登入 Windows Azure 入口網站，按一下您在上述程序建立的多因素驗證提供者。 接著，按一下 [管理] 按鈕。

    這會啟動 [Windows Azure Multi-Factor Authentication] 入口網站。

2.  在 [Windows Azure Multi-Factor Authentication] 入口網站，按一下 [下載]，然後按一下 [下載] 下載 Windows Azure Multi-Factor Authentication Server 複本。

下載 Windows Azure Multi-Factor Authentication Server 的可執行檔後，必須將它安裝在同盟伺服器上。

##### <a name="install-the-windows-azure-multi-factor-authentication-server-on-your-federation-server"></a><a name="BKMK_c"></a>在同盟伺服器上安裝 Windows Azure Multi-Factor Authentication Server

1.  下載並按兩下 Windows Azure Multi-Factor Authentication Server 的可執行檔。  安裝作業隨即會開始。

2.  在 [授權合約] 畫面上，閱讀合約，選取 [我同意]，然後按 [下一步]。

3.  確定目的地資料夾正確無誤，再按 [下一步]。

4.  安裝完成後，按一下 [完成]。

您現在可以啟動在同盟伺服器上安裝的 Windows Azure Multi-Factor Authentication Server，並將它設為一種其他驗證方法。

##### <a name="configure-windows-azure-multi-factor-authentication-as-an-additional-authentication-method"></a><a name="BKMK_d"></a>設定 Windows Azure Multi-Factor Authentication 作為其他驗證方法

1.  在同盟伺服器上從您安裝 [Windows Azure Multi-Factor Authentication]  的位置啟動它，接著在歡迎使用頁面選取 [略過使用驗證設定精靈]  核取方塊，然後按 [下一步] 。

2.  若要啟用 Multi-Factor Authentication Server，返回您在多因素驗證管理入口網站下載 Multi-Factor Authentication Server 的頁面，按一下 [產生啟用認證] 按鈕。 在 Multi-Factor Authentication Server 使用者介面，輸入產生的憑證，再按一下 [啟用]。

3.  接著，[Multi-Factor Authentication Server] 使用者介面會提示您執行 [多伺服器設定精靈]。  請選取 [否] 。

    > [!IMPORTANT]
    > 您可以略過完成 [多伺服器設定精靈]，因為用來完成此逐步解說的實驗室環境只有一部同盟伺服器。 不過，如果您的環境含有多個同盟伺服器，就必須安裝 Multi-Factor Authentication Server，並在每部同盟伺服器上完成 [多伺服器設定精靈]，才能在同盟伺服器上執行的多因素伺服器間啟用複寫。

4.  在 [Multi-Factor Authentication Server] 使用者介面，選取 [使用者] 圖示，按一下 [從 Active Directory 匯入]，選取 [Robert Hatley] 帳戶以便在 Windows Azure Multi-Factor Authentication 進行佈建，然後按一下 [匯入]。

5.  在 [使用者] 清單，選取 [Robert Hatley] 帳戶，按一下 [編輯]，然後在 [編輯使用者] 視窗提供此帳戶的手機號碼，確定已選取 [啟用] 核取方塊，接著按一下 [套用]。

6.  在 [使用者] 清單，選取 [Robert Hatley] 帳戶，再按一下 [測試]。 在 [測試使用者] 視窗，提供 [Robert Hatley] 帳戶的認證。 當行動電話環形時，請按 ' # ' 完成帳戶驗證。

7.  在 [Multi-Factor Authentication Server] 使用者介面，選取 [AD FS] 圖示，確定已選取 [允許使用者註冊]、[允許使用者選取方法] (包含 [電話通知] 和 [簡訊])、[使用遞補用的安全性問題] 及 [啟用記錄] 核取方塊，按一下 [安裝 AD FS 配接器]，然後完成 [多因素驗證 AD FS 配接器] 安裝精靈。

    > [!NOTE]
    > [多因素驗證 AD FS 配接器] 安裝精靈會在您的 Active Directory 建立一個名為 **PhoneFactor Admins** 的安全性群組，然後將您的 Federation Service AD FS 服務帳戶加入這個群組。
    >
    > 建議您在網域控制站確認 **PhoneFactor Admins** 群組確實已經建立，而且 AD FS 服務帳戶是此群組的成員。
    >
    > 如有需要，請在網域控制站上將 AD FS 服務帳戶手動新增至 **PhoneFactor Admins** 群組。

    如需安裝 AD FS 配接器的詳細資訊，請按一下 Multi-Factor Authentication Server 右上角的 [說明] 連結。

8.  若要在 Federation Service 註冊介面卡，請於同盟伺服器上啟動 Windows PowerShell 命令視窗，並執行下列命令： `\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`。 現在，介面卡已註冊為 **WindowsAzureMultiFactorAuthentication**。  您必須重新啟動 AD FS 服務，才能夠讓註冊生效。

9. 若要將 Windows Azure Multi-Factor Authentication 設為其他驗證方法，在 AD FS 管理主控台中，瀏覽到 [驗證原則] 節點，然後在 [Multi-factor Authentication] 區段按一下 [通用設定] 子區段旁的 [編輯] 連結。 在 [編輯通用驗證原則] 視窗中，選取 [Multi-Factor Authentication] 作為其他驗證方法，然後按一下 [確定]。

    > [!NOTE]
    > 您可以執行 **Set-AdfsAuthenticationProviderWebContent** Cmdlet，將 Windows Azure Multi-Factor Authentication 方法及任何已設定的協力廠商驗證方法的名稱和描述，自訂為與 AD FS UI 中顯示的一樣。 如需詳細資訊，請參閱 [https://technet.microsoft.com/library/dn479401.aspx](/powershell/module/adfs/set-adfsauthenticationproviderwebcontent)

### <a name="set-up-mfa-policy"></a><a name="BKMK_6"></a>設定 MFA 原則
若要啟用 MFA，您必須在同盟伺服器設定 MFA 原則。 在此逐步解說中，根據我們的 MFA 原則，需要 **Robert Hatley** 帳戶才能進行 mfa，因為他屬於您在 [設定實驗室環境中為 Windows Server 2012 R2 中的 AD FS](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)設定的 **財務** 群組。

您可以使用 AD FS 管理主控台或 Windows PowerShell 來設定 MFA 原則。

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-the-ad-fs-management-console"></a>透過 AD FS 管理主控台，根據使用者的群組成員資格資料設定 MFA 原則

1.  在您的同盟伺服器上，于 AD FS 管理主控台中， **Authentication Policies** 流覽至 [ \\ **每個信賴** 憑證者信任節點的驗證原則]，然後選取代表您範例應用程式的信賴憑證者信任 (**claimapp**) 。

2.  在 [動作] 頁面，或在 [claimapp] 按一下滑鼠右鍵，選取 [編輯自訂多因素驗證]。

3.  在 [編輯 claimapp 信賴憑證者信任] 視窗，按一下 [使用者/群組] 清單旁的 [新增] 按鈕。 針對您在 [Windows Server 2012 R2 中設定 AD FS 的實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)中所建立的 AD 組名輸入 **財務**，然後按一下 [**檢查名稱**]，當名稱解析完成後，按一下 **[確定]**。

4.  按一下 [編輯 claimapp 信賴憑證者信任] 視窗的 [確定]。

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-windows-powershell"></a>若要根據使用者的 ' claimapp ' 群組成員資格資料，透過 Windows PowerShell 設定 MFA 原則

1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗，並執行下列命令：

    ```
    $rp = Get-AdfsRelyingPartyTrust -Name claimapp
    ```

2.  在相同的 Windows PowerShell 命令視窗中，執行下列命令：

    ```
    $GroupMfaClaimTriggerRule = 'c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i) <group_SID>$"] => issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -AdditionalAuthenticationRules $GroupMfaClaimTriggerRule

    ```

    > [!NOTE]
    > 請記得將 <group_SID> 取代為 AD 群組 **Finance** 的 SID 值。

## <a name="step-4-verify-mfa-mechanism"></a><a name="BKMK_4"></a>步驟 4：驗證 MFA 機制
在這個步驟中，您將驗證上個步驟中設定的 MFA 功能。 您可以使用下列程序，確認 **Robert Hatley** AD 使用者可以存取您的範例應用程式，這次需要進行 MFA，因為他屬於 **Finance** 群組。

1.  在您的用戶端電腦上，開啟瀏覽器視窗，然後流覽至您的範例應用程式： **https://webserv1.contoso.com/claimapp** 。

    這個動作會將要求自動重新導向到同盟伺服器，且會提示您以使用者名稱和密碼登入。

2.  輸入 **Robert Hatley** AD 帳戶的認證。

    此時根據您已設定的 MFA 原則，系統會提示使用者進行其他驗證。 預設的文字訊息為 [基於安全性理由，我們需要其他資訊以驗證您的帳戶]。 。不過，您可以完全自訂這段文字內容。 如需如何自訂登入體驗的詳細資訊，請參閱＜ [Customizing the AD FS Sign-in Pages](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280950(v=ws.11))＞。

    如果您將憑證驗證設為其他驗證方法，預設文字訊息為 [選取驗證所要使用的憑證。如果您取消此作業，請關閉瀏覽器再重試一次]。

    如果您設定 Windows Azure Multi-factor Authentication 做為其他驗證方法，預設訊息文字是 [系統將會撥打電話以完成驗證]。 如需有關使用 Windows Azure 登入 Multi-Factor Authentication 並使用各種選項來進行慣用驗證方法的詳細資訊，請參閱 [Windows azure Multi-Factor Authentication 總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280950(v=ws.11))。

## <a name="see-also"></a>另請參閱
[使用機密應用程式](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) 
 的額外 Multi-Factor Authentication 管理風險[設定 Windows Server 2012 R2 中 AD FS 的實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
