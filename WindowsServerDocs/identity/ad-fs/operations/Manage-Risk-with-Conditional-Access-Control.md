---
ms.assetid: a0f7bb11-47a5-47ff-a70c-9e6353382b39
title: "管理條件存取控制的風險"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e2ad7d1467abd6d69077b515b8c69a65f7e70f19
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="manage-risk-with-conditional-access-control"></a>管理條件存取控制的風險

>適用於： Windows Server 2012 R2


-   [AD FS 中按鍵的概念-條件存取控制](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_1)

-   [管理條件存取控制的風險](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md#BKMK_2)

## <a name="BKMK_1"></a>主要概念-條件存取控制 AD FS 中
AD FS 的整體功能是發行包含一組宣告存取預付碼。 AD FS 接受並再問題所宣告相關受到理賠要求規則。

AD FS 中的存取控制係發行授權理賠要求規則發行允許或拒絕宣告將會判斷使用者是否可使用與或存取 AD FS 保護資源，或不會允許群組中的使用者。 授權規則只能信賴廠商信任上設定。

|規則選項|邏輯規則|
|---------------|--------------|
|允許所有使用者|如果輸入宣告類型等於*任何宣告類型*和值等*的任何值*，問題然後取得的值等*允許*|
|允許此傳入理賠要求的使用者存取|如果輸入宣告類型等於*指定宣告類型*和值等*指定宣告值*，問題然後取得的值等*允許*|
|拒絕這個傳入理賠要求的使用者存取|如果輸入宣告類型等於*指定宣告類型*和值等*指定宣告值*，問題然後取得的值等*拒絕*|

如需有關這些的規則選項並邏輯操作，請查看[使用授權理賠要求規則](https://technet.microsoft.com/library/ee913560.aspx)。

在 Windows Server 2012 R2 AD FS 中, 存取控制的多因素，包括使用者、裝置、位置及驗證資料的改善。 這是因為宣告類型更多種的授權理賠要求規則。  亦即，AD FS 在 Windows Server 2012 R2，您可以執行的使用者身分或群組成員資格網路位置，裝置為基礎的條件存取控制 (是否地點加入，如需詳細資訊，請查看[SSO 和順暢第二個因數驗證在公司應用程式加入的工作地點，從任何裝置](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md))，以及驗證狀態（是否執行要素 (MFA)）。

在 Windows Server 2012 R2，AD FS 中的條件存取控制提供下列優點：

-   彈性、易懂每個應用程式授權原則，讓您可以允許或拒絕存取權的使用者，裝置、網路位置，以及驗證狀態

-   建立發行授權信賴廠商應用程式規則

-   常見的條件存取控制案例豐富 UI 體驗

-   支援進階條件存取控制案例豐富宣告語言及 Windows PowerShell

-   自訂 (每個可以方應用程式) '存取' 訊息。 如需詳細資訊，請查看[[自訂頁面 AD FS 登入](https://technet.microsoft.com/library/dn280950.aspx)。 藉由自訂這些訊息，您可以解釋為何使用者正在無法存取和也有助於自助的補救很有可能，例如提示使用者的工作地點加入他們的裝置。 如需詳細資訊，請查看[SSO 和順暢第二個因數驗證在公司應用程式加入的工作地點，從任何裝置](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。

下表包含 AD FS 提供所有宣告類型實作條件存取控制使用 Windows Server 2012 R2。

|宣告類型|描述|
|--------------|---------------|
|電子郵件地址|使用者的電子郵件地址。|
|名字|指定的使用者名稱。|
|名稱|唯一的使用者名稱|
|UPN|使用者主體名稱 (UPN) 的使用者。|
|一般的名稱|一般的使用者名稱。|
|AD FS 1 x 電子郵件地址|使用者相互操作 AD FS 1.1 或 AD FS 1.0 時的電子郵件地址。|
|群組|群組的使用者的成員。|
|AD FS 1 x UPN|UPN 的相互操作 AD FS 1.1 或 AD FS 1.0 時的使用者。|
|角色|使用者的角色。|
|姓氏|姓氏的使用者。|
|PPID|使用者私人識別碼。|
|名稱 ID|SAML 名稱 identifier 的使用者。|
|驗證頻率|用來顯示的時間和日期，已驗證使用者。|
|驗證方法|用來驗證使用者的方法。|
|拒絕僅限群組 SID|僅限拒絕 SID 使用者群組。|
|拒絕只主要 SID|僅限拒絕主要使用者的 SID。|
|拒絕主要群組 SID|僅限拒絕主要群組使用者的 SID。|
|SID 群組|使用者群組 SID。|
|主要群組 SID|主要群組使用者的 SID。|
|主要 SID|使用者主要 SID。|
|Windows account 名稱|網域 account 的網域使用者的使用者名稱。|
|是登記使用者|使用者係使用此裝置。|
|裝置識別碼|識別碼裝置。|
|裝置登記識別碼|識別碼裝置登記。|
|裝置登記顯示名稱|顯示名稱裝置登記。|
|裝置作業系統類型|裝置作業系統類型。|
|裝置作業系統版本|作業系統版本的裝置。|
|已受管理的裝置|管理服務所管理的裝置。|
|轉送 Client IP|使用者的 IP 位址。|
|Client 應用程式|Client 應用程式類型。|
|Client 使用者代理程式|使用裝置類型 client 存取應用程式。|
|Client IP|Client 的 IP 位址。|
|端點路徑|可用來判斷使用被動式戶端與絕對端點路徑。|
|Proxy|聯盟伺服器 proxy 傳遞要求 DNS 名稱。|
|應用程式識別碼|信賴識別碼。|
|應用程式原則|應用程式原則的憑證。|
|授權金鑰識別字|簽署發行的憑證的憑證授權單位金鑰識別碼擴充功能。|
|基本限制|其中一個基本限制的憑證。|
|美化金鑰使用|請描述美化金鑰憑證的方式之一。|
|發行者|憑證授權單位發行 X.509 憑證的名稱。|
|發行者的名稱|分辨的憑證發行者的名稱。|
|使用|其中一個金鑰使用憑證的方式。|
|不之後|本地時間之後憑證已不再有效的日期。|
|不之前|本地時間憑證生效日期。|
|憑證原則|發行憑證的原則。|
|公開鍵|公開的憑證鍵。|
|憑證未經處理資料|憑證未經處理資料。|
|主旨替代名稱|其中一個替代憑證的名稱。|
|序號|憑證的序號。|
|簽章演算法|用來建立的憑證簽章的演算法。|
|主題|憑證的主題。|
|主旨按鍵識別字|主旨按鍵識別碼的憑證。|
|主體名稱|主旨分辨的憑證的名稱。|
|V2 範本名稱|當發行或更新憑證使用的版本 2 憑證範本名稱。 這是特定 Microsoft 的值。|
|V1 範本名稱|第 1 版的憑證範本發行或更新的憑證時使用的名稱。 這是特定 Microsoft 的值。|
|指紋|憑證的指紋。|
|X x509 版本|X.509 格式憑證的版本。|
|Inside 企業網路|用來表示是否要求來自在公司網路。|
|密碼到期時間|用來顯示密碼後到期的時間。|
|密碼到期日|用來顯示天數密碼到期。|
|更新密碼 URL|用來顯示更新密碼服務的網頁位址。|
|驗證方法資訊尋找參考資料|用來表示 al 驗證方法驗證使用者使用。|

## <a name="BKMK_2"></a>管理條件存取控制的風險
使用的可用設定，有許多方式可以中，您可以管理風險實作條件存取控制。

### <a name="common-scenarios"></a>常見案例
例如，想像用實作條件存取控制使用者的群組成員資格資料特定應用程式（信賴廠商信任）為基礎的簡單的方式。 亦即，您可以設定發行授權規則聯盟伺服器上為允許特定群組屬於您的廣告中的使用者網域特定應用程式 AD FS 受保護的存取。  詳細的逐步指示（使用 UI 及 Windows PowerShell）執行此案例涵蓋在[逐步解說快速入門：條件存取控制與管理的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)。 以完成本節中的步驟，您必須設定測試環境並依照[在 Windows Server 2012 R2 AD FS 設定實驗室](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)。

### <a name="advanced-scenarios"></a>若要進一步
其他實作條件存取控制在 AD FS 在 Windows Server 2012 R2 的範例包括：

-   允許應用程式才這位使用者的身分 MFA 的驗證，AD FS 受保護的存取

    您可以使用下列程式碼：

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessWithMFA"
    c:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   允許應用程式係地點結合裝置的使用者即將存取要求時，才受到 AD FS 存取

    您可以使用下列程式碼：

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "PermitAccessFromRegisteredWorkplaceJoinedDevice"
    c:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   允許應用程式存取要求來自係的工作地點結合裝置的身分 MFA 的已驗證使用者才 AD FS 受保護的存取

    您可以使用下列程式碼

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAOnRegisteredWorkplaceJoinedDevice"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser", Value =~ "^(?i)true$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

-   允許應用程式存取要求來自的使用者 MFA 的已驗證的身分，才受到 AD FS 外部網路存取。

    您可以使用下列程式碼：

    ```
    @RuleTemplate = "Authorization"
    @RuleName = "RequireMFAForExtranetAccess"
    c1:[Type == "https://schemas.microsoft.com/claims/authnmethodsreferences", Value =~ "^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$"] &&
    c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value =~ "^(?i)false$"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "PermitUsersWithClaim");

    ```

## <a name="see-also"></a>也了
[逐步解說指南：管理條件存取控制與風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Conditional-Access-Control.md)<ph x="2">
</ph>[設定實驗室 AD FS 在 Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)



