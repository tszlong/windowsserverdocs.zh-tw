---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: AD FS 2016 需求
description: 安裝 Active Directory Federation Services 的需求。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 235030ea913f2fe1860efaa00bdb4641ac56750d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188678"
---
# <a name="ad-fs-requirements"></a>AD FS 需求



部署 AD FS 的需求如下：  
  
-   [憑證需求](ad-fs-requirements.md#BKMK_1)  
  
-   [硬體需求](ad-fs-requirements.md#BKMK_2)  
  
-   [Proxy 需求](ad-fs-requirements.md#BKMK_3)  
  
-   [AD DS 需求](ad-fs-requirements.md#BKMK_4)  
  
-   [設定資料庫需求](ad-fs-requirements.md#BKMK_5)  
  
-   [瀏覽器需求](ad-fs-requirements.md#BKMK_6)  

-   [網路需求](ad-fs-requirements.md#BKMK_7)  
  
-   [權限需求](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>憑證需求  
  
### <a name="ssl-certificates"></a>SSL 憑證

每個 AD FS 和 Web 應用程式 Proxy 伺服器已服務的 HTTPS 要求至 federation service 的 SSL 憑證。  Web Application Proxy 可以有額外的 SSL 憑證，來對已發行的應用程式的服務要求。

**建議：** 所有的 AD FS 同盟伺服器和 Web 應用程式 proxy 中使用相同的 SSL 憑證。 

**需求：**

同盟伺服器上的 SSL 憑證必須符合下列需求
- 憑證是公開信任 （用於生產部署）
- 憑證包含伺服器驗證增強金鑰使用方法 (EKU) 值
- 憑證包含 federation service 名稱，例如"fs.contoso.com 」 中的主體或主體別名 (SAN)
- 連接埠 443 上的使用者憑證驗證，憑證包含"certauth。\<federation service 名稱\>"，例如 SAN 中的 「 certauth.fs.contoso.com"
- SAN 裝置註冊，或若要使用 windows 10 用戶端的內部部署資源的新式驗證，必須包含"enterpriseregistration。\<upn 尾碼\>"代表您組織中的使用中每個 UPN 尾碼。

Web Application Proxy 的 SSL 憑證必須符合下列需求
- 如果使用 proxy 來使用 Windows 整合式驗證的 proxy 的 SSL 憑證的 AD FS proxy 要求必須相同 （使用相同的索引鍵） 做為同盟伺服器 SSL 憑證
- 如果 「 ExtendedProtectionTokenCheck 」 是 AD FS 屬性啟用 （AD FS 中設定的預設值），proxy 的 SSL 憑證必須是相同 （使用相同的索引鍵） 做為同盟伺服器 SSL 憑證
- 否則為 proxy 的 SSL 憑證需求完全一樣的同盟伺服器 SSL 憑證

### <a name="service-communication-certificate"></a>服務通訊憑證
此憑證不需要大部分的 AD FS 案例，包括 Azure AD 和 Office 365。 根據預設，AD FS 會設定為服務通訊憑證的初始設定時所提供的 SSL 憑證。

**建議：**
- 當您使用 ssl，請使用相同的憑證。  

### <a name="token-signing-certificate"></a>權杖簽署憑證
此憑證用來簽署發行的權杖給信賴憑證者的合作對象，讓信賴憑證者的合作對象應用程式必須能識別的憑證和與其相關聯的金鑰做為已知且受信任。 當權杖簽署憑證的變更，例如到期時，您會設定新的憑證時，就必須更新所有信賴憑證者的合作對象。

**建議：** 使用 AD FS 預設值，在內部產生的自我簽署的權杖簽署憑證。  

**需求：**
- 如果您的組織會要求來自企業 PKI 憑證可用來簽署權杖，這可以使用安裝 AdfsFarm cmdlet SigningCertificateThumbprint 參數。
- 您是否使用內部產生的預設憑證，或外部註冊權杖簽署憑證已變更您的憑證，必須確定所有信賴憑證者的合作對象則會以新的憑證資訊更新。  否則，不會更新任何信賴憑證者合作對象的登入將會失敗。

### <a name="token-encryptingdecrypting-certificate"></a>權杖加密/解密憑證
加密簽發給 AD FS 權杖的宣告提供者會使用此憑證。

**建議：** 使用 AD FS 預設值，在內部產生的自我簽署的權杖解密憑證。  

**需求：**
- 如果您的組織會要求來自企業 PKI 憑證可用來簽署權杖，這可以使用安裝 AdfsFarm cmdlet DecryptingCertificateThumbprint 參數。
- 您是否使用內部產生的預設憑證，或外部註冊權杖解密憑證已變更您的憑證，必須確定所有的宣告提供者會更新新的憑證資訊。  否則，請登入可讓您使用任何宣告提供者不會更新將會失敗。
  
> [!CAUTION]  
> 憑證用於語彙基元\-簽署和權杖\-解密\/加密至關重要的 Federation Service 的穩定性。 客戶管理自己的語彙基元\-簽署和權杖\-解密\/加密憑證，應該確定這些憑證備份，並可獨立復原事件期間。  

### <a name="user-certificates"></a>使用者憑證
- 當使用的 x509 使用者憑證驗證，使用 AD FS 時，所有的使用者憑證必須鏈結到信任的 AD FS 和 Web 應用程式 Proxy 伺服器的根憑證授權單位。

## <a name="BKMK_2"></a>硬體需求  
AD FS 和 Web 應用程式 Proxy 的硬體需求 （實體或虛擬） 被閘道上，因此您應該調整大小，您的伺服器陣列的處理容量。  
- 使用[AD FS 2016 容量規劃試算表](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)來判斷您需要的 AD FS 和 Web 應用程式 Proxy 伺服器的數目。

AD FS 的記憶體和磁碟需求是相當靜態的請參閱下表：


|**硬體需求**|**最低需求**|**建議的需求**|
|----- | ----- |-----|
|RAM|2 GB|4 GB |
|磁碟空間|32 GB|100 GB |

**SQL Server 的硬體需求**

如果您使用 AD FS 組態資料庫的 SQL Server，調整大小的最基本的 SQL Server 建議根據 SQL Server。  AD FS 資料庫大小太小，而且 AD FS 不會讓資料庫執行個體上的大量的處理負載。  AD FS，不過，連接到資料庫多次在驗證期間，因此應該強固網路連線。  不幸的是，AD FS 設定資料庫不支援 SQL Azure。
  
## <a name="BKMK_3"></a>Proxy 需求  
  
-   外部網路存取，您必須部署 Web 應用程式 Proxy 角色服務\-遠端存取伺服器角色的一部分。 

-   協力廠商 proxy 必須支援[MS ADFSPIP 通訊協定](https://msdn.microsoft.com/en-us/library/dn392811.aspx)AD FS proxy 受到支援。  如需清單的第 3 方供應商請參閱[常見問題集](AD-FS-FAQ.md#what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip)。

-   AD FS 2016 需要 Windows Server 2016 上的 Web 應用程式 Proxy 伺服器。  舊版 proxy 無法設定 AD FS 2016 伺服器陣列 2016年伺服器陣列行為層級執行。
  
-   無法在相同電腦上安裝同盟伺服器和 Web 應用程式 Proxy 角色服務。  
  
## <a name="BKMK_4"></a>AD DS 需求  
**網域控制站的需求**  
  
- AD FS 需要執行 Windows Server 2008 或更新版本的網域控制站。

- Microsoft passport for Work 需要至少一個 Windows Server 2016 網域控制站。
  
> [!NOTE]  
> 所有與 Windows Server 2003 網域控制站的環境支援已結束。 請瀏覽[本頁](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO)如需 Microsoft 支援週期的其他資訊。  
  
**網域功能\-層級需求**  
  
 - 所有的使用者帳戶網域和 AD FS 伺服器所加入的網域必須在 Windows Server 2003 或更高版本的網域功能等級進行操作。  
  
 - Windows Server 2008 網域功能層級或更高版本都需要用戶端憑證驗證憑證明確對應到使用者的帳戶在 AD DS 中。  
  
**結構描述的需求**  
  
-   AD FS 2016 的全新安裝需要 Active Directory 2016 結構描述 （最小版本 85）。

-   引發到 2016年層級的 AD FS 伺服器陣列行為層級 (FBL) 需要 Active Directory 2016 結構描述 （最小版本 85）。  

  
**服務帳戶需求**  
  
-   可以使用任何標準的網域帳戶當做服務帳戶，適用於 AD FS。 也支援群組受管理的服務帳戶。 當您設定 AD FS 時，會自動加入在執行階段所需的權限。

-   AD 服務帳戶所需的使用者權限指派為 [以服務方式登入]

-   產生安全性稽核和 [以服務方式登入]，就會是 'NT Service\adfssrv' 和' NT Service\drs' 所需的使用者權限指派。

-   群組受管理的服務帳戶需要至少一個執行 Windows Server 2012 或更新版本的網域控制站。  GMSA 必須駐留在預設 ' CN = 受控服務帳戶的容器。  

-   Kerberos 驗證，服務主體名稱為 '`HOST/<adfs\_service\_name>`' 必須在 AD FS 服務帳戶中註冊。 根據預設，AD FS 會設定此建立新的 AD FS 伺服器陣列時。  如果失敗，例如發生衝突或沒有足夠的權限，您會看到一則警告，您應該手動將它加入。  
   
**網域需求**  
  
-   所有 AD FS 伺服器必須都是 AD DS 網域加入。  
  
-   在伺服陣列中的所有 AD FS 伺服器必須都部署在相同的網域。  
   
**多樹系需求**  
  
-   加入的 AD FS 伺服器的網域必須信任每個網域或樹系包含 AD FS 服務進行驗證的使用者。  

-   AD FS 服務帳戶所隸屬的群組、 樹系，必須信任所有的使用者登入樹系。 
  
-   AD FS 服務帳戶必須為每個包含使用者向 AD FS 服務的網域中的使用者屬性的權限。  
  
## <a name="BKMK_5"></a>設定資料庫需求  
本章節描述的需求和限制分別使用 Windows 內部資料庫 (WID) 或 SQL Server 資料庫的 AD FS 伺服器陣列：  
  
**WID**  
  
-   在 WID 伺服器陣列中不支援 SAML 2.0 的成品解析設定檔。    

-   權杖重新執行偵測功能無法支援 WID 伺服器陣列。 （這項功能只使用 AD FS 是在其中做為同盟提供者，取用來自外部的宣告提供者的安全性權杖的案例中）。  
  
下表提供 AD FS 伺服器數目的摘要在 WID vs 中支援的 SQL Server 伺服器陣列。    
  
  
|| 1-100 信賴憑證者 (rp) 信任 AD FS 中設定 | 100 個以上的 RP 信任設定  |
| --- |--- | --- |
|1-30年個 AD FS 伺服器|WID 支援|不支援使用 WID-需要 SQL Server |
|30 多個 AD FS 伺服器|不支援使用 WID-需要 SQL Server|不支援使用 WID-需要 SQL Server  
  
**SQL Server**  
  
- Windows Server 2016 中的 AD FS，則支援 SQL Server 2008 和更新版本。

- SQL Server 伺服器陣列支援 SAML 成品解析和權杖重新執行偵測。  
  
## <a name="BKMK_6"></a>瀏覽器需求  
透過瀏覽器或瀏覽器控制項執行 AD FS 驗證時，您的瀏覽器必須符合下列需求：  
  
-   必須啟用 JavaScript  
  
-   單一登入，用戶端瀏覽器必須設定成允許 cookie  
  
-   伺服器名稱指示\(SNI\)必須支援  
  
-   使用者憑證和裝置憑證驗證的瀏覽器必須支援 SSL 用戶端憑證驗證  

-   針對無縫式的登入使用 Windows 整合式驗證時，federation service 名稱 (例如 https:\/\/fs.contoso.com) 必須在近端內部網路或信任的網站區域設定。
## <a name="BKMK_7"></a>網路需求  
 
**防火牆需求**  
  
這兩個位於 Web 應用程式 Proxy 與同盟伺服器陣列和用戶端和 Web 應用程式 Proxy 之間的防火牆之間的防火牆必須有 TCP 連接埠 443 已啟用輸入。  
  
此外，如果用戶端使用者憑證驗證\(clientTLS 驗證使用 X509 使用者憑證\)需要 certauth 端點連接埠 443 上的未啟用，AD FS 2016 需要啟用 TCP 連接埠 49443輸入用戶端和 Web 應用程式 Proxy 之間的防火牆上。 這不需要在 Web 應用程式 Proxy 與同盟伺服器之間的防火牆上\)。 

如需其他有關混合式連接埠需求，請參閱[混合式身分識別連接埠和通訊協定](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports)。 

如需詳細資訊，請參閱[保護 Active Directory Federation Services 的最佳做法](..\deployment\Best-Practices-Securing-AD-FS.md)
  
**DNS 需求**  
  
-   內部網路存取，所有的用戶端存取 AD FS 服務內部的公司網路內\(內部網路\)必須能夠解析的負載平衡器的 AD FS 伺服器或 AD FS 伺服器的 AD FS 服務名稱。  
  
-   外部網路存取，所有的用戶端存取 AD FS 服務從公司網路外部\(extranet\/網際網路\)必須能夠解析的負載平衡器的 Web 應用程式 Proxy 伺服器的 AD FS 服務名稱或Web 應用程式 Proxy 伺服器。  
  
-   在 DMZ 中的每個 Web Application Proxy 伺服器必須能夠解析的負載平衡器的 AD FS 伺服器或 AD FS 伺服器的 AD FS 服務名稱。 這可以使用替代的 DNS 伺服器，在 DMZ 網路或變更使用的主機檔案的本機伺服器解析來達成。  
  
-   Windows 整合式驗證，您必須使用 DNS A 記錄\(不是 CNAME\)同盟服務名稱。  

-   使用者憑證驗證連接埠 443"certauth。\<federation service 名稱\>」 必須設定 DNS 將解析為同盟伺服器或 web 應用程式 proxy 中。

-   裝置註冊，或若要使用 windows 10 用戶端，「 enterpriseregistration 內部部署資源新式驗證。\<upn 尾碼\>"，您組織中的使用中的每個 UPN 尾碼必須解析為同盟伺服器或 web 應用程式 proxy 設定的。

**負載平衡器的需求**
- 不得在負載平衡器終止 SSL。 AD FS 支援多個使用案例，這會中斷時終止 SSL 憑證驗證。 任何使用案例不支援在負載平衡器上終止 SSL。 
- 建議使用負載平衡器支援 SNI。 在事件沒有出現，請使用後援 AD FS 上的繫結 0.0.0.0 / Web Application Proxy 伺服器應該提供因應措施。
- 建議使用 HTTP (不是 HTTPS) 的健康情況探查端點來路由傳送流量進行負載平衡器健康情況檢查。 這可避免 SNI 任何問題。 這些探查端點的回應是 HTTP 200 OK，並在本機服務與後端服務上的任何相依性。 可以透過使用路徑 '/ adfs/探查' 的 HTTP 存取的 HTTP 探查
    - http://&lt;Web 應用程式 Proxy 名稱 &gt; /adfs/探查
    - http://&lt;ADFS 伺服器名稱 &gt; /adfs/探查
    - http://&lt;Web 應用程式 Proxy IP 位址 &gt; /adfs/探查
    - http://&lt;ADFS IP 位址 &gt; /adfs/探查
- 不建議使用 DNS 循環配置資源做為負載平衡的方式。 使用這種類型的負載平衡不提供自動化的方式來使用健全狀況探查的負載平衡器移除節點。 
- 不建議用於負載平衡器中的 AD fs 的驗證流量的 IP 為基礎的工作階段親和性 」 或 「 黏性工作階段。 使用郵件用戶端的舊版驗證通訊協定連接至 Office 365 郵件服務 (Exchange Online) 時，這可能會導致特定節點的多載。 

## <a name="BKMK_13"></a>權限需求  
AD FS 伺服器上，執行安裝和初始設定的 AD FS 系統管理員必須具有本機系統管理員權限。  如果本機系統管理員沒有在 Active Directory 中建立物件的權限，他們必須先具備網域系統管理員建立所需的 AD 物件，然後設定 AD FS 伺服器陣列使用 AdminConfiguration 參數。  
  
  

