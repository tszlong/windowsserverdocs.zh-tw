---
ms.assetid: 934ac796-e2ee-490d-8265-6a818be5ee79
title: 透過其他多因素驗證管理機密應用程式的風險
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: b9f764b64a50b0c69116cf19e253097da464cdbb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954294"
---
# <a name="manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>透過其他多因素驗證管理機密應用程式的風險




-   [在 Windows Server 2012 R2 設定 AD FS 實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)

-   [逐步解說指南：透過其他多因素驗證管理機密應用程式的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

-   [設定 AD FS 的其他驗證方法](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

## <a name="in-this-guide"></a>本指南內容
本指南提供下列資訊：

-   [AD FS 中的驗證機制](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_1)-Windows Server 2012 R2 中 Active Directory 同盟服務 (AD FS) 可用的驗證機制描述

-   [案例總覽](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)-一種案例的描述，您可以使用 Active Directory 同盟服務 (AD FS) ，根據使用者的群組成員資格來啟用多重要素驗證 (MFA) 。

    > [!NOTE]
    > 在 Windows Server 2012 R2 的 AD FS 中，您可以根據網路位置、裝置身分識別，以及使用者身分識別或群組成員資格來啟用 MFA。

    如需設定和驗證此案例的詳細逐步解說指示，請參閱逐步解說[指南：透過其他多因素驗證管理機密應用程式的風險](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。

## <a name="key-concepts---authentication-mechanisms-in-ad-fs"></a><a name="BKMK_1"></a>重要概念 - AD FS 中的驗證機制

### <a name="benefits-of-authentication-mechanisms-in-ad---fs"></a>AD FS 驗證機制的好處
Windows Server 2012 R2 中的 Active Directory 同盟服務 (AD FS) 為 IT 系統管理員提供更豐富、更有彈性的工具組，來驗證想要存取公司資源的使用者。 它可讓系統管理員有彈性地控制主要和其他驗證方法，提供豐富的管理體驗，讓您透過使用者介面和 Windows PowerShell) 設定驗證原則 (，並增強存取 AD FS 所保護之應用程式和服務的使用者體驗。 以下是在 Windows Server 2012 R2 中使用 AD FS 保護您的應用程式和服務的一些優點：

-   全域驗證原則-一種集中管理功能，IT 系統管理員可以根據其存取受保護資源的網路位置，選擇要用來驗證使用者的驗證方法。 這種做法可讓系統管理員執行下列工作：

    -   強制來自外部網路的存取要求使用更安全的驗證方法。

    -   針對無縫式次要因素驗證啟用裝置驗證。 這會將使用者的身分識別系結至用來存取資源的已註冊裝置，因此在存取受保護的資源之前，會提供更安全的複合身分識別驗證。

        > [!NOTE]
        > 如需裝置物件、裝置註冊服務、Workplace Join 和裝置的詳細資訊，做為無縫式的第二因素驗證和 SSO，請參閱[從任何裝置加入工作場所以進行 SSO，以及跨公司應用程式進行無縫第二因素驗證](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

    -   設定所有外部網路存取的 MFA 需求，或根據使用者的身分識別、網路位置或用來存取受保護資源的裝置，有條件地進行。

-   設定驗證原則有更大的彈性：您可以針對具有不同商業價值的 AD FS 保護資源，設定自訂驗證原則。 例如，您可以在對業務影響較大的應用測試要求使用 MFA。

-   容易使用：簡單且直覺的管理工具（例如 GUI 型 AD FS 管理 MMC 嵌入式管理單元和 Windows PowerShell Cmdlet）可讓 IT 系統管理員以相對輕鬆的方式設定驗證原則。 您可以使用 Windows PowerShell，編寫較大規模的解決方案，並自動化一般管理工作。

-   進一步控制公司資產：身為系統管理員，您可以使用 AD FS 來設定適用于特定資源的驗證原則，您可以更充分掌控公司資源的保護方式。 應用程式不可以覆寫 IT 系統管理員指定的驗證原則。 針對機密應用程式和服務，您可以啟用 MFA 需求、裝置驗證，以及在每次存取資源後選擇性的更新驗證。

-   支援自訂 MFA 提供者：針對利用協力廠商 MFA 方法的組織，AD FS 提供順暢地併入和使用這些驗證方法的能力。

### <a name="authentication-scope"></a>驗證範圍
在 Windows Server 2012 R2 的 AD FS 中，您可以指定全域範圍的驗證原則，而該領域適用于 AD FS 保護的所有應用程式和服務。  您也可以設定特定應用程式和服務的驗證原則， (信賴憑證者信任，) 受到 AD FS 保護。 指定特定應用程式 (每個信賴憑證者信任) 的驗證原則不會覆寫通用驗證原則。 如果通用或每個信賴憑證者信任驗證原則需要 MFA，每當使用者嘗試驗證此信賴憑證者信任就會觸發 MFA。  通用驗證原則是未設定特定驗證原則之信賴憑證者信任 (應用程式和服務) 的後援。

全域驗證原則會套用至 AD FS 保護的所有信賴憑證者。 您可以在通用驗證原則中設定下列設定：

-   用於主要驗證的驗證方法

-   MFA 的設定和方法

-   是否已啟用裝置驗證。 如需詳細資訊，請參閱 [Join to Workplace from Any Device for SSO and Seamless Second Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

每個信賴憑證者信任驗證原則會特別套用以嘗試存取該特定信賴憑證者信任 (應用程式或服務)。 您可以在每個信賴憑證者信任驗證原則中設定下列設定：

-   使用者是否需要在每次登入時提供憑證

-   基於使用者/群組、裝置註冊和存取要求位置資料的 MFA 設定

### <a name="primary-and-additional-authentication-methods"></a>主要和其他驗證方法
透過 Windows Server 2012 R2 中的 AD FS，除了主要的驗證機制之外，系統管理員還可以設定其他驗證方法。 主要驗證方法是內建的，目的是用來驗證使用者的身分識別。 您可以設定其他驗證因素，要求提供有關使用者身分識別的詳細資訊，進而確保更強的驗證。

在 Windows Server 2012 R2 的 AD FS 中使用主要驗證時，您有下列選項：

-   對於要從公司網路外部存取的已發行資源，預設會選取表單驗證。 此外，您也可以啟用憑證驗證 (也就是，智慧卡驗證或 AD DS 可用的使用者用戶端憑證驗證)。

-   對於內部網路資源，預設會選取 Windows 驗證。 此外，您也可以啟用表單和 (或) 憑證驗證。

您可以選擇一種以上的驗證方法，讓使用者在應用程式或服務的登入頁面選擇驗證方法。

您也可以針對無縫式次要因素驗證啟用裝置驗證。 這會將使用者的身分識別系結至用來存取資源的已註冊裝置，因此在存取受保護的資源之前，會提供更安全的複合身分識別驗證。

> [!NOTE]
> 如需裝置物件、裝置註冊服務、Workplace Join 和裝置的詳細資訊，做為無縫式的第二因素驗證和 SSO，請參閱[從任何裝置加入工作場所以進行 SSO，以及跨公司應用程式進行無縫第二因素驗證](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

如果您指定在內部網路資源使用 Windows 驗證方法 (預設選項)，驗證要求可在支援 Windows 驗證的瀏覽器上無縫地使用這個方法。

> [!NOTE]
> 並非所有瀏覽器都支援 Windows 驗證。 Windows Server 2012 R2 AD FS 中的驗證機制會偵測使用者的瀏覽器使用者代理程式，並使用可設定的設定來判斷該使用者代理程式是否支援 Windows 驗證。 系統管理員可在此清單新增使用者代理程式 (透過 Windows PowerShell `Set-AdfsProperties -WIASupportedUserAgents` 命令)，為支援 Windows 驗證的瀏覽器指定替代使用者代理程式字串。 如果用戶端的使用者代理程式不支援 Windows 驗證，則預設的回溯方法為表單驗證。

### <a name="configuring-mfa"></a>設定 MFA
有兩個部分可在 Windows Server 2012 R2 的 AD FS 中設定 MFA：指定需要 MFA 的條件，然後選取其他驗證方法。 如需其他驗證方法的詳細資訊，請參閱[設定 AD FS 的其他驗證方法](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)。

**MFA 設定**

MFA 設定 (需要 MFA 的條件) 有下列可用選項：

-   您可以針對同盟伺服器加入的 AD 網域中的特定使用者和群組要求 MFA。

-   您可以為已註冊 (已加入工作地點) 或未註冊 (未加入工作地點) 裝置要求 MFA。

    Windows Server 2012 R2 採用以使用者為中心的方法，讓裝置物件代表與公司之間的關聯性的新式裝置 user@device 。 裝置物件是 Windows Server 2012 R2 中 AD 的新類別，可在提供應用程式和服務的存取權時，用來提供複合身分識別。 AD FS 的新元件 - 裝置註冊服務 (DRS) - 在 Active Directory 中佈建裝置身分識別，並在可用來代表裝置身分識別的消費裝置上設定憑證。 接著，您可以使用此裝置身分識別將裝置加入到工作地點，也就是將您的個人裝置連線到工作地點的 Active Directory。 當您將個人裝置加入工作地點就會變成已知裝置，並可提供無縫式的次要因素驗證來保護資源和應用程式。 換句話說，在裝置加入工作場所之後，使用者的身分識別會系結到此裝置，而且可以在存取受保護的資源之前，用來進行順暢的複合身分識別驗證。

    如需工作地方聯結和離開的詳細資訊，請參閱[從任何裝置加入工作場所以進行 SSO，以及跨公司應用程式進行無縫第二因素驗證](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

-   如果受保護資源的存取要求來自外部網路或內部網路，您可以要求 MFA。

## <a name="scenario-overview"></a><a name="BKMK_2"></a>案例總覽
在此案例中，您會根據使用者的群組成員資格資料，針對特定應用程式啟用 MFA。 換句話說，您將在同盟伺服器上設定驗證原則，在屬於特定群組的使用者要求存取裝載在網頁伺服器上的特定應用程式時要求 MFA。

更具體來說，在這個案例中，您針對名為 **claimapp** 的宣告測試應用程式啟用驗證原則，因此 AD 使用者 **Robert Hatley** 將需要使用 MFA，因為他屬於 AD 群組 **Finance**。

逐步解說[指南：透過其他多因素驗證管理機密應用程式的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)中提供設定和驗證此案例的逐步指示。 若要完成本逐步解說中的步驟，您必須設定實驗室環境，並依照在[Windows Server 2012 R2 中設定 AD FS 的實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)中的步驟進行。

在 AD FS 中啟用 MFA 的其他案例包括下列各項：

-   如果存取要求來自外部網路，則啟用 MFA。 您可以修改[逐步解說指南：透過其他多因素驗證管理機密應用程式的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)一節中顯示的程式碼，如下所示：

    ```
    'c:[type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn" );'
    ```

-   如果存取要求來自未加入工作地點的裝置，則啟用 MFA。  您可以修改[逐步解說指南：透過其他多因素驗證管理機密應用程式的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)一節中顯示的程式碼，如下所示：

    ```
    'NOT EXISTS([type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid"]) => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

-   如果使用者以加入工作地點的裝置傳送存取要求，但該裝置不是以該使用者的身分註冊，則啟用 MFA。 您可以修改[逐步解說指南：透過其他多因素驗證管理機密應用程式的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)一節中顯示的程式碼，如下所示：

    ```
    'c:[type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", value == "false"] => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

## <a name="see-also"></a>另請參閱
[逐步解說指南：透過其他多因素驗證管理機密應用程式](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md) 
 的風險[在 Windows Server 2012 R2 中設定 AD FS 的實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



