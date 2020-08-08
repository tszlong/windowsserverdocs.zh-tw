---
ms.assetid: a0f7bb11-47a5-47ff-a70c-9e6353382b39
title: 使用條件式存取控制管理風險
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 97466c5f7d0a6c89980195d7b71e6697748db334
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954214"
---
# <a name="manage-risk-with-conditional-access-control"></a>使用條件式存取控制管理風險




-   [主要概念-AD FS 中的條件式存取控制](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [使用條件式存取控制管理風險](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

## <a name="key-concepts---conditional-access-control-in-ad-fs"></a><a name="BKMK_1"></a>重要概念 - AD FS 中的條件式存取控制
AD FS 的整體功能是發行包含一組宣告的存取權杖。 關於哪些宣告 AD FS 接受，然後發出問題的決定是由宣告規則所控管。

AD FS 中的存取控制會使用發行授權宣告規則來執行，以用來發出允許或拒絕宣告，以決定是否允許使用者或使用者群組存取 AD FS 保護的資源。 只可以在信賴憑證者信任上設定授權規則。

|規則選項|規則邏輯|
|---------------|--------------|
|允許所有使用者|如果傳入宣告類型等於「任何宣告類型」** 且值等於「任何值」**，則會發行值等於「允許」** 的宣告|
|允許具有這個傳入宣告的使用者存取|如果傳入宣告類型等於「指定的宣告類型」** 且值等於「指定的宣告值」**，則會發行值等於「允許」** 的宣告|
|拒絕具有這個傳入宣告的使用者的存取|如果傳入宣告類型等於「指定的宣告類型」** 且值等於「指定的宣告值」**，則會發行值等於「拒絕」** 的宣告|

如需這些規則選項和邏輯的詳細資訊，請參閱[使用授權宣告規則的時機](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/ee913560(v=ws.11))。

在 Windows Server 2012 R2 的 AD FS 中，以多因素（包括使用者、裝置、位置和驗證資料）增強了存取控制。 這種方法之所以可行是因為授權宣告規則有多種不同的宣告類型。  換句話說，在 Windows Server 2012 R2 的 AD FS 中，您可以根據使用者身分識別或群組成員資格、網路位置、裝置 (是否已加入工作地點來強制執行條件式存取控制。如需詳細資訊，請參閱[從任何裝置加入工作場所以進行 SSO，以及在公司應用 () 程式之間進行無縫第二因素](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)驗證 () MFA ) 執行多重要素驗證。

Windows Server 2012 R2 AD FS 中的條件式存取控制提供下列優點：

-   根據每個應用程式提供彈性且易懂的授權原則，據此您可以依使用者、裝置、網路位置和驗證狀態來允許或拒絕存取

-   建立信賴憑證者應用程式的發行授權規則

-   為一般條件式存取控制案例提供豐富的 UI 使用經驗

-   為進階條件式存取控制案例提供豐富的宣告語言和 Windows PowerShell 支援

-   每個信賴憑證者應用程式的自訂 () 「拒絕存取」訊息。 如需詳細資訊，請參閱[自訂 AD FS 登入頁面](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280950(v=ws.11))。 透過自訂這些訊息，您可以解釋拒絕使用者存取的原因，同時視需要提供自助補救，例如，提示使用者將裝置加入工作地點。 如需詳細資訊，請參閱 [Join to Workplace from Any Device for SSO and Seamless Second Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

下表包含要用來執行條件式存取控制的 Windows Server 2012 R2 AD FS 中所有可用的宣告類型。

|宣告類型|描述|
|--------------|---------------|
|電子郵件地址|使用者的電子郵件地址。|
|名字|使用者的名字。|
|名稱|使用者的唯一名稱。|
|UPN|使用者的使用者主體名稱 (UPN)。|
|一般名稱|使用者的一般名稱。|
|AD FS 1 x 電子郵件地址|與 AD FS 1.1 或 AD FS 1.0 交互作業時的使用者電子郵件地址。|
|群組|使用者所屬的群組。|
|AD FS 1 x UPN|與 AD FS 1.1 或 AD FS 1.0 交互作業時的使用者 UPN。|
|角色|使用者擁有的角色。|
|Surname|使用者的姓氏。|
|PPID|使用者的私人識別碼。|
|名稱識別碼|使用者的 SAML 名稱識別碼。|
|驗證時間戳記|用來顯示使用者通過驗證的時間和日期。|
|驗證方法|用來驗證使用者的方法。|
|僅拒絕群組 SID|使用者的僅拒絕群組 SID。|
|僅拒絕主要 SID|使用者的僅拒絕主要 SID。|
|僅拒絕主要群組 SID|使用者的僅拒絕主要群組 SID。|
|群組 SID|使用者的群組 SID。|
|主要群組 SID|使用者的主要群組 SID。|
|主要 SID|使用者的主要 SID。|
|Windows 帳戶名稱|使用者的網域帳戶名稱，形式為網域\使用者。|
|是註冊的使用者|使用者已註冊使用此裝置。|
|裝置識別碼|裝置的識別碼。|
|裝置註冊識別碼|裝置註冊的識別碼。|
|裝置註冊顯示名稱|裝置註冊的顯示名稱。|
|裝置 OS 類型|裝置的作業系統類別。|
|裝置作業系統版本|裝置的作業系統版本。|
|是受管理的裝置|裝置是由管理服務負責管理。|
|轉寄的用戶端 IP|使用者的 IP 位址。|
|用戶端應用程式|用戶端應用程式的類型。|
|用戶端使用者代理程式|用戶端用來存取應用程式的裝置類型。|
|用戶端 IP|用戶端的 IP 位址。|
|端點路徑|用來判斷主動或被動用戶端的絕對端點路徑。|
|Proxy|傳送要求的同盟伺服器 Proxy DNS 名稱。|
|應用程式識別碼|信賴憑證者的識別碼。|
|應用程式原則|憑證的應用程式原則。|
|授權單位金鑰識別元|簽署已發行憑證的憑證授權單位金鑰識別元延伸。|
|基本限制|憑證的其中一個基本限制。|
|增強金鑰使用方法|說明憑證的其中一個增強金鑰使用方法。|
|Issuer|發出 X.509 憑證的憑證授權單位名稱。|
|簽發者名稱|憑證簽發者的辨別名稱。|
|金鑰使用方式|說明憑證的其中一個增強金鑰使用方法。|
|不晚於|憑證不再有效的當地時間日期。|
|生效時間|憑證生效的當地時間日期。|
|憑證原則|發行憑證所遵循的原則。|
|公開金鑰|憑證的公開金鑰。|
|憑證原始資料|憑證的原始資料。|
|主體替代名稱|憑證的其中一個替代名稱。|
|序號|憑證的序號。|
|簽章演算法|建立憑證簽章所用的演算法。|
|主體|來自憑證的主體。|
|主體金鑰識別碼|憑證的主體金鑰識別碼。|
|主體名稱|來自憑證的主體辨別名稱。|
|V2 範本名稱|發行或續約憑證時所用的版本 2 憑證範本名稱。 這是 Microsoft 特定的值。|
|V1 範本名稱|發行或續約憑證時所用的版本 1 憑證範本名稱。 這是 Microsoft 特定的值。|
|Thumbprint|憑證的指紋。|
|X 509 版本|憑證的 X.509 格式版本。|
|公司網路內部|用來指出要求是否從公司網路內部產生。|
|密碼到期時間|用來顯示密碼到期的時間。|
|密碼到期天數|用來顯示密碼到期的天數。|
|更新密碼 URL|用來顯示更新密碼服務的網址。|
|驗證方法參考|用來指出驗證使用者所用的所有驗證方法。|

## <a name="managing-risk-with-conditional-access-control"></a><a name="BKMK_2"></a>使用條件式存取控制管理風險
使用可用的設定，實作條件式存取控制來管理風險的方法有很多種。

### <a name="common-scenarios"></a>常見案例
例如，假設有一個簡單的案例，根據特定應用程式的使用者群組成員資格資料來執行條件式存取控制， (信賴憑證者信任) 。 換句話說，您可以在同盟伺服器上設定發行授權規則，讓屬於您 AD 網域中某個群組的使用者存取受 AD FS 保護的特定應用程式。  如需實作此案例的詳細逐步指示 (使用 UI 和 Windows PowerShell)，請參閱 [Walkthrough Guide: Manage Risk with Conditional Access Control](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)。 若要完成本逐步解說中的步驟，您必須設定實驗室環境，並依照在[Windows Server 2012 R2 中設定 AD FS 的實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)中的步驟進行。

### <a name="advanced-scenarios"></a>進階案例
在 Windows Server 2012 R2 中的 AD FS 中執行條件式存取控制的其他範例包括下列各項：

-   只有在此使用者的身分識別已通過 MFA 驗證時，才允許存取受 AD FS 保護的應用程式

    您可以使用下列程式碼：

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessWithMFA"
    c:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   只有在存取要求來自已加入工作場所的裝置（已向使用者註冊）時，才允許存取受 AD FS 保護的應用程式

    您可以使用下列程式碼：

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessFromRegisteredWorkplaceJoinedDevice"
    c:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   只有在存取要求是來自已加入工作場所的裝置，且該裝置已向已通過 MFA 驗證的使用者註冊時，才允許存取受 AD FS 保護的應用程式

    您可以使用下列程式碼

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAOnRegisteredWorkplaceJoinedDevice"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   只有在存取要求是來自其身分識別已通過 MFA 驗證的使用者時，才允許外部網路存取受 AD FS 保護的應用程式。

    您可以使用下列程式碼：

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAForExtranetAccess"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value =~ "^(?i)false$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

## <a name="see-also"></a>另請參閱
[逐步解說指南：使用條件式存取控制](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md) 
 管理風險[在 Windows Server 2012 R2 中設定 AD FS 的實驗室環境](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
