---
ms.assetid: 39ecc468-77c5-4938-827e-48ce498a25ad
title: "由附錄審查 AD FS 需求"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d5cfb5de77843eebfc152b9c79ac55bab1fa7727
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-a-reviewing-ad-fs-requirements"></a>答附錄審查 AD FS 需求

>適用於：Windows Server 2012

讓組織中部署 Active Directory 同盟 Services (AD FS) 合作夥伴可以成功共同作業，您必須先確定您的企業網路基礎結構已支援帳號，憑證的名稱解析，AD FS 需求。 AD FS 有下列幾種需求：  
  
> [!TIP]  
> 您可以找到其他 AD FS 資源連結，以[AD FS 內容地圖](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx)頁面上的 Microsoft TechNet Wiki。 此頁面由 AD FS 社群的成員，並會定期監視 AD FS Product 小組。  
  
## <a name="hardware-requirements"></a>硬體需求  
下列最低與建議的硬體需求適用於聯盟伺服器] 與聯盟 proxy 伺服器的電腦。  
  
|硬體需求|最低需求|建議的需求|  
|------------------------|-----------------------|---------------------------|  
|CPU 速度|單核心 1 ghz|Quad core，2 GHz|  
|RAM|1 GB|4 GB|  
|磁碟空間|50 MB|100 MB|  
  
## <a name="software-requirements"></a>軟體需求  
AD FS 依賴伺服器功能建置到 Windows Server® 2012年作業系統。  
  
> [!NOTE]  
> 聯盟服務與同盟服務 Proxy 角色服務無法並存相同的電腦上。  
  
## <a name="certificate-requirements"></a>憑證需求  
憑證播放最重要的角色保護聯盟伺服器、 聯盟的 proxy 伺服器、 宣告感知應用程式，以及 Web 戶端間通訊。 根據是否您的設定聯盟伺服器或聯盟伺服器 proxy 電腦，此一節中所述，而有所不同憑證的需求。  
  
### <a name="federation-server-certificates"></a>聯盟伺服器的憑證  
聯盟伺服器需要下表中的憑證。  
  
|憑證類型|描述|您需要知道部署之前|  
|--------------------|---------------|------------------------------------------|  
|安全通訊端層 (SSL) 憑證|這是確保聯盟伺服器戶端間通訊使用標準安全通訊端層 (SSL) 憑證。|這個憑證必須聯盟伺服器或聯盟 Proxy 伺服器繫結至預設網站網際網路資訊服務 (IIS)。  聯盟伺服器 Proxy，繫結必須設定在前順利執行聯盟 Proxy 伺服器設定精靈。<br /><br />**建議：**此憑證的必須信任的 AD FS 用，因為使用的是公用 （第三方） 憑證授權單位發行 （加拿大），例如 VeriSign 伺服器驗證憑證。 **秘訣：**這個憑證主體名稱用來表示同盟服務的名稱為每個您要部署的 AD FS 執行個體。 基於這個原因，您可能要考慮選擇主體名稱在任何新 CA 發行憑證的最佳代表您的公司或組織的合作夥伴的名稱。|  
|服務通訊的憑證|此憑證允許 WCF 訊息安全性保護之間聯盟伺服器通訊。|根據預設，為服務通訊憑證使用 SSL 憑證。  請使用 AD FS 管理主控台這可進行變更。|  
|權杖簽署的憑證|這是標準 X509 用於安全地登入所有權杖問題聯盟伺服器的憑證。|權杖簽署的憑證必須包含私密金鑰，而且它應該鏈結同盟服務中受信任的網站。 根據預設，AD FS 建立自動簽署的憑證。 不過，您可以變更此稍後 CA 發行憑證使用 AD FS 管理嵌入式管理單元，根據您的組織的需求。|  
|預付碼-解密憑證|這是用來解密任何連入權杖加密的協力廠商聯盟伺服器標準 SSL 憑證。 這也被發行聯盟中繼資料中。|根據預設，AD FS 建立自動簽署的憑證。 不過，您可以變更此稍後 CA 發行憑證使用 AD FS 管理嵌入式管理單元，根據您的組織的需求。|  
  
> [!CAUTION]  
> 憑證預付碼簽章和權杖解密所使用的重要同盟服務的穩定性。 因為遺失或計畫的移除之任何設定為這個項目的的憑證可能會服務中斷，您應該備份為這個項目的設定的任何憑證。  
  
如需聯盟伺服器使用憑證的詳細資訊，請查看[聯盟伺服器的憑證需求](Certificate-Requirements-for-Federation-Servers.md)。  
  
### <a name="federation-server-proxy-certificates"></a>聯盟 proxy 伺服器的憑證  
聯盟伺服器 proxy 需要下表中的憑證。  
  
|憑證類型|描述|您需要知道部署之前|  
|--------------------|---------------|------------------------------------------|  
|伺服器驗證憑證|這是用來保護電腦的通訊聯盟伺服器 proxy 之間網際網路 client 標準安全通訊端層 (SSL) 憑證。|這個憑證必須繫結至預設網站網際網路資訊服務 (IIS) 之前，您也可以順利執行 AD FS 聯盟伺服器 Proxy 設定精靈。<br /><br />**建議：**此憑證的必須信任的 AD FS 用，因為使用的是公用 （第三方） 憑證授權單位發行 （加拿大），例如 VeriSign 伺服器驗證憑證。<br /><br />**秘訣：**這個憑證主體名稱用來表示同盟服務的名稱為每個您要部署的 AD FS 執行個體。 基於這個原因，您可能要考慮選擇最適合代表您的公司或組織的合作夥伴名稱主體名稱。|  
  
如需聯盟的 proxy 伺服器使用憑證的詳細資訊，請查看[聯盟的 Proxy 伺服器的憑證需求](Certificate-Requirements-for-Federation-Server-Proxies.md)。  
  
## <a name="browser-requirements"></a>瀏覽器需求  
雖然才能 AD FS client 進行任何 JavaScript 功能與目前的網頁瀏覽器，提供預設網頁經過只對 7.0、 8.0 及 9.0 Mozilla Firefox 3.0 和 Safari 3.1 Windows 上的 Internet Explorer 版本。 必須支援 JavaScript，並 cookie 必須讓的瀏覽器登入且 sign-out 正常運作。  
  
Microsoft AD FS product 小組成功測試瀏覽器與作業系統設定在下表中。  
  
|瀏覽器|Windows 7|Windows Vista|  
|-----------|-------------|-----------------|  
|Internet Explorer 7.0|X|X|  
|Internet Explorer 8.0|X|X|  
|Internet Explorer 9.0|X|不測試|  
|FireFox 3.0|X|X|  
|Safari 3.1|X|X|  
  
> [!NOTE]  
> AD FS 支援 32 位元與 64 位元版本的上述表格中所顯示的瀏覽器。  
  
### <a name="cookies"></a>Cookie  
AD FS 建立工作階段架構與持續性必須登入、 sign-out、 單一登入 (SSO)，以及其他功能提供 client 電腦儲存的 cookie。 因此，必須接受 cookie 設定 client 瀏覽器。 Cookie 可用來驗證都是安全超傳輸通訊協定 」 (HTTPS) 工作階段 cookie 所撰寫的原生伺服器。 如果 client 瀏覽器不允許 cookie 這些設定，AD FS 無法正常運作。 持續 cookie 可用來保留宣告提供者的使用者選取項目。 您可以停用來設定檔登入頁面 AD FS 使用的設定。  
  
基於安全性考量需要 SSL TLS 日的支援。  
  
## <a name="network-requirements"></a>網路需求  
設定適當的網路下列服務已成功 AD FS 您在組織中部署的重要。  
  
### <a name="tcpip-network-connectivity"></a>TCP/IP 網路連接  
TCP/IP 網路連接函式 AD FS，必須存在之間 client;網域控制站;與電腦的裝載同盟服務、 同盟服務 Proxy （時使用），以及 AD FS Web 代理程式。  
  
### <a name="dns"></a>DNS  
AD FS、 Active Directory Domain Services (AD DS)，以外的重要的主要網路服務是網域名稱系統 」 (DNS)。 部署 DNS 時，使用者可以使用易記連接到電腦和其他資源 IP 網路上的易記電腦名稱。  
  
 Windows Server 2008 會使用 DNS 名稱解析，而不是 Windows nt4.0 為基礎的網路所使用的 Windows 網際網路名稱服務 」 (WINS) NetBIOS 名稱解析。 它是仍然可以使用 WINS 需要應用程式。 不過，AD DS，AD FS 需要 DNS 名稱解析。  
  
設定支援 AD FS DNS 的程序而有所不同，是否：  
  
-   您的組織已經有現有 DNS 基礎結構。 在大部分案例中，DNS 已設定在您的網路，讓您的企業網路中的網頁瀏覽器戶端具有網際網路存取權。 網際網路存取和名稱解析 AD FS 需求，因為這基礎結構假設為可供您 AD FS 部署。  
  
-   您想要新增到您的企業網路聯盟的伺服器。 為了驗證使用者企業網路，必須返回 CNAME 內部伺服器同盟服務執行的設定內部公司網路森林中的 DNS 伺服器。 如需詳細資訊，請查看[聯盟伺服器的名稱解析需求](Name-Resolution-Requirements-for-Federation-Servers.md)。  
  
-   您想要新增到周邊網路 proxy 伺服器聯盟。 當您想要驗證帳號位於組織的身分合作夥伴的企業網路時，必須設定返回 CNAME 內部聯盟 proxy 伺服器的企業網路森林中的內部 DNS 伺服器。 了解如何設定 DNS 容納聯盟的 proxy 伺服器的資訊，請查看[聯盟的 Proxy 伺服器的名稱解析需求](Name-Resolution-Requirements-for-Federation-Server-Proxies.md)。  
  
-   您的設定實驗室測試 DNS。 如果您不單一根 DNS 伺服器所在授權實驗室測試環境中使用 AD FS 計劃，就可能，您將需要設定 DNS 轉送程式，以便將適當轉送查詢兩個或更多的樹系之間的名稱。 如需如何設定 AD FS 測試實驗室的一般資訊，請查看[AD FS 逐步及如何指南](https://go.microsoft.com/fwlink/?LinkId=180357)。  
  
## <a name="attribute-store-requirements"></a>屬性市集需求  
AD FS 需要至少屬性市集驗證使用者與解壓縮安全性宣告那些使用者使用。 針對一份屬性儲存 AD FS 支援，請查看[的角色的屬性儲存](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)中的 AD FS 設計。  
  
> [!NOTE]  
> AD FS 預設會自動建立 Active Directory 屬性市集。  
  
您的組織是否做為 account 合作夥伴 （主持聯盟的使用者） 或 （裝載聯盟應用程式） 資源合作夥伴屬性市集需求而定。  
  
### <a name="ad-ds"></a>AD DS  
AD fs 順利運作，網域控制站 account 合作夥伴公司或組織資源合作夥伴必須執行 Windows Server 2003 SP1、 Windows Server 2003 R2、 Windows Server 2008 或 Windows Server 2012。  
  
AD FS 安裝並設定加入網域的電腦上，當 Active Directory 使用者 account 存放區該網域可做為可選取屬性存放區。  
  
> [!IMPORTANT]  
> AD FS 需要安裝的網際網路資訊服務 (IIS)，因為我們建議您安裝 AD FS 軟體網域控制站在基於安全性考量 production 環境中。 不過，此設定是由 Microsoft 客戶服務支援支援。  
  
#### <a name="schema-requirements"></a>架構需求  
AD FS 不需要的架構變更或到 AD DS 功能等級修改。  
  
#### <a name="functional-level-requirements"></a>功能層級需求  
大部分的 AD FS 功能不需要 AD DS 功能等級修改順利運作。 不過，Windows Server 2008 網域功能層級或更高版本，才能 client 憑證驗證憑證明確對應到 AD DS 中的使用者 account 如果順利運作。  
  
#### <a name="service-account-requirements"></a>服務 account 需求  
如果您要建立聯盟伺服器陣列，您必須先建立專用的網域型服務 account AD DS 同盟服務可以使用中。 之後，您在使用此帳號發電廠設定每個聯盟伺服器。 如需如何執行此動作，請查看[手動設定聯盟伺服器陣列服務 Account](../../ad-fs/deployment/Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md)中的 AD FS 部署。  
  
### <a name="ldap"></a>LDAP  
當您使用其他輕量型 Directory 存取通訊協定 LDAP 為基礎的屬性存放區時，您必須連接到支援的 Windows 整合式驗證 LDAP 伺服器。 RFC 2255 中所述 LDAP 連接字串必須也撰寫 LDAP URL 的格式。  
  
### <a name="sql-server"></a>SQL Server  
AD fs 順利運作，裝載屬性存放區結構化查詢的語言 (SQL) 伺服器的電腦必須執行 Microsoft SQL Server 2005 或 SQL Server 2008。 當您使用 SQL 為基礎的屬性存放區時，您還必須設定連接字串。  
  
### <a name="custom-attribute-stores"></a>自訂屬性存放區  
您可以開發自訂屬性存放區，可讓進階的案例。 建置到 AD FS 原則語言可以參考自訂屬性存放區，以便增強案例下列任一項：  
  
-   建立 [本機驗證使用者宣告  
  
-   補充外部驗證使用者宣告  
  
-   若要取得權杖使用者的授權  
  
-   若要取得行為的使用者權杖服務的授權  
  
當您使用 [自訂屬性網上商店時，您也可能設定連接字串。 此時，您可以輸入您要可讓您自訂屬性存放區的連接任何自訂代碼。 在這種情形連接字串是一組解譯為實作自訂屬性網上商店的開發人員名稱日值配對。  
  
如需有關開發和使用自訂屬性存放區的詳細資訊，請[屬性市集概觀](https://go.microsoft.com/fwlink/?LinkId=190782)。  
  
## <a name="application-requirements"></a>應用程式需求  
聯盟伺服器可以與並保護聯盟應用程式，例如宣告感知應用程式。  
  
## <a name="authentication-requirements"></a>驗證的需求  
AD FS 整合自然現有的 Windows 驗證，例如 F:kerberos 驗證、 NTLM、 智慧卡，與 v3 client 端 x.509。 聯盟伺服器驗證使用者網域中針對使用標準 F:kerberos 驗證。 戶端可以使用驗證，驗證智慧卡，與 Windows 整合式驗證，根據您如何設定驗證進行驗證。  
  
AD FS 聯盟伺服器 proxy 角色可讓中驗證外部使用 SSL client 驗證使用者的案例。 您也可以設定聯盟伺服器角色要求 SSL client 驗證，雖然最順暢的使用者體驗通常透過設定 account 聯盟伺服器的整合式 Windows 驗證。 此時，AD FS 已使用者使用適用於 Windows 桌面登入認證為何無法控制。  
  
### <a name="smart-card-logon"></a>智慧卡登入  
AD FS 可執行的認證，它會 （密碼、 SSL client 驗證或 Windows 整合式驗證） 驗證類型，雖然這不會直接執行使用智慧卡驗證。 因此，AD FS 不會提供 client 端使用者介面 (UI) 以取得智慧卡個人驗證認證號碼 (PIN)。 這是因為 windows 戶端刻意不提供聯盟伺服器或網頁伺服器使用者的認證詳細資料。  
  
### <a name="smart-card-authentication"></a>智慧卡驗證  
智慧卡驗證使用 Kerberos 通訊協定 account 聯盟伺服器的驗證。 AD FS 不能延伸到新增新的驗證方法。 不需要鏈結操之在 client 電腦上受信任的根憑證智慧卡中。 使用 AD FS 使用智慧卡基礎憑證需要下列條件：  
  
-   瀏覽器所在的電腦必須工作 reader 和密碼編譯服務提供者 (CSP) 的智慧卡。  
  
-   智慧卡憑證必須鏈到 account 聯盟伺服器 account 聯盟伺服器 proxy 上受信任的網站。  
  
-   憑證必須對應到 AD ds 帳號下列方法：  
  
    -   憑證主體名稱相當於在 AD DS 帳號 LDAP 分辨名稱。  
  
    -   憑證主旨 altname 擴充功能的使用者 account AD ds 已使用者主體名稱 (UPN)。  
  
若要支援特定驗證越需求案例中，其也可設定來建立，表示使用者如何驗證理賠要求 AD FS。 讓授權決策信賴可以使用此理賠要求。  
  
## <a name="see-also"></a>也了
[Windows Server 2012 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
