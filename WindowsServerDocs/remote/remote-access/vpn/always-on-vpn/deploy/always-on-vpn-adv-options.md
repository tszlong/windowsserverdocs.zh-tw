---
title: Always On VPN 進階功能
description: 除了此部署中提供的部署案例外，您還可以新增其他 advanced VPN 功能來改善 VPN 連線的安全性和可用性。
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 07/24/2019
ms.author: pashort, v-tea
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 73d64cd143b7bbd13e0eb9bb5fadfbb1e7c65416
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371830"
---
# <a name="advanced-features-of-always-on-vpn"></a>Always On VPN 的先進功能

>適用于： Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows 10

- [**上一步：** 瞭解 Always On VPN 技術](../always-on-vpn-technology-overview.md)
- [**下一步：** 開始規劃 Always On VPN 部署](always-on-vpn-deploy-planning.md)

除了所提供的部署案例外，您還可以新增其他 advanced VPN 功能來改善 VPN 連線的安全性和可用性。 例如，VPN 伺服器可以使用這些功能，協助確保連線的用戶端在允許連線之前狀況良好。

## <a name="high-availability"></a>高可用性

以下是高可用性的其他選項。

|選項  |描述  |
|---------|---------|
|伺服器復原和負載平衡     |在需要高可用性或支援大量要求的環境中，您可以在執行網路原則伺服器（NPS）的多部伺服器之間使用負載平衡，以提高遠端存取的效能和復原能力，並啟用遠端存取服務器群集。<p>相關檔：<ul><li>[NPS Proxy 伺服器負載平衡](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[在叢集中部署遠端存取](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|地理位置彈性     |對於以 IP 為基礎的地理位置，您可以在 Windows Server 2016 中使用全域流量管理員搭配 DNS。 如需更穩固的地理負載平衡，您可以使用全域伺服器負載平衡解決方案，例如 Microsoft Azure 流量管理員。<p>相關檔：<ul><li>[流量管理員總覽](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Microsoft Azure 流量管理員](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

## <a name="advanced-authentication"></a>先進驗證

以下是驗證的其他選項。

|選項  |描述  |
|---------|---------|
|Windows Hello 企業版     |在 Windows 10 中，Windows Hello 企業版會藉由在電腦和行動裝置上提供強式雙因素驗證來取代密碼。 這項驗證是由系結至裝置並使用生物識別或個人識別碼（PIN）的新使用者認證類型所組成。<p>Windows 10 VPN 用戶端與 Windows Hello 企業版相容。 使用者使用手勢登入後，VPN 連線會使用 Windows Hello 企業版憑證來進行以憑證為基礎的驗證。<p>相關檔：<ul><li>[Windows Hello 企業版](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>技術案例研究：[在 windows 10 中啟用 Windows Hello 企業版的遠端存取](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|Azure 多重要素驗證（MFA）     |Azure MFA 具有雲端和內部部署版本，您可以將其與 Windows VPN 驗證機制整合。<p>如需此機制運作方式的詳細資訊，請參閱[整合 RADIUS 驗證與 Azure 多因素驗證服務器](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius)。         |

## <a name="advanced-vpn-features"></a>Advanced VPN 功能

以下是 advanced 功能的其他選項。

|選項  |描述  |
|---------|---------|
|流量篩選     |如果您必須強制選擇 VPN 用戶端可以存取的應用程式，您可以啟用 VPN 流量篩選。<p>如需詳細資訊，請參閱[VPN 安全性功能](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features)。         |
|應用程式觸發的 VPN     |您可以將 VPN 設定檔設定為在特定應用程式或應用程式類型啟動時自動連接。<p>如需此和其他觸發選項的詳細資訊，請參閱[VPN 自動觸發的設定檔選項](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile)。         |
|VPN 條件式存取   |條件式存取與裝置合規性可能會要求受控裝置必須符合標準，才能連線至 VPN。 VPN 條件式存取的其中一個先進功能，可讓您將 VPN 連線限制為只有用戶端驗證憑證包含**1.3.6.1.4.1.311.87**的「AAD 條件式存取」 OID 的連接。<p>若要限制 VPN 連線，您必須執行下列動作：<ol><li>在 NPS 伺服器上，開啟 [**網路原則伺服器**] 嵌入式管理單元。</li><li>展開 [**原則**] > [**網路原則**]。</li><li>以滑鼠右鍵按一下 [**虛擬私人網路（VPN）** 連線] 網路原則，然後選取 [**屬性**]。</li><li>選取 [**設定**] 索引標籤。</li><li>選取 [**廠商特定**]，然後選取 [**新增**]。</li><li>選取 [**允許的憑證-OID** ] 選項，然後選取 [**新增**]。</li><li>貼上**1.3.6.1.4.1.311.87**的 AAD 條件式存取 OID 做為屬性值，然後選取 **[確定]** 兩次。</li><li>選取 [**關閉**]，然後選取 [套用 **]。**<p>在您遵循這些步驟之後，當 VPN 用戶端嘗試使用短期雲端憑證以外的任何憑證進行連線時，連線會失敗。</li></ol>如需條件式存取的詳細資訊，請參閱[VPN 和條件式存取](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)。   |


---
## <a name="blocking-vpn-clients-that-use-revoked-certificates"></a>封鎖使用撤銷憑證的 VPN 用戶端
  
安裝更新之後，RRAS 伺服器可以針對使用 IKEv2 和電腦憑證進行驗證的 Vpn 強制執行憑證撤銷，例如裝置通道 Always on Vpn。 這表示對於這類 Vpn，RRAS 伺服器可以拒絕嘗試使用已撤銷憑證之用戶端的 VPN 連線。

**可用性**

下表列出包含每個 Windows 版本之修正程式的版本。

|作業系統版本 |發行  |
|---------|---------|
|Windows Server 版本 1903  |[KB4501375](https://support.microsoft.com/help/4501375/windows-10-update-kb4501375) |
|Windows Server 2019<br />Windows Server，版本1809  |[KB4505658](https://support.microsoft.com/help/4505658/windows-10-update-kb4505658)  |
|Windows Server 1803 版  |[KB4507466](https://support.microsoft.com/help/4507466/windows-10-update-kb4507466)  |
|Windows Server 1709 版  |[KB4507465](https://support.microsoft.com/help/4507465/windows-10-update-kb4507465)  |
|Windows Server 2016，版本1607  |[KB4503294](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294) |

**如何設定必要條件** 

1. 在 Windows 更新可供使用時加以安裝。
1. 請確定您使用的所有 VPN 用戶端和 RRAS 伺服器憑證都具有 CDP 專案，而且 RRAS 伺服器可以連線到各自的 Crl。
1. 在 RRAS 伺服器上，使用 VpnAuthProtocol PowerShell Cmdlet 來**設定** **RootCertificateNameToAccept**參數。<br /><br />
   下列範例會列出執行此動作的命令。 在此範例中， **CN = Contoso 根憑證授權**單位代表根憑證授權單位的辨別名稱。 
   ``` powershell
   $cert1 = ( Get-ChildItem -Path cert:LocalMachine\root | Where-Object -FilterScript { $_.Subject -Like "*CN=Contoso Root Certification Authority*" } )
   Set-VpnAuthProtocol -RootCertificateNameToAccept $cert1 -PassThru
   ```
**如何設定 RRAS 伺服器，以針對以 IKEv2 機器憑證為基礎的 VPN 連線強制執行憑證撤銷**

1. 在 [命令提示字元] 視窗中，執行下列命令： 
   ```
   reg add HKLM\SYSTEM\CurrentControlSet\Services\RemoteAccess\Parameters\Ikev2 /f /v CertAuthFlags /t REG_DWORD /d "4"
   ```

1. 重新開機**路由及遠端存取**服務。
  
若要停用這些 VPN 連線的憑證撤銷，請設定**CertAuthFlags = 2**或移除**CertAuthFlags**值，然後重新開機**路由及遠端存取**服務。 

**如何撤銷以 IKEv2 機器憑證為基礎之 VPN 連線的 VPN 用戶端憑證**
1. 撤銷憑證授權單位單位的 VPN 用戶端憑證。
1. 從憑證授權單位單位發佈新的 CRL。
1. 在 RRAS 伺服器上，開啟系統管理命令提示字元視窗，然後執行下列命令：
   ```
   certutil -urlcache * delete
   certutil -setreg chain\ChainCacheResyncFiletime @now
   ```

**如何確認 IKEv2 電腦憑證型 VPN 連線的憑證撤銷是否正常運作**  
>[!Note]  
> 使用此程式之前，請確定您已啟用 CAPI2 操作事件記錄檔。
1. 遵循先前的步驟來撤銷 VPN 用戶端憑證。
1. 嘗試使用具有已撤銷憑證的用戶端連線到 VPN。 RRAS 伺服器應拒絕連線並顯示訊息，例如「IKE 驗證認證無法接受」。
1. 在 RRAS 伺服器上，開啟事件檢視器，然後流覽至 [**應用程式及服務] [Logs/Microsoft/Windows/CAPI2**]。 
1. 搜尋具有下列資訊的事件：
   * 記錄名稱： **CAPI2/操作 microsoft-windows-CAPI2/營運**
   * 事件識別碼： **41** 
   * 事件包含下列文字： **subject = "*client FQDN*"** （*用戶端 fqdn*代表擁有已撤銷憑證之用戶端的完整功能變數名稱）。 

   事件資料的 **<Result>** 欄位應包含**已撤銷的憑證**。 例如，請參閱下列事件的摘錄：
   ```xml
   Log Name:      Microsoft-Windows-CAPI2/Operational Microsoft-Windows-CAPI2/Operational  
   Source:        Microsoft-Windows-CAPI2  
   Date:          5/20/2019 1:33:24 PM  
   Event ID:      41  
   ...  
   Event Xml:
   <Event xmlns="https://schemas.microsoft.com/win/2004/08/events/event">
    <UserData>  
     <CertVerifyRevocation>  
      <Certificate fileRef="C97AE73E9823E8179903E81107E089497C77A720.cer" subjectName="client01.corp.contoso.com" />  
      <IssuerCertificate fileRef="34B1AE2BD868FE4F8BFDCA96E47C87C12BC01E3A.cer" subjectName="Contoso Root Certification Authority" />
      ...
      <Result value="80092010">The certificate is revoked.</Result>
     </CertVerifyRevocation>
    </UserData>
   </Event>
   ```

---
## <a name="additional-protection"></a>其他保護

### <a name="trusted-platform-module-tpm-key-attestation"></a>信賴平臺模組（TPM）金鑰證明

具有 TPM 證明金鑰的使用者憑證提供較高的安全性保證，由非匯出性、反 anti-hammering 和 TPM 所提供的金鑰隔離所備份。

如需有關 Windows 10 中 TPM 金鑰證明的詳細資訊，請參閱[Tpm 金鑰證明](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation)。

## <a name="next-step"></a>後續步驟

[開始規劃 ALWAYS ON VPN 部署](always-on-vpn-deploy-planning.md)：在您要用來做為 VPN 伺服器的電腦上安裝遠端存取服務器角色之前，請執行下列工作。 適當規劃之後，您可以部署 Always On VPN，並選擇性地使用 Azure AD 設定 VPN 連線的條件式存取。  

## <a name="related-topics"></a>相關主題
- [NPS Proxy 伺服器負載平衡](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)：遠端驗證撥入使用者服務（RADIUS）用戶端是網路存取伺服器，例如虛擬私人網路（VPN）伺服器和無線存取點、建立連線要求，並將它們傳送至 RADIUS 伺服器（例如 NPS）。 在某些情況下，NPS 伺服器可能會一次收到太多連線要求，因而導致效能降低或超載。

- [流量管理員的總覽](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)：本主題提供 Azure 流量管理員的總覽，讓您控制服務端點的使用者流量分配。 流量管理員使用網域名稱系統（DNS），根據流量路由方法和端點的健康情況，將用戶端要求導向到最適當的端點。 

- [Windows Hello 企業版](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)：本主題提供必要條件，例如僅限雲端部署和混合式部署。  本主題也會列出 Windows Hello 企業版的相關常見問題。

- [技術案例研究：在 windows 10 中啟用 Windows Hello 企業版的遠端存取](https://msdn.microsoft.com/library/mt728163.aspx)：在此技術案例研究中，您會瞭解 Microsoft 如何使用 Windows Hello 企業版來執行遠端存取。  Windows Hello 企業版是私用/公開金鑰，或以憑證為基礎的驗證方法，適用于超過密碼的組織和取用者。 這種形式的驗證依賴金鑰組認證，其可取代密碼並可抵抗漏洞、竊取及網路釣魚。 

- 將[radius 驗證與 Azure 多因素驗證服務器整合](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius)：本主題將逐步引導您使用 Azure 多因素驗證服務器來新增和設定 radius 用戶端驗證。 RADIUS 是接受驗證要求以及處理這些要求的標準通訊協定。 Azure 多因素驗證服務器可作為 RADIUS 伺服器。 

- [Vpn 安全性功能](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features)：本主題提供您用於鎖定 Vpn、Windows 資訊保護（WIP）與 vpn 整合的 VPN 安全性方針，以及流量篩選準則。 

- [Vpn 自動觸發的設定檔選項](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile)：本主題提供 vpn 自動觸發的設定檔選項，例如應用程式觸發程式、以名稱為基礎的觸發程式，以及 Always On。

- [VPN 和條件式存取](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)：本主題提供雲端型條件式存取平臺的總覽，以提供遠端用戶端的裝置相容性選項。 「條件式存取」是原則型評估引擎，可讓您為任何 Azure Active Directory (Azure AD) 已連線的應用程式建立存取規則。 

- [TPM 金鑰證明](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation)：本主題提供可信賴平臺模組（tpm）的總覽，以及部署 TPM 金鑰證明的步驟。 您也可以尋找疑難排解資訊和解決問題的步驟。
