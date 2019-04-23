---
ms.assetid: ''
title: 在 Active Directory Federation Services 2.0 的用戶端存取控制原則
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 216af933aee643ee56feff71c59d9ecc2e62998c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842989"
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>AD FS 2.0 中的用戶端存取控制原則
在 Active Directory Federation Services 2.0 的用戶端存取原則可讓您限制或授予使用者存取資源。  本文件說明如何啟用 AD FS 2.0 中的用戶端存取原則以及如何設定的最常見的案例。

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>啟用 AD FS 2.0 中的用戶端存取原則

若要啟用用戶端存取原則，請遵循下列步驟。

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>步驟 1：封裝您的 AD FS 伺服器的 AD fs 2.0 更新彙總套件 2 的安裝

下載[更新彙總套件 2，針對 Active Directory Federation Services (AD FS) 2.0](https://support.microsoft.com/en-us/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0)封裝，然後將它安裝在所有的同盟伺服器和同盟伺服器 proxy。

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>步驟 2：新增五個宣告規則至 Active Directory 宣告提供者信任

一旦所有 proxy 與 AD FS 伺服器上都安裝更新彙總套件 2 之後，使用下列程序來新增一組宣告規則，對原則引擎可讓新的宣告類型。

若要這樣做，您將新增五個的接受轉換規則的每個新的要求內容的宣告類型使用下列程序。

Active Directory 宣告提供者信任中，建立新的接受轉換規則通過每個新的要求內容宣告類型。

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>若要加入至 Active Directory 的宣告規則的五個內容的每個宣告提供者信任宣告類型：


1. 按一下 開始、 指向程式、 指向 系統管理工具，然後按一下 AD FS 2.0 管理。
2. 在主控台樹狀目錄中，在 AD FS 2.0 \ 信任關係，按一下 宣告提供者信任，以滑鼠右鍵按一下 Active Directory，然後按一下 編輯宣告規則。
3. 在 編輯宣告規則 對話方塊中，選取 接受轉換規則 索引標籤，然後按一下新增的規則，以啟動規則精靈。
4. 在選取規則範本] 頁面上的 [宣告規則範本，選取 [通過或篩選傳入宣告從清單中，然後按一下下一步]。
5. 在設定規則 頁面上的 宣告規則名稱，輸入 顯示名稱，此規則中;在連入宣告類型，輸入下列宣告類型 URL，然後選取傳遞透過所有宣告值。</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. 若要驗證規則，在清單中選取它並按一下 編輯規則，然後按一下 檢視規則語言。 宣告規則語言應如下所示： `c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. 按一下 [完成]。
8. 在 [編輯宣告規則] 對話方塊中，按一下 [確定] 以儲存規則。
9. 重複步驟 2 至 6，直到建立所有五個規則，如下所示其餘的四個宣告類型的每個建立的其他宣告規則。

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>步驟 3：更新 Microsoft Office 365 識別平台信賴憑證者信任

選擇其中一個 Microsoft Office 365 身分識別平台上信賴憑證者信任，最符合您組織的需求設定宣告規則的範例案例。

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>適用於 AD FS 2.0 的用戶端存取原則案例
下列各節將說明 AD fs 2.0 存在的案例

### <a name="scenario-1-block-all-external-access-to-office-365"></a>案例 1：封鎖所有對 Office 365 的外部存取

此用戶端存取原則案例可讓您存取所有的內部用戶端和區塊外部的用戶端的 IP 位址為基礎的所有外部用戶端。 規則集為基礎的預設發行授權規則允許所有使用者存取。 您可以使用下列程序，將發行授權規則新增至 Office 365 信賴憑證者信任。

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>若要建立規則以封鎖所有對 Office 365 的外部存取



1. 按一下 開始、 指向程式、 指向 系統管理工具，然後按一下 AD FS 2.0 管理。
2. 在主控台樹狀目錄中，在 AD FS 2.0 \ 信任關係，按一下 信賴憑證者信任，以滑鼠右鍵按一下 Microsoft Office 365 識別平台信任，然後按一下 編輯宣告規則。 
3. 在 編輯宣告規則 對話方塊中，選取 發佈授權規則 索引標籤，然後按一下新增規則，以啟動 宣告規則精靈。
4. 在選取規則範本] 頁面上的 [宣告規則範本，請選取傳送宣告使用自訂規則，，，然後按一下 [下一步]。
5. 在設定規則] 頁面上的 [宣告規則名稱，輸入此規則的顯示名稱。 在自訂規則之下輸入或貼上下列宣告規則語言語法： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");` 
6. 按一下 [完成]。 確認新的規則會立即出現下方發行授權規則清單中的所有使用者規則允許存取。
7. 若要儲存規則，在 [編輯宣告規則] 對話方塊中，按一下 [確定]。

>[!NOTE]
>必須將 「 公用 ip 位址的 regex 」 以上的值取代為有效的 IP 運算式;建立 IP 位址範圍運算式，如需詳細資訊，請參閱。


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>案例 2：封鎖所有對 Office 365 Exchange ActiveSync 以外的外部存取

下列範例可讓所有的 Office 365 應用程式，包括 來自內部用戶端，包括 Outlook 的 Exchange Online 的存取。 用戶端的 IP 位址，例如智慧型手機的 Exchange ActiveSync 用戶端除了所示，它會封鎖從位於公司網路之外的用戶端存取。 規則集為基礎的標題為 允許所有使用者存取的預設發行授權規則。 若要將發行授權規則新增至 Office 365 信賴憑證者信任使用宣告規則精靈 中使用下列步驟：

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>若要建立規則以封鎖所有對 Office 365 的外部存取



1. 按一下 開始、 指向程式、 指向 系統管理工具，然後按一下 AD FS 2.0 管理。
2. 在主控台樹狀目錄中，在 AD FS 2.0 \ 信任關係，按一下 信賴憑證者信任，以滑鼠右鍵按一下 Microsoft Office 365 識別平台信任，然後按一下 編輯宣告規則。 
3. 在 編輯宣告規則 對話方塊中，選取 發佈授權規則 索引標籤，然後按一下新增規則，以啟動 宣告規則精靈。
4. 在選取規則範本] 頁面上的 [宣告規則範本，請選取傳送宣告使用自訂規則，，，然後按一下 [下一步]。
5. 在設定規則] 頁面上的 [宣告規則名稱，輸入此規則的顯示名稱。 在自訂規則之下輸入或貼上下列宣告規則語言語法： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.Autodiscover"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.ActiveSync"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 按一下 [完成]。 確認新的規則會立即出現下方發行授權規則清單中的所有使用者規則允許存取。
7. 若要儲存規則，在 [編輯宣告規則] 對話方塊中，按一下 [確定]。

>[!NOTE]
>必須將 「 公用 ip 位址的 regex 」 以上的值取代為有效的 IP 運算式;建立 IP 位址範圍運算式，如需詳細資訊，請參閱。

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>案例 3：封鎖所有對外部存取 Office 365 除了瀏覽器為基礎的應用程式

規則集為基礎的標題為 允許所有使用者存取的預設發行授權規則。 若要將發行授權規則新增至 Microsoft Office 365 識別平台信賴憑證者信任使用宣告規則精靈 中使用下列步驟：

>[!NOTE]
>此案例中不支援使用協力廠商 proxy 因為被動 （以 Web 為基礎） 的要求的用戶端存取原則標頭的限制。

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>若要建立規則以封鎖所有對外部存取 Office 365 除了瀏覽器為基礎的應用程式



1. 按一下 開始、 指向程式、 指向 系統管理工具，然後按一下 AD FS 2.0 管理。
2. 在主控台樹狀目錄中，在 AD FS 2.0 \ 信任關係，按一下 信賴憑證者信任，以滑鼠右鍵按一下 Microsoft Office 365 識別平台信任，然後按一下 編輯宣告規則。 
3. 在 編輯宣告規則 對話方塊中，選取 發佈授權規則 索引標籤，然後按一下新增規則，以啟動 宣告規則精靈。
4. 在選取規則範本] 頁面上的 [宣告規則範本，請選取傳送宣告使用自訂規則，，，然後按一下 [下一步]。
5. 在設定規則] 頁面上的 [宣告規則名稱，輸入此規則的顯示名稱。 在自訂規則之下輸入或貼上下列宣告規則語言語法： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 按一下 [完成]。 確認新的規則會立即出現下方發行授權規則清單中的所有使用者規則允許存取。
7. 若要儲存規則，在 [編輯宣告規則] 對話方塊中，按一下 [確定]。

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>案例 4:封鎖所有對外部存取 Office 365 為指定的 Active Directory 群組

下列範例會啟用從內部 IP 位址為基礎的用戶端存取。 它會封鎖從位於公司網路外部的用戶端具有外部用戶端 IP 位址存取，除了指定的 Active Directory 群組規則中的個人集是根據標題為 允許存取的預設發行授權規則所有使用者。 若要將發行授權規則新增至 Microsoft Office 365 識別平台信賴憑證者信任使用宣告規則精靈 中使用下列步驟：

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>若要建立規則以封鎖所有對外部存取 Office 365 為指定的 Active Directory 群組



1. 按一下 開始、 指向程式、 指向 系統管理工具，然後按一下 AD FS 2.0 管理。
2. 在主控台樹狀目錄中，在 AD FS 2.0 \ 信任關係，按一下 信賴憑證者信任，以滑鼠右鍵按一下 Microsoft Office 365 識別平台信任，然後按一下 編輯宣告規則。 
3. 在 編輯宣告規則 對話方塊中，選取 發佈授權規則 索引標籤，然後按一下新增規則，以啟動 宣告規則精靈。
4. 在選取規則範本] 頁面上的 [宣告規則範本，請選取傳送宣告使用自訂規則，，，然後按一下 [下一步]。
5. 在設定規則] 頁面上的 [宣告規則名稱，輸入此規則的顯示名稱。 在自訂規則之下輸入或貼上下列宣告規則語言語法： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 按一下 [完成]。 確認新的規則會立即出現下方發行授權規則清單中的所有使用者規則允許存取。
7. 若要儲存規則，在 [編輯宣告規則] 對話方塊中，按一下 [確定]。


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>在上述案例中使用宣告規則語言語法的描述

|描述|宣告規則語言語法|
|-----|-----| 
|預設的 AD FS 規則，以允許所有使用者存取。 Microsoft Office 365 識別平台信賴憑證者信任發行授權規則清單中，應該已有此規則。|=> issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");| 
|將此子句加入至新的自訂規則指定的要求是來自同盟伺服器 proxy （亦即，它有 x ms proxy 標頭）
所有規則都包含這個建議。|exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"])| 
|用來建立要求是從用戶端，以定義可接受的範圍中的 IP。|NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value=~"customer-provided public ip address regex"])| 
|這個子句用來指定是否正在存取的應用程式不是 Microsoft.Exchange.ActiveSync 應該會拒絕要求。|NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value=="Microsoft.Exchange.ActiveSync"])| 
|此規則可讓您判斷是否呼叫是透過網頁瀏覽器，並將不會遭到拒絕。|NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])| 
|此規則會指出只有特定的 Active Directory 群組 （根據 SID 值） 中的使用者應該被拒絕。 無法加入這個陳述式，表示一群使用者會被允許，不論位置為何。|exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "{Group SID value of allowed AD group}"])| 
|這是必要的子句，以符合所有先前的條件時，發出拒絕。|=> issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");|

### <a name="building-the-ip-address-range-expression"></a>建立 IP 位址範圍運算式

X-ms-轉送-用戶端-ip 宣告會填入目前設定只能由 Exchange Online，會將驗證要求傳遞至 AD FS 時填入標頭的 HTTP 標頭。 宣告的值可能是下列其中一項：

>[!Note] 
>Exchange Online 目前支援僅 IPV4，而非 IPV6 位址。

單一 IP 位址：用戶端 IP 位址直接連線到 Exchange Online

>[!Note] 
>公司網路上的用戶端的 IP 位址會顯示為組織的連出 proxy 或閘道的外部介面 IP 位址。

為內部的公司用戶端或外部的用戶端的 VPN 或 DA.組態而定，可能會出現或由 Microsoft DirectAccess (DA) VPN 連線到公司網路的用戶端

一或多個 IP 位址：當 Exchange Online 無法判斷連線的用戶端的 IP 位址時，它會根據設定值 x 轉送的標頭的值，非標準的標頭可以包含在 HTTP 為基礎的要求，並支援許多用戶端，負載平衡器與市場上的 proxy。

>[!Note]
>多個 IP 位址，表示用戶端 IP 位址並傳遞要求中，每個 proxy 的位址會以逗號分隔。

Exchange Online 的基礎結構相關的 IP 位址不會出現在清單中。


#### <a name="regular-expressions"></a>規則運算式

當您有相符的 IP 位址範圍時，而又需要建構規則運算式來執行比較。 在下一系列的步驟中，我們將提供如何建構這類運算式，以符合下列位址範圍 （請注意，您必須變更這些範例，以符合您的公用 IP 範圍） 的範例：


- 192.168.1.1 – 192.168.1.25
- 10.0.0.1 – 10.0.0.14

首先，會比對單一 IP 位址的基本模式如下所示： \b###\.###\.###\.# # # \b

擴充這種情況，我們可以比使用 OR 運算式的兩個不同 IP 位址，如下所示： \b###\.###\.###\.# # # \b|\b###\. ### \. ### \.# # # \b

因此，要比對兩個位址 （例如 192.168.1.1 或 10.0.0.1） 範例： \b192\.168\.1\.1\b | \b10\.0\.0\.1\b

這可讓您用中，您可以輸入任意數目的地址的技巧。 比對的位址範圍必須允許，例如 192.168.1.1-192.168.1.25，其中字元的字元必須以： \b192\.168\.1\.([1-9] | [0-9] 1 | 2 [0-5]) \b

>[!Note] 
>IP 位址會被視為字串並不是數字。


此規則細分，如下所示： \b192\.168\.1\.

這會比對任何以 192.168.1 值開頭的。

下列比對的最後一個小數點後面所需的位址部分的範圍：


- （[1-9] 相符項目地址結尾為 1-9
- | 1 [0-9] 比對結尾為 10-19 的位址
- 結尾為 20-25 |2[0-5]) 相符項目位址

>[!Note]
>括號必須正確地定位，以便您不要啟動比對 IP 位址的其他部分。

比對 192 區塊時，我們可以撰寫類似的運算式，針對 10 個區塊： \b10\.0\.0\.([1-9] | [0-4] 1) \b

並將它們放在一起，下列運算式應符合 「 192.168.1.1~25"和"10.0.0.1~14"的所有地址： \b192\.168\.1\.([1-9] | [0-9] 1 | 2 [0-5]) \b|\b10\.0\.0\.([1-9] | [0-4] 1) \b

#### <a name="testing-the-expression"></a>測試運算式

Regex 運算式可能會變得相當棘手，因此強烈建議使用 regex 驗證工具。 如果您在網際網路上搜尋 「 線上的 regex 運算式產生器 」，您會發現幾個良好的線上公用程式，可讓您試用您針對取樣資料的運算式。

測試運算式時，您必須了解要預期一定要相符的項目。 Exchange online 系統可能會傳送多個 IP 位址，並以逗號分隔。 在上面提供的運算式都適用於此。 不過，務必測試您的 regex 運算式時，想想這個問題。 比方說，其中可能會使用下列範例來驗證上述的範例輸入： 

192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20 

10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1 











## <a name="validating-the-deployment"></a>驗證部署

### <a name="security-audit-logs"></a>安全性稽核記錄檔

若要確認新的要求內容的宣告傳送，而且可供 AD FS 宣告處理管線，可讓 AD FS 伺服器上記錄的稽核。 接著傳送某些驗證要求並檢查宣告值在標準安全性稽核記錄項目。 

若要啟用的安全性事件記錄在 AD FS 伺服器的稽核記錄，請遵循在 AD FS 2.0 的稽核設定。

### <a name="event-logging"></a>事件記錄

根據預設，失敗的要求記錄到應用程式事件記錄檔位於 Applications and Services Logs \ AD FS 2.0 \ Admin.For 上的 AD FS 事件記錄的詳細資訊，請參閱[設定 AD FS 2.0 事件記錄](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx)。

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>設定追蹤記錄的詳細資訊的 AD FS

AD FS 追蹤事件會記錄到 AD FS 2.0 的偵錯記錄檔。 若要啟用追蹤時，請參閱[設定為 AD FS 2.0 偵錯追蹤](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx)。

已啟用追蹤之後，請使用下列命令列語法來啟用詳細資訊記錄層次： wevtutil.exe sl"AD FS 2.0 追蹤/偵錯 」 /l:5  

## <a name="related"></a>相關
如需新的宣告類型詳細資訊，請參閱[AD FS 宣告型別](AD-FS-Claims-Types.md)。
 
