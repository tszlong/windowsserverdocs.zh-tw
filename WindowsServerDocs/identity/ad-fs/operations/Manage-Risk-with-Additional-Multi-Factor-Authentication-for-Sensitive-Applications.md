---
ms.assetid: 934ac796-e2ee-490d-8265-6a818be5ee79
title: 透過其他多因素驗證管理機密應用程式的風險
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a37c0b0a1de06b65e0cb867ec4d160bbb0373a15
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189069"
---
# <a name="manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>透過其他多因素驗證管理機密應用程式的風險




-   [在 Windows Server 2012 R2 設定 AD FS 實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)

-   [逐步解說指南：透過機密應用程式的其他多因素驗證管理風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

-   [適用於 AD FS 中設定其他驗證方法](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

## <a name="in-this-guide"></a>本指南內容
本指南提供下列資訊：

-   [AD FS 驗證機制](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_1)-Windows Server 2012 R2 中的 Active Directory Federation Services (AD FS) 中可用的驗證機制的描述

-   [案例概觀](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)-若要啟用多重要素驗證 (MFA) 使用 Active Directory Federation Services (AD FS) 的案例的描述會根據使用者的群組成員資格。

    > [!NOTE]
    > 在 Windows Server 2012 R2 中 AD FS 中，您可以啟用 MFA 根據網路位置、 裝置身分識別，以及使用者的身分識別或群組的成員資格。

    設定和驗證此案例的詳細的逐步指示，請參閱[逐步解說指南：透過機密應用程式的其他多因素驗證管理風險](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。

## <a name="BKMK_1"></a>重要概念-AD FS 驗證機制

### <a name="benefits-of-authentication-mechanisms-in-ad---fs"></a>AD FS 驗證機制的好處
Windows Server 2012 R2 中的 active Directory Federation Services (AD FS) 會提供 IT 系統管理員提供一組更豐富且更有彈性的工具，用於驗證想要存取公司資源的使用者。 它可讓系統管理員有彈性地控制主要和其他驗證方法，提供豐富的管理體驗，來設定驗證原則 （無論透過使用者介面和 Windows PowerShell），並增強存取應用程式和受保護的 AD FS 服務的使用者體驗。 以下是一些保護您的應用程式和 Windows Server 2012 R2 中的 AD FS 服務的優點：

-   全域驗證原則-中央管理功能，IT 系統管理員可以從中選擇何種驗證方法用來驗證使用者存取受保護的資源的網路位置為基礎。 這種做法可讓系統管理員執行下列工作：

    -   強制來自外部網路的存取要求使用更安全的驗證方法。

    -   針對無縫式次要因素驗證啟用裝置驗證。 這會繫結至已註冊的裝置用來存取資源，因此提供更安全的複合身分識別驗證，才能存取受保護的資源的使用者的身分識別。

        > [!NOTE]
        > 以無縫式次要因素驗證和單一登入的裝置物件、 裝置註冊服務，工作地點加入，和裝置的相關資訊，請參閱[從任何裝置加入工作地點網路提供 SSO 和無縫式的第二個因素驗證所有公司應用程式](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

    -   設定 MFA 需求，針對所有外部網路存取，或有條件地根據使用者的身分識別、 網路位置或裝置用來存取受保護的資源。

-   在設定驗證原則更大的彈性： 您可以使用不同的商業價值來設定 AD FS 保護資源的自訂驗證原則。 例如，您可以在對業務影響較大的應用測試要求使用 MFA。

-   容易使用： 簡單且直覺式的管理工具，例如以 GUI 為基礎 AD FS 管理 MMC 嵌入式管理單元和 Windows PowerShell cmdlet 可讓 IT 系統管理員使用相對簡單的方法設定驗證原則。 您可以使用 Windows PowerShell，編寫較大規模的解決方案，並自動化一般管理工作。

-   更有效控制公司資產： 身為系統管理員，您可以使用 AD FS 來設定驗證原則會套用到特定的資源，您可以更因為控制保護公司資源的。 應用程式不可以覆寫 IT 系統管理員指定的驗證原則。 針對機密應用程式和服務，您可以啟用 MFA 需求、裝置驗證，以及在每次存取資源後選擇性的更新驗證。

-   支援自訂 MFA 提供者： 針對使用協力廠商 MFA 方法的組織，AD FS 可讓您合併並無縫使用這些驗證方法。

### <a name="authentication-scope"></a>驗證範圍
在 Windows Server 2012 R2 中 AD FS 中，您可以指定適用於所有的應用程式和受保護的 AD FS 服務的全域範圍的驗證原則。  您也可以設定特定的應用程式和服務 （信賴憑證者信任） 受保護的 AD FS 的驗證原則。 指定特定應用程式 (每個信賴憑證者信任) 的驗證原則不會覆寫通用驗證原則。 如果通用或每個信賴憑證者信任驗證原則需要 MFA，每當使用者嘗試驗證此信賴憑證者信任就會觸發 MFA。  通用驗證原則是未設定特定驗證原則之信賴憑證者信任 (應用程式和服務) 的後援。

在全域驗證原則套用至所有信賴憑證者的合作對象在受保護的 AD FS。 您可以在通用驗證原則中設定下列設定：

-   用於主要驗證的驗證方法

-   MFA 的設定和方法

-   是否已啟用裝置驗證。 如需詳細資訊，請參閱 [從任何裝置加入工作地點，並在公司的各個應用程式提供 SSO 和無縫式的次要因素驗證](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

每個信賴憑證者信任驗證原則會特別套用以嘗試存取該特定信賴憑證者信任 (應用程式或服務)。 您可以在每個信賴憑證者信任驗證原則中設定下列設定：

-   使用者是否需要在每次登入時提供憑證

-   基於使用者/群組、裝置註冊和存取要求位置資料的 MFA 設定

### <a name="primary-and-additional-authentication-methods"></a>主要和其他驗證方法
除了主要驗證機制、、 Windows Server 2012 R2 中的 AD FS 系統管理員可以設定其他驗證方法。 主要驗證方法已內建，用於驗證使用者的身分識別。 您可以設定其他驗證因素，要求會提供 關於使用者的身分識別的詳細資訊，並因此確保更強大的驗證。

在 Windows Server 2012 R2 中的 AD FS 中的主要驗證，您有下列選項：

-   對於要從公司網路外部存取的已發行資源，預設會選取表單驗證。 此外，您也可以啟用憑證驗證 (也就是，智慧卡驗證或 AD DS 可用的使用者用戶端憑證驗證)。

-   對於內部網路資源，預設會選取 Windows 驗證。 此外，您也可以啟用表單和 (或) 憑證驗證。

您可以選擇一種以上的驗證方法，讓使用者在應用程式或服務的登入頁面選擇驗證方法。

您也可以針對無縫式次要因素驗證啟用裝置驗證。 這會繫結至已註冊的裝置用來存取資源，因此提供更安全的複合身分識別驗證，才能存取受保護的資源的使用者的身分識別。

> [!NOTE]
> 以無縫式次要因素驗證和單一登入的裝置物件、 裝置註冊服務，工作地點加入，和裝置的相關資訊，請參閱[從任何裝置加入工作地點網路提供 SSO 和無縫式的第二個因素驗證所有公司應用程式](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

如果您指定在內部網路資源使用 Windows 驗證方法 (預設選項)，驗證要求可在支援 Windows 驗證的瀏覽器上無縫地使用這個方法。

> [!NOTE]
> 並非所有瀏覽器都支援 Windows 驗證。 Windows Server 2012 R2 中 AD fs 驗證機制會偵測使用者的瀏覽器使用者代理程式，並使用可設定的設定來判斷使用者代理程式是否支援 Windows 驗證。 系統管理員可在此清單新增使用者代理程式 (透過 Windows PowerShell `Set-AdfsProperties -WIASupportedUserAgents` 命令)，為支援 Windows 驗證的瀏覽器指定替代使用者代理程式字串。 如果用戶端的使用者代理程式不支援 Windows 驗證，預設的後援方法就是表單驗證。

### <a name="configuring-mfa"></a>設定 MFA
若要在 Windows Server 2012 R2 中的 AD FS 中設定 MFA 的兩部分： 指定的條件下 MFA 是必要項，並選取其他驗證方法。 如需有關其他驗證方法的詳細資訊，請參閱[適用於 AD FS 設定其他驗證方法](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)。

**MFA 設定**

MFA 設定 (需要 MFA 的條件) 有下列可用選項：

-   您可以針對同盟伺服器加入的 AD 網域中的特定使用者和群組要求 MFA。

-   您可以為已註冊 (已加入工作地點) 或未註冊 (未加入工作地點) 裝置要求 MFA。

    Windows Server 2012 R2 其中裝置物件代表之間的關聯性的現代裝置採用以使用者為中心方法user@device和公司。 裝置物件是新的類別可以用來提供應用程式和服務的存取權時負責提供複合身分識別的 Windows Server 2012 R2 中 AD 中。 AD FS 的新元件 - 裝置註冊服務 (DRS) - 在 Active Directory 中佈建裝置身分識別，並在可用來代表裝置身分識別的消費裝置上設定憑證。 接著，您可以使用此裝置身分識別將裝置加入到工作地點，也就是將您的個人裝置連線到工作地點的 Active Directory。 當您將個人裝置加入工作地點就會變成已知裝置，並可提供無縫式的次要因素驗證來保護資源和應用程式。 換句話說，裝置已加入工作場所之後，使用者的身分識別會繫結到此裝置，並存取受保護的資源之前可使用無縫式的複合身分識別驗證。

    如需有關工作場所加入和離開的詳細資訊，請參閱 <<c0> [ 加入工作地點，從任何裝置提供 SSO 和無縫式第二個因素 Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

-   如果受保護資源的存取要求來自外部網路或內部網路，您可以要求 MFA。

## <a name="BKMK_2"></a>案例概觀
在此案例中，您可以啟用特定應用程式的使用者的群組成員資格資料為基礎的 MFA。 換句話說，您將在同盟伺服器上設定驗證原則，在屬於特定群組的使用者要求存取裝載在網頁伺服器上的特定應用程式時要求 MFA。

更具體來說，在這個案例中，您針對名為 **claimapp** 的宣告測試應用程式啟用驗證原則，因此 AD 使用者 **Robert Hatley** 將需要使用 MFA，因為他屬於 AD 群組 **Finance**。

逐步解說指示設定和驗證此案例中提供[逐步解說指南：透過機密應用程式的其他多因素驗證管理風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。 若要完成本逐步解說中的步驟，您必須設定實驗室環境，並依照[適用於 Windows Server 2012 R2 中的 AD FS 設定實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

啟用 MFA AD FS 中的其他案例包括下列各項：

-   如果存取要求來自外部網路，則啟用 MFA。 您可以修改 「 設定 MFA 原則 > 一節中提供的程式碼[逐步解說指南：管理機密應用程式透過其他多因素驗證的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)取代下列項目：

    ```
    'c:[type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn" );'
    ```

-   如果存取要求來自未加入工作地點的裝置，則啟用 MFA。  您可以修改 「 設定 MFA 原則 > 一節中提供的程式碼[逐步解說指南：管理機密應用程式透過其他多因素驗證的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)取代下列項目：

    ```
    'NOT EXISTS([type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid"]) => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

-   如果使用者以加入工作地點的裝置傳送存取要求，但該裝置不是以該使用者的身分註冊，則啟用 MFA。 您可以修改 「 設定 MFA 原則 > 一節中提供的程式碼[逐步解說指南：管理機密應用程式透過其他多因素驗證的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)取代下列項目：

    ```
    'c:[type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", value == "false"] => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

## <a name="see-also"></a>另請參閱
[逐步解說指南：管理機密應用程式透過其他多因素驗證的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)
[設定適用於 Windows Server 2012 R2 中的 AD FS 實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



