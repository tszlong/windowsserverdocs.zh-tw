---
ms.assetid: 39ecc468-77c5-4938-827e-48ce498a25ad
title: 附錄 A-審查 AD FS 需求
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: ad4275bf7b6231692171209b19c4c60190e30126
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87942974"
---
# <a name="appendix-a-reviewing-ad-fs-requirements"></a>附錄 A：檢閱 AD FS 需求

因此，您 Active Directory 同盟服務中 (AD FS) 部署的組織夥伴可以成功共同作業，您必須先確定您的公司網路基礎結構已設定為支援帳戶、名稱解析和憑證的 AD FS 需求。 AD FS 具有以下類型的需求：

> [!TIP]
> 您可以在[瞭解重要的 AD FS 概念](https://docs.microsoft.com/windows-server/identity/ad-fs/technical-reference/understanding-key-ad-fs-concepts)中找到其他 AD FS 資源連結。

## <a name="hardware-requirements"></a>硬體需求
下列最低和建議硬體需求適用于同盟伺服器和同盟伺服器 proxy 電腦。

|硬體需求|最低需求|建議需求|
|------------------------|-----------------------|---------------------------|
|CPU 速度|單核心，1 GHz|四核心，2 GHz|
|RAM|1 GB|4 GB|
|磁碟空間|50 MB|100 MB|

## <a name="software-requirements"></a>軟體需求
AD FS 依賴 Windows Server 2012 作業系統內建的伺服器功能 &reg; 。

> [!NOTE]
> Federation Service 和 Federation Service Proxy 角色服務無法並存於相同的電腦上。

## <a name="certificate-requirements"></a>憑證需求
憑證在同盟伺服器、同盟伺服器 Proxy、宣告感知 (Claims-Aware) 應用程式及 Web 用戶端之間進行安全通訊時扮演最重要的角色。 憑證的需求會根據您是否設定同盟伺服器或同盟伺服器 Proxy 電腦而有所不同，如本節所述。

### <a name="federation-server-certificates"></a>同盟伺服器憑證
同盟伺服器需要下表中的憑證。

|憑證類型|描述|您需要在部署之前了解的內容|
|--------------------|---------------|------------------------------------------|
|安全通訊端層 (SSL) 憑證|這是標準的安全通訊端層 (SSL) 憑證，可用來保護同盟伺服器與用戶端之間的通訊安全。|這個憑證必須繫結到網際網路資訊服務 (IIS) 中預設的網站，以供同盟伺服器或同盟伺服器 Proxy 使用。  針對同盟伺服器 Proxy，必須先在 IIS 中設定繫結才能成功執行 [同盟伺服器 Proxy 設定精靈]。<p>**建議：** 由於此憑證必須受到 AD FS 的用戶端信任，因此，請使用由公用 (協力廠商) 證書)  (頒發機構單位（例如 VeriSign）所發行的伺服器驗證憑證。 **秘訣：** 此憑證的主體名稱用來代表您所部署 AD FS 的每個實例的同盟服務名稱。 基於這個理由，您可能想要考慮在任何 CA 簽發的新憑證上，選擇對合作夥伴而言最能代表貴公司或組織名稱的主體名稱。|
|服務通訊憑證|這個憑證會啟用 WCF 訊息安全性，來保護同盟伺服器之間的通訊。|根據預設，SSL 憑證會用來做為服務通訊憑證。  這可以使用 AD FS 管理主控台來變更。|
|權杖簽署憑證|這是標準的 X509 憑證，可以用來安全地簽署同盟伺服器簽發的所有權杖。|權杖簽署憑證必須包含一個私密金鑰，而且應該鏈結到 Federation Service 中受信任的根目錄。 根據預設，AD FS 會建立自我簽署憑證。 但是，根據組織需求而定，您稍後可以使用 AD FS 管理嵌入式管理單元，將這個憑證變更為 CA 簽發的憑證。|
|權杖解密憑證|這是標準的 SSL 憑證，可以用來解密任何由夥伴同盟伺服器加密的傳入權杖。 它也會在同盟中繼資料中發佈。|根據預設，AD FS 會建立自我簽署憑證。 但是，根據組織需求而定，您稍後可以使用 AD FS 管理嵌入式管理單元，將這個憑證變更為 CA 簽發的憑證。|

> [!CAUTION]
> 用來進行權杖簽署和權杖解密的憑證對於 Federation Service 的穩定性而言相當重要。 因為遺失或意外移除基於此目的設定的任何憑證都會中斷服務，所以您應該備份基於此目的設定的所有憑證。

如需同盟伺服器所使用的憑證詳細資訊，請參閱[同盟伺服器的憑證需求](Certificate-Requirements-for-Federation-Servers.md)。

### <a name="federation-server-proxy-certificates"></a>同盟伺服器 Proxy 憑證
同盟伺服器 Proxy 需要下表中的憑證。

|憑證類型|描述|您需要在部署之前了解的內容|
|--------------------|---------------|------------------------------------------|
|伺服器驗證憑證|這是標準的安全通訊端層 (SSL) 憑證，可用來在同盟伺服器 Proxy 與網際網路用戶端電腦之間進行安全通訊。|這個憑證必須先繫結到網際網路資訊服務 (IIS) 中預設的網站，您才能成功執行 [AD FS 同盟伺服器 Proxy 設定精靈]。<p>**建議：** 由於此憑證必須受到 AD FS 的用戶端信任，因此，請使用由公用 (協力廠商) 證書)  (頒發機構單位（例如 VeriSign）所發行的伺服器驗證憑證。<p>**秘訣：** 此憑證的主體名稱用來代表您所部署 AD FS 的每個實例的同盟服務名稱。 基於這個理由，您可能想要考慮選擇對合作夥伴而言最能代表貴公司或組織名稱的主體名稱。|

如需同盟伺服器 Proxy 所使用的憑證相關詳細資訊，請參閱[同盟伺服器 Proxy 的憑證需求](Certificate-Requirements-for-Federation-Server-Proxies.md)。

## <a name="browser-requirements"></a>瀏覽器需求
儘管所有目前具備 JavaScript 功能的 Web 瀏覽器都可以用來做為 AD FS 用戶端，但預設提供的網頁只有針對 Windows 上的 Internet Explorer 版本 7.0、8.0 及 9.0、Mozilla Firefox 3.0 以及 Safari 3.1 測試過。 JavaScript 必須啟用，而且必須啟用 Cookie，以瀏覽器為基礎的登入和登出才能正常運作。

Microsoft 的 AD FS 產品小組已成功測試下表中的瀏覽器和作業系統設定。

|瀏覽器|Windows 7|Windows Vista|
|-----------|-------------|-----------------|
|Internet Explorer 7.0|X|X|
|Internet Explorer 8.0|X|X|
|Internet Explorer 9.0|X|未測試|
|FireFox 3.0|X|X|
|Safari 3.1|X|X|

> [!NOTE]
> AD FS 支援上表中顯示之所有瀏覽器的 32 位元和 64 位元版本。

### <a name="cookies"></a>Cookie
AD FS 會建立以工作階段為基礎的永續性 Cookie，這類 Cookie 必須儲存於用戶端電腦上，以提供登入、登出、單一登入 (SSO) 及其他功能。 因此，用戶端瀏覽器必須設定為接受 Cookie。 用來驗證的 Cookie 一律為安全超文字傳輸通訊協定 (HTTPS) 工作階段 Cookie，它們是針對原始伺服器所撰寫的。 如果未將用戶端瀏覽器設定為允許這些 Cookie，AD FS 就無法正確運作。 永續性 Cookie 可用來保留宣告提供者的使用者選項。 您可以在適用於 AD FS 登入頁面的設定檔中使用組態設定來停用它們。

基於安全理由，需要支援 TLS/SSL。

## <a name="network-requirements"></a>網路需求
適當地設定以下網路服務是在組織中成功部署 AD FS 的要素。

### <a name="tcpip-network-connectivity"></a>TCP/IP 網路連線
若要讓 AD FS 運作，下列各項之間必須要有 TCP/IP 網路連線：用戶端；網域控制站；以及裝載 Federation Service、Federation Service Proxy (使用時) 與 AD FS 網路代理程式的電腦。

### <a name="dns"></a>DNS
Active Directory Domain Services (AD DS) 以外的 AD FS 操作的主要網路服務是網域名稱系統 (DNS) 。 部署 DNS 時，使用者可以使用容易記住的好記電腦名稱，連接到電腦及 IP 網路上的其他資源。

 Windows Server 2008 使用 DNS 進行名稱解析，而不是 Windows 網際網路名稱服務 (WINS) 以 Windows NT 4.0 為基礎的網路中使用的 NetBIOS 名稱解析。 您還是能夠針對需要 WINS 的應用程式使用它。 不過，AD DS 和 AD FS 需要 DNS 名稱解析。

設定 DNS 以支援 AD FS 的程式會因下列情況而異：

-   您的組織現在已經具備 DNS 基礎結構。 在大部分的情況下，DNS 已經在您的整個網路中設定完成，讓貴公司網路中的網頁瀏覽器用戶端都能存取網際網路。 因為網際網路存取和名稱解析是 AD FS 的需求，所以會假設此基礎結構已適用于您的 AD FS 部署。

-   您想要將同盟伺服器新增到公司網路。 基於在公司網路中驗證使用者的目的，必須設定公司網路樹系中的內部 DNS 伺服器，以傳回正在執行 Federation Service 的內部伺服器 CNAME。 如需詳細資訊，請參閱[同盟伺服器的名稱解析需求](Name-Resolution-Requirements-for-Federation-Servers.md)。

-   您想要將同盟伺服器 Proxy 新增到周邊網路。 當您想要驗證位於身分識別夥伴組織的公司網路中的使用者帳戶時，必須設定公司網路樹系中的內部 DNS 伺服器，以傳回內部同盟伺服器 proxy 的 CNAME。 如需如何設定 DNS 以配合同盟伺服器 proxy 新增的詳細資訊，請參閱[同盟伺服器 proxy 的名稱解析需求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。

-   您正在設定適用於測試實驗室環境的 DNS。 如果您打算在測試實驗室環境中使用 AD FS，而不會有任何單一根 DNS 伺服器授權，您可能必須設定 DNS 轉寄站，以便在兩個或多個樹系之間的名稱查詢會適當地轉送。 如需如何設定 AD FS 測試實驗室環境的一般資訊，請參閱[AD FS 逐步解說和操作指南](https://go.microsoft.com/fwlink/?LinkId=180357)。

## <a name="attribute-store-requirements"></a>屬性存放區需求
AD FS 需要至少一個用來驗證使用者的屬性存放區，並將這些使用者的安全性宣告解壓縮。 如需 AD FS 支援的屬性存放區清單，請參閱《AD FS 設計指南》中的[屬性存放區的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)。

> [!NOTE]
> 根據預設，AD FS 會自動建立 Active Directory 屬性存放區。

屬性存放區需求需視您的組織是帳戶夥伴 (主控同盟使用者) 或資源夥伴 (主控同盟應用程式) 而定。

### <a name="adds"></a>AD DS
若要讓 AD FS 順利操作，帳戶夥伴組織或資源夥伴組織中的網域控制站必須執行 Windows Server 2003 SP1、Windows Server 2003 R2、Windows Server 2008 或 Windows Server 2012。

在已加入網域的電腦上安裝並設定 AD FS 時，該網域的 Active Directory 使用者帳戶存放區會變成可選取的屬性存放區。

> [!IMPORTANT]
> 由於 AD FS 需要 (IIS) 安裝 Internet Information Services，因此基於安全性考慮，建議您不要在生產環境中的網域控制站上安裝 AD FS 軟體。 但是，Microsoft 客戶服務支援部門支援這個設定。

#### <a name="schema-requirements"></a>結構描述需求
AD FS 不需要對 AD DS 進行架構變更或功能層級修改。

#### <a name="functional-level-requirements"></a>功能等級需求
大部分的 AD FS 功能不需要進行 AD DS 功能等級修改，就能成功運作。 但是，如果憑證明確對應到 AD DS 中的使用者帳戶，則用戶端憑證驗證需要 Windows Server 2008 網域功能等級或更高等級才能成功運作。

#### <a name="service-account-requirements"></a>服務帳戶需求
如果您正在建立同盟伺服器陣列，就必須先在 Federation Service 可以使用的 AD DS 中，建立以網域為基礎的專屬服務帳戶。 稍後，您可以在伺服陣列中設定每台同盟伺服器，以使用這個帳戶。 如需如何執行此動作的詳細資訊，請參閱《AD FS 部署指南》中的[手動設定同盟伺服器陣列的服務帳戶](../../ad-fs/deployment/Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)。

### <a name="ldap"></a>LDAP
當您使用其他以輕量型目錄存取通訊協定 (LDAP) 為基礎的屬性存放區時，必須連線到支援 Windows 整合式驗證的 LDAP 伺服器。 LDAP 連線字串也必須以 LDAP URL 的格式來書寫，如 RFC 2255 中所述。

### <a name="sql-server"></a>SQL Server
若要讓 AD FS 能夠順利操作，裝載結構化查詢語言 (SQL)  (SQL) 伺服器屬性存放區的電腦，必須執行 Microsoft SQL Server 2005 或 SQL Server 2008。 當您使用以 SQL 為基礎的屬性存放區時，也必須設定連線字串。

### <a name="custom-attribute-stores"></a>自訂屬性存放區
您可以開發自訂屬性存放區，以啟用進階案例。 AD FS 內建的原則語言可以參考自訂屬性存放區，因此能夠增強下列任一個案例：

-   建立適用於本機驗證使用者的宣告

-   補充適用於外部驗證使用者的宣告

-   授權使用者取得權杖

-   授權服務代表使用者取得權杖

當您使用自訂屬性存放區時，可能也必須設定連線字串。 在這個情況下，您可以輸入任何想要的自訂碼，以啟用自訂屬性存放區的連線。 這個情況中的連線字串是一組名稱/值對，解譯的方式就像是自訂屬性存放區的開發人員所實作的一樣。

如需開發和使用自訂屬性存放區的詳細資訊，請參閱[屬性存放區總覽](https://go.microsoft.com/fwlink/?LinkId=190782)。

## <a name="application-requirements"></a>應用程式需求
同盟伺服器可以與同盟應用程式通訊，並保護同盟應用程式，例如，宣告感知應用程式。

## <a name="authentication-requirements"></a>驗證需求
AD FS 與現有的 Windows 驗證自然整合，例如 Kerberos 驗證、NTLM、智慧卡和 x.509 v3 用戶端憑證。 同盟伺服器會使用標準的 Kerberos 驗證，根據網域來驗證使用者。 用戶端可以根據您設定驗證的方式，使用表單型驗證、智慧卡驗證，以及 Windows 整合式驗證來進行驗證。

AD FS 同盟伺服器 Proxy 角色讓使用者能夠從外部使用 SSL 用戶端驗證來進行驗證。 儘管通常最順暢的使用者經驗是藉由設定帳戶同盟伺服器以進行 Windows 整合式驗證來達成，您也可以設定同盟伺服器角色來要求 SSL 用戶端驗證。 在這個情況下，AD FS 對於使用者利用哪些憑證來進行 Windows 桌面登入沒有任何控制權。

### <a name="smart-card-logon"></a>智慧卡登入
雖然 AD FS 可以強制執行用於驗證 (密碼、SSL 用戶端驗證或 Windows 整合式驗證) 的認證類型，但它不會直接使用智慧卡強制執行驗證。 因此，AD FS 不會提供用戶端使用者介面 (UI) 來取得智慧卡個人識別碼 (PIN) 認證。 這是因為以 Windows 為基礎的用戶端刻意不會提供使用者認證詳細資料給同盟伺服器或網頁伺服器。

### <a name="smart-card-authentication"></a>智慧卡驗證
智慧卡驗證會使用 Kerberos 通訊協定向帳戶同盟伺服器進行驗證。 AD FS 無法擴充以加入新的驗證方法。 不需使用智慧卡中的憑證來鏈結到用戶端電腦上信任的根目錄。 使用智慧卡憑證搭配 AD FS 需要下列條件：

-   智慧卡的讀取器和密碼編譯服務提供者 (CSP) 必須在瀏覽器所在的電腦上運作。

-   智慧卡憑證必須與帳戶同盟伺服器和帳戶同盟伺服器 proxy 上的根信任連結。

-   憑證必須使用下列任一個方法，對應到 AD DS 中的使用者帳戶：

    -   憑證主體名稱會對應到 AD DS 中使用者帳戶的 LDAP 辨別名稱。

    -   憑證主體 altname 延伸含有 AD DS 中使用者帳戶的使用者主體名稱 (UPN)。

若要在某些情況下支援特定的驗證強度需求，也能夠設定 AD FS，以建立宣告來指出使用者已通過驗證。 信賴憑證者接著可以使用此宣告來做出授權決定。

## <a name="see-also"></a>另請參閱
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
