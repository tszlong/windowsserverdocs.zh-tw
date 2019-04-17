---
ms.assetid: 5fd4063d-34dc-4b15-9a88-cc6c1fff455a
title: "逐步解說指南-管理其他多因素驗證敏感的應用程式的風險"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 414f37e86f0072863e5fa2f107c39e5518e560ec
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-guide-manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>逐步解說指南： 管理其他多因素驗證敏感的應用程式的風險

>適用於： Windows Server 2012 R2


## <a name="about-this-guide"></a>有關本指南
本節提供要素 (MFA) 設定在 Windows Server 2012 R2 的 Active Directory 同盟 Services (AD FS) 中的指示根據使用者群組成員資格資料。

如需 AD FS 機制 MFA 與驗證的詳細資訊，請查看[管理其他多因素驗證敏感的應用程式的風險](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。

本節下列各節所組成：

-   [步驟 1： 實驗室設定](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [步驟 2： 驗證預設機制驗證，AD FS](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)

-   [步驟 3： 聯盟伺服器上設定 MFA](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_3)

-   [步驟 4： 確認 MFA 機制](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_4)

## <a name="BKMK_1"></a>步驟 1： 實驗室設定
完成本節，您必須環境，包含下列元件：

-   測試使用者和群組帳號，並執行 Windows Server 2012 R2 或 Windows Server 2008、 Windows Server 2008 R2 或 Windows Server 2012 上執行升級到 Windows Server 2012 R2 其架構 Active Directory domain Active Directory domain

-   Windows Server 2012 R2 上執行的聯盟伺服器

-   網頁伺服器裝載範例應用程式

-   Client 電腦，您可以存取的範例應用程式

> [!WARNING]
> 我們建議 （兩者皆 production 和測試環境），不做聯盟伺服器和您的網頁伺服器使用相同的電腦。

在此環境中聯盟伺服器問題，讓使用者可以存取的範例應用程式所需的宣告。 Web 伺服器裝載的範例應用程式，將信任的使用者提供宣告聯盟伺服器的問題。

如何設定此環境中的指示，請查看[在 Windows Server 2012 R2 AD FS 設定實驗室](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

## <a name="BKMK_2"></a>步驟 2： 驗證預設機制驗證，AD FS
您將會在此步驟驗證預設 AD FS 存取控制機制 (**表單驗證**的外部和**Windows 驗證**的內部)、 位置使用者會重新導向至 AD FS 登入頁面、 提供有效的認證，並會授與應用程式存取。 您可以使用**劉小龍 Hatley** AD account 和**claimapp**範例應用程式中設定[設定在 Windows Server 2012 R2 AD FS 實驗室](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

1.  您 client 在電腦上，開放瀏覽器視窗，並瀏覽到您的範例應用程式： **https://webserv1.contoso.com/claimapp**。

    這個動作會自動重新導向至 amc 要求聯盟伺服器，並提示您使用的使用者名稱和密碼登入。

2.  輸入認證**劉小龍 Hatley**您在建立廣告 account[設定實驗室 AD FS 在 Windows Server 2012 R2 的](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

    您將會授與應用程式的存取。

## <a name="BKMK_3"></a>步驟 3： 聯盟伺服器上設定 MFA
有兩個部分設定 MFA AD FS 在 Windows Server 2012 R2 中：

-   [選取 [額外的驗證方法](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_5)

-   [設定 MFA 原則](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_6)

### <a name="BKMK_5"></a>選取 [額外的驗證方法
為了 MFA 設定，您必須選取額外的驗證方法。 在本節額外的驗證方法，您可以選擇下列選項：

-   選取 [[憑證驗證](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_7)在 Windows Server 2012 R2 AD FS 使用預設的方法

-   設定並選取 [ [Windows Azure 多因素驗證](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_8)

#### <a name="BKMK_7"></a>憑證驗證
完成其中一項下列程序，選取憑證驗證做為額外的驗證方法：

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-the-ad-fs-management-console"></a>若要設定憑證驗證做為額外的驗證方法，AD FS 管理主控台透過

1.  聯盟伺服器，在 AD FS 管理主控台中，瀏覽至**驗證原則**節點，然後在**多因素驗證**區段中，按一下 [**編輯**旁邊的連結**通用設定**子區段。

2.  在**編輯全球驗證原則**視窗中，選取**憑證驗證**做為額外的驗證方法、，然後按一下 [ **[確定]**。

###### <a name="to-configure-certificate-authentication-as-an-additional-authentication-method-via-windows-powershell"></a>若要設定憑證驗證做為額外的驗證方法，透過 Windows PowerShell

1.  在您聯盟的伺服器，開放 Windows PowerShell 命令視窗中，執行下列命令：

    ```
    Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider CertificateAuthentication

    ```

    > [!WARNING]
    > 若要確認已順利執行這個命令時，您可以執行`Get-AdfsGlobalAuthenticationPolicy`命令。

#### <a name="BKMK_8"></a>Windows Azure 多因素驗證
完成下列程序，才能下載並設定，然後選取 [ **Windows Azure 多因素驗證**額外的驗證聯盟伺服器上為：

1.  [建立 Windows 透過多因素驗證提供者 Azure 入口網站](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_a)

2.  [下載 Windows Azure 多因素驗證 Server](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_b)

3.  [聯盟伺服器上安裝 Windows Azure 多因素驗證 Server](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_c)

4.  [設定 Windows Azure 多因素驗證做為額外的驗證方法](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_d)

##### <a name="BKMK_a"></a>建立 Windows 透過多因素驗證提供者 Azure 入口網站

1.  登入 Windows Azure 入口網站，以系統管理員。

2.  在左邊，選取 [Active Directory。

3.  在 Active Directory 頁面上，於最上方，選取 [**多因素驗證提供者**。  然後在下方，按一下**新增]**。

4.  在**App 服務 Active Directory]-> [**，請選取**多因素驗證提供者**，然後選取 [**快速建立**。

5.  在**應用程式服務**，請選取**使用驗證提供者**，然後選取 [**快速建立**。

6.  填入下列欄位，然後選取**建立**。

    1.  **名稱**-多因素驗證提供者的名稱。

    2.  **使用的機型**-使用模式的多因素驗證提供者。

        -   **每個驗證**-購買型號的每個驗證充電。 通常會用來在消費者攝影機的應用程式使用 Windows Azure 多因素驗證案例。

        -   **每個讓使用者**-購買型號的每個讓使用者充電。  常用員工面向案例，例如 Office 365。

        使用型號上的其他資訊，請查看[Windows Azure 計價的詳細資料](http://www.windowsazure.com/pricing/details/active-directory/)。

    3.  **Directory** -多因素驗證提供者相關聯的 Windows Azure Active Directory 承租人。 這是選擇性不必連結到 Windows Azure Active Directory 時保護先應用程式提供者。

7.  只要按建立多因素驗證提供者將會建立，您應該會看到的訊息，指出： 成功地建立多因素驗證提供者。  按一下**[確定]**。

接下來，您必須在下載 Windows Azure 多因素驗證伺服器。 您可以透過 Windows Azure 入口網站 Windows Azure 多因素驗證 Portal 這是第一個。

##### <a name="BKMK_b"></a>下載 Windows Azure 多因素驗證 Server

1.  登入 Windows Azure 入口網站，以系統管理員的身分，並按一下您在上面的程序中建立多因素驗證提供者。 然後按一下**管理**按鈕。

    這時限**Windows Azure 多因素驗證**入口網站。

2.  在**Windows Azure 多因素驗證**入口網站，按一下 [**下載**，然後按一下 [**下載**下載一份 Windows Azure 多因素驗證伺服器。

在您的 Windows Azure 多因素驗證伺服器下載可執行檔，您必須聯盟伺服器上安裝它。

##### <a name="BKMK_c"></a>聯盟伺服器上安裝 Windows Azure 多因素驗證 Server

1.  下載，並可執行檔上按兩下 [Windows Azure 多因素驗證伺服器。  這將會開始安裝。

2.  在 [授權合約畫面中，朗讀 「 合約 」，選取**我同意**，按一下 [**下**。

3.  確定是正確的目的地資料夾，然後按一下**下一步**。

4.  在安裝完成時，請按一下**完成**。

您現在已經上市聯盟伺服器上安裝 Windows Azure 多因素驗證伺服器，並將其設定為額外的驗證方法。

##### <a name="BKMK_d"></a>設定 Windows Azure 多因素驗證做為額外的驗證方法

1.  上市**Windows Azure 多因素驗證**在您安裝該聯盟伺服器，並在 [歡迎使用] 頁面上，請檢查**使用驗證組態精靈略過**核取方塊，按一下 [**下一步**。

2.  若要啟動多因素驗證伺服器，請返回頁面中的多因素驗證管理入口網站下載多因素驗證伺服器，然後按一下**產生啟動認證**按鈕。 在多因素驗證伺服器使用者介面中，輸入認證的程式，然後按一下**Activate**。

3.  下一步]**多因素驗證伺服器**使用者介面會提示您先執行**多伺服器設定精靈]**。  選取 [**否]**。

    > [!IMPORTANT]
    > 您可以略過完成**多伺服器設定精靈**提供與用來完成本節只有一個聯盟伺服器實驗室。 不過，如果您的環境中包含數個聯盟伺服器，您必須安裝多因素驗證伺服器，並完成**多伺服器設定精靈]**為了讓複寫之間聯盟伺服器上執行之多因素伺服器每個聯盟伺服器上。

4.  在**多因素驗證伺服器**使用者介面中，選取**使用者**圖示，按**匯入的 Active Directory**，請選取**劉小龍 Hatley**帳號提供 Windows Azure 多因素驗證中的它，然後按一下 [**匯入**。

5.  在**使用者**清單中，選取**劉小龍 Hatley**帳號，請按一下 [**編輯**，並在**編輯使用者**] 視窗中，提供行動電話號碼此帳號，請確定**啟用**核取方塊已選取，然後按一下 [**套用**。

6.  在**使用者**清單中，選取**劉小龍 Hatley**帳號，並按**測試**。 在**測試使用者**視窗中，提供的認證**劉小龍 Hatley** account。 手機鈴響中，按下時 '#'，才能完成 account 驗證。

7.  在**多因素驗證伺服器**使用者介面中，選取**AD FS**圖示，確定**允許使用者註冊**，**讓使用者選取方法**(包括**電話**和**簡訊**)，**使用後援安全性問題**和**可以登入**簽核取方塊、 按一下 [**安裝 AD FS 介面卡**，並完成**多因素驗證，AD FS 介面卡**安裝精靈中。

    > [!NOTE]
    > **多因素驗證，AD FS 介面卡**安裝精靈中建立安全性群組名為**PhoneFactor 系統管理員**您 Active Directory 中，然後新增此群組 AD FS 服務帳號，您的同盟服務。
    > 
    > 我們建議您確認您的網域控制站上的**PhoneFactor 管理員**確實建立群組，而且 AD FS 服務 account 此群組成員。
    > 
    > 視需要新增 AD FS 服務帳號**PhoneFactor 管理員**在您的網域控制站手動群組。

    安裝 AD FS 介面卡上的其他詳細資料，按一下右上角的多因素驗證伺服器中協助連結。

8.  為隊伍登記中同盟服務，在聯盟伺服器上的顯示卡上市 Windows PowerShell 命令視窗中，並執行下列命令： `\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`。 介面卡現在係為**WindowsAzureMultiFactorAuthentication**。  您必須重新才會生效登記您 AD FS 服務。

9. 若要設定 Windows Azure 多因素驗證做為額外的驗證方法、 在 AD FS 管理主控台中，瀏覽至**驗證原則**節點，然後在**多因素驗證**區段中，按一下 [**編輯**旁邊的連結**通用設定**子區段。 在**編輯全球驗證原則**視窗中，選取**多因素驗證**做為額外的驗證方法、，然後按一下 [ **[確定]**。

    > [!NOTE]
    > 您可以自訂名稱，並描述 Windows Azure 多因素驗證方法，以及任何設定第三方驗證方法、，AD FS UI 中顯示執行**設定為 AdfsAuthenticationProviderWebContent** cmdlet。 如需詳細資訊，請查看[https://technet.microsoft.com/library/dn479401.aspx](https://technet.microsoft.com/library/dn479401.aspx)

### <a name="BKMK_6"></a>設定 MFA 原則
為了讓 MFA，您必須設定 MFA 原則聯盟伺服器上。 本節每次我們 MFA 原則，針對**劉小龍 Hatley** account 所需經歷 MFA 因為他屬於**財經**群組中設定[設定在 Windows Server 2012 R2 AD FS 實驗室](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

您可以設定 「 透過 AD FS 管理主控台中，或使用 Windows PowerShell MFA 原則。

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-the-ad-fs-management-console"></a>若要設定根據透過 AD FS 管理主控台 'claimapp 使用者的群組成員資格資料 MFA 原則

1.  聯盟伺服器，在 AD FS 管理主控台中，瀏覽至**驗證原則**\\**每可以廠商信任**] 節點，然後選取，表示您的範例應用程式信賴廠商信任 (**claimapp**)。

2.  在**動作**頁面上，或以滑鼠右鍵按一下**claimapp**、 選取**編輯自訂多因素驗證**。

3.  在**編輯可以方信任 claimapp 的**視窗中，按一下 [**新增**按鈕旁**使用者或群組**清單。 輸入**財經**您在建立廣告群組的名稱[設定實驗室 AD FS 在 Windows Server 2012 R2 的](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)，並按一下 [**檢查名稱**的名稱解析時，按一下 [ **[確定]**。

4.  按一下**[確定]**在**編輯可以方信任 claimapp 的**] 視窗。

##### <a name="to-configure-the-mfa-policy-based-on-users-group-membership-data-for-claimapp--via-windows-powershell"></a>若要設定 MFA 原則的 Windows PowerShell 透過 'claimapp 使用者的群組成員資格資料

1.  在您聯盟的伺服器，開放 Windows PowerShell 命令視窗中，執行下列命令：

    ```
    $rp = Get-AdfsRelyingPartyTrust -Name claimapp
    ```

2.  在同一個 Windows PowerShell 命令視窗中，執行下列命令：

    ```
    $GroupMfaClaimTriggerRule = 'c:[Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "^(?i) <group_SID>$"] => issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
    Set-AdfsRelyingPartyTrust -TargetRelyingParty $rp -AdditionalAuthenticationRules $GroupMfaClaimTriggerRule

    ```

    > [!NOTE]
    > 請確定您的廣告群組 SID 的值與取代 < group_SID >**財經**。

## <a name="BKMK_4"></a>步驟 4： 確認 MFA 機制
您將會在此步驟來驗證您設定一個步驟中 MFA 功能。 您可以使用下列程序，以確認**劉小龍 Hatley** AD 使用者都可以存取您的範例應用程式和所需經歷 MFA 他屬於因為這次**財經**群組。

1.  您 client 在電腦上，開放瀏覽器視窗，並瀏覽到您的範例應用程式： **https://webserv1.contoso.com/claimapp**。

    這個動作會自動重新導向至 amc 要求聯盟伺服器，並提示您使用的使用者名稱和密碼登入。

2.  憑證中的輸入**劉小龍 Hatley** AD account。

    此時，因為您所設定的 MFA 原則，將會提示使用者進行額外的驗證。 預設的訊息文字是**基於安全性考量，我們需要驗證您的其他資訊。** 不過，這文字可完全自訂。 如需了解如何自訂體驗登入資訊，請查看[[自訂頁面 AD FS 登入](https://technet.microsoft.com/library/dn280950.aspx)。

    如果您設定憑證驗證做為額外的驗證方法、 預設的訊息文字是**選擇您想要使用的驗證憑證。如果您取消操作，請關閉瀏覽器，然後再試一次。**

    如果您設定 Windows Azure 多因素驗證做為額外的驗證方法、 預設的訊息文字是**通話將會放到您的手機來完成您的驗證。** 如需關於 Windows Azure 多因數驗證以登入和驗證的建議方式使用各種不同的選項，請查看[Windows Azure 多因素驗證的概觀](https://technet.microsoft.com/library/dn249479.aspx)。

## <a name="see-also"></a>也了
[管理敏感的應用程式與其他多因素驗證風險](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
[設定實驗室 AD FS 在 Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



