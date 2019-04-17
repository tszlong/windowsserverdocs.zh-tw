---
ms.assetid: b7bf7579-ca53-49e3-a26a-6f9f8690762f
title: "AD FS 和 Web 應用程式 Proxy 安全的最佳做法"
description: "本文件會提供最佳的做法安全計劃以及 Active Directory 同盟 Services (AD FS) 和 Web 應用程式 Proxy 部署。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6026246228b22fdea6001528ab7621a1704f1983
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
## <a name="best-practices-for-securing-active-directory-federation-services"></a>最佳做法保護 Active Directory 同盟服務

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本文件會提供最佳的做法安全計劃以及 Active Directory 同盟 Services (AD FS) 和 Web 應用程式 Proxy 部署。  它包含這些元件與建議的組織使用特定案例和安全性需求額外的安全性設定預設行為的相關資訊。

本文件適用於 AD FS 和 Windows Server 2012 R2 與 Windows Server 2016 （預覽版） WAP。  在場所網路或裝載的雲端 Microsoft Azure 例如環境中部署基礎結構是否可以使用這些建議。

## <a name="standard-deployment-topology"></a>標準部署拓撲
先環境中部署，我們建議您標準部署拓撲一或多個 AD FS 上伺服器企業連絡，以 DMZ 或外部網路中的一或多個 Web 應用程式 Proxy (WAP) 伺服器所組成。  AD FS 和 WAP，每個層級硬體或軟體負載平衡器放伺服器陣列前面，而處理流量路由。  每個 （FS 和 proxy） 農場前面防火牆位於為所需的外部負載平衡器 IP 位址前面。

![AD FS 標準拓撲](media/Best-Practices-Securing-AD-FS/adfssec1.png)

## <a name="ports-required"></a>需要連接埠
圖表如下描述防火牆連接埠必須在 AD FS 和 WAP 部署 」 的元件和之間會支援。  如果部署不包含 Azure AD / 被略過 Office 365、 同步需求。

>如果使用者憑證使用驗證，這是選擇性 Azure AD 所需的連接埠 49443 只有的筆記與 Office 365。

![AD FS 標準拓撲](media/Best-Practices-Securing-AD-FS/adfssec2.png)

### <a name="azure-ad-connect-and-federation-serverswap"></a>Azure AD 連接和聯盟伺服器日 WAP
下表描述的連接埠和通訊協定進行通訊 Azure AD 連接伺服器之間聯盟日 WAP 伺服器。  

通訊協定 |連接埠 |描述
--------- | --------- |---------
HTTP|80 (TCP 日 UDP)|用來下載 Crl （憑證撤銷列出） 來確認 SSL 憑證。
HTTPS|443(TCP/UDP)|使用 Azure AD 的同步處理。
WinRM|5985| WinRM 其實

### <a name="wap-and-federation-servers"></a>WAP 和聯盟伺服器
下表描述的連接埠與所需的通訊聯盟伺服器 WAP 伺服器間通訊協定。

通訊協定 |連接埠 |描述
--------- | --------- |---------
HTTPS|443(TCP/UDP)|使用進行驗證。

### <a name="wap-and-users"></a>WAP 和使用者
下表描述的連接埠與所需的使用者與 WAP 伺服器間通訊的通訊協定。

通訊協定 |連接埠 |描述
--------- | --------- |--------- |
HTTPS|443(TCP/UDP)|用於裝置的驗證。
TCP|49443 (TCP)|用於憑證驗證。

如需詳細資訊，所需的連接埠與所需的混合部署看到文件通訊協定[在此](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-ports/)。

連接埠和通訊協定 Azure AD 所需的詳細資訊的 Office 365 部署，查看 [文件和[在此](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US)。

### <a name="endpoints-enabled"></a>支援的端點

AD FS 和 WAP 安裝時，預設設定端點 AD FS 的才在同盟服務和 proxy。  根據最常要求並使用案例選擇預設值，並不需要變更。  

### <a name="optional-min-set-of-endpoints-proxy-enabled-for-azure-ad--office-365"></a>[選擇性]最小值設定端點 proxy 支援 Azure AD 的日 Office 365
將 AD FS 和 WAP 部署的 Azure AD 只組織與 Office 365 案例] 可以限制進一步達成較小的攻擊 surface 支援 proxy 上 AD FS 端點的數目。
以下是清單中的端點必須會在這些案例中 proxy 功能：

|端點|用途
|-----|-----
|1 月 adfs 日 ！|瀏覽器驗證流量與目前的版本的 Microsoft Office 使用 Azure AD 從此端點進行與 Office 365 驗證
|/adfs/services/trust/2005/usernamemixed|使用適用於 Exchange Online Office 戶端超過 Office 2013 5 2015 update。  稍後戶端使用被動式 \adfs\ls 結束點。
|/adfs/services/trust/13/usernamemixed|使用適用於 Exchange Online Office 戶端超過 Office 2013 5 2015 update。  稍後戶端使用被動式 \adfs\ls 結束點。
|oauth2 日 adfs 日|使用此 （prem 上或在雲端中） 的現代化應用程式，您已經設定直接向 （也就是不是透過 AAD) AD FS 進行驗證
|/adfs/services/trust/mex|使用適用於 Exchange Online Office 戶端超過 Office 2013 5 2015 update。  稍後戶端使用被動式 \adfs\ls 結束點。
|/adfs/ls/federationmetadata/2007-06/federationmetadata.xml |適用於任何被動式流量; 需求使用 Office 365 / Azure AD，以檢查 AD FS 憑證，並


AD FS 端點可以使用下列 PowerShell cmdlet proxy 上已停用：
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath <address path> -Proxy $false

例如：
    
    PS:\>Set-AdfsEndpoint -TargetAddressPath /adfs/services/trust/13/certificatemixed -Proxy $false
    

### <a name="extended-protection-for-authentication"></a>驗證延伸的保護
驗證延伸的保護是緩和男人 (MITM) 攻擊並 AD FS 使用的預設支援的功能。

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>若要確認設定，您可以執行下列動作：
該設定可以使用驗證 PowerShell commandlet 下方。  
    
   `PS:\>Get-ADFSProperties`

屬性是`ExtendedProtectionTokenCheck`。  預設值是允許，以便可以的瀏覽器不支援的功能與相容性問題，而達成安全性優點。  

### <a name="congestion-control-to-protect-the-federation-service"></a>壅塞控制保護同盟服務
同盟服務 proxy （WAP 部） 提供壅塞控制 AD FS 服務防止大量的要求。  如果偵測到的應用程式網路 Proxy 與聯盟伺服器之間的延遲為多聯盟伺服器載應用程式網路 Proxy 將拒絕外部 client 驗證要求。  這項功能與建議的延遲閾值來改善層級預設設定。

#### <a name="to-verify-the-settings-you-can-do-the-following"></a>若要確認設定，您可以執行下列動作：
1.  Web 應用程式 Proxy 電腦時，[開始] 視窗中提升權限的命令。
2.  瀏覽至 ADFS directory，WINDIR%\adfs\config %。
3.  變更壅塞控制設定為預設值，以]<congestionControl latencyThresholdInMSec="8000" minCongestionWindowSize="64" enabled="true" />'。
4.  儲存，並關閉檔案。
5.  將 AD FS 服務執行 'net 停止 adfssrv' 然後 '網路開始 adfssrv' 重新開機。
可供您參考，找到此功能的指導方針[在此](https://msdn.microsoft.com/en-us/library/azure/dn528859.aspx )。

### <a name="standard-http-request-checks-at-the-proxy"></a>標準 HTTP 要求檢查 proxy。
Proxy 也會執行下列對所有流量標準檢查：

- FS-P 本身向 AD FS 透過短暫的憑證。  在可疑 dmz 伺服器危害的案例中，AD FS 可 」 撤銷 proxy 信任 」，它不會再信任的任何傳入要求可能危害 proxy。 撤銷 proxy 信任撤銷每個 proxy 自己的憑證，使其無法通過驗證適用於任何用途 AD FS 伺服器
- FS P 終止所有連接和連絡上建立新的 HTTP 連接 AD FS 服務。 這會提供 AD FS 服務外接式裝置之間工作階段層級緩衝。 直接與服務 AD FS 不會連接的外部裝置。
- FS P 執行 HTTP 要求驗證，尤其是篩選掉不需要的 AD FS 服務 HTTP 標頭。

## <a name="recommended-security-configurations"></a>建議的安全性設定
確保所有 AD FS 和 WAP 伺服器接收最重要的安全性 AD FS 基礎結構建議以都確定您有一種方法可以保留目前的所有安全性更新，以及這些選用的更新為重要 AD fs 此頁面上指定的伺服器 AD FS 和 WAP 的最新的更新。

Azure AD 針對監視和保持最新的建議方式其基礎結構是透過 Azure AD 連接健康 AD fs，Azure AD Premium 的功能。  Azure AD 連接健康包含顯示器和觸發 AD FS 或 WAP 電腦是專為 AD FS 和 WAP 遺失其中一個重要更新通知。

安裝 Azure AD 連接健康的 AD FS 可找到詳細資訊[在此](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-health-agent-install/)。

## <a name="additional-security-configurations"></a>更多安全性設定
可以提供額外的保護所提供的預設部署選擇性設定以下的額外功能。

### <a name="extranet-soft-lockout-protection-for-accounts"></a>「 軟體 」 的外部鎖定帳號保護
使用外部鎖定功能在 Windows Server 2012 R2，AD FS 管理員可以設定最多允許的數量驗證失敗的要求 (ExtranetLockoutThreshold) 並 ' 觀察視窗的時間間隔 (ExtranetObservationWindow)。 當達到上限 (ExtranetLockoutThreshold) 的驗證要求時，將會阻止 AD FS 想要的設定的期間 (ExtranetObservationWindow) 驗證 AD FS 提供的 account 憑證。 這個動作防止 AD 鎖定此帳號，亦即，它可以避免此 account 公司資源 AD FS 進行驗證的使用者所仰賴喪失存取權。 這些設定套用到所有網域驗證，AD FS 服務。

您可以使用下列的 Windows PowerShell 命令來設定 AD FS 外部鎖定 （範例）： 

    PS:\>Set-AdfsProperties -EnableExtranetLockout $true -ExtranetLockoutThreshold 15 -ExtranetObservationWindow ( new-timespan -Minutes 30 )

如需參考資料，這項功能的公開文件是[在此](https://technet.microsoft.com/en-us/library/dn486806.aspx )。 

### <a name="differentiate-access-policies-for-intranet-and-extranet-access"></a>區分存取原則內部和外部網路的存取權
AD FS 有區分存取原則的要求，源自會從網際網路透過 proxy 本機、 公司網路與要求的功能。  這可以在每個應用程式或全球完成。  高價值應用程式或應用程式與敏感或個人資訊，請考慮將需要使用多監視器因素驗證。  這可透過 AD FS 嵌入式管理單元完成。  

### <a name="require-multi-factor-authentication-mfa"></a>需要多因數驗證 (MFA)
您可以設定 AD FS 需要 （例如，使用多監視器因素驗證） 穩固驗證要求透過 proxy 提供專為個人應用程式，並條件存取這兩個 Azure AD 日 Office 365 和場所資源。  支援的方法的 MFA 包含 Microsoft Azure MFA 和協力廠商提供者。  提示使用者提供額外的資訊 (例如包含一個簡訊文字時間程式碼)，並 AD FS 使用插件，以允許存取特定提供者。  

支援的外部 MFA 提供者包含中所列出[這個](https://technet.microsoft.com/en-us/library/dn758113.aspx)頁面上，以及 HDI 全球。

### <a name="hardware-security-module-hsm"></a>硬體安全性單元 (HSM)
預設設定，一律不登入權杖按鍵 AD FS 使用會保留在企業網路聯盟伺服器。  它們有永遠不會存在 DMZ 或 proxy 電腦上。  也提供額外的保護，這些按鍵可保護單元硬體安全性連接到 AD FS。  Microsoft 並不會發出 HSM product，但有許多支援 AD FS 市面上。  為了執行這個建議，請依照下列建立 X509 廠商指導方針憑證來簽署及加密，然後使用 AD FS 安裝 powershell commandlets，指定您自訂的憑證，如下所示：

    PS:\>Install-AdfsFarm -CertificateThumbprint <String> -DecryptionCertificateThumbprint <String> -FederationServiceName <String> -ServiceAccountCredential <PSCredential> -SigningCertificateThumbprint <String>

位置：


- `CertificateThumbprint` 為您的 SSL 憑證。
- `SigningCertificateThumbprint` 您專屬的簽署憑證 （具有 HSM 保護鍵）
- `DecryptionCertificateThumbprint` 為您的加密憑證 （具有 HSM 保護鍵）



