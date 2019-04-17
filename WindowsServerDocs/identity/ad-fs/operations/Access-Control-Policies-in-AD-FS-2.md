---
ms.assetid: 
title: "在 Active Directory 同盟服務 2.0 client 存取控制原則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 00b9cd17c7a5c206e06bea12fd90762adb336715
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="client-access-control-policies-in-ad-fs-20"></a>AD FS 2.0 client 存取控制原則
在 Active Directory 同盟服務 2.0 client 存取原則可限制或權限授與的使用者資源。  本文件告訴您如何讓 AD FS 2.0 中的 client 存取原則，以及如何設定最常見的案例。

## <a name="enabling-client-access-policy-in-ad-fs-20"></a>讓 AD FS 2.0 Client 存取原則

若要讓 client 存取原則，請依照下列步驟。

### <a name="step-1-install-the-update-rollup-2-for-ad-fs-20-package-on-your-ad-fs-servers"></a>步驟 1：安裝 AD FS 伺服器上套件的 AD FS 2.0 的更新彙總套件 2

下載[適用於 Active Directory 同盟 Services (AD FS) 更新彙總套件 2 2.0](https://support.microsoft.com/en-us/help/2681584/description-of-update-rollup-2-for-active-directory-federation-services-ad-fs-2.0)套件，並安裝到所有伺服器聯盟和聯盟的 proxy 伺服器上。

### <a name="step-2-add-five-claim-rules-to-the-active-directory-claims-provider-trust"></a>步驟 2：新增五取得 Active Directory 宣告提供者信任規則

一旦上的所有 AD FS 伺服器和 proxy 已經都安裝更新彙總套件 2，使用下列程序新增一組宣告規則，讓原則引擎新型理賠要求。

若要這樣做，您將會新增五接受轉換規則每個使用下列程序新要求操作宣告類型。

在 Active Directory 宣告提供者信任，建立新接受轉換規則通過每個新的要求操作宣告類型。

#### <a name="to-add-a-claim-rule-to-the-active-directory-claims-provider-trust-for-each-of-the-five-context-claim-types"></a>Active directory 新增理賠要求規則宣告提供者信任的五個操作宣告類型：


1. 按一下 [開始] 畫面，指向 [程式集，指向 [系統管理工具]，然後按一下 AD FS 2.0 管理。
2. 主機樹，AD FS 2.0\Trust 關聯性，在按一下宣告提供者信任，Active Directory，以滑鼠右鍵按一下，然後按一下 [編輯理賠要求規則。
3. 在編輯理賠要求規則 \] 對話方塊中，選取接受轉換規則] 索引標籤，，然後按一下 [新增規則開始規則精靈。
4. 在選取 [規則範本頁面上，理賠要求規則範本，在傳遞透過選取或篩選從清單中，輸入宣告，然後按一下 [下一步]。
5. 在設定規則頁面上，理賠要求規則名稱底下輸入顯示名稱本規則。在連入宣告類型，輸入下列理賠要求輸入 URL，然後選取 Pass 透過所有理賠要求值。</br>
        `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`</br>
6. 若要確認規則，在清單中選取它編輯規則，再按一下 [檢視規則語言。 宣告規則語言應該會顯示如下： `c:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip"] => issue(claim = c);`
7. 按一下 [完成]。
8. 在 [編輯理賠要求規則 \] 對話方塊中，按一下 [確定儲存規則。
9. 重複步驟 2 到 6 建立其他理賠要求規則針對每個剩餘的四個宣告類型之前已建立五的所有規則如下所示。

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`


    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`

    `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`

### <a name="step-3-update-the-microsoft-office-365-identity-platform-relying-party-trust"></a>步驟 3：更新 Microsoft Office 365 的身分平台廠商信任只依賴

選擇其中一項下列設定理賠要求規則 Microsoft Office 365 的身分平台上可以廠商信任最符合您的組織的需求示範案例。

## <a name="client-access-policy-scenarios-for-ad-fs-20"></a>AD fs 2.0 client 存取原則案例
下列區段會描述存在 AD fs 2.0 案例

### <a name="scenario-1-block-all-external-access-to-office-365"></a>案例 1：封鎖所有外部存取 Office 365

此 client 存取原則案例可讓您存取所有內部戶端和封鎖所有的外部戶端根據外部 client 的 IP 位址。 預設的 \ [發行授權規則允許所有使用者的存取權是根據規則集。 您可以使用下列程序發行授權規則加入派對信任做為基礎的 Office 365。

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>若要建立封鎖所有外部存取 Office 365 規則



1. 按一下 [開始] 畫面，指向 [程式集，指向 [系統管理工具]，然後按一下 AD FS 2.0 管理。
2. 主機樹，AD FS 2.0\Trust 關聯性，在按一下可以信任派對、Microsoft Office 365 的身分平台信任，以滑鼠右鍵按一下，然後按一下編輯理賠要求規則。 
3. 在編輯理賠要求規則 \] 對話方塊中，選取 \ [發行授權規則] 索引標籤，，然後按一下 [新增規則開始理賠要求規則精靈。
4. 在下理賠要求規則範本，選取 [規則範本頁面上，選取 [傳送主張使用自訂規則，，然後按一下 [下一步]。
5. 在設定規則頁面上，在理賠要求規則名稱，輸入顯示名稱本規則。 自訂規則，在輸入或下列理賠要求規則語言語法貼上： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");` 
6. 按一下 [完成]。 請確認新規則會立即出現下方發行授權規則清單中的所有使用者規則允許存取。
7. 若要儲存規則，在 [編輯理賠要求規則 \] 對話方塊中，按一下 [確定]。

>[!NOTE]
>您必須更換上方的值為「公用 ip 位址 regex」的有效的 IP 運算式;查看建置 IP 位址範圍運算式如需詳細資訊。


### <a name="scenario-2-block-all-external-access-to-office-365-except-exchange-activesync"></a>案例 2：封鎖所有外部存取 Exchange ActiveSync 以外的 Office 365

下列範例可讓存取所有的 Office 365 應用程式，包括 Online 換貨，從內部包含 Outlook。 它會封鎖從位於以外的公司網路的存取（如同指示）client IP 位址，除了 Exchange ActiveSync 戶端，例如智慧型手機。 規則集的組建要介紹標題為 [允許所有使用者存取預設發行授權規則。 使用下列步驟來新增授權發行規則與可以使用理賠要求規則精靈廠商信任 Office 365:

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365"></a>若要建立封鎖所有外部存取 Office 365 規則



1. 按一下 [開始] 畫面，指向 [程式集，指向 [系統管理工具]，然後按一下 AD FS 2.0 管理。
2. 主機樹，AD FS 2.0\Trust 關聯性，在按一下可以信任派對、Microsoft Office 365 的身分平台信任，以滑鼠右鍵按一下，然後按一下編輯理賠要求規則。 
3. 在編輯理賠要求規則 \] 對話方塊中，選取 \ [發行授權規則] 索引標籤，，然後按一下 [新增規則開始理賠要求規則精靈。
4. 在下理賠要求規則範本，選取 [規則範本頁面上，選取 [傳送主張使用自訂規則，，然後按一下 [下一步]。
5. 在設定規則頁面上，在理賠要求規則名稱，輸入顯示名稱本規則。 自訂規則，在輸入或下列理賠要求規則語言語法貼上： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.Autodiscover"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application",
    Value=="Microsoft.Exchange.ActiveSync"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 按一下 [完成]。 請確認新規則會立即出現下方發行授權規則清單中的所有使用者規則允許存取。
7. 若要儲存規則，在 [編輯理賠要求規則 \] 對話方塊中，按一下 [確定]。

>[!NOTE]
>您必須更換上方的值為「公用 ip 位址 regex」的有效的 IP 運算式;查看建置 IP 位址範圍運算式如需詳細資訊。

### <a name="scenario-3-block-all-external-access-to-office-365-except-browser-based-applications"></a>案例 3：封鎖所有外部存取 Office 365 以外的瀏覽器為基礎的應用程式

規則集的組建要介紹標題為 [允許所有使用者存取預設發行授權規則。 加入 Microsoft Office 365 的身分平台可以使用理賠要求規則精靈廠商信任發行授權規則使用下列步驟：

>[!NOTE]
>本案例不支援和第三方 proxy 因為 client 被動式（Web 架構）要求存取原則標頭的限制。

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>若要建立封鎖所有外部存取 Office 365 以外的瀏覽器為基礎的應用程式規則



1. 按一下 [開始] 畫面，指向 [程式集，指向 [系統管理工具]，然後按一下 AD FS 2.0 管理。
2. 主機樹，AD FS 2.0\Trust 關聯性，在按一下可以信任派對、Microsoft Office 365 的身分平台信任，以滑鼠右鍵按一下，然後按一下編輯理賠要求規則。 
3. 在編輯理賠要求規則 \] 對話方塊中，選取 \ [發行授權規則] 索引標籤，，然後按一下 [新增規則開始理賠要求規則精靈。
4. 在下理賠要求規則範本，選取 [規則範本頁面上，選取 [傳送主張使用自訂規則，，然後按一下 [下一步]。
5. 在設定規則頁面上，在理賠要求規則名稱，輸入顯示名稱本規則。 自訂規則，在輸入或下列理賠要求規則語言語法貼上： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value == "/adfs/ls/"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 按一下 [完成]。 請確認新規則會立即出現下方發行授權規則清單中的所有使用者規則允許存取。
7. 若要儲存規則，在 [編輯理賠要求規則 \] 對話方塊中，按一下 [確定]。

### <a name="scenario-4-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>案例 4：封鎖所有外部存取 Office 365 指定 Active Directory 群組

下列範例可讓存取從內部根據 IP 位址。 它會封鎖存取從位於以外的公司網路有外部 client IP 位址，除了這些中指定的 Active Directory Group.The 規則的個人設定的組建要介紹標題為 [允許所有使用者存取預設發行授權規則。 加入 Microsoft Office 365 的身分平台可以使用理賠要求規則精靈廠商信任發行授權規則使用下列步驟：

#### <a name="to-create-a-rule-to-block-all-external-access-to-office-365-for-designated-active-directory-groups"></a>若要建立封鎖所有外部存取 Office 365 指定 Active Directory 群組規則



1. 按一下 [開始] 畫面，指向 [程式集，指向 [系統管理工具]，然後按一下 AD FS 2.0 管理。
2. 主機樹，AD FS 2.0\Trust 關聯性，在按一下可以信任派對、Microsoft Office 365 的身分平台信任，以滑鼠右鍵按一下，然後按一下編輯理賠要求規則。 
3. 在編輯理賠要求規則 \] 對話方塊中，選取 \ [發行授權規則] 索引標籤，，然後按一下 [新增規則開始理賠要求規則精靈。
4. 在下理賠要求規則範本，選取 [規則範本頁面上，選取 [傳送主張使用自訂規則，，然後按一下 [下一步]。
5. 在設定規則頁面上，在理賠要求規則名稱，輸入顯示名稱本規則。 自訂規則，在輸入或下列理賠要求規則語言語法貼上： `exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy"]) &&
    exists([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "Group SID value of allowed AD group"]) &&
    NOT exists([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip",
    Value=~"customer-provided public ip address regex"])
    => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "true");`
6. 按一下 [完成]。 請確認新規則會立即出現下方發行授權規則清單中的所有使用者規則允許存取。
7. 若要儲存規則，在 [編輯理賠要求規則 \] 對話方塊中，按一下 [確定]。


### <a name="descriptions-of-the-claim-rule-language-syntax-used-in-the-above-scenarios"></a>宣告規則語言語法上述案例中使用的描述

|描述|取得規則語言語法|
|-----|-----| 
|預設 AD FS 規則允許所有使用者的存取。 此規則應該已經存在於 Microsoft Office 365 的身分平台可以廠商信任發行授權規則清單。|= > 問題 (輸入 =」https://schemas.microsoft.com/authorization/claims/permit」，值 =」true」)。| 
|新增到新的自訂規則本節指定要求，已來自聯盟 proxy 伺服器（也就是具有 x-ms-proxy 標頭）
包含的所有規則這建議。|有 ([輸入 =」https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy」])| 
|用來建立要求從 client IP 定義可接受的範圍中使用。|不存在 ([輸入 =」https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip」，值 = ~」客戶提供公用 ip 位址 regex「])| 
|本節指定，如果應用程式存取不 Microsoft.Exchange.ActiveSync 要求應該無法使用。|不存在 ([輸入 =」https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application」，Value=="Microsoft.Exchange.ActiveSync」])| 
|此規則可讓您以判斷是否通話是透過網頁瀏覽器，並都會不無法。|不存在 ([類型 =」https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path」，值 =」日 adfs 日！日」])| 
|此規則會指出無法應該到特定的 Active Directory 群組（根據 SID 值）只使用者。 新增不到此聲明，表示群組中的使用者會允許，無論位置。|存在 ([輸入 =」https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid」，值 = ~」{群組 SID 值允許 AD 群組的}」])| 
|這是發行 deny 符合所有上述條件時所需的條款。|= > 問題 (輸入 =」https://schemas.microsoft.com/authorization/claims/deny」，值 =」true」)。|

### <a name="building-the-ip-address-range-expression"></a>建置 IP 位址範圍運算式

HTTP 標頭目前設定僅供換貨 Online 填入標頭，以 AD FS 傳遞驗證要求時，會填入 x ms-轉送-client-ip 理賠要求。 宣告值可能下列其中一個動作：

>[!Note] 
>換貨 Online 目前支援只 IPV4 與不 IPV6 位址。

單一 IP 位址：直接連接至換貨 Online client 的 IP 位址

>[!Note] 
>Client 公司網路上的 IP 位址會出現外部介面組織的輸出 proxy 或閘道 IP 位址。

為內部公司戶端或外部戶端 VPN 或 DA.的設定而定，可能會出現戶端的或由 Microsoft DirectAccess (DA) VPN 連接到企業網路

一或多個 IP 位址：時 Exchange Online 無法判斷連接 client 的 IP 位址，它將會設定 x 轉送的標頭，會納入 HTTP 為基礎的要求，支援許多戶端、負載平衡器，與市面上的 proxy 的非標準標頭的值為基礎的值。

>[!Note]
>將會以逗號分隔多個 IP 位址，指出 client IP 位址和傳遞要求，每個 proxy 的地址。

不有關 Exchange Online 基礎結構的 IP 位址會出現在清單中。


#### <a name="regular-expressions"></a>一般運算式

當您必須符合 IP 位址時，您需要建構運算式來進行比較。 在接下來的步驟，我們將會提供範例，以了解如何建立這類運算式符合下列地址範圍（請注意，您將會有變更成符合您公用 IP 範圍這些範例）：


- 192.168.1.1 – 192.168.1.25
- 10.0.0.1 – 10.0.0.14

第一次，符合單一 IP 位址的基本模式時，如下所示：\b###\.###\.###\.###\b

將它擴展這，我們可以符合兩個或運算式使用不同的 IP 位址，如下所示：\b###\.###\.###\.###\b|\b###\.###\.###\.###\b

因此，以符合兩個（例如 192.168.1.1 或 10.0.0.1）的地址就會：\b192\.168\.1\.1\b|\b10\.0\.0\.1\b

這會讓您可以輸入地址任何數字的技術。 當您需要有各種不同的地址允許，例如 192.168.1.1 – 192.168.1.25，符合必須完成字元依字元：\b192\.168\.1\。([1-9] | 1 [0 9] | 2 [0 5]) \b

>[!Note] 
>IP 位址會被視為字串，並不是數字。


規則作業切割，如下所示：\b192\.168\.1\。

這比對任何值 192.168.1 開頭。

下列符合最終小數點之後所需的地址的部分的範圍：


- （[1-9] 相符項目的地址結尾 1-9
- | 1 [0 9] 符合 10 至 19 結尾的地址
- 在 20-25 結束 |2[0-5]) 相符項目的地址

>[!Note]
>括弧必須正確位於，以便開始不符合其他部分的 IP 位址。

符合 192 區塊時，我們可以撰寫 10 封鎖類似運算式：\b10\.0\.0\。([1-9] | 1 [0 4]) \b

並將它們放在一起，為以下運算式應該符合」192.168.1.1~25」和「10.0.0.1~14」的所有地址：\b192\.168\.1\。([1-9]|1[0-9]|2[0-5])\b|\b10\.0\.0\。([1-9] | 1 [0 4]) \b

#### <a name="testing-the-expression"></a>測試運算式

Regex 運算式變成很難，因此建議使用 regex 驗證工具。 如果您在網際網路上的搜尋適用於「online regex 運算式」，您會發現幾個良好的 online 公用程式，可讓您可以嘗試範例資料對您運算式。

在測試運算式時，很重要，您知道必須符合的預期行為。 換貨 online 系統可能會傳送，以逗號分隔的許多 IP 位址。 這能運算式上面提供。 不過，請務必思考這測試 regex 運算式時。 例如，一可能會使用輸入驗證上述範例以下的範例： 

192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20 

10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1 











## <a name="validating-the-deployment"></a>驗證部署

### <a name="security-audit-logs"></a>安全性稽核登

若要確認宣告傳送且 AD FS 使用的新要求操作宣告處理管線，可讓稽核登入 AD FS 伺服器。 然後傳送標準安全性稽核登入的項目某些驗證要求並檢查有理賠要求值。 

要使用的安全性事件登入 AD FS 伺服器稽核登入，請遵循的步驟，AD FS 2.0 的稽核設定。

### <a name="event-logging"></a>事件登入

根據預設，登位於 [應用程式及服務登應用程式事件登入失敗的要求 \ AD FS 2.0 \ Admin.For 詳細資訊，AD fs 事件登入查看[設定 AD FS 2.0 事件登入](https://technet.microsoft.com/library/adfs2-troubleshooting-configuring-computers.aspx)。

### <a name="configuring-verbose-ad-fs-tracing-logs"></a>設定追蹤登的詳細資訊 AD FS

AD FS 追蹤事件登 AD FS 2.0 偵錯登入。 要描圖，請查看[設定為 AD FS 2.0 偵錯追蹤](https://technet.microsoft.com/en-us/library/adfs2-troubleshooting-configuring-computers.aspx)。

您有支援追蹤之後，使用下列命令列語法層級詳細資訊的登入以便：wevtutil.exe sl」AD FS 2.0 追蹤/偵錯「/l: 5  

## <a name="related"></a>相關
如需有關宣告新型查看[AD FS 宣告類型](AD-FS-Claims-Types.md)。
 
