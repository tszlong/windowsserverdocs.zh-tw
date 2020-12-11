---
description: 深入瞭解： AD FS 2.0 中的用戶端存取控制原則
title: Active Directory 同盟服務2.0 中的用戶端存取控制原則
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: b4402b2e8186f723c2c2a46ad10616f8986e21e5
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044346"
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>AD FS 2.0 中的用戶端存取控制原則
Active Directory 同盟服務2.0 中的用戶端存取原則可讓您限制或授與使用者對資源的存取權。  本檔說明如何在 AD FS 2.0 中啟用用戶端存取原則，以及如何設定最常見的案例。

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>在 AD FS 2.0 中啟用用戶端存取原則

若要啟用用戶端存取原則，請遵循下列步驟。

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>步驟1：在您的 AD FS 伺服器上安裝 AD FS 2.0 套件的更新彙總套件2

下載 [Active Directory 同盟服務 (AD FS) 2.0 套件的更新彙總套件 2](https://support.microsoft.com/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0) ，並將其安裝在所有同盟伺服器和同盟伺服器 proxy 上。

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>步驟2：將五個宣告規則新增至 Active Directory 宣告提供者信任

在所有的 AD FS 伺服器和 proxy 上安裝更新彙總套件2之後，請使用下列程式新增一組宣告規則，讓原則引擎可以使用新的宣告類型。

若要這樣做，您將使用下列程式，為每個新的要求內容宣告類型新增五個接受轉換規則。

在 Active Directory 宣告提供者信任上，建立新的驗收轉換規則，以通過每一個新的要求內容宣告類型。

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>若要將宣告規則新增至五個內容宣告類型的 Active Directory 宣告提供者信任：


1. 按一下 [開始]，指向 [程式]，指向 [系統管理工具]，然後按一下 [AD FS 2.0 管理]。
2. 在主控台樹的 [AD FS 2.0 \ 信任關係] 下，按一下 [宣告提供者信任]，以滑鼠右鍵按一下 Active Directory，然後按一下 [編輯宣告規則]。
3. 在 [編輯宣告規則] 對話方塊中，選取 [接受轉換規則] 索引標籤，然後按一下 [新增規則] 以啟動規則嚮導。
4. 在 [選取規則範本] 頁面的 [宣告規則範本] 下，從清單中選取 [傳遞或篩選傳入宣告]，然後按 [下一步]。
5. 在 [設定規則] 頁面的 [宣告規則名稱] 底下，輸入此規則的顯示名稱。在 [傳入宣告類型] 中，輸入下列宣告類型 URL，然後選取 [傳遞所有宣告值]。</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. 若要驗證規則，請在清單中選取該規則，然後按一下 [編輯規則]，再按一下 [視圖規則語言]。 宣告規則語言應該會如下所示： `c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. 按一下 [完成]。
8. 在 [編輯宣告規則] 對話方塊中，按一下 [確定] 以儲存規則。
9. 重複步驟2到6，針對下列每個剩餘的四個宣告類型建立額外的宣告規則，直到建立所有五個規則為止。

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


~~~
`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

`https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`
~~~

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>步驟3：更新 Microsoft Office 365 身分識別平臺信賴憑證者信任

選擇下列其中一個範例案例，以在最符合您組織需求的 Microsoft Office 365 身分識別平臺信賴憑證者信任上設定宣告規則。

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>AD FS 2.0 的用戶端存取原則案例
下列各節將說明 AD FS 2.0 存在的案例

### <a name="scenario-1-block-all-external-access-to-office-365"></a>案例1：封鎖所有對 Office 365 的外部存取

此用戶端存取原則案例允許從所有內部用戶端存取，並根據外部用戶端的 IP 位址封鎖所有外部用戶端。 規則集是以預設的發行授權規則為基礎，允許所有使用者的存取權。 您可以使用下列程式，將發行授權規則新增至 Office 365 信賴憑證者信任。

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>若要建立規則以封鎖所有對 Office 365 的外部存取



1. 按一下 [開始]，指向 [程式]，指向 [系統管理工具]，然後按一下 [AD FS 2.0 管理]。
2. 在主控台樹的 [AD FS 2.0 \ 信任關係] 下，按一下 [信賴憑證者信任]，以滑鼠右鍵按一下 [Microsoft Office 365 身分識別平臺信任]，然後按一下 [編輯宣告規則]。
3. 在 [編輯宣告規則] 對話方塊中，選取 [發佈授權規則] 索引標籤，然後按一下 [新增規則] 以啟動 [宣告規則]。
4. 在 [選取規則範本] 頁面的 [宣告規則範本] 下，選取 [使用自訂規則傳送宣告]，然後按 [下一步]。
5. 在 [設定規則] 頁面的 [宣告規則名稱] 底下，輸入此規則的顯示名稱。 在 [自訂規則] 下，輸入或貼上下列宣告規則語言語法： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 按一下 [完成]。 確認新的規則會出現在 [發佈授權規則] 清單中的 [允許存取所有使用者] 規則下方。
7. 若要儲存規則，請在 [編輯宣告規則] 對話方塊中，按一下 [確定]。

>[!NOTE]
>您必須使用有效的 IP 運算式來取代上述的「公用 ip 位址 RegEx」值;如需詳細資訊，請參閱建立 IP 位址範圍運算式。


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>案例2：封鎖 Exchange ActiveSync 以外的所有 Office 365 外部存取

下列範例可讓您從內部用戶端（包括 Outlook）存取所有的 Office 365 應用程式（包括 Exchange Online）。 它會封鎖來自公司網路外部用戶端的存取，如用戶端 IP 位址所示，但 Exchange ActiveSync 用戶端（例如智慧型手機）除外。 規則集會以預設的發佈授權規則為基礎，其標題為允許所有使用者的存取權。 使用下列步驟，使用 [宣告規則] Wizard 將發佈授權規則新增至 Office 365 信賴憑證者信任：

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>若要建立規則以封鎖所有對 Office 365 的外部存取



1. 按一下 [開始]，指向 [程式]，指向 [系統管理工具]，然後按一下 [AD FS 2.0 管理]。
2. 在主控台樹的 [AD FS 2.0 \ 信任關係] 下，按一下 [信賴憑證者信任]，以滑鼠右鍵按一下 [Microsoft Office 365 身分識別平臺信任]，然後按一下 [編輯宣告規則]。
3. 在 [編輯宣告規則] 對話方塊中，選取 [發佈授權規則] 索引標籤，然後按一下 [新增規則] 以啟動 [宣告規則]。
4. 在 [選取規則範本] 頁面的 [宣告規則範本] 下，選取 [使用自訂規則傳送宣告]，然後按 [下一步]。
5. 在 [設定規則] 頁面的 [宣告規則名稱] 底下，輸入此規則的顯示名稱。 在 [自訂規則] 下，輸入或貼上下列宣告規則語言語法： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.Autodiscover"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.ActiveSync"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 按一下 [完成]。 確認新的規則會出現在 [發佈授權規則] 清單中的 [允許存取所有使用者] 規則下方。
7. 若要儲存規則，請在 [編輯宣告規則] 對話方塊中，按一下 [確定]。

>[!NOTE]
>您必須使用有效的 IP 運算式來取代上述的「公用 ip 位址 RegEx」值;如需詳細資訊，請參閱建立 IP 位址範圍運算式。

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>案例3：除了瀏覽器架構的應用程式以外，封鎖所有對 Office 365 的外部存取

規則集會以預設的發佈授權規則為基礎，其標題為允許所有使用者的存取權。 使用下列步驟，使用 [宣告規則] Wizard 將發佈授權規則新增至 Microsoft Office 365 身分識別平臺信賴憑證者信任：

>[!NOTE]
>協力廠商 proxy 不支援此案例，因為具有被動 (Web) 要求的用戶端存取原則標頭有所限制。

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>若要建立規則以封鎖對 Office 365 的所有外部存取，但瀏覽器應用程式除外



1. 按一下 [開始]，指向 [程式]，指向 [系統管理工具]，然後按一下 [AD FS 2.0 管理]。
2. 在主控台樹的 [AD FS 2.0 \ 信任關係] 下，按一下 [信賴憑證者信任]，以滑鼠右鍵按一下 [Microsoft Office 365 身分識別平臺信任]，然後按一下 [編輯宣告規則]。
3. 在 [編輯宣告規則] 對話方塊中，選取 [發佈授權規則] 索引標籤，然後按一下 [新增規則] 以啟動 [宣告規則]。
4. 在 [選取規則範本] 頁面的 [宣告規則範本] 下，選取 [使用自訂規則傳送宣告]，然後按 [下一步]。
5. 在 [設定規則] 頁面的 [宣告規則名稱] 底下，輸入此規則的顯示名稱。 在 [自訂規則] 下，輸入或貼上下列宣告規則語言語法： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 按一下 [完成]。 確認新的規則會出現在 [發佈授權規則] 清單中的 [允許存取所有使用者] 規則下方。
7. 若要儲存規則，請在 [編輯宣告規則] 對話方塊中，按一下 [確定]。

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>案例4：封鎖對指定 Active Directory 群組之 Office 365 的所有外部存取

下列範例會根據 IP 位址啟用內部用戶端的存取。 它會封鎖來自公司網路外部用戶端 IP 位址以外之用戶端的存取權，但指定的 Active Directory 群組中的人員除外。此規則集會以預設的發佈授權規則為基礎，其標題為允許所有使用者的存取權。 使用下列步驟，使用 [宣告規則] Wizard 將發佈授權規則新增至 Microsoft Office 365 身分識別平臺信賴憑證者信任：

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>若要建立規則以封鎖對指定 Active Directory 群組之 Office 365 的所有外部存取



1. 按一下 [開始]，指向 [程式]，指向 [系統管理工具]，然後按一下 [AD FS 2.0 管理]。
2. 在主控台樹的 [AD FS 2.0 \ 信任關係] 下，按一下 [信賴憑證者信任]，以滑鼠右鍵按一下 [Microsoft Office 365 身分識別平臺信任]，然後按一下 [編輯宣告規則]。
3. 在 [編輯宣告規則] 對話方塊中，選取 [發佈授權規則] 索引標籤，然後按一下 [新增規則] 以啟動 [宣告規則]。
4. 在 [選取規則範本] 頁面的 [宣告規則範本] 下，選取 [使用自訂規則傳送宣告]，然後按 [下一步]。
5. 在 [設定規則] 頁面的 [宣告規則名稱] 底下，輸入此規則的顯示名稱。 在 [自訂規則] 下，輸入或貼上下列宣告規則語言語法： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 按一下 [完成]。 確認新的規則會出現在 [發佈授權規則] 清單中的 [允許存取所有使用者] 規則下方。
7. 若要儲存規則，請在 [編輯宣告規則] 對話方塊中，按一下 [確定]。


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>上述案例中所使用之宣告規則語言語法的描述

|                                                                                                   描述                                                                                                   |                                                                     宣告規則語言語法                                                                     |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              預設 AD FS 規則，以允許存取所有使用者。 這項規則應該已經存在於 Microsoft Office 365 身分識別平臺信賴憑證者信任發佈授權規則清單中。              |                                  => 問題 (Type = " <https://schemas.microsoft.com/authorization/claims/permit> "，Value = "true" ) ;                                   |
|                               將這個子句新增至新的自訂規則，會指定要求是來自同盟伺服器 proxy (也就是具有 x-ms proxy 標頭)                                 |                                                                                                                                                                    |
|                                                                                 建議所有規則都包含這項功能。                                                                                  |                                    存在 ( [Type = = " <https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy> "] )                                     |
|                                                         用來建立要求是來自用戶端，而該用戶端的 IP 位於定義的可接受範圍內。                                                         | 不存在 ( [Type = = " <https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip> "，Value = ~ "客戶提供的公用 ip 位址 RegEx"] )  |
|                                    這個子句是用來指定如果存取的應用程式不是由 Microsoft. Exchange，則應該拒絕要求。                                     |       不存在 ( [Type = = " <https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application> "，Value = = "Microsoft. ActiveSync"] )         |
|                                                      此規則可讓您判斷呼叫是透過網頁瀏覽器，而且不會被拒絕。                                                      |              不存在 ( [Type = = " <https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path> "，Value = = "/adfs/ls/"] )                |
| 這項規則指出特定 Active Directory 群組中 (的唯一使用者，應該被拒絕) 的 SID 值。 若不使用此語句，則會允許使用者群組，不論位置為何。 |             存在 ( [Type = = " <https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid> "，Value = ~ "{GROUP SID Value of 允許 AD Group}"] )               |
|                                                                這是在符合上述所有條件時發出拒絕的必要子句。                                                                 |                                   => 問題 (Type = " <https://schemas.microsoft.com/authorization/claims/deny> "，Value = "true" ) ;                                    |

### <a name="building-the-ip-address-range-expression"></a>建立 IP 位址範圍運算式

從目前僅由 Exchange Online 設定的 HTTP 標頭會填入 x-ms 轉送的用戶端 ip 宣告，而此標頭會在將驗證要求傳遞至 AD FS 時填入標頭。 宣告的值可能是下列其中一項：

>[!Note]
>Exchange Online 目前僅支援 IPV4，不支援 IPV6 位址。

單一 IP 位址：直接連線至 Exchange Online 之用戶端的 IP 位址

>[!Note]
>公司網路上用戶端的 IP 位址會顯示為組織輸出 proxy 或閘道的外部介面 IP 位址。

由 VPN 或 Microsoft DirectAccess (DA) 連線到公司網路的用戶端，可能會顯示為內部公司用戶端，或根據 VPN 或 DA 的設定，以外部用戶端的形式出現。

一或多個 IP 位址：當 Exchange Online 無法判斷連線用戶端的 IP 位址時，它會根據 x 轉寄的標頭值（可包含在 HTTP 型要求中的非標準標頭）來設定值，而且市場上的許多用戶端、負載平衡器和 proxy 都支援此標頭。

>[!Note]
>多個 IP 位址，指出傳遞要求的每個 proxy 的用戶端 IP 位址和位址，將會以逗號分隔。

與 Exchange Online 基礎結構相關的 IP 位址不會出現在清單中。


#### <a name="regular-expressions"></a>[規則運算式]

當您必須符合某個範圍的 IP 位址時，就必須建立正則運算式來執行比較。 在接下來的步驟中，我們將提供如何建立這類運算式以符合下列位址範圍的範例 (請注意，您必須變更這些範例，以符合您的公用 IP 範圍) ：


- 192.168.1.1 –192.168.1.25
- 10.0.0.1 –10.0.0.14

首先，將符合單一 IP 位址的基本模式如下： \b # # # # # \. ### \. ### \. # \b

擴充此項，我們可以使用或運算式來比對兩個不同的 IP 位址，如下所示： \b # # # # # # # \. ### \. ### \. \b | \b # # # # # \. ### \. ### \. # \b

因此，只符合兩個位址 (例如192.168.1.1 或 10.0.0.1) 的範例是： \b192 \. 168 \. 1 \. 1 \ b | \b10 \. 0 \. 0 \. 1 \ b

這提供您可以輸入任意數目之位址的技巧。 在需要允許的位址範圍（例如192.168.1.1 –192.168.1.25）中，比對必須依字元完成字元： \b192 \. 168 \. 1 \. ( [1-9] | 1 [0-9] | 2 [0-5] ) \b

>[!Note]
>IP 位址會視為字串，而不是數位。


此規則會細分如下： \b192 \. 168 \. 1\.

這會比對開頭為 192.168.1. 的任何值。

下列專案符合最後一個小數點之後的位址部分所需的範圍：


-  ( [1-9] 符合以1-9 結尾的位址
- | 1 [0-9] 符合以10-19 結尾的位址
- | 2 [0-5] ) 符合以20-25 結尾的位址

>[!Note]
>括弧必須正確放置，如此您才不會開始比對 IP 位址的其他部分。

在符合192區塊的情況下，我們可以為10個區塊撰寫類似的運算式： \b10 \. 0 \. 0 \. ( [1-9] | 1 [0-4] ) \b

將它們組合在一起之後，下列運算式應符合 "192.168.1.1 ~ 25" 和 "10.0.0.1 ~ 14" 的所有位址： \b192 \. 168 \. 1 \. ( [1-9] | 1 [0-9] | 2 [0-5] ) \b | \b10 \. 0 \. 0 \. ( [1-9] | 1 [0-4] ) \b

#### <a name="testing-the-expression"></a>測試運算式

Regex 運算式可能會變得相當複雜，因此強烈建議使用 RegEx 驗證工具。 如果您在網際網路上搜尋「線上 RegEx 運算式產生器」，您會發現幾個好用的線上公用程式，可讓您針對範例資料試用您的運算式。

測試運算式時，請務必瞭解預期需要符合的事項。 Exchange online 系統可能會傳送許多 IP 位址，並以逗號分隔。 以上提供的運算式可用於此作業。 不過，在測試 RegEx 運算式時，請務必考慮這一點。 例如，您可以使用下列範例輸入來確認上述範例：

192.168.1.1、192.168.1.2、192.169.1.1。 192.168.12.1、192.168.1.10、192.168.1.25、192.168.1.26、192.168.1.30、1192.168.1.20

10.0.0.1、10.0.0.5、10.0.0.10、10.0.1.0、10.0.1.1、110.0.0.1、10.0.0.14、10.0.0.15、10.0.0.10、10、0.0。1











## <a name="validating-the-deployment"></a>驗證部署

### <a name="security-audit-logs"></a>安全性審核記錄

若要確認是否正在傳送新的要求內容宣告，並且可供 AD FS 的宣告處理管線使用，請在 AD FS 伺服器上啟用 audit 記錄。 然後傳送一些驗證要求，並檢查標準安全性審核記錄專案中的宣告值。

若要在 AD FS 伺服器上啟用安全性記錄檔的審核事件記錄，請依照設定 AD FS 2.0 的審核步驟。

### <a name="event-logging"></a>事件記錄

根據預設，失敗的要求會記錄到位於 [應用程式及服務記錄檔 \ AD FS 2.0 \ 管理員] 下的應用程式事件記錄檔。如需 AD FS 事件記錄的詳細資訊，請參閱 [設定 AD FS 2.0 事件記錄](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff641696(v=ws.10))。

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>設定詳細資訊 AD FS 追蹤記錄

AD FS 的追蹤事件會記錄到 AD FS 2.0 debug 記錄檔中。 若要啟用追蹤，請參閱 [設定 AD FS 2.0 的偵錯工具追蹤](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff641696(v=ws.10))。

啟用追蹤之後，請使用下列命令列語法來啟用詳細資訊記錄層級： wevtutil.exe sl "AD FS 2.0 追蹤/Debug"/l：5

## <a name="related"></a>相關參考
如需新宣告類型的詳細資訊，請參閱 [AD FS 宣告類型](AD-FS-Claims-Types.md)。
