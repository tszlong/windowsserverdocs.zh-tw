---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: 保護 AD FS 和 Web 應用程式 Proxy 的最佳做法
description: 本文件提供安全的規劃和部署 Active Directory Federation Services (AD FS) 和 Web 應用程式 Proxy 的最佳作法。
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 958bf8455d03ddc04395fafe83e70a49c7659c96
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192439"
---
## <a name="best-practices-for-securing-active-directory-federation-services"></a>保護 Active Directory Federation Services 的最佳做法


本文件提供安全的規劃和部署 Active Directory Federation Services (AD FS) 和 Web 應用程式 Proxy 的最佳作法。  它包含這些元件和其他安全性設定之組織中特定的使用案例和安全性需求與建議的預設行為的相關資訊。

本文件適用於 AD FS 和 WAP 中 Windows Server 2012 R2 和 Windows Server 2016 （預覽）。  是否在內部部署網路，或在 Microsoft Azure 等雲端託管環境中部署基礎結構，可以使用這些建議。

## <a name="standard-deployment-topology"></a>標準的部署拓撲
針對部署在內部部署環境中，我們會建議標準的部署拓撲，其中包含一或多個 AD FS 伺服器上內部的公司網路，與 DMZ 或外部網路的網路中的一或多個 Web 應用程式 Proxy (WAP) 伺服器。  AD FS 和 WAP，每個層級的硬體或軟體負載平衡器會位於伺服器陣列，並會處理流量路由。  防火牆會放置每個 （FS 和 proxy） 的伺服器陣列之前視需要在前面的外部 IP 位址的負載平衡器。

![AD FS 標準拓樸](media/Best-Practices-Securing-AD-FS/adfssec1.png)

## <a name="ports-required"></a>所需的連接埠
下圖說明必須啟用之間，以及在 AD FS 和 WAP 部署的元件之間的防火牆連接埠。  如果部署不包含 Azure AD / Office 365，同步處理需求可以予以忽略。

>請注意，連接埠 49443 只是需要如果使用者憑證驗證會使用，這是 Azure AD 的選擇性和 Office 365。

![AD FS 標準拓樸](media/Best-Practices-Securing-AD-FS/adfssec2.png)

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD Connect 和同盟伺服器 /WAP
下表描述的連接埠和通訊協定所需的 Azure AD Connect 伺服器與同盟 /WAP 伺服器之間的通訊。  

Protocol |連接埠 |描述
--------- | --------- |---------
HTTP|80 (TCP/UDP)|用來下載 Crl （憑證撤銷清單） 來驗證 SSL 憑證。
HTTPS|443(TCP/UDP)|用來與 Azure AD 同步處理。
WinRM|5985| WinRM 接聽程式

### <a name="wap-and-federation-servers"></a>WAP 和同盟伺服器
下表描述的連接埠和同盟伺服器與 WAP 伺服器之間通訊所需的通訊協定。

Protocol |連接埠 |描述
--------- | --------- |---------
HTTPS|443(TCP/UDP)|用於驗證。

### <a name="wap-and-users"></a>WAP 和使用者
下表描述的連接埠和通訊協定所需的使用者與 WAP 伺服器之間的通訊。

Protocol |連接埠 |描述
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)|用於裝置驗證。
TCP|49443 (TCP)|用於憑證驗證。

如需有關必要的連接埠和混合式部署，請參閱文件所需的通訊協定[此處](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-ports/)。

如需連接埠和通訊協定所需的 Azure AD 和 Office 365 部署，請參閱文件[此處](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US)。

### <a name="endpoints-enabled"></a>啟用端點

AD FS 和 WAP 安裝時，federation service 和 proxy 上，會啟用一組預設的 AD FS 端點。  這些預設值根據最常需要和使用案例選擇並不需要加以變更。  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>[選用]最小值組的端點 proxy 已啟用 Azure ad / Office 365
部署 AD FS 和 WAP 只適用於 Azure AD 的組織和 Office 365 案例可以更進一步的數目限制 AD FS 端點已啟用 proxy，才能達到更基本的受攻擊面。
以下是必須在這些情況下在 proxy 啟用的端點清單：

|端點|用途
|-----|-----
|/adfs/ls|瀏覽器型驗證流程，以及目前版本的 Microsoft Office 將此端點用於 Azure AD 和 Office 365 驗證
|/adfs/services/trust/2005/usernamemixed|適用於 Exchange Online 搭配 Office 2013 年 2015年更新比舊的 Office 用戶端。  更新版本的用戶端使用被動 \adfs\ls 端點。
|/adfs/services/trust/13/usernamemixed|適用於 Exchange Online 搭配 Office 2013 年 2015年更新比舊的 Office 用戶端。  更新版本的用戶端使用被動 \adfs\ls 端點。
|/adfs/oauth2|會使用此工作區 （內部部署或雲端中） 的現代應用程式中，您已設定直接與 AD FS （也就是不是透過 AAD) 進行驗證
|/adfs/services/trust/mex|適用於 Exchange Online 搭配 Office 2013 年 2015年更新比舊的 Office 用戶端。  更新版本的用戶端使用被動 \adfs\ls 端點。
|/adfs/ls/federationmetadata/2007-06/federationmetadata.xml |任何被動流程; 的需求並且會使用 Office 365 / Azure AD 中，以檢查 AD FS 憑證


可以使用下列 PowerShell cmdlet 在 proxy 上停用 AD FS 端點：
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

例如: 
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>驗證擴充的保護
驗證擴充的保護是一項功能，可以降低對中間人 (MITM) 攻擊，並且與 AD FS 預設會啟用。

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>若要確認設定，您可以執行下列作業：
可驗證設定，使用下列 PowerShell cmdlet。  
    
   `PS:\>Get-ADFSProperties`

屬性是`ExtendedProtectionTokenCheck`。  預設值為 [允許]，以便可以達成的安全性優點，而不需要與不支援功能的瀏覽器的相容性考量。  

### <a name="congestion-control-to-protect-the-federation-service"></a>若要保護的 federation service 的壅塞控制
Federation service proxy （WAP 的一部分） 提供防止大量的要求中的 AD FS 服務的控制阻塞。  Web Application Proxy 將拒絕外部用戶端驗證要求，如果同盟伺服器多載，因為偵測到的 Web 應用程式 Proxy 與同盟伺服器之間的延遲。  這項功能是建議的延遲臨界值層級的預設設定。

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>若要確認設定，您可以執行下列作業：
1.  在您的 Web 應用程式 Proxy 電腦上啟動提升權限的命令視窗。
2.  瀏覽至 %windir%\adfs\config 的 ADFS 目錄。
3.  從其預設值，以變更壅塞控制設定 '<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />'。
4.  儲存並關閉檔案。
5.  執行 'net stop adfssrv' 然後 'net start adfssrv' 來重新啟動 AD FS 服務。
供您參考，可以找到這項功能的指引[此處](https://msdn.microsoft.com/en-us/library/azure/dn528859.aspx )。

### <a name="standard-http-request-checks-at-the-proxy"></a>標準的 HTTP 要求會檢查在 proxy
Proxy 也會執行標準的下列檢查，針對所有流量：

- 本身 FS-P 向 AD FS，透過短期存在的憑證。  在懷疑洩露的 dmz 伺服器的案例中，AD FS 可以 [撤銷 proxy 信任]，讓它將不再信任的任何連入要求可能會危害 proxy。 Proxy 信任撤銷每個 proxy 自己的憑證，讓它無法成功驗證用於任何用途，AD FS 伺服器
- FS-P 會終止所有的連線，並在內部網路上建立新的 HTTP 連接到 AD FS 服務。 這會提供工作階段層級之間的緩衝區外部裝置和 AD FS 服務。 外部裝置永遠不會直接連線到 AD FS 服務。
- FS-P 執行特別會篩選掉不需要由 AD FS 服務的 HTTP 標頭的 HTTP 要求驗證。

## <a name="recommended-security-configurations"></a>建議的安全性組態
確保所有的 AD FS 和 WAP 伺服器收到最新最重要的安全性建議，為您的 AD FS 基礎結構可確保您採用一種方法來保留您的 AD FS 和 WAP 伺服器的所有安全性更新，以及這些選擇性的更新適用於 AD FS，在此頁面上指定為重要的更新。

監視並隨時保持最新的 Azure AD 客戶的建議方式，其基礎結構會是透過 Azure AD Connect Health for AD FS 中，Azure AD Premium 的功能。  Azure AD Connect Health 可包含的監視與 AD FS 或 WAP 電腦遺漏其中一項重要的更新特別針對 AD FS 和 WAP 時觸發的警示。

安裝 Azure AD Connect Health for AD FS 可以找到詳細資訊[此處](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-health-agent-install/)。

## <a name="additional-security-configurations"></a>額外的安全性組態
下列額外的功能可以選擇性地設定，以提供額外的保護，以所提供的預設部署中。

### <a name="extranet-soft-lockout-protection-for-accounts"></a>外部網路的 「 軟性 」 鎖定保護帳戶
Windows Server 2012 R2 中的外部網路鎖定功能，使用 AD FS 系統管理員可以設定的驗證要求失敗 (ExtranetLockoutThreshold) 數上限和 ' 觀測視窗的時間週期 (ExtranetObservationWindow)。 當到達此數目上限 (ExtranetLockoutThreshold) 的驗證要求時，AD FS 會停止嘗試驗證提供的帳戶認證與 AD FS 設定的時間週期 (ExtranetObservationWindow)。 這個動作可防止 AD 帳戶鎖定此帳戶，換句話說，它可防止此帳戶無法存取公司資源依賴 AD FS 進行驗證的使用者。 這些設定套用到所有 AD FS 服務可驗證的網域。

您可以使用下列 Windows PowerShell 命令來設定 AD FS 外部網路鎖定 （例如）： 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

這項功能的公開文件的參考，[此處](https://technet.microsoft.com/library/dn486806.aspx )。 

### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>區分內部網路和外部網路存取的存取原則
AD FS 具有能夠區分的要求是源自於來自網際網路透過 proxy 的本機、 公司網路與要求的存取原則。  這可以在每個應用程式或全域完成。  高商業價值的應用程式或應用程式的機密或可識別個人身分的資訊，請考慮需要多重要素驗證。  這可以透過 AD FS 管理嵌入式管理單元完成。  

### <a name="require-multi-factor-authentication-mfa"></a>需要多重要素驗證 (MFA)
AD FS 可以設定為需要強式驗證 （例如多重要素驗證），專為透過 proxy 傳入要求、 個別的應用程式，以及這兩個 Azure AD 條件式存取 / Office 365 和內部部署資源。  支援的 MFA 的方法包括 Microsoft Azure MFA 與協力廠商的提供者。  系統會提示使用者提供的其他資訊 (例如 SMS 文字，其中包含一個時間的程式碼)，和 AD FS 搭配外掛程式，以允許存取的特定提供者。  

支援外部的 MFA 提供者包括列入[這](https://technet.microsoft.com/library/dn758113.aspx) 頁面上，以及全域 HDI。

### <a name="hardware-security-module-hsm"></a>硬體安全性模組 (HSM)
以預設設定，用來簽署權杖永遠不會的 AD FS 的金鑰會保留在內部網路上同盟伺服器。  永遠不會出現在 DMZ 中，或在 proxy 電腦上。  （選擇性） 若要提供額外的保護，這些金鑰可保護附加至 AD FS 的硬體安全性模組中。  Microsoft 不會產生 HSM 產品，但有幾個支援 AD FS 的市場上。  若要實作這項建議，請遵循廠商指導來建立 X509 憑證進行簽署和加密，然後使用 AD FS 安裝 powershell cmdlet，請指定您自訂的憑證，如下所示：

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

其中：


- `CertificateThumbprint` 為您的 SSL 憑證
- `SigningCertificateThumbprint` 為您的簽署憑證 （使用受 HSM 保護金鑰）
- `DecryptionCertificateThumbprint` 為您的加密憑證 （使用受 HSM 保護金鑰）



