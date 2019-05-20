---
ms.assetid: 5fd4063d-34dc-4b15-9a88-cc6c1fff455a
title: 逐步解說指南-管理機密應用程式的其他多因素驗證的風險
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 414f37e86f0072863e5fa2f107c39e5518e560ec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860859"
---
# <a name="walkthrough-guide-manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>逐步解說指南：透過其他多因素驗證管理機密應用程式的風險

>適用於：Windows Server 2012 R2


## <a name="about-this-guide"></a>關於本指南
本逐步解說會提供使用者的群組成員資格資料為基礎的 Windows Server 2012 R2 中的 Active Directory Federation Services (AD FS) 中設定多重要素驗證 (MFA) 的指示。

如需有關 AD FS 的 MFA 和驗證機制的詳細資訊，請參閱[Manage Risk with Additional Multi-factor Authentication 為機密的應用程式](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。

本逐步解說包含下列各節：

-   [步驟 1：設定實驗室環境](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [步驟 2：驗證預設的 AD FS 驗證機制](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)

-   [步驟 3：在您的同盟伺服器上設定 MFA](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_3)

-   [步驟 4：驗證 MFA 機制](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_4)

## <a name="BKMK_1"></a>步驟 1:設定實驗室環境
若要完成本逐步解說，您的環境必須包含下列元件：

-   含有測試使用者和群組帳戶，在 Windows Server 2012 R2 或 Windows Server 2008、 Windows Server 2008 R2 或 Windows Server 2012 上執行結構描述要升級到 Windows Server 2012 R2 的 Active Directory 網域上執行 Active Directory 網域

-   Windows Server 2012 R2 上執行的同盟伺服器

-   裝載範例應用程式的網頁伺服器

-   用來存取範例應用程式的用戶端電腦

> [!WARNING]
> 強烈建議 (在生產和測試環境中) 您不要使用相同的電腦作為同盟伺服器和網頁伺服器。

在這個環境中，同盟伺服器會發行所需的宣告，讓使用者能夠存取範例應用程式。 裝載範例應用程式的網頁伺服器會信任出示同盟伺服器發行之宣告的使用者。

如需有關如何設定此環境的指示，請參閱 <<c0> [ 適用於 Windows Server 2012 R2 中的 AD FS 設定實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

## <a name="BKMK_2"></a>步驟 2:驗證預設的 AD FS 驗證機制
在這個步驟您要驗證預設的 AD FS 存取控制機制 (外部網路為 [表單驗證] ，內部網路為 [Windows 驗證]  )，使用者會被重新導向到 AD FS 登入頁面、提供有效的認證，然後授與應用程式的存取權。 您可以使用**Robert Hatley** AD 帳戶而**claimapp**範例應用程式中設定[適用於 Windows Server 2012 R2 中的 AD FS 設定實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

1.  在用戶端電腦，開啟瀏覽器視窗，並巡覽至範例應用程式： **https://webserv1.contoso.com/claimapp**。

    這個動作會將要求自動重新導向到同盟伺服器，且會提示您以使用者名稱和密碼登入。

2.  輸入的認證**Robert Hatley** AD 帳戶中建立[適用於 Windows Server 2012 R2 中的 AD FS 設定實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

    您將會獲得應用程式的存取權。

## <a name="BKMK_3"></a>步驟 3:在同盟伺服器上設定 MFA
有要在 Windows Server 2012 R2 中的 AD FS 中設定 MFA 的兩個部分：

-   [選取其他驗證方法](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_5)

-   [設定 MFA 原則](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_6)

### <a name="BKMK_5"></a>選取其他驗證方法
若要設定 MFA，您必須選取其他驗證方法。 在這個逐步解說中，若要設定其他驗證方法，您可以選擇下列選項：

-   選取 [憑證驗證](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_7)依預設使用 Windows Server 2012 R2 中的 AD FS 中的方法

-   設定並選取 [Windows Azure Multi-Factor Authentication](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_8)

#### <a name="BKMK_7"></a>憑證驗證
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

#### <a name="BKMK_8"></a>Windows Azure Multi-factor Authentication
完成下列其中一個程序，以下載並設定和選取 [Windows Azure Multi-Factor Authentication]  作為同盟伺服器上的其他驗證：

1.  [建立多因素驗證提供者透過 Windows Azure 入口網站](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_a)

2.  [下載 Windows Azure Multi-factor Authentication Server](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_b)

3.  [您的同盟伺服器上安裝 Windows Azure Multi-factor Authentication Server](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_c)

4.  [設定 Windows Azure Multi-factor Authentication 作為其他驗證方法](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_d)

##### <a name="BKMK_a"></a>建立多因素驗證提供者透過 Windows Azure 入口網站

1.  以系統管理員身分登入 Windows Azure 入口網站。

2.  在左側選取 [Active Directory]。

3.  在 [Active Directory] 頁面的上方選取 [多因素驗證提供者]。  然後在下方按一下 [新增]。

4.  在 [應用程式服務->Active Directory] 下，選取 [多因素驗證提供者]，再選取 [快速建立]。

5.  在 [應用程式服務] 下，選取 [Active 驗證提供者]，再選取 [快速建立]。

6.  填入下列欄位，然後選取 [建立]。

    1.  **名稱**-Multi-factor Auth 提供者的名稱。

    2.  **使用量模型**-Multi-factor Authentication 提供者的使用模式。

        -   **每次驗證**-按驗證計費的購買模型。 通常用於消費者導向應用程式中使用 Windows Azure Multi-Factor Authentication 的案例。

        -   **每個啟用的使用者**-按啟用的使用者計費的購買模型。  通常用於員工導向案例，例如 Office 365。

        如需使用模式的詳細資訊，請參閱 [Windows Azure 定價詳細資料](http://www.windowsazure.com/pricing/details/active-directory/)。

    3.  **目錄**-Multi-factor Authentication 提供者相關聯的 Windows Azure Active Directory 租用戶。 這是選擇性的欄位，因為在保護內部部署應用程式安全性時，提供者不一定要與 Windows Azure Active Directory 連結。

7.  按下建立之後，就會建立多因素驗證提供者，且應該會看到下列訊息：已成功建立多因素驗證提供者。  按一下 [確定]。

接著，您必須下載 Windows Azure Multi-Factor Authentication Server。 您可以從 Windows Azure 入口網站啟動 Windows Azure Multi-Factor Authentication 入口網站來完成此動作。

##### <a name="BKMK_b"></a>下載 Windows Azure Multi-factor Authentication Server

1.  以系統管理員身分登入 Windows Azure 入口網站，按一下您在上述程序建立的多因素驗證提供者。 接著，按一下 [管理]  按鈕。

    這會啟動 [Windows Azure Multi-Factor Authentication]  入口網站。

2.  在 [Windows Azure Multi-Factor Authentication]  入口網站，按一下 [下載] ，然後按一下 [下載]  下載 Windows Azure Multi-Factor Authentication Server 複本。

下載 Windows Azure Multi-Factor Authentication Server 的可執行檔後，必須將它安裝在同盟伺服器上。

##### <a name="BKMK_c"></a>您的同盟伺服器上安裝 Windows Azure Multi-factor Authentication Server

1.  下載並按兩下 Windows Azure Multi-Factor Authentication Server 的可執行檔。  隨即開始進行安裝。

2.  在 [授權合約] 畫面上，閱讀合約，選取 [我同意]  ，然後按 [下一步] 。

3.  確定目的地資料夾正確無誤，再按 [下一步] 。

4.  安裝完成後，按一下 [完成]。

您現在可以啟動在同盟伺服器上安裝的 Windows Azure Multi-Factor Authentication Server，並將它設為一種其他驗證方法。

##### <a name="BKMK_d"></a>設定 Windows Azure Multi-factor Authentication 作為其他驗證方法

1.  在同盟伺服器上從您安裝 [Windows Azure Multi-Factor Authentication]  的位置啟動它，接著在歡迎使用頁面選取 [略過使用驗證設定精靈]  核取方塊，然後按 [下一步] 。

2.  若要啟用 Multi-Factor Authentication Server，返回您在多因素驗證管理入口網站下載 Multi-Factor Authentication Server 的頁面，按一下 [產生啟用認證]  按鈕。 在 Multi-Factor Authentication Server 使用者介面，輸入產生的憑證，再按一下 [啟用] 。

3.  接著，[Multi-Factor Authentication Server]  使用者介面會提示您執行 [多伺服器設定精靈] 。  選取 [否]。

    > [!IMPORTANT]
    > 您可以略過完成 [多伺服器設定精靈]，因為用來完成此逐步解說的實驗室環境只有一部同盟伺服器。 不過，如果您的環境含有多個同盟伺服器，就必須安裝 Multi-Factor Authentication Server，並在每部同盟伺服器上完成 [多伺服器設定精靈]  ，才能在同盟伺服器上執行的多因素伺服器間啟用複寫。

4.  在 [Multi-Factor Authentication Server]  使用者介面，選取 [使用者]  圖示，按一下 [從 Active Directory 匯入] ，選取 [Robert Hatley]  帳戶以便在 Windows Azure Multi-Factor Authentication 進行佈建，然後按一下 [匯入] 。

5.  在 [使用者]  清單，選取 [Robert Hatley]  帳戶，按一下 [編輯] ，然後在 [編輯使用者]  視窗提供此帳戶的手機號碼，確定已選取 [啟用]  核取方塊，接著按一下 [套用] 。

6.  在 [使用者] 清單，選取 [Robert Hatley] 帳戶，再按一下 [測試]。 在 [測試使用者]  視窗，提供 [Robert Hatley]  帳戶的認證。 當手機響起時，請按 「 # 」 完成帳戶驗證。

7.  在 [Multi-Factor Authentication Server]  使用者介面，選取 [AD FS]  圖示，確定已選取 [允許使用者註冊] 、[允許使用者選取方法]  (包含 [電話通知]  和 [簡訊] )、[使用遞補用的安全性問題]  及 [啟用記錄]  核取方塊，按一下 [安裝 AD FS 配接器] ，然後完成 [多因素驗證 AD FS 配接器]  安裝精靈。

    > [!NOTE]
    > [多因素驗證 AD FS 配接器] 安裝精靈會在您的 Active Directory 建立一個名為 **PhoneFactor Admins** 的安全性群組，然後將您的 Federation Service AD FS 服務帳戶加入這個群組。
    > 
    > 建議您在網域控制站確認 **PhoneFactor Admins** 群組確實已經建立，而且 AD FS 服務帳戶是此群組的成員。
    > 
    > 如有需要，手動將 AD FS 服務帳戶新增到網域控制站上的 **PhoneFactor Admins** 群組。

    如需安裝 AD FS 配接器的詳細資訊，請按一下 Multi-Factor Authentication Server 右上角的 [說明] 連結。

8.  若要在 Federation Service 註冊介面卡，請於同盟伺服器上啟動 Windows PowerShell 命令視窗，並執行下列命令： `\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`。 現在，介面卡已註冊為 **WindowsAzureMultiFactorAuthentication**。  您必須重新啟動 AD FS 服務，才能夠讓註冊生效。

9. 若要將 Windows Azure Multi-Factor Authentication 設為其他驗證方法，在 AD FS 管理主控台中，瀏覽到 [驗證原則]  節點，然後在 [Multi-factor Authentication]  區段按一下 [通用設定]  子區段旁的 [編輯]  連結。 在 [編輯通用驗證原則]  視窗中，選取 [Multi-Factor Authentication]  作為其他驗證方法，然後按一下 [確定] 。

    > [!NOTE]
    > 您可以執行 **Set-AdfsAuthenticationProviderWebContent** Cmdlet，將 Windows Azure Multi-Factor Authentication 方法及任何已設定的協力廠商驗證方法的名稱和描述自訂為與 AD FS UI 中顯示的一樣。 如需詳細資訊，請參閱 [https://technet.microsoft.com/library/dn479401.aspx](https://technet.microsoft.com/library/dn479401.aspx)

### <a name="BKMK_6"></a>設定 MFA 原則
若要啟用 MFA，您必須在同盟伺服器設定 MFA 原則。 此逐步解說中，我們的 MFA 原則，每**Robert Hatley**帳戶，才可進行 MFA，因為他屬於**財務**中設定的群組[設定實驗室環境中的 AD fsWindows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

您可以使用 AD FS 管理主控台或 Windows PowerShell 來設定 MFA 原則。

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-the-ad-fs-management-console"></a>若要設定 MFA 原則，根據 'claimapp' 透過 AD FS 管理主控台的使用者群組成員資格資料

1.  在您的同盟伺服器，在 AD FS 管理主控台中，瀏覽至**驗證原則**\\**每個信賴憑證者信任**節點，然後選取信賴憑證者信任，表示您範例應用程式 (**claimapp**)。

2.  在 [動作]  頁面，或在 [claimapp] 按一下滑鼠右鍵，選取 [編輯自訂多因素驗證] 。

3.  在 [編輯 claimapp 信賴憑證者信任]  視窗，按一下 [使用者/群組]  清單旁的 [新增]  按鈕。 在中輸入**財務**您在中所建立的 AD 群組的名稱[適用於 Windows Server 2012 R2 中的 AD FS 設定實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)，，按一下 **檢查名稱**，以及名稱時解決，請按一下**確定**。

4.  按一下 [編輯 claimapp 信賴憑證者信任] 視窗的 [確定]。

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-windows-powershell"></a>若要設定 MFA 原則，根據 'claimapp'，透過 Windows PowerShell 的使用者群組成員資格資料

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

## <a name="BKMK_4"></a>步驟 4:驗證 MFA 機制
在這個步驟中，您將驗證上個步驟中設定的 MFA 功能。 您可以使用下列程序，確認 **Robert Hatley** AD 使用者可以存取您的範例應用程式，這次需要進行 MFA，因為他屬於 **Finance** 群組。

1.  在用戶端電腦，開啟瀏覽器視窗，並巡覽至範例應用程式： **https://webserv1.contoso.com/claimapp**。

    這個動作會將要求自動重新導向到同盟伺服器，且會提示您以使用者名稱和密碼登入。

2.  輸入 **Robert Hatley** AD 帳戶的認證。

    此時根據您已設定的 MFA 原則，系統會提示使用者進行其他驗證。 預設訊息文字是「基於安全性考量，我們需要額外的資訊，以驗證您的帳戶。」 。不過，您可以完全自訂這段文字內容。 如需如何自訂登入體驗的詳細資訊，請參閱＜ [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx)＞。

    如果您設定憑證驗證作為其他驗證方法時，預設訊息文字是**選取您想要用於驗證的憑證。如果您取消此作業，請關閉瀏覽器，然後再試一次。**

    如果您設定 Windows Azure Multi-factor Authentication 做為其他驗證方法，預設訊息文字是 [系統將會撥打電話以完成驗證]。 。如需使用 Windows Azure Multi-Factor Authentication 登入並使用偏好驗證方法各種選項的詳細資訊，請參閱 [Windows Azure Multi-Factor Authentication 概觀](https://technet.microsoft.com/library/dn249479.aspx)。

## <a name="see-also"></a>另請參閱
[管理機密應用程式透過其他多因素驗證的風險](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
[設定適用於 Windows Server 2012 R2 中的 AD FS 實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



