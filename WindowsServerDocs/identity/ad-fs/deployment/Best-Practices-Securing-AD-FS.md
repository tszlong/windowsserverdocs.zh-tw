---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: 保護 AD FS 和 Web 應用程式 Proxy 的最佳做法
description: 安全規劃和部署 Active Directory 同盟服務（AD FS）和 Web 應用程式 Proxy 的最佳作法。
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8206ddc43eab7a220a9f0f988c294c627bc8c977
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853021"
---
# <a name="best-practices-for-securing-active-directory-federation-services"></a>保護 Active Directory 同盟服務的最佳做法

本檔提供安全規劃和部署 Active Directory 同盟服務（AD FS）和 Web 應用程式 Proxy 的最佳作法。  其中包含這些元件預設行為的相關資訊，以及具有特定使用案例和安全性需求之組織的其他安全性設定建議。

本檔適用于 Windows Server 2012 R2 和 Windows Server 2016 （預覽）中的 AD FS 和 WAP。  不論基礎結構是部署在內部部署網路或雲端託管環境（例如 Microsoft Azure）中，都可以使用這些建議。

## <a name="standard-deployment-topology"></a>標準部署拓撲
若要在內部部署環境中部署，建議採用標準部署拓朴，此拓撲是由內部公司網路上的一或多部 AD FS 伺服器所組成，其中包含一或多個 Web 應用程式 Proxy （WAP）伺服器，位於 DMZ 或外部網路。  在每一層，AD FS 和 WAP，硬體或軟體負載平衡器會放在伺服器陣列的前方，並處理流量路由。  防火牆會在負載平衡器的外部 IP 位址前面放在每個（FS 和 proxy）伺服器陣列前面。

![AD FS 標準拓撲](media/Best-Practices-Securing-AD-FS/adfssec1.png)

>[!NOTE]
> AD FS 需要完整的可寫入網域控制站，才能正常運作，而不是唯讀網域控制站。 如果規劃的拓撲包含唯讀網域控制站，唯讀網域控制站就可以用於驗證，但是 LDAP 宣告處理將需要連線到可寫入的網域控制站。

## <a name="ports-required"></a>需要端口
下圖說明在 AD FS 和 WAP 部署的元件之間，必須啟用的防火牆埠。  如果部署不包含 Azure AD/Office 365，則可以忽略同步處理需求。

>請注意，只有在使用使用者憑證驗證時才需要端口49443，這對於 Azure AD 和 Office 365 而言是選擇性的。

![AD FS 標準拓撲](media/Best-Practices-Securing-AD-FS/adfssec2.png)

>[!NOTE]
> 埠808（Windows Server 2012R2）或埠1501（Windows Server 2016 +）是用於本機 WCF 端點的 Net.tcp AD FS 埠，可將設定資料傳送至服務處理常式和 Powershell。 您可以藉由執行 Set-adfsproperties 來查看此埠 |選取 [NetTcpPort]。 這是不需要在防火牆中開啟，但會顯示在埠掃描中的本機埠。 

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD Connect 和同盟伺服器/WAP
下表描述 Azure AD Connect 伺服器與同盟/WAP 伺服器之間通訊所需的埠和通訊協定。  

通訊協定 |連接埠 |描述
--------- | --------- |---------
HTTP|80（TCP/UDP）|用來下載 Crl （憑證撤銷清單）以驗證 SSL 憑證。
HTTPS|443（TCP/UDP）|用來與 Azure AD 同步處理。
WinRM|5985| WinRM 接聽程式

### <a name="wap-and-federation-servers"></a>WAP 和同盟伺服器
下表描述同盟伺服器與 WAP 伺服器之間通訊所需的埠和通訊協定。

通訊協定 |連接埠 |描述
--------- | --------- |---------
HTTPS|443（TCP/UDP）|用於驗證。

### <a name="wap-and-users"></a>WAP 和使用者
下表描述使用者與 WAP 伺服器之間通訊所需的埠和通訊協定。

通訊協定 |連接埠 |描述
--------- | --------- |--------- |
HTTPS|443（TCP/UDP）|用於裝置驗證。
TCP|49443（TCP）|用於憑證驗證。

如需混合式部署所需的必要端口和通訊協定的詳細資訊，請參閱[這裡](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-ports)的檔。

如需 Azure AD 和 Office 365 部署所需之埠和通訊協定的詳細資訊，請參閱[這裡](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US)的檔。

### <a name="endpoints-enabled"></a>已啟用端點

安裝 AD FS 和 WAP 時，同盟服務和 proxy 上會啟用一組預設的 AD FS 端點。  這些預設值是根據最常見的必要和使用案例所選擇，而且不需要變更它們。  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>選擇性為 Azure AD/Office 365 啟用的最低端點 proxy 集合
僅針對 Azure AD 和 Office 365 案例部署 AD FS 和 WAP 的組織，可以更進一步限制在 proxy 上啟用的 AD FS 端點數目，以達到較小的受攻擊面。
以下是在這些情況下，必須在 proxy 上啟用的端點清單：

|端點|用途
|-----|-----
|/adfs/ls|以瀏覽器為基礎的驗證流程和目前版本的 Microsoft Office 使用此端點進行 Azure AD 和 Office 365 驗證
|/adfs/services/trust/2005/usernamemixed|用於 Exchange Online 與 Office 2013 舊的 Office 用戶端可能會更新2015。  較新的用戶端會使用被動 \adfs\ls 端點。
|/adfs/services/trust/13/usernamemixed|用於 Exchange Online 與 Office 2013 舊的 Office 用戶端可能會更新2015。  較新的用戶端會使用被動 \adfs\ls 端點。
|/adfs/oauth2|這項功能適用于任何現代化應用程式（在內部部署或雲端上）您已設定為直接向 AD FS 進行驗證（亦即不透過 AAD）
|/adfs/services/trust/mex|用於 Exchange Online 與 Office 2013 舊的 Office 用戶端可能會更新2015。  較新的用戶端會使用被動 \adfs\ls 端點。
|/adfs/ls/federationmetadata/2007-06/federationmetadata.xml    |任何被動流程的需求;Office 365/Azure AD 用來檢查 AD FS 憑證


您可以使用下列 PowerShell Cmdlet，在 proxy 上停用 AD FS 的端點：
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

例如，
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>驗證擴充保護
驗證的擴充保護是一項功能，可減少中間人（MITM）攻擊中的人，並且預設會使用 AD FS 來啟用。

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>若要驗證設定，您可以執行下列動作：
您可以使用下列 PowerShell commandlet 來驗證此設定。  
    
   `PS:\>Get-ADFSProperties`

屬性為 `ExtendedProtectionTokenCheck`。  預設值為 [允許]，如此就能在沒有支援功能的瀏覽器相容性問題的情況下，達到安全性優勢。  

### <a name="congestion-control-to-protect-the-federation-service"></a>保護 federation service 的擁塞控制
Federation service proxy （WAP 的一部分）提供擁塞控制來保護 AD FS 服務免于大量的要求。  如果 Web 應用程式 Proxy 與同盟伺服器之間的延遲偵測到同盟伺服器超載，Web 應用程式 Proxy 就會拒絕外部用戶端驗證要求。  此功能預設會設定為建議的延遲臨界值層級。

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>若要驗證設定，您可以執行下列動作：
1.    在 Web 應用程式 Proxy 電腦上，啟動提升權限的命令視窗。
2.    流覽至 ADFS 目錄，網址為%WINDIR%\adfs\config。
3.    將 [擁塞控制設定] 從其預設值變更為 [<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />]。
4.    儲存並關閉檔案。
5.    執行 ' net stop adfssrv '，然後按 [net start adfssrv]，以重新開機 AD FS 服務。
如需這項功能的指引，請參閱[這裡](https://msdn.microsoft.com/library/azure/dn528859.aspx )。

### <a name="standard-http-request-checks-at-the-proxy"></a>Proxy 上的標準 HTTP 要求檢查
Proxy 也會對所有流量執行下列標準檢查：

- FS-P 本身會透過短期憑證來驗證 AD FS。  在可疑的 dmz 伺服器危害案例中，AD FS 可以「撤銷 proxy 信任」，使其不再信任來自可能遭盜用之 proxy 的任何連入要求。 撤銷 proxy 信任會撤銷每個 proxy 本身的憑證，使其無法成功驗證 AD FS 伺服器的任何用途
- FS-P 會終止所有連線，並建立新的 HTTP 連線，連至內部網路上的 AD FS 服務。 這會在外部裝置和 AD FS 服務之間提供工作階段層級的緩衝區。 外部裝置永遠不會直接連接到 AD FS 服務。
- FS-P 會執行 HTTP 要求驗證，專門篩選出 AD FS 服務不需要的 HTTP 標頭。

## <a name="recommended-security-configurations"></a>建議的安全性設定
確保所有 AD FS 和 WAP 伺服器都能收到最新的更新，AD FS 基礎結構最重要的安全性建議是確保您有一個方法可以讓 AD FS 和 WAP 伺服器以所有安全性更新為最新，以及在此頁面上指定為重要 AD FS 的選擇性更新。

Azure AD 客戶監視並保持最新基礎結構的建議方式是透過 Azure AD Connect Health AD FS，這是 Azure AD Premium 的一項功能。  Azure AD Connect Health 包括當 AD FS 或 WAP 電腦缺少特別針對 AD FS 和 WAP 的其中一個重要更新時，所觸發的監視和警示。

您可以在[這裡](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-health-agent-install/)找到為 AD FS 安裝 Azure AD Connect Health 的相關資訊。

## <a name="additional-security-configurations"></a>其他安全性設定
您可以選擇性地設定下列其他功能，以提供預設部署中所提供的額外保護。

### <a name="extranet-soft-lockout-protection-for-accounts"></a>帳戶的外部網路「軟」鎖定保護
有了 Windows Server 2012 R2 中的外部網路鎖定功能，AD FS 系統管理員可以設定允許的驗證要求數目上限（ExtranetLockoutThreshold）和「觀察時間範圍」（ExtranetObservationWindow）。 當達到驗證要求的最大數目（ExtranetLockoutThreshold）時，AD FS 會停止嘗試針對設定時間週期（ExtranetObservationWindow）的 AD FS 驗證所提供的帳號憑證。 此動作會保護此帳戶免于 AD 帳戶鎖定，換句話說，它會保護此帳戶不會遺失依賴 AD FS 的公司資源存取權來驗證使用者。 這些設定適用于 AD FS 服務可以驗證的所有網域。

您可以使用下列 Windows PowerShell 命令來設定 AD FS 外部網路鎖定（範例）： 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

如需參考，請參閱這項功能的公開[檔。](https://technet.microsoft.com/library/dn486806.aspx ) 

### <a name="disable-ws-trust-windows-endpoints-on-the-proxy-ie-from-extranet"></a>停用 proxy 上的 WS-TRUST Windows 端點，即來自外部網路

WS-TRUST Windows 端點（ */adfs/services/trust/2005/windowstransport*和 */adfs/services/trust/13/windowstransport*）僅適用于在 HTTPS 上使用 WIA 系結的內部網路面向端點。 將它們公開到外部網路，可能會允許對這些端點的要求略過鎖定保護。 您應該在 proxy 上停用這些端點（也就是從外部網路停用），以使用下列 PowerShell 命令來保護 AD 帳戶鎖定。 藉由停用 proxy 上的這些端點，並不會影響到已知的終端使用者。

    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/2005/windowstransport -Proxy $false
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/windowstransport -Proxy $false
    
### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>區分內部網路和外部網路存取的存取原則
AD FS 能夠區分源自本機、公司網路與要求的存取原則，而這些要求是透過 proxy 從網際網路傳入。  這可在每個應用程式或全域執行。  對於具有敏感性或個人識別資訊的高商業價值應用程式或應用程式，請考慮需要多重要素驗證。  這可以透過 [AD FS 管理] 嵌入式管理單元來完成。  

### <a name="require-multi-factor-authentication-mfa"></a>需要多重要素驗證（MFA）
AD FS 可以設定為需要增強式驗證（例如多重要素驗證），特別是針對透過 proxy 傳入的要求、個別的應用程式，以及適用于 Azure AD/Office 365 和內部部署資源的條件式存取。  支援的 MFA 方法包括 Microsoft Azure MFA 和協力廠商提供者。  系統會提示使用者提供其他資訊（例如包含單次代碼的 SMS 文字），而 AD FS 會與提供者特定外掛程式搭配使用，以允許存取。  

支援的外部 MFA 提供者包括[此](https://technet.microsoft.com/library/dn758113.aspx)頁面中所列的和 HDI Global。

### <a name="hardware-security-module-hsm"></a>硬體安全性模組 (HSM)
在預設設定中，AD FS 用來簽署權杖的金鑰永遠不會離開內部網路上的同盟伺服器。  它們永遠不會出現在 DMZ 或 proxy 機器上。  （選擇性）若要提供額外的保護，可以在附加至 AD FS 的硬體安全模組中保護這些金鑰。  Microsoft 不會產生 HSM 產品，但市場上有數個支援 AD FS。  若要執行這項建議，請遵循廠商指引來建立簽署和加密的 X509 憑證，然後使用 AD FS 安裝 powershell commandlet，指定您的自訂憑證，如下所示：

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

其中：


- `CertificateThumbprint` 是您的 SSL 憑證
- `SigningCertificateThumbprint` 是您的簽署憑證（使用受 HSM 保護的金鑰）
- `DecryptionCertificateThumbprint` 是您的加密憑證（使用受 HSM 保護的金鑰）



