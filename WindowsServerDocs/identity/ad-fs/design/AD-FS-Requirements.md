---
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: 使用 SQL Server 的同盟伺服器陣列
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9f8375ffb73ff6be290b534d59e6ce5c8a7be27b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858061"
---
# <a name="ad-fs-requirements"></a>AD FS 需求

以下是您在部署 AD FS 時必須符合的各種需求：  
  
-   [憑證需求](AD-FS-Requirements.md#BKMK_1)  
  
-   [硬體需求](AD-FS-Requirements.md#BKMK_2)  
  
-   [軟體需求](AD-FS-Requirements.md#BKMK_3)  
  
-   [AD DS 需求](AD-FS-Requirements.md#BKMK_4)  
  
-   [設定資料庫需求](AD-FS-Requirements.md#BKMK_5)  
  
-   [瀏覽器需求](AD-FS-Requirements.md#BKMK_6)  
  
-   [外部網路需求](AD-FS-Requirements.md#BKMK_extranet)  
  
-   [網路需求](AD-FS-Requirements.md#BKMK_7)  
  
-   [屬性存放區需求](AD-FS-Requirements.md#BKMK_8)  
  
-   [應用程式需求](AD-FS-Requirements.md#BKMK_9)  
  
-   [驗證需求](AD-FS-Requirements.md#BKMK_10)  
  
-   [工作地點加入需求](AD-FS-Requirements.md#BKMK_11)  
  
-   [密碼編譯需求](AD-FS-Requirements.md#BKMK_12)  
  
-   [權限需求](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="certificate-requirements"></a><a name="BKMK_1"></a>憑證需求  
憑證在保護同盟伺服器、Web 應用程式 proxy、宣告\-感知應用程式和 Web 用戶端之間的通訊時，扮演最重要的角色。 憑證的需求會根據您設定的是同盟伺服器或 proxy 電腦而有所不同，如本節所述。  
  
**同盟伺服器憑證**  
  
|||  
|-|-|  
|**憑證類型**|**需求、支援 & 須知**|  
|**安全通訊端層 \(SSL\) 憑證：** 這是標準的 SSL 憑證，用於保護同盟伺服器和用戶端之間的通訊安全。|-此憑證必須是公開信任的\* X509 v3 憑證。<br />-所有存取任何 AD FS 端點的用戶端都必須信任此憑證。 強烈建議使用由公用 \(第三\-方\) 憑證授權單位單位 \(CA\)所發行的憑證。 您可以在測試實驗室環境中的同盟伺服器上，順利使用自我\-的已簽署 SSL 憑證。 不過，在生產環境中，我們建議您從公用 CA 取得憑證。<br />-支援 Windows Server 2012 R2 針對 SSL 憑證支援的任何金鑰大小。<br />-不支援使用 CNG 金鑰的憑證。<br />-與 Workplace Join\/裝置註冊服務一起使用時，AD FS 服務的 SSL 憑證主體別名必須包含值 enterpriseregistration，後面接著您組織的使用者主體名稱 \(UPN\) 尾碼，例如 enterpriseregistration.contoso.com。<br />-支援萬用字元憑證。 當您建立 AD FS 伺服器陣列時，系統會提示您提供 AD FS 服務的服務名稱 \(例如**adfs.contoso.com**。<br />-強烈建議對 Web 應用程式 Proxy 使用相同的 SSL 憑證。 不過，透過 Web 應用程式 Proxy 支援 Windows 整合式驗證端點，以及開啟 擴充保護驗證 \(預設設定\) 時，這是**必要**的。<br />-此憑證的主體名稱用來代表您所部署 AD FS 之每個實例的同盟服務名稱。 基於這個理由，您可能會想要考慮在任何新 CA 上選擇主體名稱，\-發行的憑證，其最能代表您公司或組織的名稱給合作夥伴。<br />    憑證的身分識別必須符合 federation service 名稱 \(例如 fs.contoso.com\)。身分識別可以是 dNSName 類型的主體別名延伸，如果沒有主體別名專案，則主體名稱會指定為一般名稱。 憑證中可以有多個主體替代名稱專案，前提是其中一個符合 federation service 名稱。<br />-   **重要：** 強烈建議您在 AD FS 伺服器陣列的所有節點以及 AD FS 伺服器陣列中的所有 Web 應用程式 proxy 之間使用相同的 SSL 憑證。|  
|**服務通訊憑證：** 此憑證可啟用 WCF 訊息安全性，以保護同盟伺服器之間的通訊安全。|-根據預設，SSL 憑證會用來做為服務通訊憑證。  但您也可以選擇將另一個憑證設定為服務通訊憑證。<br />-   **重要事項：** 如果您使用 ssl 憑證做為服務通訊憑證，當 ssl 憑證過期時，請務必將已更新的 ssl 憑證設定為您的服務通訊憑證。 這不會自動發生。<br />-此憑證必須受到使用 WCF 訊息安全性之 AD FS 的用戶端所信任。<br />-建議您使用公用 \(第三\-方\) 憑證授權單位單位 \(CA\)所發行的伺服器驗證憑證。<br />-服務通訊憑證不可以是使用 CNG 金鑰的憑證。<br />-您可以使用 AD FS 管理主控台來管理此憑證。|  
|**權杖\-簽署憑證：** 這是標準的 X509 憑證，用來安全地簽署同盟伺服器所發出的所有權杖。|-根據預設，AD FS 會建立具有2048位金鑰的自我\-簽署憑證。<br />-也支援 CA 發行的憑證，並且可以使用中的 [AD FS 管理] 嵌入式管理單元來加以變更\-<br />-CA 發行的憑證必須儲存 & 透過 CSP 加密提供者存取。<br />-權杖簽署憑證不可以是使用 CNG 金鑰的憑證。<br />-AD FS 不需要外部註冊的憑證來簽署權杖。<br />    AD FS 會在這些自我\-簽署的憑證到期之前自動予以更新，先將新憑證設定為次要憑證，以允許合作夥伴取用它們，然後在稱為「自動憑證變換」的進程中，將其翻轉至主要。我們建議您使用自動產生的預設憑證來簽署權杖。<br />    如果您的組織有需要針對權杖簽署設定不同憑證的原則，您可以在安裝時使用 Powershell 指定憑證 \(使用 Install\-AdfsFarm Cmdlet\)的– SigningCertificateThumbprint 參數。  安裝之後，您可以使用 AD FS 管理主控台或\-Get-adfscertificate 設定的 Powershell Cmdlet 來查看和管理權杖簽署憑證，並取得\-Get-adfscertificate。<br />    當外部註冊的憑證用於權杖簽署時，AD FS 不會執行自動憑證更新或變換。  此程式必須由系統管理員執行。<br />    若要在其中一個憑證接近過期時允許憑證變換，可以在 AD FS 中設定次要權杖簽署憑證。 根據預設，所有權杖簽署憑證會在同盟中繼資料中發佈，但 AD FS 只會使用主要權杖\-簽署憑證來實際簽署權杖。|  
|**權杖\-解密\/加密憑證：** 這是標準的 X509 憑證，用來解密\/加密任何傳入的權杖。 它也會在同盟中繼資料中發佈。|-根據預設，AD FS 會建立具有2048位金鑰的自我\-簽署憑證。<br />-也支援 CA 發行的憑證，並且可以使用中的 [AD FS 管理] 嵌入式管理單元來加以變更\-<br />-CA 發行的憑證必須儲存 & 透過 CSP 加密提供者存取。<br />-權杖\-解密\/加密憑證不能是使用 CNG 金鑰的憑證。<br />-根據預設，AD FS 會產生並使用它自己的內部產生和自我\-簽署的憑證，以進行權杖解密。  基於此目的，AD FS 不需要外部註冊的憑證。<br />    此外，AD FS 會在這些自我\-簽署的憑證到期之前自動加以更新。<br />    **我們建議您使用自動產生的預設憑證來進行權杖解密。**<br />    如果您的組織有需要針對權杖解密設定不同憑證的原則，您可以在安裝時使用 Powershell 指定憑證 \(使用 Install\-AdfsFarm Cmdlet\)的– DecryptionCertificateThumbprint 參數。  安裝之後，您可以使用 AD FS 管理主控台或\-Get-adfscertificate 設定的 Powershell Cmdlet 來查看和管理權杖解密憑證，並取得\-Get-adfscertificate。<br />    **當外部註冊的憑證用於權杖解密時，AD FS 不會執行自動憑證更新。 此程式必須由系統管理員執行**。<br />-AD FS 服務帳戶必須能夠存取本機電腦之個人存放區中的權杖\-簽署憑證的私密金鑰。 安裝程式會負責此動作。 您也可以使用中的 [AD FS 管理] 嵌入式\-管理單元來確保此存取權（如果您後續變更權杖\-簽署憑證）。|  
  
> [!CAUTION]  
> 用來進行權杖簽署和權杖解密\-加密的憑證對於同盟服務的穩定性而言相當重要。 管理自有權杖簽署和權杖解密\-加密憑證的客戶應確保這些憑證已完成備份，並可在復原事件期間獨立取得。  
  
> [!NOTE]  
> 在 AD FS 您可以將用於數位簽章的安全雜湊演算法 \(SHA\) 層級變更為 SHA\-1 或 SHA\-256 \(更安全的\)。 AD FS 不支援將憑證與其他雜湊方法搭配使用，例如 MD5 \(與 Makecert 命令\-行工具\)搭配使用的預設雜湊演算法。 基於安全性最佳作法的考慮，我們建議您使用 SHA\-256 \(預設會針對所有簽章設定\)。 只有在必須與不支援使用 SHA\-256 進行通訊的產品相交互操作的情況下（例如非\-的 Microsoft 產品或舊版的 AD FS），才建議使用 SHA\-1。  
  
> [!NOTE]  
> 在您收到來自 CA 的憑證之後，請確定會將所有憑證匯入本機電腦的個人憑證存放區。 您可以使用中的憑證 MMC 嵌入式管理\-，將憑證匯入到個人存放區。  
  
## <a name="hardware-requirements"></a><a name="BKMK_2"></a>硬體需求  
下列最低和建議硬體需求適用于 Windows Server 2012 R2 中的 AD FS 同盟伺服器：  
  
||||  
|-|-|-|  
|**硬體需求**|**最低需求**|**建議需求**|  
|CPU 速度|1.4 GHz 64\-位處理器|四\-核心，2 GHz|  
|RAM|512 MB|4 GB|  
|磁碟空間|32 GB|100 GB|  
  
## <a name="software-requirements"></a><a name="BKMK_3"></a>軟體需求  
下列 AD FS 需求適用于 Windows Server&reg; 2012 R2 作業系統內建的伺服器功能：  
  
-   針對外部網路存取，您必須將 Web 應用程式 Proxy 角色服務部署 \- Windows Server&reg; 2012 R2 遠端存取服務器角色的一部分。 Windows Server&reg; 2012 R2 中的 AD FS 不支援先前版本的同盟伺服器 proxy。  
  
-   同盟伺服器和 Web 應用程式 Proxy 角色服務無法安裝在同一部電腦上。  
  
## <a name="ad-ds-requirements"></a><a name="BKMK_4"></a>AD DS 需求  
**網域控制站需求**  
  
所有使用者網域中的網域控制站，以及 AD FS 伺服器所加入的網域，都必須執行 Windows Server 2008 或更新版本。  
  
> [!NOTE]  
> Windows server 2003 網域控制站環境的所有支援將于 Windows Server 2003 的延伸支援結束日期之後結束。 強烈建議客戶儘快升級其網域控制站。 如需 Microsoft 支援服務生命週期的詳細資訊，請造訪[此頁面](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO)。 對於特定于 Windows Server 2003 網域控制站環境的問題，將只會針對安全性問題發出修正，而且如果在 Windows Server 2003 的延伸支援到期之前，可以發出修正程式。  



>[!NOTE]
> AD FS 需要完整的可寫入網域控制站，才能正常運作，而不是唯讀網域控制站。 如果規劃的拓撲包含唯讀網域控制站，唯讀網域控制站就可以用於驗證，但是 LDAP 宣告處理將需要連線到可寫入的網域控制站。
  
**網域功能等級的需求\-  
  
所有使用者帳戶的網域以及 AD FS 伺服器所加入的網域，都必須在 Windows Server 2003 或更新版本的網域功能等級上運作。  
  
大部分的 AD FS 功能都不需要 AD DS 功能\-層級修改才能成功運作。 但是，如果憑證明確對應到 AD DS 中的使用者帳戶，則用戶端憑證驗證需要 Windows Server 2008 網域功能等級或更高等級才能成功運作。  
  
**結構描述需求**  
  
-   AD FS 不需要架構變更或功能\-層級修改來 AD DS。  
  
-   若要使用 Workplace Join 功能，AD FS 伺服器加入之樹系的架構必須設定為 Windows Server 2012 R2。  
  
**服務帳戶需求**  
  
-   任何標準服務帳戶都可以用來做為 AD FS 的服務帳戶。 也支援群組受管理的服務帳戶。 這需要至少一個網域控制站 \(建議您部署兩個或更多執行 Windows Server 2012 或更高版本的\)。  
  
-   若要讓 Kerberos 驗證在網域\-加入的用戶端和 AD FS 之間運作，必須在服務帳戶上將 ' HOST\/< adfs\_服務\_名稱 > ' 註冊為 SPN。 根據預設，如果有足夠的許可權可執行這項作業，AD FS 會在建立新的 AD FS 伺服器陣列時進行設定。  
  
-   在包含向 AD FS 服務驗證的使用者的每個使用者網域中，都必須信任 AD FS 服務帳戶。  
  
**網域需求**  
  
-   所有 AD FS 伺服器都必須加入 AD DS 網域。  
  
-   伺服器陣列中的所有 AD FS 伺服器都必須部署在單一網域中。  
  
-   AD FS 伺服器加入的網域必須信任每個使用者帳戶網域，其中包含向 AD FS 服務驗證的使用者。  
  
**多樹系需求**  
  
-   AD FS 伺服器加入的網域，必須信任包含向 AD FS 服務驗證之使用者的每個使用者帳戶網域或樹系。  
  
-   在包含向 AD FS 服務驗證的使用者的每個使用者網域中，都必須信任 AD FS 服務帳戶。  
  
## <a name="configuration-database-requirements"></a><a name="BKMK_5"></a>設定資料庫需求  
以下是根據設定存放區類型所適用的需求和限制：  
  
**WID**  
  
-   如果您有100或較少的信賴憑證者信任，WID 伺服器陣列的限制為30部同盟伺服器。  
  
-   在 WID 設定資料庫中，不支援 SAML 2.0 中的成品解析設定檔。  WID 設定資料庫不支援權杖重新執行偵測。 只有在 AD FS 做為同盟提供者，並從外部宣告提供者取用安全性權杖時，才會使用這項功能。  
  
-   只要伺服器數目不超過30個，就支援在不同的資料中心部署容錯移轉或地理負載平衡的 AD FS 伺服器。  
  
下表提供使用 WID 伺服器陣列的摘要。  使用它來規劃您的實施。  
  
||||  
|-|-|-|  
||1 \- 100 RP 信任|超過 100 RP 信任|  
|1 \- 30 AD FS 節點|支援 WID|不支援使用 WID \- SQL 需要|  
|超過30個 AD FS 節點|不支援使用 WID \- SQL 需要|不支援使用 WID \- SQL 需要|  
  
**SQL Server**  
  
針對 Windows Server 2012 R2 中的 AD FS，您可以使用 SQL Server 2008 及更新版本  
  
## <a name="browser-requirements"></a><a name="BKMK_6"></a>瀏覽器需求  
透過瀏覽器或瀏覽器控制項執行 AD FS 驗證時，您的瀏覽器必須符合下列需求：  
  
-   必須啟用 JavaScript  
  
-   Cookie 必須開啟  
  
-   必須支援伺服器名稱指示 \(SNI\)  
  
-   針對使用者憑證 & 裝置憑證驗證 \(workplace join 功能\)，瀏覽器必須支援 SSL 用戶端憑證驗證  
  
有數個主要的瀏覽器和平臺已通過驗證轉譯和功能，如下所列的詳細資料。 如果瀏覽器和裝置符合以上所列的需求，仍然會受到支援：  
  
|||  
|-|-|  
|**器**|**平臺**|  
|IE 10。0|Windows 7、Windows 8.1、Windows Server 2008 R2、Windows Server 2012、Windows Server 2012 R2|  
|IE 11。0|Windows7、Windows 8.1、Windows Server 2008 R2、Windows Server 2012、Windows Server 2012 R2|  
|Windows Web 驗證訊息代理程式|Windows 8.1|  
|Firefox \[v21\]|Windows 7，Windows 8。1|  
|Safari \[v7\]|iOS 6、Mac OS\-X 10。7|  
|Chrome \[v27\]|Windows 7、Windows 8.1、Windows Server 2012、Windows Server 2012 R2、Mac OS\-X 10。7|  
  
> [!IMPORTANT]  
> \- Firefox 的已知問題：使用裝置憑證識別裝置的 Workplace Join 功能在 Windows 平臺上無法運作。 Firefox 目前不支援使用布建至 Windows 用戶端上使用者憑證存放區的憑證來執行 SSL 用戶端憑證驗證。  
  
**Cookie**  
  
AD FS 會建立必須儲存在用戶端電腦上的會話\-型和持續性 cookie，以便在中提供登入\-、簽署\-、\-SSO \(上的單一登入\)和其他功能。 因此，用戶端瀏覽器必須設定為接受 Cookie。 用於驗證的 cookie 一律是安全超文字傳輸通訊協定，\(HTTPS\) 為源伺服器所撰寫的會話 cookie。 如果未將用戶端瀏覽器設定為允許這些 Cookie，AD FS 就無法正確運作。 永續性 Cookie 可用來保留宣告提供者的使用者選項。 您可以使用頁面中 AD FS sign\-設定檔案中的設定來停用它們。 基於安全性理由，需要對 TLS\/SSL 的支援。  
  
## <a name="extranet-requirements"></a><a name="BKMK_extranet"></a>外部網路需求  
若要提供 AD FS 服務的外部網路存取，您必須將 Web 應用程式 Proxy 角色服務部署為對 AD FS 服務的安全方式 proxy 驗證要求的外部網站角色。 這會提供 AD FS 服務端點的隔離，以及隔離所有的安全性金鑰 \(例如從來自網際網路的要求\) 權杖簽署憑證。 此外，諸如「軟外部網路」帳戶鎖定之類的功能都需要使用 Web 應用程式 Proxy。 如需 Web 應用程式 Proxy 的詳細資訊，請參閱[Web 應用程式 proxy](https://technet.microsoft.com/library/dn584107.aspx)。  
  
如果您想要使用第三個\-方 proxy 進行外部網路存取，第三個\-方 proxy 必須支援 HTTP：\/中定義的通訊協定[\/download.microsoft.com\/下載\/9\/5\/E\/95EF66AF\-9026\-4BB0\-A41D\-A4F81802D92C\/% 5bMS\-ADFSPIP %5 d. .pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf)。  
  
## <a name="network-requirements"></a><a name="BKMK_7"></a>網路需求  
適當地設定下列網路服務對於在組織中成功部署 AD FS 非常重要：  
  
**設定公司防火牆**  
  
位於 Web 應用程式 Proxy 與同盟伺服器陣列之間的防火牆，以及位於用戶端與 Web 應用程式 Proxy 之間的防火牆，都必須啟用 TCP 連接埠 443 的輸入。  
  
此外，如果用戶端使用者憑證驗證 \(使用 X509 使用者憑證進行 Clienttls 是驗證\) 是必要的，在 Windows Server 2012 R2 中 AD FS 會要求在用戶端與 Web 應用程式 Proxy 之間的防火牆上啟用 TCP 埠49443。 Web 應用程式 Proxy 與同盟伺服器之間的防火牆則不需要這麼做。  

> [!NOTE]
> 也請確定 AD FS 和 Web 應用程式 Proxy 伺服器上的任何其他服務都未使用埠49443。

**設定 DNS**  
  
-   若要進行內部網路存取，所有存取內部公司網路內 AD FS 服務的用戶端 \(內部網路\) 都必須能夠將 SSL 憑證所提供的 AD FS 服務名稱 \(名稱，解析\) 伺服器或 AD FS 伺服器的負載平衡器。  
  
-   針對外部網路存取，從公司網路外部存取 AD FS 服務的所有用戶端 \(外部網路\/網際網路\) 必須能夠將 SSL 憑證所提供 AD FS 服務名稱 \(名稱解析為 Web 應用程式 Proxy 伺服器或 Web 應用程式 Proxy 伺服器的負載平衡器。\)  
  
-   若要讓外部網路存取正常運作，DMZ 中的每個 Web 應用程式 Proxy 伺服器都必須能夠解析 SSL 憑證所提供的 AD FS 服務名稱 \(名稱，\) 到 AD FS 伺服器或 AD FS 伺服器的負載平衡器。 使用 DMZ 網路中的替代 DNS 伺服器，或使用 HOSTS 檔案變更本機伺服器解析，即可達成此目的。  
  
-   若要讓 Windows 整合式驗證在網路內和網路外部工作，以取得透過 Web 應用程式 Proxy 公開的端點子集，您必須使用 A 記錄 \(不是 CNAME\) 以指向負載平衡器。  
  
如需針對 federation service 和裝置註冊服務設定公司 DNS 的相關資訊，請參閱[Configure 同盟服務和 DRS 的公司 dns](https://technet.microsoft.com/library/dn486786.aspx)。  
  
如需設定 Web 應用程式 Proxy 之公司 DNS 的詳細資訊，請參閱[步驟1：設定 Web 應用程式 Proxy 基礎結構](https://technet.microsoft.com/library/dn383644.aspx)中的「設定 DNS」一節。  
  
如需有關如何使用 NLB 設定叢集 IP 位址或叢集 FQDN 的詳細資訊，請參閱在[HTTP：\/\/go.microsoft.com\/fwlink\/指定叢集參數？LinkId\=75282](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
## <a name="attribute-store-requirements"></a><a name="BKMK_8"></a>屬性存放區需求  
AD FS 需要至少一個用來驗證使用者的屬性存放區，並將這些使用者的安全性宣告解壓縮。 如需 AD FS 支援的屬性存放區清單，請參閱[屬性存放區的角色](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)。  
  
> [!NOTE]  
> AD FS 預設會自動建立 "Active Directory" 屬性存放區。 屬性存放區需求取決於您的組織是否做為帳戶夥伴 \(裝載同盟使用者\) 或裝載同盟應用程式\)\(的資源夥伴。  
  
**LDAP 屬性存放區**  
  
當您使用其他輕量型目錄存取協定 \(LDAP\)\-屬性存放區時，您必須連接到支援 Windows 整合式驗證的 LDAP 伺服器。 LDAP 連線字串也必須以 LDAP URL 的格式來書寫，如 RFC 2255 中所述。  
  
此外，AD FS 服務的服務帳戶也必須擁有在 LDAP 屬性存放區中取得使用者資訊的許可權。  
  
**SQL Server 屬性存放區**  
  
若要讓 Windows Server 2012 R2 中的 AD FS 順利運作，裝載 SQL Server 屬性存放區的電腦必須執行 Microsoft SQL Server 2008 或更高版本。 當您使用以 SQL\-為基礎的屬性存放區時，您也必須設定連接字串。  
  
**自訂屬性存放區**  
  
您可以開發自訂屬性存放區，以啟用進階案例。  
  
-   AD FS 內建的原則語言可以參考自訂屬性存放區，因此能夠增強下列任一個案例：  
  
    -   建立適用於本機驗證使用者的宣告  
  
    -   補充適用於外部驗證使用者的宣告  
  
    -   授權使用者取得權杖  
  
    -   授權服務代表使用者取得權杖  
  
    -   在 AD FS 發行給信賴憑證者的安全性權杖中發出額外的資料。  
  
-   所有自訂屬性存放區都必須建立在 .NET 4.0 或更高版本之上。  
  
當您使用自訂屬性存放區時，您可能也必須設定連接字串。 在這種情況下，您可以輸入您選擇的自訂程式碼，以啟用與您的自訂屬性存放區的連接。 在此情況下，連接字串是由自訂屬性存放區的開發人員所執行的一組名稱\/值配對。如需開發和使用自訂屬性存放區的詳細資訊，請參閱[屬性存放區總覽](https://go.microsoft.com/fwlink/?LinkId=190782)。  
  
## <a name="application-requirements"></a><a name="BKMK_9"></a>應用程式需求  
AD FS 支援使用下列通訊協定的宣告\-感知應用程式：  
  
-   WS\-同盟  
  
-   WS\-信任  
  
-   使用 IDPLite 的 SAML 2.0 通訊協定，SPLite & eGov 1.5 設定檔。  
  
-   OAuth 2.0 授權授與設定檔  
  
AD FS 也支援 Web 應用程式 Proxy 支援的任何非\-宣告\-感知應用程式的驗證和授權。  
  
## <a name="authentication-requirements"></a><a name="BKMK_10"></a>驗證需求  
**AD DS 驗證 \(主要驗證\)**  
  
針對內部網路存取，支援下列 AD DS 標準驗證機制：  
  
-   使用 Negotiate for Kerberos & NTLM 的 Windows 整合式驗證  
  
-   使用使用者名稱\/密碼的表單驗證  
  
-   在 AD DS 中使用對應至使用者帳戶之憑證的憑證驗證  
  
針對外部網路存取，支援下列驗證機制：  
  
-   使用使用者名稱\/密碼的表單驗證  
  
-   在 AD DS 中使用對應至使用者帳戶之憑證的憑證驗證  
  
-   使用 Negotiate 的 Windows 整合式驗證 \(僅適用于 WS\-信任端點的 NTLM\)，接受 Windows 整合式驗證。  
  
憑證驗證：  
  
-   延伸到可以釘選保護的智慧卡。  
  
-   AD FS 不會提供輸入其 pin 之使用者的 GUI，而且必須是使用用戶端 TLS 時所顯示之用戶端作業系統的一部分。  
  
-   智慧卡的讀取器和密碼編譯服務提供者 \(CSP\) 必須在瀏覽器所在的電腦上工作。  
  
-   智慧卡憑證必須在所有 AD FS 伺服器和 Web 應用程式 Proxy 伺服器上，連結到受信任的根目錄。  
  
-   憑證必須透過下列任一方法對應至 AD DS 中的使用者帳戶：  
  
    -   憑證主體名稱會對應至 AD DS 中使用者帳戶的 LDAP 辨別名稱。  
  
    -   憑證主體 altname 延伸具有 AD DS 中使用者帳戶的使用者主體名稱 \(UPN\)。  
  
對於在內部網路中使用 Kerberos 的無縫 Windows 整合式驗證，  
  
-   服務名稱必須是信任的網站或近端內部網路網站的一部分。  
  
-   此外，您必須在執行 > 伺服器陣列的服務帳戶上設定 < adfs\_服務\_名稱 AD FS SPN 的主機\/。  
  
**多重\-因素驗證**  
  
AD FS 支援 AD DS\) 使用提供者模型所支援的其他驗證 \(，而廠商\/客戶可以建立自己的多個\-要素驗證介面卡，供系統管理員在登入期間註冊及使用。  
  
每個 MFA 介面卡都必須建立在 .NET 4.5 之上。  
  
如需 MFA 的詳細資訊，請參閱透過[其他多因素驗證管理機密應用程式的風險](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。  
  
**裝置驗證**  
  
AD FS 在使用者工作場所加入裝置的動作期間，使用裝置註冊服務所布建的憑證來支援裝置驗證。  
  
## <a name="workplace-join-requirements"></a><a name="BKMK_11"></a>工作地點加入需求  
使用者可以使用 AD FS，將其裝置加入組織的工作場所。 AD FS 中的裝置註冊服務可支援此功能。 如此一來，使用者就能在 AD FS 支援的應用程式中獲得 SSO 的額外優點。 此外，系統管理員可以藉由限制只有已加入工作場所的裝置才能存取應用程式，來管理風險。 以下是啟用此案例的下列需求。  
  
-   AD FS 支援 Windows 8.1 和 iOS 5\+ 裝置的 workplace join  
  
-   若要使用 Workplace Join 功能，AD FS 伺服器加入之樹系的架構必須是 Windows Server 2012 R2。  
  
-   AD FS 服務之 SSL 憑證的主體替代名稱必須包含值 enterpriseregistration，後面接著您組織的使用者主體名稱 \(UPN\) 尾碼，例如 enterpriseregistration.corp.contoso.com。  
  
## <a name="cryptography-requirements"></a><a name="BKMK_12"></a>密碼編譯需求  
下表提供有關 AD FS 權杖簽署、權杖加密\/解密功能的其他密碼編譯支援資訊：  
  
||||  
|-|-|-|  
|**演算法**|**金鑰長度**|**\/應用程式\/批註的通訊協定**|  
|TripleDES –預設 192 \(支援 192-256\) \- [HTTP：\/\/www.w3.org\/2001\/04\/xmlenc\#TripleDES\-cbc](http://www.w3.org/2001/04/xmlenc#tripledes-cbc)|>\= 192|支援解密安全性權杖的演算法。 不支援使用此演算法來加密安全性權杖。|  
|AES128 \- [HTTP：\/\/www.w3.org\/2001\/04\/xmlenc\#AES128\-cbc](http://www.w3.org/2001/04/xmlenc#aes128-cbc)|128|支援解密安全性權杖的演算法。 不支援使用此演算法來加密安全性權杖。|  
|AES192 \- [HTTP：\/\/www.w3.org\/2001\/04\/xmlenc\#AES192\-cbc](http://www.w3.org/2001/04/xmlenc#aes192-cbc)|192|支援解密安全性權杖的演算法。 不支援使用此演算法來加密安全性權杖。|  
|AES256 \- [HTTP：\/\/www.w3.org\/2001\/04\/xmlenc\#AES256\-cbc](http://www.w3.org/2001/04/xmlenc#aes256-cbc)|256|**預設值**。 支援用來加密安全性權杖的演算法。|  
|TripleDESKeyWrap \- [HTTP：\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-tripledes](http://www.w3.org/2001/04/xmlenc#kw-tripledes)|.NET 4.0\+ 支援的所有金鑰大小|支援加密加密安全性權杖之對稱金鑰的演算法。|  
|AES128KeyWrap \- [HTTP：\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes128](http://www.w3.org/2001/04/xmlenc#kw-aes128)|128|支援加密安全性權杖之對稱金鑰的演算法。|  
|AES192KeyWrap \- [HTTP：\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes192](http://www.w3.org/2001/04/xmlenc#kw-aes192)|192|支援加密安全性權杖之對稱金鑰的演算法。|  
|AES256KeyWrap \- [HTTP：\/\/www.w3.org\/2001\/04\/xmlenc\#kw\-aes256](http://www.w3.org/2001/04/xmlenc#kw-aes256)|256|支援加密安全性權杖之對稱金鑰的演算法。|  
|RsaV15KeyWrap \- [HTTP：\/\/www.w3.org\/2001\/04\/xmlenc\#rsa\-1\_5](http://www.w3.org/2001/04/xmlenc#rsa-1_5)|1024|支援加密安全性權杖之對稱金鑰的演算法。|  
|RsaOaepKeyWrap \- [HTTP：\/\/www.w3.org\/2001\/04\/xmlenc\#rsa\-oaep\-rsa-oaep-mgf1p 代表](http://www.w3.org/2001/04/xmlenc#rsa-oaep-mgf1p)|1024|Default： 支援加密安全性權杖之對稱金鑰的演算法。|  
|SHA1\-[HTTP：\/\/www.w3.org\/圖片\/DSig\/SHA1\_1\_0 .html](http://www.w3.org/PICS/DSig/SHA1_1_0.html)|N\/A|由成品 SourceId 產生中的 AD FS 伺服器使用：在此案例中，STS 會根據 SAML 2.0 標準\) 中的建議使用 SHA1 \(，以建立成品 sourceiD 的簡短160位值。<p>ADFS web 代理 \(程式也會使用 WS2003 時間範圍內的舊版元件\) 來識別「上次更新」時間值的變更，讓它知道何時要從 STS 更新資訊。|  
|SHA1withRSA\-<p>[HTTP：\/\/www.w3.org\/圖片\/DSig\/RSA\-SHA1\_1\_0 .html](http://www.w3.org/PICS/DSig/RSA-SHA1_1_0.html)|N\/A|用於 AD FS 伺服器驗證 SAML AuthenticationRequest 簽章、簽署成品解析要求或回應、建立權杖\-簽署憑證的情況。<p>在這些情況下，SHA256 是預設值，只有在合作夥伴 \(信賴憑證者\) 無法支援 SHA256 且必須使用 SHA1 時，才會使用 SHA1。|  
  
## <a name="permissions-requirements"></a><a name="BKMK_13"></a>權限需求  
執行安裝的系統管理員和 AD FS 的初始設定，必須擁有本機網域中的網域系統管理員許可權 \(換句話說，即為同盟伺服器加入的網域。\)  
  
## <a name="see-also"></a>另請參閱  
[Windows Server 2012 R2 中的 AD FS 設計指南](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  
