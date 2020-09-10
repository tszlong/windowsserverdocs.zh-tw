---
title: 驗證原則和驗證原則定址接收器
description: Windows Server 安全性
ms.topic: article
ms.assetid: 7eb0e640-033d-49b5-ab44-3959395ad567
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 931ebeda8b865c16dc6f67ae765b6bc6f7aaed1f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621929"
---
# <a name="authentication-policies-and-authentication-policy-silos"></a>驗證原則和驗證原則定址接收器

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題適用於 IT 專業人員，說明驗證原則定址接收器以及限制這些定址接收器帳戶的原則。 它也會說明驗證原則如何用來限制帳戶的範圍。

驗證原則定址接收器和伴隨的原則提供一個方法，將高權限認證限制在只與選取的使用者、電腦或服務相關的系統。 定址接收器可在 Active Directory 網域服務 (AD DS) 中加以定義和管理，方法是透過 Active Directory 管理中心和 Active Directory Windows PowerShell Cmdlet。

驗證原則定址接收器是一種容器，系統管理員可以對其指派使用者帳戶、電腦帳戶和服務帳戶。 然後便可使用套用至該容器的驗證原則來管理帳戶的集合。 這可減少系統管理員追蹤個別帳戶對於資源的存取，並可協助防止惡意使用者透過竊取認證來存取其他資源。

Windows Server 2012 R2 中引進的功能，可讓您建立驗證原則定址接收器，以裝載一組高許可權使用者。 然後您可以指派這個容器的驗證原則，限制網域中使用權限帳戶的位置。 當帳戶屬於 Protected Users 安全性群組時，會套用其他控制項，例如獨佔使用 Kerberos 通訊協定。

您可以使用這些功能，限制高價值帳戶只能用於高價值主機。 例如，您可以建立新的樹系系統管理員定址接收器，其中包含企業、結構描述以及網域系統管理員。 之後您便可以使用驗證原則設定定址接收器，使得網域控制站及網域系統管理員主控台以外的系統，其密碼和智慧卡驗證都會失敗。

如需設定驗證原則定址接收器和驗證原則的相關資訊，請參閱[如何設定受保護的帳戶](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md)。

### <a name="about-authentication-policy-silos"></a>關於驗證原則定址接收器
驗證原則定址接收器控制受到定址接收器限制的帳戶，並定義要套用至成員的驗證原則。 您可以根據組織的需求建立定址接收器。 定址接收器是使用者、電腦及服務的 Active Directory 物件，由下表中結構描述所定義。

**驗證原則定址接收器的 Active Directory 結構描述**

|顯示名稱|描述|
|--------|--------|
|驗證原則定址接收器|此類別的執行個體定義指派的使用者、電腦及服務的驗證原則和相關的行為。|
|驗證原則定址接收器|此類別的容器可以包含驗證原則定址接收器物件。|
|強制執行驗證原則定址接收器|指定是否要強制執行驗證原則定址接收器。<p>不強制執行時，原則預設處於稽核模式。 會產生表示可能成功和失敗的事件，但是不會在系統套用保護。|
|指派的驗證原則定址接收器返回連結|這個屬性是 msDS-AssignedAuthNPolicySilo 的返回連結。|
|驗證原則定址接收器成員|指定要指派給 AuthNPolicySilo 的主體。|
|驗證原則定址接收器成員返回連結|這個屬性是 msDS-AuthNPolicySiloMembers 的返回連結。|

使用 Active Directory 管理主控台或 Windows PowerShell 可以設定驗證原則定址接收器。 如需詳細資訊，請參閱[如何設定受保護的帳戶](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md)。

### <a name="about-authentication-policies"></a>關於驗證原則
驗證原則定義帳戶類型的 Kerberos 通訊協定票證授與票證 (TGT) 的存留期屬性以及驗證存取控制條件。 這個原則根據稱為驗證原則定址接收器的 AD DS 容器來建置，而且它可以控制該容器。

驗證原則控制下列項目：

-   帳戶的 TGT 存留期，設定為不可更新。

-   裝置帳戶需要符合的條件，才能使用密碼或憑證登入。

-   使用者和裝置需要符合的條件，才能向屬於帳戶中部分執行的服務進行驗證。

Active Directory 帳戶類型會將呼叫端的角色判斷為下列其中一項：

-   **User**

    使用者應該一律是 Protected Users 安全性群組的成員，它預設會拒絕使用 NTLM 進行驗證的嘗試。

    您可以設定原則，將使用者帳戶的 TGT 存留期設定為較短的值，或限制使用者帳戶可以登入的裝置。 您可以在驗證原則中設定豐富的運算式，以控制使用者和其裝置必須符合才能向服務進行驗證的準則。

    如需詳細資訊，請參閱 [Protected Users 安全性群組](protected-users-security-group.md)。

-   **服務**

    將會使用獨立受管理的服務帳戶、群組受管理的服務帳戶，或衍生自這兩種服務帳戶類型的自訂帳戶物件。 原則可以設定裝置的存取控制條件，用來將受管理的服務帳戶認證限制為具有 Active Directory 身分識別的特定裝置。 服務一律不應該是 Protected Users 安全性群組的成員，因為所有連入驗證皆會失敗。

-   **電腦**

    將會使用電腦帳戶物件或衍生自電腦帳戶物件的自訂帳戶物件。 原則可以根據使用者和裝置屬性，設定允許驗證帳戶所需的存取控制條件。 電腦一律不應該是 Protected Users 安全性群組的成員，因為所有連入驗證皆會失敗。 預設會拒絕使用 NTLM 進行驗證的嘗試。 電腦帳戶不應該設定 TGT 存留期。

> [!NOTE]
> 可以設定一組帳戶的驗證原則，而不將原則與驗證原則定址接收器關聯。 當您需要保護單一帳戶時，可以使用此策略。

**驗證原則的 Active Directory 結構描述**

使用者、電腦及服務的 Active Directory 物件的原則是由下表中結構描述來定義。

|類型|顯示名稱|描述|
|----|--------|--------|
|原則|驗證原則|此類別的執行個體定義指派主體的驗證原則行為。|
|原則|驗證原則|此類別的容器可以包含驗證原則物件。|
|原則|強制執行驗證原則|指定是否要強制執行驗證原則。<p>未強制執行時，原則預設處於稽核模式，而且會產生表示可能成功和失敗的事件，但不會在系統套用保護。|
|原則|指派的驗證原則返回連結|這個屬性是 msDS-AssignedAuthNPolicy 的返回連結。|
|原則|指派的驗證原則|指定套用到這個主體的 AuthNPolicy。|
|User|使用者驗證原則|針對指派給這個定址接收器物件的使用者，指定要套用的 AuthNPolicy。|
|User|使用者驗證原則返回連結|這個屬性是 msDS-UserAuthNPolicy 的返回連結。|
|User|ms-DS-User-Allowed-To-Authenticate-To|這個屬性用來決定允許向使用者帳戶下執行之服務驗證的一組主體。|
|User|ms-DS-User-Allowed-To-Authenticate-From|這個屬性用來決定使用者帳戶具有登入權限的一組裝置。|
|User|使用者 TGT 存留期|指定發行給使用者的 Kerberos TGT 的最長使用期限 (以秒表示)。 結果 TGT 是不可更新。|
|電腦|電腦驗證原則|針對指派給這個定址接收器物件的電腦，指定要套用的 AuthNPolicy。|
|電腦|電腦驗證原則返回連結|這個屬性是 msDS-ComputerAuthNPolicy 的返回連結。|
|電腦|ms-DS-Computer-Allowed-To-Authenticate-To|這個屬性用來決定允許向電腦帳戶下執行之服務驗證的一組主體。|
|電腦|電腦 TGT 存留期|指定發行給電腦的 Kerberos TGT 的最長使用期限 (以秒表示)。 不建議變更這個設定。|
|服務|服務驗證原則|針對指派給這個定址接收器物件的服務，指定要套用的 AuthNPolicy。|
|服務|服務驗證原則返回連結|這個屬性是 msDS-ServiceAuthNPolicy 的返回連結。|
|服務|ms-DS-Service-Allowed-To-Authenticate-To|這個屬性用來決定允許向服務帳戶下執行之服務驗證的一組主體。|
|服務|ms-DS-Service-Allowed-To-Authenticate-From|這個屬性用來決定服務帳戶具有登入權限的一組裝置。|
|服務|服務 TGT 存留期|指定發行給服務的 Kerberos TGT 的最長使用期限 (以秒表示)。|

使用 [Active Directory 管理主控台] 或 Windows PowerShell，可以為每個定址接收器設定驗證原則。 如需詳細資訊，請參閱[如何設定受保護的帳戶](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md)。

## <a name="how-it-works"></a>運作方式
本節描述 Windows 中驗證原則定址接收器和驗證原則如何與 Protected Users 安全性群組和 Kerberos 通訊協定實作搭配運作。

-   [如何搭配使用 Kerberos 通訊協定與驗證定址接收器和原則](#BKMK_HowKerbUsed)

-   [限制使用者登入的運作方式](#BKMK_HowRestrictingSignOn)

-   [限制服務票證發行的運作方式](#BKMK_HowRestrictingServiceTicket)

**受保護的帳戶**

Protected Users 安全性群組會在執行 Windows Server 2012 R2 和 Windows 8.1 的裝置和主機電腦上，以及在主域控制站執行 Windows Server 2012 R2 的網域中的網域控制站上，觸發不可設定的保護。 根據帳戶的網域功能等級，Protected Users 安全性群組的成員會得到進一步保護，因為 Windows 支援的驗證方法有所變更。

-   Protected Users 安全性群組的成員無法使用 NTLM、摘要式驗證或 CredSSP 預設認證委派進行驗證。 在執行 Windows 8.1 且 (Ssp) 的任何安全性支援提供者的裝置上，當帳戶是 Protected Users 安全性群組的成員時，網域的驗證將會失敗。

-   Kerberos 通訊協定不會在預先驗證處理程序中使用較弱的 DES 或 RC4 加密類型。 這表示網域必須設定為至少支援 AES 加密類型。

-   無法使用 Kerberos 限制或不受限制的委派委派使用者的帳戶。 這表示如果使用者是 Protected Users 安全性群組的成員，則先前到其他系統的連線可能會失敗。

-   四個小時的預設 Kerberos TGT 存留期，可透過 Active Directory 管理中心使用驗證原則和定址接收器來加以設定。 這表示經過四小時之後，使用者就必須再驗證一次。

如需此安全性群組的詳細資訊，請參閱 [Protected Users 群組的運作方式](protected-users-security-group.md#BKMK_HowItWorks)。

**定址接收器和驗證原則**

驗證原則定址接收器和驗證原則利用現有的 Windows 驗證基礎結構。 拒絕使用 NTLM 通訊協定，並使用具有較新加密類型的 Kerberos 通訊協定。 驗證原則除了為服務和電腦的帳戶提供限制之外，還提供一種方法將可設定的限制套用到帳戶，藉此輔助 Protected Users 安全性群組。 Kerberos 通訊協定驗證服務 (AS) 或票證授與服務 (TGS) 交換期間會強制執行驗證原則。 如需關於 Windows 如何使用 Kerberos 通訊協定，以及支援驗證原則定址接收器和驗證原則所做變更的詳細資訊，請參閱：

-   [Kerberos 版本5驗證通訊協定的運作方式](/previous-versions/windows/it-pro/windows-server-2003/cc772815(v=ws.10))

-    (Windows Server 2008 R2 和 Windows 7) [的 Kerberos 驗證變更](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560670(v=ws.10))

### <a name="how-the-kerberos-protocol-is-used-with-authentication-policy-silos-and-policies"></a><a name="BKMK_HowKerbUsed"></a>如何搭配使用 Kerberos 通訊協定與驗證原則定址接收器和原則
當網域帳戶連結到驗證原則定址接收器且使用者登入時，安全性帳戶管理員會新增驗證原則定址接收器的宣告類型，其中將定址接收器做為值。 帳戶上的這個宣告提供存取目標定址接收器。

當強制執行驗證原則且網域控制站上收到網域帳戶的驗證服務要求時，網域控制站會傳回已設定存留期的不可更新 TGT (除非網域 TGT 存留時間較短)。

> [!NOTE]
> 網域帳戶必須有已設定的 TGT 存留期，而且必須直接連結到原則或間接透過定址接收器成員資格連結。

當驗證原則處於稽核模式，而且網域控制站上收到網域帳戶的驗證服務要求時，網域控制站會檢查裝置是否允許驗證，以便出現失敗時可以記錄警告。 稽核的驗證原則不會變更處理程序，所以如果驗證要求不符合原則的需求，它們也不會失敗。

> [!NOTE]
> 網域帳戶必須直接連結到原則或間接透過定址接收器成員資格連結。

當強制執行驗證原則且驗證服務受到防護，在網域控制站上收到網域帳戶的驗證服務要求時，網域控制站會檢查裝置是否允許驗證。 如果失敗，網域控制站會傳回錯誤訊息，並記錄事件。

> [!NOTE]
> 網域帳戶必須直接連結到原則或間接透過定址接收器成員資格連結。

當驗證原則處於稽核模式，且網域控制站的網域控制站收到票證授與服務要求時，網域控制站會根據要求的票證許可權屬性憑證 (PAC) 資料，檢查是否允許驗證，如果失敗，則會記錄警告訊息。 PAC 包含各種類型的授權資料，包括使用者所屬的群組、使用者具有的權限以及套用到使用者的原則。 這項資訊是用來產生使用者的存取權杖。 如果這是強制執行的驗證原則，允許對使用者、裝置或服務進行驗證，則網域控制站會根據要求的票證 PAC 資料，檢查是否允許驗證。 如果失敗，網域控制站會傳回錯誤訊息，並記錄事件。

> [!NOTE]
> 網域帳戶必須直接連結或透過定址器成員資格連結到稽核的驗證原則，允許驗證使用者、裝置或服務。

您可以為定址接收器的所有成員使用單一驗證原則，或是為使用者、電腦和受管理的服務帳戶使用個別原則。

使用 [Active Directory 管理主控台] 或 Windows PowerShell，可以為每個定址接收器設定驗證原則。 如需詳細資訊，請參閱[如何設定受保護的帳戶](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md)。

### <a name="how-restricting-a-user-sign-in-works"></a><a name="BKMK_HowRestrictingSignOn"></a>限制使用者登入的運作方式
因為這些驗證原則會被套用至帳戶，所以也會套用到服務使用的帳戶。 如果您想要對特定主機限制使用服務密碼，此設定非常有用。 例如，在允許主機從 Active Directory 網域服務擷取密碼之處，可設定群組受管理的服務帳戶。 不過，可以從所有主機使用該密碼於初始驗證。 藉由套用存取控制條件，可將密碼限制在可擷取密碼的一組主機，而建立額外的一層保護。

當以 system、network service 或其他本機服務身分識別執行的服務連接到網路服務時，它們會使用主機的電腦帳戶。 不能限制電腦帳戶。 因此即使服務正在使用不是 Windows 主機的電腦帳戶，也不能限制它。

限制使用者登入特定主機需要網域控制站驗證主機的身分識別。 使用 Kerberos 驗證搭配 Kerberos 防護 (這是動態存取控制的一部分) 時，會為金鑰發佈中心提供驗證使用者的主機 TGT。 這個防護的 TGT 內容可用來完成存取檢查，以判斷是否允許主機。

當使用者登入 Windows 或在應用程式的認證提示中輸入其網域認證時，Windows 預設會將未防護的 AS-REQ 傳送給網域控制站。 如果使用者從不支援防護的電腦傳送要求，例如執行 Windows 7 或 Windows Vista 的電腦，則要求會失敗。

下列清單描述該程序：

-   執行 Windows Server 2012 R2 之網域中的網域控制站會查詢使用者帳戶，並判斷它是否設定了限制需要防護要求的初始驗證的驗證原則。

-   網域控制站會拒絕要求。

-   因為需要防護，所以使用者可以使用執行 Windows 8.1 或 Windows 8 的電腦嘗試登入，此電腦已啟用支援 Kerberos 防護以重試登入程式。

-   Windows 偵測到網域支援 Kerberos 防護，並傳送防護的 AS-REQ 來重試登入要求。

-   網域控制站會在用來盔甲要求的 TGT 中使用設定的存取控制條件和用戶端作業系統的身分識別資訊，以執行存取檢查。

-   如果存取檢查失敗，網域控制站會拒絕要求。

即使作業系統支援 Kerberos 防護，也可以套用存取控制需求，而且授與存取權之前必須符合這些需求。 使用者登入 Windows，或在應用程式的認證提示中輸入其網域認證。 根據預設，Windows 會傳送未防護的 AS-REQ 給網域控制站。 如果使用者從支援防護的電腦傳送要求，例如 Windows 8.1 或 Windows 8，則會評估驗證原則，如下所示：

1.  執行 Windows Server 2012 R2 之網域中的網域控制站會查詢使用者帳戶，並判斷它是否設定了限制需要防護要求的初始驗證的驗證原則。

2.  網域控制站會在用來盔甲要求的 TGT 中使用設定的存取控制條件和系統的身分識別資訊，以執行存取檢查。 存取檢查成功。

    > [!NOTE]
    > 如果設定了傳統的工作群組限制，則也必須符合這些限制。

3.  網域控制站使用防護回覆 (AS-REP) 來回覆，然後繼續驗證。

### <a name="how-restricting-service-ticket-issuance-works"></a><a name="BKMK_HowRestrictingServiceTicket"></a>限制服務票證發行的運作方式
如果不允許帳戶，且具有 TGT 的使用者嘗試連線到服務 (例如藉由開啟需要驗證服務的應用程式，而該服務是由服務的服務主體名稱 (SPN) 所識別，則會發生下列順序：

1.  嘗試從 SPN 連線到 SPN1 時，Windows 會傳送一個 TGS-REQ 給要求 SPN1 服務票證的網域控制站。

2.  執行 Windows Server 2012 R2 之網域中的網域控制站會查詢查詢 SPN1，以找出服務的 Active Directory Domain Services 帳戶，並判斷該帳戶是否使用限制服務票證發行的驗證原則進行設定。

3.  網域控制站會使用設定的存取控制條件，以及 TGT 中的使用者身分識別資訊來執行存取檢查。 存取檢查失敗。

4.  網域控制站會拒絕要求。

因為帳戶符合驗證原則所設定的存取控制條件，且具有 TGT 的使用者嘗試連線到服務 (例如藉由開啟需要驗證服務的應用程式（由服務的 SPN) 所識別），所以允許使用帳戶，會發生下列順序：

1.  嘗試連線到 SPN1 時，Windows 會傳送一個 TGS-REQ 給要求 SPN1 服務票證的網域控制站。

2.  執行 Windows Server 2012 R2 之網域中的網域控制站會查詢查詢 SPN1，以找出服務的 Active Directory Domain Services 帳戶，並判斷該帳戶是否使用限制服務票證發行的驗證原則進行設定。

3.  網域控制站會使用設定的存取控制條件，以及 TGT 中的使用者身分識別資訊來執行存取檢查。 存取檢查成功。

4.  網域控制站使用票證授與服務回覆 (TGS-REP) 回覆要求。

## <a name="associated-error-and-informational-event-messages"></a><a name="BKMK_ErrorandEvents"></a>相關的錯誤和資訊事件訊息
下表描述與 Protected Users 安全性群組相關的事件，以及套用到驗證原則定址接收器的驗證原則。

事件會記錄在 **Microsoft\Windows\Authentication** 的應用程式及服務記錄檔中。

如需使用這些事件的疑難排解步驟，請參閱[疑難排解驗證原則](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md#BKMK_CreateAuthNPolicies)和[疑難排解與 Protected Users 相關的事件](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md#BKMK_TrubleshootingEvents)。

|事件識別碼和記錄檔|描述|
|----------|--------|
|101<p>**AuthenticationPolicyFailures-DomainController**|原因：因為已設定驗證原則，所以發生 NTLM 登入失敗。<p>事件會記錄在網域控制站，指出 NTLM 驗證失敗，因為需要存取控制限制，而且這些限制不能套用到 NTLM。<p>顯示帳戶、裝置、原則及定址接收器的名稱。|
|105<p>**AuthenticationPolicyFailures-DomainController**|原因：因為不允許來自特定裝置的驗證，所以發生 Kerberos 限制失敗。<p>事件會記錄在網域控制站，指出 Kerberos TGT 被拒絕，因為裝置不符合強制執行的存取控制限制。<p>顯示帳戶、裝置、原則、定址接收器的名稱以及 TGT 存留期。|
|305<p>**AuthenticationPolicyFailures-DomainController**|原因：可能因為不允許來自特定裝置的驗證，所以可能發生 Kerberos 限制失敗。<p>在稽核模式中，資訊事件會記錄在網域控制站，以判斷是否因為裝置不符合存取控制限制而拒絕 Kerberos TGT。<p>顯示帳戶、裝置、原則、定址接收器的名稱以及 TGT 存留期。|
|106<p>**AuthenticationPolicyFailures-DomainController**|原因：因為不允許使用者或裝置向伺服器驗證，所以發生 Kerberos 限制失敗。<p>事件會記錄在網域控制站，指出 Kerberos TGT 被拒絕，因為使用者、裝置或兩者不符合強制執行的存取控制限制。<p>顯示裝置、原則及定址接收器的名稱。|
|306<p>**AuthenticationPolicyFailures-DomainController**|原因：因為不允許使用者或裝置向伺服器驗證，所以可能發生 Kerberos 限制失敗。<p>在稽核模式中，資訊事件會記錄在網域控制站，指出 Kerberos 服務票證將被拒絕，因為使用者、裝置或兩者不符合存取控制限制。<p>顯示裝置、原則及定址接收器的名稱。|

## <a name="additional-references"></a>其他參考資料
[如何設定受保護的帳戶](../../identity/ad-ds/manage/how-to-configure-protected-accounts.md)

[認證保護和管理](credentials-protection-and-management.md)

[Protected Users 安全性群組](protected-users-security-group.md)