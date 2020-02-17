---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: AD FS 2016 需求
description: Active Directory 同盟服務的安裝需求。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c8ab160699bc6a961f4fbed6c58cf072a395a313
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407417"
---
# <a name="ad-fs-requirements"></a>AD FS 需求



以下是 AD FS 的部署需求：  
  
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

每個 AD FS 和 Web 應用程式 Proxy 伺服器都有 SSL 憑證可服務對同盟服務所提出的 HTTPS 要求。  Web 應用程式 Proxy 可能會有額外的 SSL 憑證可服務對已發佈的應用程式所提出的要求。

**建議：** 請針對所有 AD FS 同盟伺服器和 Web 應用程式 Proxy 使用相同的 SSL 憑證。 

**需求：**

同盟伺服器上的 SSL 憑證必須符合下列需求
- 憑證是公開信任的 (適用於生產部署)
- 憑證包含伺服器驗證增強金鑰使用方法 (EKU) 值
- 憑證包含同盟服務名稱，例如主體或主體別名 (SAN) 中的 "fs.contoso.com"
- 若為連接埠 443 上的使用者憑證驗證，憑證會包含 "certauth.\<同盟服務名稱\>"，例如 SAN 中的 "certauth.fs.contoso.com"
- 針對裝置註冊，或使用 Windows 10 之前的用戶端對內部部署資源所進行的新式驗證，SAN 則必須針對組織使用中的每個 UPN 尾碼來包含 "enterpriseregistration.\<upn 尾碼\>"。

Web 應用程式 Proxy 上的 SSL 憑證必須符合下列需求
- 如果 Proxy 用來代理使用 Windows 整合式驗證的 AD FS 要求，則 Proxy SSL 憑證必須與同盟伺服器 SSL 憑證相同 (使用相同金鑰)
- 如果已啟用 AD FS 屬性 "ExtendedProtectionTokenCheck" (AD FS 中的預設設定)，則 Proxy SSL 憑證必須與同盟伺服器 SSL 憑證相同 (使用相同金鑰)
- 否則，Proxy SSL 憑證的需求會與同盟伺服器 SSL 憑證的需求相同

### <a name="service-communication-certificate"></a>服務通訊憑證
大部分的 AD FS 案例 (包括 Azure AD 和 Office 365) 都不需要此憑證。 根據預設，AD FS 會將初始設定時所提供的 SSL 憑證設定為服務通訊憑證。

**建議：**
- 使用與您用於 SSL 相同的憑證。  

### <a name="token-signing-certificate"></a>權杖簽署憑證
此憑證可用來簽署對信賴憑證者所發行的權杖，因此信賴憑證者應用程式必須將憑證及其相關聯的金鑰辨識為已知且受信任。 當權杖簽署憑證變更時 (例如，當憑證過期且您設定了新憑證時)，所有信賴憑證者都必須隨之更新。

**建議：** 使用 AD FS 內部產生的預設自我簽署權杖簽署憑證。  

**需求：**
- 如果您的組織要求將來自企業 PKI 的憑證用於權杖簽署，則可以使用 Install-AdfsFarm Cmdlet 的 SigningCertificateThumbprint 參數來完成此操作。
- 無論您使用內部產生的預設憑證還是外部註冊的憑證，當權杖簽署憑證變更時，您就必須確定所有的信賴憑證者都有使用新的憑證資訊加以更新。  否則，在登入未更新的任何信賴憑證者時便會失敗。

### <a name="token-encryptingdecrypting-certificate"></a>權杖加密/解密憑證
會對核發給 AD FS 的權杖進行加密的宣告提供者會使用此憑證。

**建議：** 使用 AD FS 內部產生的預設自我簽署權杖解密憑證。  

**需求：**
- 如果您的組織要求將來自企業 PKI 的憑證用於權杖簽署，則可以使用 Install-AdfsFarm Cmdlet 的 DecryptingCertificateThumbprint 參數來完成此操作。
- 無論您使用內部產生的預設憑證還是外部註冊的憑證，當權杖解密憑證變更時，您就必須確定所有的宣告提供者都有使用新的憑證資訊加以更新。  否則，使用任何未更新的宣告提供者來登入時便會失敗。
  
> [!CAUTION]  
> 用來進行權杖簽署和權杖解密\/加密的憑證對於同盟服務的穩定性而言相當重要。 管理自有權杖簽署和權杖解密\/加密憑證的客戶應確保這些憑證已完成備份，並可在復原事件期間獨立取得。  

### <a name="user-certificates"></a>使用者憑證
- 搭配 AD FS 使用 x509 使用者憑證驗證時，所有使用者憑證都必須鏈結到 AD FS 和 Web 應用程式 Proxy 伺服器所信任的根憑證授權單位。

## <a name="BKMK_2"></a>硬體需求  
AD FS 和 Web 應用程式 Proxy 硬體需求 (實體或虛擬) 會受到 CPU 的節制，因此請調整伺服器陣列的大小以獲得處理容量。  
- 使用 [AD FS 2016 容量規劃試算表](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)來判斷您需要的 AD FS 和 Web 應用程式 Proxy 伺服器數目。

AD FS 的記憶體和磁碟需求相當靜態，請參閱下表：


|**硬體需求**|**最低需求**|**建議需求**|
|----- | ----- |-----|
|RAM|2 GB|4 GB |
|磁碟空間|32 GB|100 GB |

**SQL Server 的硬體需求**

如果您使用 SQL Server 來作為 AD FS 的設定資料庫，請根據最基本的 SQL Server 建議來調整 SQL Server 的大小。  AD FS 的資料庫規模很小，因此 AD FS 不會在資料庫執行個體上放置大量的處理負載。  不過，AD FS 的確會在驗證期間連線至資料庫多次，因此網路連線必須穩固。  可惜的是，SQL Azure 不支援作為 AD FS 的設定資料庫。
  
## <a name="BKMK_3"></a>Proxy 需求  
  
-   若要能夠存取外部網路，您必須部署 Web 應用程式 Proxy 角色服務 \- 遠端存取伺服器角色的一部分。 

-   第三方 Proxy 必須支援 [MS-ADFSPIP 通訊協定](https://msdn.microsoft.com/library/dn392811.aspx)才可支援作為 AD FS Proxy。  如需第三方廠商的清單，請參閱[常見問題集](AD-FS-FAQ.md)。

-   AD FS 2016 需要將 Web 應用程式 Proxy 伺服器放在 Windows Server 2016 上。  您無法針對在 2016 陣列行為層級執行的 AD FS 2016 伺服器陣列，設定舊版的 Proxy。
  
-   同盟伺服器和 Web 應用程式 Proxy 角色服務無法安裝在同一部電腦上。  
  
## <a name="BKMK_4"></a>AD DS 需求  
**網域控制站需求**  
  
- AD FS 會要求網域控制站執行 Windows Server 2008 或更新版本。

- Microsoft Passport for Work 至少需要一個 Windows Server 2016 網域控制站。
  
> [!NOTE]  
> 對擁有 Windows Server 2003 網域控制站的環境所提供的支援已全部終止。 如需 Microsoft 支援服務生命週期的其他資訊，請瀏覽[此頁面](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO)。  
  
**網域功能等級的需求**  
  
 - 所有使用者帳戶的網域以及 AD FS 伺服器所加入的網域，都必須在 Windows Server 2003 或更新版本的網域功能等級上運作。  
  
 - 如果憑證明確對應到 AD DS 中的使用者帳戶，則用戶端憑證驗證需要 Windows Server 2008 網域功能等級或更高等級。  
  
**結構描述需求**  
  
-   新安裝的 AD FS 2016 需要 Active Directory 2016 結構描述 (最低版本 85)。

-   若要將 AD FS 陣列行為層級 (FBL) 提高至 2016 層級，則需要使用 Active Directory 2016 結構描述 (最低版本 85)。  

  
**服務帳戶需求**  
  
-   任何標準的網域帳戶都可以作為 AD FS 的服務帳戶。 也支援群組受管理的服務帳戶。 當您設定 AD FS 時，系統會自動新增執行階段所需的權限。

-   AD 服務帳戶所需的使用者權限指派是「以服務方式登入」

-   'NT Service\adfssrv' 和 'NT Service\drs' 所需的使用者權限指派是「產生安全性稽核」和「以服務方式登入」。

-   若要群組受管理的服務帳戶，則需要至少一個執行 Windows Server 2012 或更新版本的網域控制站。  GMSA 必須存留在預設的 'CN=Managed Service Accounts' 容器底下。  

-   若要進行 Kerberos 驗證，則必須在 AD FS 服務帳戶上註冊服務主體名稱 ‘`HOST/<adfs\_service\_name>`'。 根據預設，AD FS 會在建立新的 AD FS 伺服器陣列時設定此項目。  如果這項作業失敗，例如發生衝突或權限不足，則會看到警告，並請以手動方式來新增。  
   
**網域需求**  
  
-   所有 AD FS 伺服器都必須加入 AD DS 網域。  
  
-   伺服器陣列中的所有 AD FS 伺服器都必須部署在相同的網域中。  
   
**多樹系需求**  
  
-   AD FS 伺服器所加入的網域必須信任向 AD FS 服務進行驗證的使用者所在的每個網域或樹系。  

-   AD FS 服務帳戶所屬的樹系，必須信任所有使用者登入樹系。 
  
-   AD FS 服務帳戶必須有權在向 AD FS 服務進行驗證的使用者所在的每個網域中讀取使用者屬性。  
  
## <a name="BKMK_5"></a>設定資料庫需求  
本節會說明分別使用 Windows 內部資料庫 (WID) 或 SQL Server 作為資料庫的 AD FS 伺服器陣列有何需求和限制：  
  
**WID**  
  
-   WID 伺服器陣列中不支援 SAML 2.0 的成品解析設定檔。    

-   WID 伺服器陣列不支援權杖重新偵測。 (這項功能僅適用於 AD FS 作為同盟提供者並從外部宣告提供者取用安全性權杖的案例)。  
  
下表摘要說明 WID 與 SQL Server 伺服器陣列中支援的 AD FS 伺服器數目。    
  
  
|| 在 AD FS 中設定了 1 到 100 個信賴憑證者 (RP) 信任 | 設定了超過 100 個 RP 信任  |
| --- |--- | --- |
|1 到 30 個 AD FS 伺服器|支援 WID|不支援使用 WID - 需要 SQL Server |
|超過 30 個 AD FS 伺服器|不支援使用 WID - 需要 SQL Server|不支援使用 WID - 需要 SQL Server  
  
**SQL Server**  
  
- Windows Server 2016 中的 AD FS 支援 SQL Server 2008 和更高版本。

- SQL Server 伺服器陣列同時支援 SAML 成品解析和權杖重新偵測。  
  
## <a name="BKMK_6"></a>瀏覽器需求  
透過瀏覽器或瀏覽器控制項執行 AD FS 驗證時，您的瀏覽器必須符合下列需求：  
  
- 必須啟用 JavaScript  
  
- 若要進行單一登入，則必須將用戶端瀏覽器設定為允許 Cookie  
  
- 必須支援伺服器名稱指示 \(SNI\)  
  
- 若要驗證使用者憑證和裝置憑證，瀏覽器必須支援 SSL 用戶端憑證驗證  

- 若要使用 Windows 整合式驗證來進行無縫登入，則必須在近端內部網路區域或信任的網站區域中設定同盟服務名稱 (例如 https:\/\/fs.contoso.com)。
  ## <a name="BKMK_7"></a>網路需求  
 
**防火牆需求**  
  
位於 Web 應用程式 Proxy 與同盟伺服器陣列之間的防火牆，以及位於用戶端與 Web 應用程式 Proxy 之間的防火牆，都必須啟用 TCP 連接埠 443 的輸入。  
  
此外，如果用戶端使用者憑證驗證 \(使用 X509 使用者憑證的 clientTLS 驗證\) 是必要的，而且連接埠 443 上的 certauth 端點未啟用，則 AD FS 2016 會要求在用戶端與 Web 應用程式 Proxy 之間的防火牆上，啟用 TCP 連接埠 49443 的輸入。 Web 應用程式 Proxy 與同盟伺服器之間的防火牆則不需要這麼做。 

如需混合式連接埠需求的詳細資訊，請參閱[混合式身分識別的連接埠和通訊協定](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports)。 

如需其他資訊，請參閱[保護 Active Directory 同盟服務的最佳做法](../deployment/Best-Practices-Securing-AD-FS.md)
  
**DNS 需求**  
  
-   若要能夠存取內部網路，所有存取內部公司網路 \(內部網路\) 內 AD FS 服務的用戶端都必須能夠將 AD FS 服務名稱解析到一或多個 AD FS 伺服器的負載平衡器。  
  
-   若要能夠存取外部網路，從公司網路外部 \(外部網路\/網際網路\) 存取 AD FS 服務的所有用戶端都必須能夠將 AD FS 服務名稱解析到一或多個 Web 應用程式 Proxy 伺服器的負載平衡器。  
  
-   DMZ 中的每個 Web 應用程式 Proxy 伺服器都必須能夠將 AD FS 服務名稱解析到一或多個 AD FS 伺服器的負載平衡器。 使用 DMZ 網路中的替代 DNS 伺服器，或使用 HOSTS 檔案變更本機伺服器解析，即可達成此目的。  
  
-   若要進行 Windows 整合式驗證，您必須使用 DNS A 記錄 \(而不是 CNAME\) 來作為同盟服務名稱。  

-   若要在連接埠 443 上進行使用者憑證驗證，則必須在 DNS 中將 "certauth.\<同盟服務名稱\>" 設定為解析到同盟伺服器或 Web 應用程式 Proxy。

-   若要註冊裝置或使用 Windows 10 之前的用戶端向內部部署資源進行新式驗證，則必須將組織中正在使用的每個 UPN 尾碼 "enterpriseregistration.\<UPN 尾碼\>" 設定為解析到同盟伺服器或 Web 應用程式 Proxy。

**負載平衡器需求**
- 負載平衡器不得終止 SSL。 AD FS 支援多個使用憑證驗證的使用案例，但這些案例會在終止 SSL 時中斷。 任何使用案例都不支援在負載平衡器上終止 SSL。 
- 建議使用支援 SNI 的負載平衡器。 如果無法這麼做，則在 AD FS/Web 應用程式 Proxy 伺服器上使用 0.0.0.0 後援繫結應可作為因應措施。
- 建議使用 HTTP (而不是 HTTPS) 健康情況探查端點來執行負載平衡器健康情況檢查，以便路由流量。 這可避免任何與 SNI 相關的問題。 這些探查端點的回應是「HTTP 200 OK」，因此會在本機提供服務，而不需要依賴後端服務。 HTTP 探查可以透過 HTTP 使用路徑 ‘/adfs/probe' 來存取
    - http://&lt;Web 應用程式 Proxy 名稱&gt;/adfs/probe
    - http://&lt;ADFS 伺服器名稱&gt;/adfs/probe
    - http://&lt;Web 應用程式 Proxy IP 位址&gt;/adfs/probe
    - http://&lt;ADFS IP 位址&gt;/adfs/probe
- 不建議使用 DNS 循環配置資源來作為負載平衡的方法。 使用這類負載平衡並無法使用健康情況探查自動從負載平衡器移除節點。 
- 不建議使用 IP 型工作階段親和性或黏性工作階段，來向負載平衡器內的 AD FS 驗證流量。 當郵件用戶端使用舊版驗證通訊協定來連線至 Office 365 郵件服務 (Exchange Online) 時，這可能會造成某些節點發生多載情形。 

## <a name="BKMK_13"></a>權限需求  
執行 AD FS 安裝和初始設定的系統管理員，必須具有 AD FS 伺服器上的本機系統管理員權限。  如果本機系統管理員沒有在 Active Directory 中建立物件的權限，則必須先讓網域管理員建立必要的 AD 物件，然後才能使用 AdminConfiguration 參數來設定 AD FS 伺服器陣列。  
  
  

