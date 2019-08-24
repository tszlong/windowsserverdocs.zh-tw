---
ms.assetid: 28f4a518-1341-4a10-8a4e-5f84625b314b
title: AD FS 2016 需求
description: 安裝 Active Directory 同盟服務的需求。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c330b5f65b2862628fd23e288c95e81653da5c5b
ms.sourcegitcommit: 4fa147d552481d8279a5390f458a9f7788061977
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/23/2019
ms.locfileid: "70009073"
---
# <a name="ad-fs-requirements"></a>AD FS 需求



以下是部署 AD FS 的需求:  
  
-   [憑證需求](ad-fs-requirements.md#BKMK_1)  
  
-   [硬體需求](ad-fs-requirements.md#BKMK_2)  
  
-   [Proxy 需求](ad-fs-requirements.md#BKMK_3)  
  
-   [AD DS 需求](ad-fs-requirements.md#BKMK_4)  
  
-   [設定資料庫需求](ad-fs-requirements.md#BKMK_5)  
  
-   [瀏覽器需求](ad-fs-requirements.md#BKMK_6)  

-   [網路需求](ad-fs-requirements.md#BKMK_7)  
  
-   [許可權需求](ad-fs-requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>憑證需求  
  
### <a name="ssl-certificates"></a>SSL 憑證

每個 AD FS 和 Web 應用程式 Proxy 伺服器都有 SSL 憑證, 可向 federation service 服務 HTTPS 要求。  Web 應用程式 Proxy 可以有額外的 SSL 憑證來服務對已發佈應用程式的要求。

**建議**針對所有 AD FS 同盟伺服器和 Web 應用程式 proxy 使用相同的 SSL 憑證。 

**滿足**

同盟伺服器上的 SSL 憑證必須符合下列需求
- 憑證是公開信任的 (適用于生產部署)
- 憑證包含伺服器驗證的增強金鑰使用方法 (EKU) 值
- 憑證包含同盟服務名稱, 例如主體或主體替代名稱 (SAN) 中的 "fs.contoso.com"
- 若為埠443上的使用者憑證驗證, 憑證會包含 "certauth"。同盟服務名稱\>", 例如 SAN 中的" certauth.fs.contoso.com " \<
- 針對裝置註冊, 或使用 Windows 10 之前的用戶端對內部部署資源進行新式驗證, SAN 必須包含「enterpriseregistration。\<upn尾碼\>」, 用於貴組織中使用的每個 upn 尾碼。

Web 應用程式 Proxy 上的 SSL 憑證必須符合下列需求
- 如果 proxy 是用來 proxy AD FS 使用 Windows 整合式驗證的要求, 則 proxy SSL 憑證必須相同 (使用相同的金鑰) 做為同盟伺服器 SSL 憑證
- 如果已啟用 AD FS 屬性 "ExtendedProtectionTokenCheck" (AD FS 中的預設設定), proxy SSL 憑證必須相同 (使用相同的金鑰) 做為同盟伺服器 SSL 憑證
- 否則, proxy SSL 憑證的需求與同盟伺服器 SSL 憑證的需求相同

### <a name="service-communication-certificate"></a>服務通訊憑證
大部分的 AD FS 案例 (包括 Azure AD 和 Office 365) 都不需要此憑證。 根據預設, AD FS 會設定初始設定時所提供的 SSL 憑證做為服務通訊憑證。

**建議**
- 使用與您用於 SSL 的憑證相同。  

### <a name="token-signing-certificate"></a>權杖簽署憑證
此憑證是用來將已發行的權杖簽章給信賴憑證者, 因此信賴憑證者應用程式必須辨識憑證, 以及其相關聯的金鑰為已知且受信任。 當令牌簽署憑證變更時 (例如當它過期且您設定新憑證時), 所有信賴憑證者都必須更新。

**建議**使用 AD FS 預設、內部產生的自我簽署權杖簽署憑證。  

**滿足**
- 如果您的組織要求將來自企業 PKI 的憑證用於權杖簽署, 則可以使用 AdfsFarm 指令程式的 SigningCertificateThumbprint 參數來完成這項作業。
- 無論您是使用預設內部產生的憑證或外部註冊的憑證, 當令牌簽署憑證變更時, 您都必須確定所有信賴憑證者都已更新為新的憑證資訊。  否則, 未更新之任何信賴憑證者的登入將會失敗。

### <a name="token-encryptingdecrypting-certificate"></a>權杖加密/解密憑證
加密簽發給 AD FS 之權杖的宣告提供者會使用此憑證。

**建議**使用 AD FS 預設、內部產生的自我簽署權杖解密憑證。  

**滿足**
- 如果您的組織要求將來自企業 PKI 的憑證用於權杖簽署, 則可以使用 AdfsFarm 指令程式的 DecryptingCertificateThumbprint 參數來完成這項作業。
- 無論您是使用預設的內部產生的憑證或外部註冊的憑證, 當令牌解密憑證變更時, 您都必須確定所有宣告提供者都已更新為新的憑證資訊。  否則, 使用任何未更新之宣告提供者的登入將會失敗。
  
> [!CAUTION]  
> 用於權杖\-簽署和權杖\-解密\/加密的憑證, 對於同盟服務的穩定性而言非常重要。 管理其權杖\-簽署 & 權杖\-解密\/加密憑證的客戶, 應確保這些憑證已備份, 並可在復原事件期間獨立取得。  

### <a name="user-certificates"></a>使用者憑證
- 搭配 AD FS 使用 x509 使用者憑證驗證時, 所有使用者憑證都必須與 AD FS 和 Web 應用程式 Proxy 伺服器所信任的根憑證授權單位進行連結。

## <a name="BKMK_2"></a>硬體需求  
AD FS 和 Web 應用程式 Proxy 硬體需求 (實體或虛擬) 會在 CPU 上閘道, 因此您應該調整伺服器陣列的大小來處理容量。  
- 使用[AD FS 2016 容量規劃試算表](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)來判斷您將需要的 AD FS 和 Web 應用程式 Proxy 伺服器數目。

AD FS 的記憶體和磁片需求相當靜態, 請參閱下表:


|**硬體需求**|**最低需求**|**建議的需求**|
|----- | ----- |-----|
|RAM|2 GB|4 GB |
|磁碟空間|32 GB|100 GB |

**SQL Server 硬體需求**

如果您為 AD FS 設定資料庫使用 SQL Server, 請根據最基本的 SQL Server 建議來調整 SQL Server 的大小。  AD FS 資料庫大小很小, 而且 AD FS 不會對資料庫實例造成大量的處理負載。  不過, AD FS 會在驗證期間連接到資料庫多次, 因此網路連線應該是健全的。  可惜的是, AD FS 設定資料庫不支援 SQL Azure。
  
## <a name="BKMK_3"></a>Proxy 需求  
  
-   針對外部網路存取, 您必須部署「遠端存取」伺服器\-角色的「Web 應用程式 Proxy」角色服務部分。 

-   協力廠商 proxy 必須支援[ADFSPIP 通訊協定](https://msdn.microsoft.com/library/dn392811.aspx), 才能支援作為 AD FS proxy。  如需協力廠商廠商的清單, 請參閱[常見問題](AD-FS-FAQ.md)。

-   AD FS 2016 需要 Windows Server 2016 上的 Web 應用程式 Proxy 伺服器。  無法針對在2016伺服器陣列行為層級執行的 AD FS 2016 伺服器陣列, 設定下層 proxy。
  
-   同盟伺服器和 Web 應用程式 Proxy 角色服務無法安裝在同一部電腦上。  
  
## <a name="BKMK_4"></a>AD DS 需求  
**網域控制站需求**  
  
- AD FS 需要執行 Windows Server 2008 或更新版本的網域控制站。

- Microsoft Passport for Work 至少需要一個 Windows Server 2016 網域控制站。
  
> [!NOTE]  
> Windows Server 2003 網域控制站環境的所有支援已結束。 如需 Microsoft 支援服務生命週期的詳細資訊, 請造訪[此頁面](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO)。  
  
**網域功能\-等級需求**  
  
 - 所有使用者帳戶網域以及 AD FS 伺服器所加入的網域, 都必須在 Windows Server 2003 或更高版本的網域功能等級操作。  
  
 - 如果憑證明確對應至 AD DS 中的使用者帳戶, 則需要 Windows Server 2008 網域功能等級或更新版本才能進行用戶端憑證驗證。  
  
**架構需求**  
  
-   AD FS 2016 的新安裝需要 Active Directory 2016 架構 (最低版本 85)。

-   若要將 AD FS 伺服器陣列行為層級 (FBL) 提高至2016層級, 則需要 Active Directory 2016 架構 (最低版本 85)。  

  
**服務帳戶需求**  
  
-   任何標準網域帳戶都可以用來做為 AD FS 的服務帳戶。 也支援群組受管理的服務帳戶。 當您設定 AD FS 時, 將會自動新增在執行時間所需的許可權。

-   AD 服務帳戶所需的使用者權限指派是「以服務方式登入」

-   ' NT Service\adfssrv ' 與 ' NT Service\drs ' 所需的使用者權限指派為「產生安全性審核」和「以服務方式登入」。

-   群組受管理的服務帳戶需要至少一個執行 Windows Server 2012 或更高版本的網域控制站。  GMSA 必須存留在預設的 [CN = 受管理的服務帳戶] 容器之下。  

-   針對 Kerberos 驗證, 必須在 AD FS 服務帳戶`HOST/<adfs\_service\_name>`上註冊服務主體名稱 ' '。 根據預設, AD FS 會在建立新的 AD FS 伺服器陣列時設定此項。  如果這項作業失敗, 例如發生衝突或許可權不足, 您會看到警告, 您應該手動將它加入。  
   
**網域需求**  
  
-   所有 AD FS 伺服器都必須加入 AD DS 網域。  
  
-   伺服器陣列中的所有 AD FS 伺服器都必須部署在相同的網域中。  
   
**多樹系需求**  
  
-   AD FS 伺服器加入的網域必須信任每個網域或樹系, 其中包含向 AD FS 服務驗證的使用者。  

-   AD FS 服務帳戶為其成員的樹系, 必須信任所有使用者登入樹系。 
  
-   AD FS 服務帳戶必須有權讀取每個網域中的使用者屬性, 其中包含向 AD FS 服務驗證的使用者。  
  
## <a name="BKMK_5"></a>設定資料庫需求  
本節說明分別使用 Windows 內部資料庫 (WID) 或 SQL Server 作為資料庫之 AD FS 伺服器陣列的需求和限制:  
  
**WID**  
  
-   在 WID 伺服器陣列中不支援 SAML 2.0 的成品解析設定檔。    

-   WID 伺服器陣列不支援權杖重新執行偵測。 (這項功能僅適用于 AD FS 做為同盟提供者並使用來自外部宣告提供者的安全性權杖的案例)。  
  
下表摘要說明 WID 與 SQL Server 服務器陣列中支援的 AD FS 伺服器數目。    
  
  
|| 在 AD FS 中設定的 1-100 信賴憑證者 (RP) 信任 | 已設定超過 100 RP 信任  |
| --- |--- | --- |
|1-30 AD FS 伺服器|支援 WID|不支援使用 WID-SQL Server 需要 |
|超過30部 AD FS 伺服器|不支援使用 WID-SQL Server 需要|不支援使用 WID-SQL Server 需要  
  
**SQL Server**  
  
- 針對 Windows Server 2016 中的 AD FS, 支援 SQL Server 2008 和更新版本。

- SQL Server 服務器陣列中都支援 SAML 成品解析和權杖重新執行偵測。  
  
## <a name="BKMK_6"></a>瀏覽器需求  
透過瀏覽器或瀏覽器控制項執行 AD FS 驗證時, 您的瀏覽器必須符合下列需求:  
  
- 必須啟用 JavaScript  
  
- 針對單一登入, 用戶端瀏覽器必須設定為允許 cookie  
  
- 必須\(支援\)伺服器名稱指示 SNI  
  
- 針對使用者憑證 & 裝置憑證驗證, 瀏覽器必須支援 SSL 用戶端憑證驗證  

- 若要使用 Windows 整合式驗證順暢登入, 必須在 [近端內部網路區域\/] 或 [信任的網站] 區域中設定同盟服務名稱 (例如 HTTPs:\/fs.contoso.com)。
  ## <a name="BKMK_7"></a>網路需求  
 
**防火牆需求**  
  
位於 Web 應用程式 Proxy 與同盟伺服器陣列之間的防火牆, 以及用戶端與 Web 應用程式 Proxy 之間的防火牆, 都必須啟用 TCP 埠443的輸入。  
  
此外, 如果需要使用 X509 使用者憑證\( \)進行用戶端使用者憑證驗證 clienttls 是驗證, 且未啟用埠443上的 certauth 端點, AD FS 2016 會要求啟用 TCP 埠49443在用戶端與 Web 應用程式 Proxy 之間的防火牆上輸入。 這在 Web 應用程式 Proxy 與同盟伺服器\)之間的防火牆上不是必要的。 

如需混合式埠需求的詳細資訊, 請參閱混合式身分[識別埠和通訊協定](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports)。 

如需其他資訊, 請參閱[保護 Active Directory 同盟服務的最佳做法](../deployment/Best-Practices-Securing-AD-FS.md)
  
**DNS 需求**  
  
-   若要進行內部網路存取, 所有存取內部公司網路\( \)內 AD FS 服務的用戶端, 都必須能夠將 AD FS 服務名稱解析為 AD FS 伺服器或 AD FS 伺服器的負載平衡器。  
  
-   若要存取外部網路, 所有從公司網路外部網路\( \/網際網路\)存取 AD FS 服務的用戶端, 都必須能夠將 AD FS 服務名稱解析為 Web 應用程式 Proxy 伺服器的負載平衡器, 或Web 應用程式 Proxy 伺服器。  
  
-   DMZ 中的每個 Web 應用程式 Proxy 伺服器都必須能夠將 AD FS 服務名稱解析成 AD FS 伺服器或 AD FS 伺服器的負載平衡器。 使用 DMZ 網路中的替代 DNS 伺服器, 或使用 HOSTS 檔案變更本機伺服器解析, 即可達成此目的。  
  
-   針對 Windows 整合式驗證, 您必須使用 DNS a 記錄\(, 而\)不是同盟服務名稱的 CNAME。  

-   針對埠443上的使用者憑證驗證, "certauth。同盟服務名稱\>」必須在 DNS 中設定, 才能解析為同盟伺服器或 web 應用程式 proxy。 \<

-   針對裝置註冊, 或使用 Windows 10 之前的用戶端對內部部署資源進行新式驗證, 請 enterpriseregistration。\<upn尾碼\>」, 針對組織中使用的每個 upn 尾碼, 必須設定為解析為同盟伺服器或 web 應用程式 proxy。

**Load Balancer 需求**
- 負載平衡器不得終止 SSL。 AD FS 支援使用憑證驗證的多個使用案例, 這會在終止 SSL 時中斷。 任何使用案例都不支援在負載平衡器上終止 SSL。 
- 建議使用支援 SNI 的負載平衡器。 在此情況下, 在您的 AD FS/Web 應用程式 Proxy 伺服器上使用 0.0.0.0 fallback 系結應該提供因應措施。
- 建議使用 HTTP (不是 HTTPS) 健康情況探查端點來執行路由流量的負載平衡器健全狀況檢查。 這可避免任何與 SNI 相關的問題。 對這些探查端點的回應是 HTTP 200 正常, 並在本機提供服務, 而不需依賴後端服務。 HTTP 探查可以透過 HTTP 使用路徑 '/adfs/probe ' 來存取
    - HTTP://&lt;Web 應用程式 Proxy&gt;名稱/adfs/probe
    - HTTP://&lt;ADFS 伺服器名稱&gt;/adfs/probe
    - HTTP://&lt;Web 應用程式 Proxy IP&gt;位址/adfs/probe
    - HTTP://&lt;ADFS IP 位址&gt;/adfs/probe
- 不建議使用 DNS 迴圈配置資源做為負載平衡的方式。 使用這種類型的負載平衡, 並不會提供自動化的方式, 從負載平衡器使用健康情況探查來移除節點。 
- 不建議使用以 IP 為基礎的會話親和性會話, 或在負載平衡器中 AD FS 驗證流量的網路。 當郵件用戶端使用舊版驗證通訊協定連接到 Office 365 郵件服務 (Exchange Online) 時, 這可能會造成某些節點的多載。 

## <a name="BKMK_13"></a>許可權需求  
執行安裝的系統管理員和 AD FS 的初始設定, 必須具有 AD FS 伺服器上的本機系統管理員許可權。  如果本機系統管理員沒有在 Active Directory 中建立物件的許可權, 他們必須先具有網域系統管理員, 才能建立必要的 AD 物件, 然後使用 AdminConfiguration 參數設定 AD FS 伺服器陣列。  
  
  

