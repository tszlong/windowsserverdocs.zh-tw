---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: 保護 AD FS 與 Web 應用程式 Proxy 安全的最佳作法
description: Active Directory 同盟服務 (AD FS) 與 Web 應用程式 Proxy 的安全規劃與部署的最佳作法。
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: e70a62a80b3a40b70c261e96b841db92a996bb46
ms.sourcegitcommit: 7674bbe49517bbfe0e2c00160e08240b60329fd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/20/2021
ms.locfileid: "98603413"
---
# <a name="best-practices-for-securing-active-directory-federation-services"></a>保護 Active Directory 同盟服務的最佳作法

本檔提供在 Active Directory 同盟服務 (AD FS) 及 Web 應用程式 Proxy 的安全規劃與部署的最佳作法。  其中包含這些元件之預設行為的相關資訊，以及具有特定使用案例和安全性需求之組織的其他安全性設定建議。

本檔適用于 Windows Server 2012 R2 和 Windows Server 2016 (preview) 中的 AD FS 和 WAP。  無論基礎結構是部署在內部部署網路或雲端託管環境中（例如 Microsoft Azure），都可以使用這些建議。

## <a name="standard-deployment-topology"></a>標準部署拓撲
針對內部部署環境中的部署，我們建議使用由內部公司網路上的一或多部 AD FS 伺服器組成的標準部署拓撲，其中包含一個或多個 Web 應用程式 Proxy (DMZ 或外部網路網路中的 WAP) 伺服器。  在每一層 AD FS 和 WAP，硬體或軟體負載平衡器都會放置在伺服器陣列前面，並處理流量路由。  防火牆會在負載平衡器的外部 IP 位址前面放置於每個 (FS 和 proxy) 伺服器陣列前面。

![描述標準 A D F S 拓撲的圖表。](media/Best-Practices-Securing-AD-FS/adfssec1.png)

>[!NOTE]
> AD FS 需要完整的可寫入網域控制站，才能正常運作，而不是 Read-Only 網域控制站。 如果規劃的拓撲包含 Read-Only 網域控制站，則 Read-Only 網域控制站可以用來進行驗證，但 LDAP 宣告處理將需要連接到可寫入的網域控制站。

## <a name="ports-required"></a>需要端口
下圖說明必須在 AD FS 和 WAP 部署的元件之間和之間啟用的防火牆埠。  如果部署不包含 Azure AD/Office 365，則可以忽略同步處理需求。

>請注意，只有在使用使用者憑證驗證時才需要端口49443，這對 Azure AD 和 Office 365 而言是選擇性的。

![顯示 D F S 部署所需埠和通訊協定的圖表。](media/Best-Practices-Securing-AD-FS/adfssec2.png)

>[!NOTE]
> 埠 808 (Windows Server 2012R2) 或埠 1501 (Windows Server 2016 +) 是 AD FS 用於本機 WCF 端點的 Net.tcp 埠，可將設定資料傳輸至服務處理常式和 Powershell。 您可以藉由執行 Get-AdfsProperties 來查看此埠 |選取 [NetTcpPort]。 這是本機埠，將不需要在防火牆中開啟，但會顯示在埠掃描中。

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD Connect 和同盟伺服器/WAP
此表說明 Azure AD Connect 伺服器與同盟/WAP 伺服器之間通訊所需的連接埠和通訊協定。

通訊協定 |連接埠 |描述
--------- | --------- |---------
HTTP|80 (TCP/UDP)|用於下載 CRL (憑證撤銷清單) 以驗證 SSL 憑證。
HTTPS|443(TCP/UDP)|用來與 Azure AD 同步處理。
WinRM|5985| WinRM 接聽程式

### <a name="wap-and-federation-servers"></a>WAP 和同盟伺服器
此表說明同盟伺服器與 WAP 伺服器之間通訊所需的連接埠和通訊協定。

通訊協定 |連接埠 |描述
--------- | --------- |---------
HTTPS|443(TCP/UDP)|用於進行驗證。

### <a name="wap-and-users"></a>WAP 和使用者
此表說明使用者與 WAP 伺服器之間通訊所需的連接埠和通訊協定。

通訊協定 |連接埠 |描述
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)|用於裝置驗證。
TCP|49443 (TCP)|用於憑證驗證。

如需混合式部署所需的必要端口和通訊協定的詳細資訊，請參閱 [此處](/azure/active-directory/hybrid/reference-connect-ports)的檔。

如需有關 Azure AD 和 Office 365 部署所需的埠和通訊協定的詳細資訊，請參閱 [此處](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US)的檔。

### <a name="endpoints-enabled"></a>端點已啟用

安裝 AD FS 和 WAP 時，同盟服務和 proxy 上會啟用一組預設的 AD FS 端點。  這些預設值是根據最常使用和使用的案例所選擇，並不需要加以變更。

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>參數針對 Azure AD/Office 365 啟用的最小端點集合 proxy
只有 Azure AD 和 Office 365 案例部署 AD FS 和 WAP 的組織可以限制在 proxy 上啟用的 AD FS 端點數目，以達到更少的攻擊面。
以下是在這些案例中，必須在 proxy 上啟用的端點清單：

|端點|目的 |
|-----|----- |
|/adfs/ls|以瀏覽器為基礎的驗證流程和目前版本的 Microsoft Office 使用此端點進行 Azure AD 和 Office 365 驗證 |
|/adfs/services/trust/2005/usernamemixed|適用于與 Office 2013 之前的 Office 用戶端搭配使用的 Exchange Online，可能會有2015更新。  之後的用戶端會使用被動 \adfs\ls 端點。 |
|/adfs/services/trust/13/usernamemixed|適用于與 Office 2013 之前的 Office 用戶端搭配使用的 Exchange Online，可能會有2015更新。  之後的用戶端會使用被動 \adfs\ls 端點。 |
|/adfs/oauth2|這項功能適用于任何新式應用程式 (內部內部部署或雲端) 您已設定為直接驗證 AD FS (亦即非透過 AAD)  |
|/adfs/services/trust/mex|適用于與 Office 2013 之前的 Office 用戶端搭配使用的 Exchange Online，可能會有2015更新。  之後的用戶端會使用被動 \adfs\ls 端點。|
|/adfs/ls/federationmetadata/2007-06/federationmetadata.xml    |任何被動流程的需求;並由 Office 365/Azure AD 用來檢查 AD FS 的憑證。 |

您可以使用下列 PowerShell Cmdlet，在 proxy 上停用 AD FS 端點：

```powershell
Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false
```

例如：

```powershell
Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
```

### <a name="extended-protection-for-authentication"></a>驗證擴充保護
驗證的擴充保護功能可緩和 (MITM) 的攻擊，並預設會與 AD FS 一起啟用。

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>若要驗證設定，您可以執行下列動作：
您可以使用下列 PowerShell Cmdlet 來驗證設定。

```powershell
Get-ADFSProperties
```

屬性是 `ExtendedProtectionTokenCheck`。  預設設定為 [允許]，如此一來，就能在不支援不支援此功能之瀏覽器的相容性問題的情況下達成安全性優勢。

### <a name="congestion-control-to-protect-the-federation-service"></a>保護 federation service 的擁塞控制
Federation service proxy (WAP 的一部分) 提供擁塞控制來保護 AD FS 服務免于大量的要求。  如果同盟伺服器是由 Web 應用程式 Proxy 與同盟伺服器之間的延遲所偵測到，則 Web 應用程式 Proxy 將會拒絕外部用戶端驗證要求。  這項功能預設會設定為建議的延遲閾值層級。

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>若要驗證設定，您可以執行下列動作：
1. 在 Web 應用程式 Proxy 電腦上，啟動提升權限的命令視窗。
2. 流覽至 ADFS 目錄，位於%WINDIR%\adfs\config。
3. 將 [擁塞控制] 設定從其預設值變更為 `<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />` 。
4. 儲存並關閉檔案。
5. 依序執行 `net stop adfssrv` 和 `net start adfssrv` 來重新啟動 AD FS 服務。
如需此功能的相關指引，請參閱 [這裡](/previous-versions/azure/azure-services/dn528859(v=azure.100))。

### <a name="standard-http-request-checks-at-the-proxy"></a>Proxy 上的標準 HTTP 要求檢查
Proxy 也會針對所有流量執行下列標準檢查：

- FS-P 本身會透過短期憑證驗證 AD FS。  在有可疑的 dmz 伺服器入侵案例中，AD FS 可以「撤銷 proxy 信任」，讓它不再信任任何來自可能遭入侵 proxy 的連入要求。 撤銷 proxy 信任會撤銷每個 proxy 自己的憑證，使其無法成功驗證 AD FS 伺服器的任何用途
- FS-P 會終止所有連線，並建立連至內部網路上 AD FS 服務的新 HTTP 連線。 這會提供外部裝置與 AD FS 服務之間的工作階段層級緩衝區。 外部裝置永遠不會直接連接到 AD FS 服務。
- FS-P 會執行 HTTP 要求驗證，以明確篩選出 AD FS 服務不需要的 HTTP 標頭。

## <a name="recommended-security-configurations"></a>建議的安全性設定
確定所有 AD FS 和 WAP 伺服器都會收到最新的更新。您的 AD FS 基礎結構最重要的安全性建議，是確保您有一種方式，讓您的 AD FS 和 WAP 伺服器保持最新的安全性更新，以及針對此頁面上 AD FS 指定的選擇性更新。

Azure AD 客戶監視和保持最新基礎結構的建議方式是透過 Azure AD Connect Health AD FS Azure AD Premium 的功能。  Azure AD Connect Health 包含的監視器和警示，會在 AD FS 或 WAP 電腦遺漏特別針對 AD FS 和 WAP 的其中一個重要更新時觸發。

您可以在 [這裡](/azure/active-directory/hybrid/how-to-connect-health-agent-install)找到有關為 AD FS 安裝 Azure AD Connect Health 的資訊。

## <a name="additional-security-configurations"></a>其他安全性設定
您可以選擇性地設定下列額外的功能，以提供在預設部署中提供的額外保護。

### <a name="extranet-soft-lockout-protection-for-accounts"></a>適用于帳戶的外部網路「軟」鎖定保護
利用 Windows Server 2012 R2 中的外部網路鎖定功能，AD FS 的系統管理員可以設定允許的失敗驗證要求數目上限 (ExtranetLockoutThreshold) 以及 `observation window` (ExtranetObservationWindow) 的時間週期。 當達到此最大值 (ExtranetLockoutThreshold) 的驗證要求時，AD FS 會停止嘗試針對設定的時間週期 (ExtranetObservationWindow) 的 AD FS 驗證提供的帳號憑證。 此動作會從 AD 帳戶鎖定保護此帳戶，換句話說，它會保護此帳戶免于失去依賴 AD FS 的公司資源存取權，以進行使用者驗證。 這些設定適用于 AD FS 服務可以驗證的所有網域。

您可以使用下列 Windows PowerShell 命令設定 AD FS 的外部網路鎖定 (範例) ：

```powershell
Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )
```

如需參考，請 [參閱這項](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486806(v=ws.11))功能的公開檔。

### <a name="disable-ws-trust-windows-endpoints-on-the-proxy-ie-from-extranet"></a>停用 proxy 上的 WS-Trust Windows 端點，亦即來自外部網路

WS-Trust Windows 端點 (*/adfs/services/trust/2005/windowstransport* 和 */adfs/services/trust/13/windowstransport*) 的目的，只是在 HTTPS 上使用 WIA 系結的內部網路面向端點。 將它們公開給外部網路可能會允許對這些端點的要求略過鎖定保護。 您應在 proxy 上停用這些端點 (亦即，從外部網路) 停用，以使用下列 PowerShell 命令來保護 AD 帳戶鎖定。 在 proxy 上停用這些端點，就不會有已知的終端使用者影響。

```powershell
Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/2005/windowstransport -Proxy $false
Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/windowstransport -Proxy $false
```

### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>區分內部網路和外部網路存取的存取原則
AD FS 能夠將源自本機、公司網路要求的要求，與透過 proxy 從網際網路傳入的要求區分。  這可以針對每個應用程式或全域進行。  針對具有敏感性或個人識別資訊的高商業價值應用程式或應用程式，請考慮需要多重要素驗證。  這可以透過 AD FS 管理] 嵌入式管理單元來完成。

### <a name="require-multi-factor-authentication-mfa"></a> (MFA) 需要多重要素驗證
AD FS 可以設定為需要強式驗證 (例如多重要素驗證) 特別針對透過 proxy 傳入的要求、個別應用程式，以及 Azure AD/Office 365 和內部部署資源的條件式存取。  MFA 支援的方法包括 Microsoft Azure MFA 和協力廠商提供者。  系統會提示使用者提供其他資訊 (例如，包含一次程式碼) 的 SMS 文字，而 AD FS 與提供者特定的外掛程式一起使用以允許存取。

支援的外部 MFA 提供者包括 [此](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn758113(v=ws.11)) 頁面中所列的，以及 HDI Global。

### <a name="hardware-security-module-hsm"></a>硬體安全性模組 (HSM)
在其預設設定中，AD FS 用來簽署權杖的金鑰永遠不會離開內部網路上的同盟伺服器。  它們永遠不會出現在 DMZ 或 proxy 電腦上。  （選擇性）若要提供額外的保護，可以在連接到 AD FS 的硬體安全模組中保護這些金鑰。  Microsoft 不會產生 HSM 產品，但支援 AD FS 的市場上有數個。  若要執行這項建議，請遵循廠商指導方針來建立用於簽署和加密的 X509 憑證，然後使用 AD FS 安裝 powershell commandlet，指定您的自訂憑證，如下所示：

```powershell
Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>
```

其中：

- `CertificateThumbprint` 是您的 SSL 憑證
- `SigningCertificateThumbprint` 您的簽署憑證 (有 HSM 保護的金鑰) 
- `DecryptionCertificateThumbprint` 加密憑證 (有 HSM 保護的金鑰) 
