---
ms.assetid: 934ac796-e2ee-490d-8265-6a818be5ee79
title: "管理其他多因素驗證敏感的應用程式的風險"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e9ee275e6fe38005394cd071e9cfe9a7999350e8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="manage-risk-with-additional-multi-factor-authentication-for-sensitive-applications"></a>管理其他多因素驗證敏感的應用程式的風險

>適用於： Windows Server 2012 R2


-   [在 Windows Server 2012 R2 AD FS 設定實驗室](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)

-   [逐步解說指南： 管理其他多因素驗證敏感的應用程式的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

-   [AD fs 中設定額外的驗證方法](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)

## <a name="in-this-guide"></a>本指南
本指南提供下列資訊：

-   [AD FS 驗證機制](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_1)-描述在 Windows Server 2012 R2 的 Active Directory 同盟 Services (AD FS) 中可用的驗證機制

-   [案例概觀](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md#BKMK_2)-，以便要素 (MFA) 使用 Active Directory 同盟 Services (AD FS) 案例的描述以使用者的群組成員資格。

    > [!NOTE]
    > 在 Windows Server 2012 R2 AD FS 中，您可以讓 MFA 根據網路位置，裝置的身分，以及使用者身分或群組成員資格。

    設定和驗證本案例詳細的逐步指示，請查看[逐步解說指南：管理其他多因素驗證敏感的應用程式的風險](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。

## <a name="BKMK_1"></a>主要概念-AD FS 機制驗證

### <a name="benefits-of-authentication-mechanisms-in-ad---fs"></a>AD FS 驗證機制的優點
在 Windows Server 2012 R2 的 active Directory 同盟 Services (AD FS) 提供 IT 系統管理員的工具一組更豐富的、更具彈性驗證使用者想要存取公司資源。 這讓系統管理員彈性控制主要和額外的驗證方法、提供豐富的管理體驗設定驗證原則（同時透過使用者介面及 Windows PowerShell）、和美化存取應用程式與服務都會受到 AD FS 終端使用者的使用體驗。 以下是一些保護您的應用程式與服務的 AD FS 在 Windows Server 2012 R2 的優點：

-   全球驗證原則的集中管理功能，IT 系統管理員可以選擇的驗證方法用來驗證使用者根據網路位置，存取受保護的資源。 這可讓系統管理員，執行下列動作：

    -   從外部命令使用存取要求更安全的驗證方法。

    -   讓裝置驗證順暢的第二個雙因素驗證。 這繫結到用來存取資源，存取受保護的資源之前，因此提供更安全的身分複合驗證且已裝置的使用者的身分。

        > [!NOTE]
        > 如順暢的第二個雙因素驗證和 SSO 裝置物件、裝置登記服務、加入的工作地點，以及裝置的相關詳細資訊，請查看[SSO 和順暢第二個因數驗證在公司應用程式加入的工作地點，從任何裝置](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

    -   設定適用於所有外部網路存取 MFA 需求或條件根據使用者的身分、網路位置或裝置可用來存取受保護的資源。

-   設定驗證原則彈性：您可以設定 AD FS 保護資源自訂驗證原則的不同的企業值。 例如，您可以要求 MFA 高企業影響的應用程式。

-   使用「輕鬆：簡單且直覺式管理工具，例如 gui AD FS 管理 MMC 嵌入式管理單元及 Windows PowerShell cmdlet 讓 IT 系統管理員輕鬆設定驗證原則。 使用 Windows PowerShell 中，您可以指令碼方案適用於縮放以及自動化例行管理工作。

-   更多的控制企業資產：因為您可以使用 AD FS 進行驗證原則的特定資源適用於系統管理員身分、為您擁有更控制透過如何公司資源的安全。 應用程式不會覆寫 IT 系統管理員所指定的驗證原則。 適用於機密應用程式與服務，您可以讓 MFA 需求，裝置驗證，並選擇新的驗證每次存取資源。

-   自訂 MFA 提供者的支援：利用第三方 MFA 方法的組織，AD FS 可讓您加入及使用這些驗證方法順暢地進行。

### <a name="authentication-scope"></a>驗證範圍
您可以在 Windows Server 2012 R2 中 AD FS 指定在適用於所有應用程式與服務都會受到 AD FS 全域範圍驗證原則。  您也可以設定驗證原則的特定應用程式與服務（可以廠商信任）受到 AD FS。 指定驗證原則，針對特定應用程式 (每個可以廠商信任) 不覆寫全球驗證原則。 如果全球或每個可以當使用者想要這個信賴廠商信任驗證時，將會觸發驗證原則需要 MFA，MFA 廠商信任。  全球驗證原則是後援信賴廠商信任（應用程式和服務），不需要特定驗證原則設定。

適用於所有信賴的對象，AD FS 受到全球驗證原則。 您可以為全球驗證原則的一部分，設定下列設定：

-   用於主要驗證的驗證方法

-   設定及 MFA 的方法

-   是否被讓裝置驗證。 如需詳細資訊，請查看[SSO 和順暢第二個因數驗證在公司應用程式加入的工作地點，從任何裝置](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

每個可以廠商信任驗證原則套用專門用來存取該信賴廠商信任（應用程式或服務）。 您可以為每個可以廠商信任驗證原則的一部分，設定下列設定：

-   使用者是否需要提供認證每次登入。

-   MFA 設定為基礎群組使用者/裝置登記、，存取要求位置資料

### <a name="primary-and-additional-authentication-methods"></a>主要和額外的驗證方法
在 Windows Server 2012 R2 的主要驗證機制，除了 AD FS 使用系統管理員可以設定額外的驗證方法。 建及要驗證使用者身分主要的驗證方法。 您可以設定要求的身分使用者的詳細資訊，提供額外的驗證因素和因此確保較驗證。

主要驗證，AD FS 在 Windows Server 2012 R2 中，您有下列選項：

-   資源發行以外的公司網路的存取，預設會選取表單驗證。 此外，您也可以讓憑證驗證（亦即，智慧卡驗證或使用者 client 憑證驗證的搭配 AD DS）。

-   適用於 intranet 資源，預設會選取 Windows 驗證。 除了您也可以讓表單和/或驗證憑證。

選取一個以上的驗證方法，可以透過讓使用者可以選擇在登入頁面上的應用程式或服務的驗證方法。

您也可以讓裝置驗證順暢的第二個雙因素驗證。 這繫結到用來存取資源，存取受保護的資源之前，因此提供更安全的身分複合驗證且已裝置的使用者的身分。

> [!NOTE]
> 如順暢的第二個雙因素驗證和 SSO 裝置物件、裝置登記服務、加入的工作地點，以及裝置的相關詳細資訊，請查看[SSO 和順暢第二個因數驗證在公司應用程式加入的工作地點，從任何裝置](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

如果您可以指定 Windows 驗證方法（預設選項）內部網路資源，驗證要求經歷這個地在支援 Windows 驗證的瀏覽器的方法。

> [!NOTE]
> Windows 驗證不支援所有的瀏覽器。 在 Windows Server 2012 R2 AD FS 驗證機制偵測使用者的瀏覽器使用者代理程式，並使用，您可以設定判斷是否使用者代理程式都支援 Windows 驗證。 系統管理員可以新增到此清單中的使用者代理 (透過 Windows PowerShell`Set-AdfsProperties -WIASupportedUserAgents`命令，以指定支援 Windows 驗證的瀏覽器的其他使用者代理程式字串。 如果 client 的使用者代理人不支援 Windows 驗證，預設回溯方法是表單驗證。

### <a name="configuring-mfa"></a>設定 MFA
有兩個組件中的 Windows Server 2012 R2 AD FS 進行 MFA：指定下 MFA 不需要的條件，並選取 [額外的驗證方法。 適用於額外的驗證方法的相關詳細資訊，請查看[設定額外的驗證方法 AD FS](../../ad-fs/operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)。

**MFA 設定**

下列選項可供 MFA 設定（這需要 MFA 在條件）：

-   您可以要求 MFA 特定使用者和群組中已加入您的聯盟伺服器 AD 網域。

-   您可以要求 MFA 登記（地點加入）或註冊（不地點加入）的裝置。

    Windows Server 2012 R2 會使用者為主的方式現代裝置所在裝置物件代表之間的關係移至user@device和公司。 裝置物件是一種新中可以用來提供複合層的身分提供應用程式和服務的存取權時，Windows Server 2012 R2 的廣告。 AD FS 裝置登記服務 (DRS)-的新元件 provisions Active Directory 中裝置的身分，並設定將會用來表示裝置的身分消費者裝置上的憑證。 您可以使用此裝置的工作地點的身分加入您的裝置，亦即，將您的個人裝置連接到您的工作地點的 Active Directory。 當您到您的工作場所加入您的個人裝置時，在 [已知的裝置並將會提供順暢的第二個雙因素驗證受保護的資源和應用程式。 亦即，裝置地點加入之後，使用者的身分繫結到此裝置，存取受保護的資源之前，可以使用順暢複合身分驗證的需求。

    適用於更多的工作地點加入並保留的詳細資訊，請查看[加入到的工作場所 SSO 和順暢第二個因數驗證在公司應用程式的任何裝置上的](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

-   從外部網路或內部網路存取受保護的資源要求時，您可以要求 MFA。

## <a name="BKMK_2"></a>案例概觀
在本案例中，您可以根據針對特定應用程式的使用者的群組成員資格資料 MFA。 亦即，您將會設定驗證原則聯盟伺服器上為需要 MFA 時的特定群組的使用者要求存取特定應用程式的網頁伺服器上。

尤其，在本案例中，您可以宣告根據測試應用程式呼叫驗證原則**claimapp**，即 AD 使用者**劉小龍 Hatley**才能經歷 MFA 因為他屬於 AD 群組**財經**。

步驟來執行「步驟的指示設定，並確認本案例中提供[逐步解說快速入門：管理其他多因素驗證敏感的應用程式的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。 以完成本節中的步驟，您必須設定測試環境並依照[在 Windows Server 2012 R2 AD FS 設定實驗室](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

讓 MFA AD FS 中的其他案例，包含下列類型：

-   如果您要求存取其中的外部，讓 MFA。 您可以修改」設定好 MFA 原則」一節中所呈現的程式碼[逐步解說快速入門：管理其他多因素驗證敏感的應用程式的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)的下列：

    ```
    'c:[type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn" );'
    ```

-   可讓 MFA，如果存取要求來自結合非地點的裝置。  您可以修改」設定好 MFA 原則」一節中所呈現的程式碼[逐步解說快速入門：管理其他多因素驗證敏感的應用程式的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)的下列：

    ```
    'NOT EXISTS([type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid"]) => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

-   可讓 MFA，如果存取來自使用者加入，但無法登記給這位使用者的工作地點的裝置使用。 您可以修改」設定好 MFA 原則」一節中所呈現的程式碼[逐步解說快速入門：管理其他多因素驗證敏感的應用程式的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)的下列：

    ```
    'c:[type=="https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", value == "false"] => issue (type="https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "https://schemas.microsoft.com/claims/multipleauthn");'

    ```

## <a name="see-also"></a>也了
[逐步解說指南：管理敏感的應用程式與其他多因素驗證風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)<ph x="2">
</ph>[設定實驗室 AD FS 在 Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



