---
title: "驗證原則和驗證原則筒倉"
description: "Windows Server 安全性"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7eb0e640-033d-49b5-ab44-3959395ad567
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 460837c79c0e0d2c48331ddaaffcd118fd16ebc1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="authentication-policies-and-authentication-policy-silos"></a>驗證原則和驗證原則筒倉

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題適用於 IT 專業人員描述驗證原則筒倉和只這些筒倉帳號的原則。 它也會解釋限制帳號的範圍如何使用驗證原則。

驗證原則筒倉和隨附原則提供包含高權限認證系統只所選的使用者、 電腦，或服務相關的方式。 筒倉可以定義，並使用 Active Directory 管理中心和 Active Directory Windows PowerShell cmdlet 管理在 Active Directory Domain Services (AD DS)。

驗證原則筒倉是的容器的系統管理員可以指定帳號，電腦帳號，並帳號服務。 帳號的設定然後受驗證原則已經套用至該容器。 這降低需要系統管理員存取資源個人帳號，並可防止惡意使用者透過認證竊取存取其他資源。

在 Windows Server 2012 R2 推出的功能可讓您建立驗證原則筒倉，裝載高權限使用者的設定。 然後您可以指定容器此驗證原則特殊權限的帳號，可用於網域限制。 當帳號保護使用者安全性群組中，其他控制項會套用，例如專屬使用 Kerberos 通訊協定。

這些功能，您可以限制高價值的主機的高價值 account 使用量。 例如，您可以建立新的樹系的系統管理員筒倉包含企業版、 架構，以及網域系統管理員。 然後，讓密碼，以及從系統網域控制站和網域系統管理員主機以外的智慧卡驗證會失敗，您可能會設定筒倉驗證原則的。

適用於驗證原則筒倉和驗證原則設定的相關資訊，請查看[設定保護帳號如何](how-to-configure-protected-accounts.md)。

### <a name="about-authentication-policy-silos"></a>關於驗證原則筒倉
驗證原則筒倉控制哪些帳號會受到筒倉和定義驗證原則套用到的成員。 您可以建立筒倉根據您的組織的需求。 筒倉的 Active Directory 物件的使用者、 電腦及服務在下表中的架構所定義。

**Active Directory 架構的驗證原則筒倉**

|顯示名稱|描述|
|--------|--------|
|驗證原則筒倉|這個課程的執行個體定義驗證原則和相關的行為，適用於已指派的使用者、 電腦及服務。|
|驗證原則筒倉|本等級容器可包含驗證原則筒倉物件。|
|驗證原則筒倉執行|指定驗證原則筒倉是否執行。<br /><br />不執行，預設的原則時稽核模式。 專活動，表示可能成功和失敗，但保護不會套用至系統。|
|指派的驗證原則筒倉 Backlink|這是屬性返回 msDS-AssignedAuthNPolicySilo 的連結。|
|驗證原則筒倉成員|指定 AuthNPolicySilo 来指派的原則。|
|驗證原則筒倉成員 Backlink|這是屬性返回 msDS-AuthNPolicySiloMembers 的連結。|

驗證原則筒倉可以使用的 Active Directory 系統管理主控台 」 或 「 Windows PowerShell 來設定。 如需詳細資訊，請查看[設定保護帳號如何](how-to-configure-protected-accounts.md)。

### <a name="about-authentication-policies"></a>關於驗證原則
驗證原則定義 Kerberos 通訊協定票證授與票證 (TGT) 期間屬性和驗證存取控制項條件帳號類型。 原則建置，以及控制稱為 [驗證原則筒倉 AD DS 容器。

驗證原則控制下列動作：

-   適用於帳號，它會設定為非儲值 TGT 期間。

-   帳號裝置需要使用密碼或憑證登入符合的條件。

-   使用者和裝置需要驗證服務執行的一部分 account 符合的條件。

Active Directory account 類型判斷播報來電者的角色為下列其中一個動作：

-   **使用者**

    使用者必須，預設會嘗試使用 NTLM 驗證拒絕的受保護的使用者安全性群組成員。

    您可以設定較短的值帳號 TGT 期間或限制的使用者 account 可以登入的裝置設定原則。 豐富運算式可以在控制使用者，他們的裝置需要驗證服務符合的條件驗證原則設定。

    如需詳細資訊請查看[受保護的使用者安全性群組](protected-users-security-group.md)。

-   **服務**

    獨立管理服務帳號，受管理的群組服務帳號或衍生帳號的所用的服務的這兩種類型的自訂 account 物件。 原則可以設定的裝置存取控制項條件，可用來管理的服務 account 認證限制的 Active Directory 身分特定裝置。 服務不應保護使用者安全性群組成員因為所有收到的驗證將會失敗。

-   **電腦**

    使用電腦 account 物件或衍生從電腦 account 物件自訂 account 物件。 原則可以設定存取控制項條件，以便驗證使用者及裝置屬性 account 所需的。 電腦一律不應該保護使用者安全性群組成員因為所有收到的驗證將會失敗。 根據預設，取消所嘗試使用 NTLM 驗證。 適用於電腦帳號不應該設定 TGT 期間。

> [!NOTE]
> 很可能不相關的原則，以驗證原則筒倉帳號一組設定驗證原則。 當您有保護單一帳號，您可以使用此策略。

**Active Directory 架構的驗證原則**

下表中的結構描述定義使用者、 電腦及服務的 Active Directory 物件的原則。

|輸入|顯示名稱|描述|
|----|--------|--------|
|原則|驗證原則|這個課程的執行個體定義指派主體驗證原則的行為。|
|原則|驗證原則|本等級容器可包含驗證原則物件。|
|原則|執行驗證原則|指定是否執行此驗證原則。<br /><br />無法在執行，預設的原則在稽核模式，並專活動，表示可能成功和失敗，但保護不會套用至系統。|
|原則|指派的驗證原則 Backlink|這是屬性返回 msDS-AssignedAuthNPolicy 的連結。|
|原則|指派的驗證原則|指定 AuthNPolicy 應該這項原則套用。|
|使用者|使用者驗證原則|指定 AuthNPolicy 應該會套用至已指派給此筒倉物件的使用者。|
|使用者|使用者驗證原則 Backlink|這是屬性返回 msDS-UserAuthNPolicy 的連結。|
|使用者|ms-DS-User-Allowed-To-Authenticate-To|此屬性用來判斷允許執行帳號服務驗證原則的集合。|
|使用者|ms-DS-User-Allowed-To-Authenticate-From|此屬性用來判斷的裝置的使用者 account 有權限來登入的設定。|
|使用者|使用者 TGT 期間|指定最大世紀 Kerberos TGT 發行給使用者 （以秒）。 結果 Tgt 的非儲值。|
|電腦|電腦驗證原則|指定 AuthNPolicy 應該會套用到電腦已指派給此筒倉物件。|
|電腦|電腦驗證原則 Backlink|這是屬性返回 msDS-ComputerAuthNPolicy 的連結。|
|電腦|ms-DS-Computer-Allowed-To-Authenticate-To|此屬性用來判斷允許在的電腦帳號執行的服務驗證原則的設定。|
|電腦|電腦 TGT 期間|指定最大值 （以秒） 的電腦發給 Kerberos TGT 的年齡。 若要變更此設定不建議。|
|服務|服務驗證原則|指定 AuthNPolicy 應該會套用至已指派給此筒倉物件的服務。|
|服務|服務驗證原則 Backlink|這是屬性返回 msDS-ServiceAuthNPolicy 的連結。|
|服務|ms-DS-Service-Allowed-To-Authenticate-To|此屬性用來判斷的已獲授權的服務正在執行的服務帳號驗證原則設定。|
|服務|ms-DS-Service-Allowed-To-Authenticate-From|此屬性用來判斷的裝置的服務 account 有權限來登入的設定。|
|服務|服務 TGT 期間|指定發出 （以秒） 服務來 Kerberos TGT 世紀最大。|

使用 Windows PowerShell 的 Active Directory 系統管理主控台驗證原則可以設定為每個筒倉。 如需詳細資訊，請查看[設定保護帳號如何](how-to-configure-protected-accounts.md)。

## <a name="how-it-works"></a>它的運作方式
本節驗證原則筒倉和驗證原則的受保護的使用者安全性群組和實作 Kerberos 通訊協定，以在 Windows 中搭配運作的方式。

-   [如何使用 Kerberos 通訊協定與筒倉驗證原則](#BKMK_HowKerbUsed)

-   [如何限制使用者登入的運作方式](#BKMK_HowRestrictingSignOn)

-   [限制服務票證發行的運作方式](#BKMK_HowRestrictingServiceTicket)

**帳號受保護狀態**

保護使用者安全性群組觸發網域中執行 Windows Server 2012 R2 的主要網域控制站的網域控制站和裝置主機電腦執行 Windows 8.1、 Windows Server 2012 R2 上的非可設定保護。 根據網域功能 account 的等級，保護使用者安全性群組成員進一步受保護的驗證方法支援在 Windows 中變更因為。

-   使用 NTLM、 摘要驗證或 CredSSP 預設認證委派無法驗證保護使用者安全小組的成員。 在裝置上執行 Windows 8.1，可以使用其中一種這些安全性支援提供者 （層），將會失敗網域驗證，account 時的受保護的使用者安全性群組成員。

-   Kerberos 通訊協定不會預先驗證程序使用較弱 DES 或 RC4 加密類型。 這表示支援至少好一段加密類型，必須設定的網域。

-   無法使用 Kerberos 限制或受限制地委派帳號委派。 這表示如果使用者的受保護的使用者安全性群組成員先前連接到其他系統可能會失敗。

-   藉由驗證原則和筒倉，您可以存取 Active Directory 管理中心透過是設定的四個小時的預設 Kerberos Tgt 期間設定。 這表示當超過四小時的時間，使用者必須驗證再試一次。

如需有關這個安全性群組的詳細資訊，請查看[的受保護的使用者如何群組運作](protected-users-security-group.md#BKMK_HowItWorks)。

**筒倉和驗證原則**

驗證原則筒倉和驗證原則利用現有的 Windows 驗證基礎結構。 遭拒使用 NTLM 通訊協定，並用 Kerberos 通訊協定，以使用較加密類型。 驗證原則補充提供一種方式可設定的限制適用於帳號，除了限制帳號提供的服務和電腦的受保護的使用者安全性群組。 驗證原則的票證授與服務 (TGS) 換貨或 Kerberos 通訊協定驗證服務 （為） 期間執行。 如需有關如何使用 Windows Kerberos 通訊協定，以及變更所做支援驗證原則筒倉和驗證原則，查看：

-   [版本 Kerberos 驗證 5 通訊協定的運作方式](https://technet.microsoft.com/library/cc772815(v=ws.10).aspx)

-   [變更 Kerberos 驗證](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx)（Windows Server 2008 R2 和 Windows 7）

### <a name="BKMK_HowKerbUsed"></a>如何使用 Kerberos 通訊協定與驗證原則筒倉原則
當核對連結到驗證原則筒倉，使用者登入時，安全性帳號管理員新增理賠要求驗證原則筒倉包含筒倉值類型。 此宣告帳號提供目標筒倉存取。

驗證原則執行為網域控制站收到核對驗證服務要求，網域控制站傳回設定期間的非儲值 TGT （除非網域 TGT 期間是短）。

> [!NOTE]
> 核對必須設定的 TGT 期間，而且必須直接連結原則或透過筒倉成員資格間接連結。

當驗證原則是以稽核模式，網域控制站上收到驗證服務要求核對的網域控制站會檢查是否允許驗證的裝置，讓它可以登入一則警告如果失敗。 稽核的驗證原則不會變更程序，所以如果不符合原則的需求，無法將會失敗驗證要求。

> [!NOTE]
> 核對必須直接連接到原則或透過筒倉成員資格間接連結。

當驗證原則執行與驗證服務裝甲時，核對驗證服務要求收到網域控制站時，網域控制站檢查驗證是否允許該裝置。 如果失敗，網域控制站傳回錯誤訊息，並登約會。

> [!NOTE]
> 核對必須直接連接到原則或透過筒倉成員資格間接連結。

驗證原則是以稽核模式時，核對的網域控制站收到服務票證授與要求，網域控制站檢查是否允許要求的票證權限屬性憑證 (PAC) 的資料，以驗證，如果您無法登入一則警告訊息。 PAC 包含授權資訊，包括的使用者成員使用者有，權限的群組原則套用到使用者不同的類型。 這項資訊用來產生使用者存取預付碼。 如果是執行的驗證原則可讓使用者、 裝置或服務的驗證，驗證允許的網域控制站檢查基礎要求的票證 PAC 資料。 如果失敗，網域控制站傳回錯誤訊息，並登約會。

> [!NOTE]
> 核對必須將直接連結或透過筒倉成員資格稽核的驗證原則可讓使用者、 裝置或服務的驗證連結

您可以使用單一驗證原則的所有成員筒倉，或您可以使用原則的不同的使用者、 電腦及帳號受管理的服務。

使用 Windows PowerShell 的 Active Directory 系統管理主控台驗證原則可以設定為每個筒倉。 如需詳細資訊，請查看[設定保護帳號如何](how-to-configure-protected-accounts.md)。

### <a name="BKMK_HowRestrictingSignOn"></a>如何限制使用者登入的運作方式
這些驗證原則已經套用到帳號，因為它也適用於帳號所使用的服務。 如果您想要使用特定主機服務的密碼，則此設定會非常有用。 例如，群組管理設定帳號主機允許從 Active Directory Domain Services 擷取密碼的位置服務。 不過，該密碼可從任何主機的初始驗證。 藉由套用存取控制項條件，可以藉由限制只可以擷取密碼主機一組密碼達成額外的保護層級。

當系統、 網路的服務，或其他區域服務的身分執行的服務連接到網路的服務時，它們會使用主機的電腦 account。 電腦帳號不會受到限制。 即使服務會使用許多 Windows 不是電腦帳號，它無法限制。

特定的主機限制使用者登入需要驗證主機的身分網域控制站。 當使用 F:kerberos 驗證 Kerberos 保護 \ （即動態存取控制部分），金鑰 Distribution 中心隨附從中驗證使用者的主機 TGT。 使用這個裝甲 TGT 的 content 完成存取檢查以判斷是否允許該主機。

當使用者登入 Windows，或預設的應用程式中，輸入他們網域認證 credential 提示字元中時，Windows 會將傳送網域控制站護身的為需求。 如果使用者傳送要求的電腦不支援，保護 \，例如電腦執行的是 Windows 7 或 Windows Vista、 要求失敗。

下面描述處理程序：

-   執行 Windows Server 2012 R2 網域中的網域控制站查詢使用者帳號，並判斷它設定的限制初始需要裝甲的要求驗證，驗證原則。

-   網域控制站將會失敗要求。

-   因為需要護板，使用者可以嘗試使用電腦執行的是 Windows 8.1 或 Windows 8 功能的支援 Kerberos 保護 \ 來再試一次登入程序登入。

-   Windows 會偵測到網域支援 Kerberos 保護 \ 傳送裝甲的為-複製到再試一次登入的要求。

-   網域控制站執行存取檢查使用中，用於裝甲要求 TGT 設定的存取控制項條件和 client 作業系統的身分的資訊。

-   如果您無法存取檢查，網域控制站請求。

作業系統支援 Kerberos 保護 \，即使存取控制需求可在套用和必須符合才能存取。 使用者登入 Windows，或他們網域認證 credential 提示字元中輸入應用程式。 根據預設，Windows 會傳送給網域控制站護身的為需求。 如果使用者要求的支援護板，例如 Windows 8.1 或 Windows 8 的電腦傳送驗證原則的評估方式如下：

1.  執行 Windows Server 2012 R2 網域中的網域控制站查詢使用者帳號，並判斷它設定的限制初始需要裝甲的要求驗證，驗證原則。

2.  網域控制站執行存取檢查使用中，用於裝甲要求 TGT 設定的存取控制項條件和系統身分的資訊。 存取檢查成功。

    > [!NOTE]
    > 設定舊版群組限制，如果那些也必須符合。

3.  網域控制站回覆與裝甲回覆 （以代表，），並驗證繼續。

### <a name="BKMK_HowRestrictingServiceTicket"></a>限制服務票證發行的運作方式
時不允許帳號，並具有 TGT 的使用者，嘗試連接的服務 (例如下列程序發生，請打開需要驗證服務的服務主體名稱 (SPN) 由服務的應用程式，：

1.  在嘗試從 SPN 連接到 SPN1，Windows 會將傳送 TGS 需求網域控制站 SPN1 服務票證要求。

2.  執行 Windows Server 2012 R2 網域中的網域控制站尋找 SPN1 尋找服務的 Active Directory Domain Services 帳號，並判斷該設定的限制服務票證發行驗證原則 account。

3.  網域控制站執行存取檢查使用中 TGT 設定的存取控制項條件和的使用者身分的資訊 存取檢查將會失敗。

4.  網域控制站請求。

因為 account 符合存取控制項條件驗證原則，來設定，並嘗試服務連接的使用者，有 TGT 時允許 account (例如打開應用程式所需要驗證服務由服務的 SPN)，下列程序發生：

1.  在連接到 SPN1 嘗試，Windows 會將傳送 TGS 需求網域控制站 SPN1 服務票證要求。

2.  執行 Windows Server 2012 R2 網域中的網域控制站尋找 SPN1 尋找服務的 Active Directory Domain Services 帳號，並判斷該設定的限制服務票證發行驗證原則 account。

3.  網域控制站執行存取檢查使用中 TGT 設定的存取控制項條件和的使用者身分的資訊 存取檢查成功。

4.  網域控制站回覆要求票證授與服務回覆 （TGS 代表） 使用。

## <a name="BKMK_ErrorandEvents"></a>相關的錯誤和資訊事件訊息
下表描述保護使用者安全性群組相關聯的事件和適用於驗證原則筒倉驗證原則。

事件記錄的應用程式與服務登，在**Microsoft\Windows\Authentication**。

疑難排解步驟使用這些事件，請查看[疑難排解驗證原則](how-to-configure-protected-accounts.md#BKMK_TroubleshootAuthnPolicies)和[疑難排解事件相關保護使用者](how-to-configure-protected-accounts.md#BKMK_TrubleshootingEvents)。

|事件 ID 和登入|描述|
|----------|--------|
|101<br /><br />**AuthenticationPolicyFailures-DomainController**|理由： NTLM 登入失敗就會發生驗證原則。<br /><br />事件被登入，表示該 NTLM 驗證失敗，因為存取控制限制的需要，這些限制不會套用到 NTLM 網域控制站。<br /><br />顯示 account、 裝置、 原則和筒倉名稱。|
|105<br /><br />**AuthenticationPolicyFailures-DomainController**|理由： 不允許的特定裝置驗證是因為發生錯誤 Kerberos 限制。<br /><br />事件被登入網域控制站指出 Kerberos TGT 遭拒因為裝置不符合執行的存取控制限制。<br /><br />顯示帳號，裝置、 原則、 筒倉名稱、 和 TGT 期間。|
|305<br /><br />**AuthenticationPolicyFailures-DomainController**|理由： 潛在 Kerberos 限制錯誤可能是因為不允許的特定裝置驗證。<br /><br />稽核模式，資訊的事件會登入網域控制站判斷是否 Kerberos TGT 都會無法因為裝置不符合存取控制限制。<br /><br />顯示帳號，裝置、 原則、 筒倉名稱、 和 TGT 期間。|
|106<br /><br />**AuthenticationPolicyFailures-DomainController**|理由： 伺服器驗證不允許的使用者或裝置，因為發生錯誤 Kerberos 限制。<br /><br />事件被登入網域控制站指出 Kerberos 服務票證遭拒因為使用者、 裝置或兩者不符合執行的存取控制限制。<br /><br />裝置、 原則和筒倉顯示的名稱。|
|306<br /><br />**AuthenticationPolicyFailures-DomainController**|理由： Kerberos 限制錯誤可能是因為驗證伺服器不允許的使用者或裝置。<br /><br />稽核模式，資訊的事件登入，表示該 Kerberos 服務票證都會無法因為使用者、 裝置或兩者不符合存取控制限制的網域控制站。<br /><br />裝置、 原則和筒倉顯示的名稱。|

## <a name="see-also"></a>也了
[如何設定帳號受保護狀態](how-to-configure-protected-accounts.md)

[認證保護與管理](credentials-protection-and-management.md)

[使用者安全性群組受保護狀態](protected-users-security-group.md)


