---
title: 設定 Windows 10 用戶端 Always On VPN 連線
description: 在此步驟中，您可以深入了解 ProfileXML 選項以及結構描述，並設定 Windows 10 用戶端電腦通訊的 VPN 連線與該基礎結構。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 05/29/2018
ms.assetid: d165822d-b65c-40a2-b440-af495ad22f42
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.reviewer: deverette
ms.openlocfilehash: fdf640d7e98781ca054ab982027bdfe8c3fb64d6
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749629"
---
# <a name="step-6-configure-windows-10-client-always-on-vpn-connections"></a>步驟 6. 設定 Windows 10 用戶端一律開啟 」 VPN 連線

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows 10

- [**前一個：** 步驟 5.設定 DNS 和防火牆設定](vpn-deploy-dns-firewall.md)<br>
- [**下一步:** 步驟 7.（選擇性）使用 Azure AD 的 VPN 連線能力的條件式存取](../../ad-ca-vpn-connectivity-windows10.md)

在此步驟中，您將了解 ProfileXML 選項與結構描述，並設定 Windows 10 用戶端電腦，與該基礎結構具有 VPN 連線進行通訊。

您可以設定一律開啟 」 VPN 用戶端，透過 PowerShell、 SCCM 或 Intune。 這三個需要 XML VPN 設定檔來設定適當的 VPN 設定。 自動化 PowerShell 的組織未安裝 SCCM 或 Intune 註冊有可能。

>[!NOTE]
>群組原則不包括系統管理範本設定 Windows 10 的遠端存取一律在 VPN 用戶端。  不過，您可以使用登入指令碼。

## <a name="profilexml-overview"></a>ProfileXML 概觀

ProfileXML 是 VPNv2 CSP 中的 [URI] 節點。 而不是個別設定每個 VPNv2 CSP 節點 — 例如觸發程序，將路由清單和驗證通訊協定，來設定 Windows 10 VPN 用戶端提供的所有設定為單一的 CSP 節點的單一 XML 區塊中使用此節點。 ProfileXML 結構描述符合 VPNv2 CSP 節點的結構描述幾乎相同，但某些詞彙會稍有不同。

您可以使用 ProfileXML 此部署的描述，包括 Windows PowerShell、 System Center Configuration Manager 和 Intune 的所有傳遞方法。 有兩種方式可在此部署中設定 ProfileXML VPNv2 CSP 節點：

- **OMA DM**。 一種方式是使用稍早所述的一節中的 MDM 提供者使用 OMA DM， [VPNv2 CSP 節點](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes)。 使用此方法，您可以輕鬆地將 VPN 設定檔設定 XML 標記插入 ProfileXML CSP 節點使用 Intune 時。

- **Windows Management Instrumentation (WMI)--CSP 橋接器**。 設定 ProfileXML CSP 節點的第二個方法是使用 WMI 至 CSP 橋接器 — 呼叫 WMI 類別**MDM_VPNv2_01**— VPNv2 CSP 和 ProfileXML 節點可存取。 當您建立該 WMI 類別的新執行個體時，WMI 會使用 CSP 來使用 Windows PowerShell 和 System Center Configuration Manager 時建立 VPN 設定檔。

即使這些組態方法不同，兩者都需要格式正確的 XML VPN 設定檔。 若要使用 ProfileXML VPNv2 CSP 設定，您可以建構 XML 使用 ProfileXML 結構描述來設定簡單的部署案例所需的標記。 如需詳細資訊，請參閱 < [ProfileXML XSD](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd)。

下面您會找到每個必要的設定和其對應的 ProfileXML 標記。 您設定每個設定 ProfileXML 結構描述中的特定標記中，並非所有的原生的設定檔下找到。 其他標記放置的功能，請參閱 ProfileXML 結構描述。

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**連線類型：** Native IKEv2

ProfileXML 項目：

```xml
<NativeProtocolType>IKEv2</NativeProtocolType>
```

**路由：** 分割通道

ProfileXML 項目：

```xml
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>
```

**名稱解析：** 網域名稱資訊清單和 DNS 尾碼

ProfileXML 項目：

```xml
<DomainNameInformation>
<DomainName>.corp.contoso.com</DomainName>
<DnsServers>10.10.1.10,10.10.1.50</DnsServers>
</DomainNameInformation>

<DnsSuffix>corp.contoso.com</DnsSuffix>
```

**觸發：** Always On 且受信任網路偵測

ProfileXML 項目：

```xml
<AlwaysOn>true</AlwaysOn>
<TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

**驗證：** PEAP-TLS 使用 TPM 保護的使用者憑證

ProfileXML 項目：

```xml
<Authentication>
<UserMethod>Eap</UserMethod>
<Eap>
<Configuration>...</Configuration>
</Eap>
</Authentication>
```

若要設定某些 VPN 驗證機制，您可以使用簡單的標記。 不過，EAP 與 PEAP 是更為複雜。 建立 XML 標記的最簡單方式是使用它的 EAP 設定，設定 VPN 用戶端，然後將該組態匯出到 XML。

如需有關 EAP 設定的詳細資訊，請參閱[EAP 設定](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/eap-configuration)。

## <a name="manually-create-a-template-connection-profile"></a>手動建立範本的連線設定檔

在此步驟中，您可以使用受保護的可延伸驗證通訊協定 (PEAP) 來保護用戶端與伺服器之間的通訊。 不同於簡單的使用者名稱和密碼，此連線需要唯一的 EAPConfiguration 一節中的 VPN 設定檔，才能運作。

而不會說明如何從頭開始建立 XML 標記，您用於設定 Windows 來建立 VPN 設定檔範本。 建立 VPN 設定檔的範本之後, 您可以使用 Windows PowerShell 取用從該範本，以建立稍後在部署的部署最終 ProfileXML EAPConfiguration 部分。

### <a name="record-nps-certificate-settings"></a>記錄 NPS 憑證設定

在之前建立的範本，請記下的主機名稱或 伺服器憑證的 NPS 伺服器的完整的網域名稱 (FQDN) 和發行憑證的 CA 的名稱。

**程序：**

1. 在 NPS 伺服器上，開啟 網路原則伺服器。

2. 在 NPS 主控台中，在 原則 下按一下 **網路原則**。

3. 以滑鼠右鍵按一下**虛擬私人網路 (VPN) 連線**，然後按一下**屬性**。

4. 按一下 **條件約束**索引標籤，然後按一下**驗證方法**。

5. 在 EAP 類型 中，按一下**Microsoft:受保護的 EAP (PEAP)** ，然後按一下**編輯**。

6. 記錄的值**憑證發給**並**簽發者**。

    您即將推出的 VPN 範本組態中使用這些值。 例如，如果伺服器的 FQDN 是 nps01.corp.contoso.com 主機名稱是 NPS01，憑證名稱為基礎的 FQDN 或 DNS 名稱伺服器 — 比方說，nps01.corp.contoso.com。

7. 取消編輯受保護的 EAP 內容 對話方塊。

8. 取消虛擬私人網路 (VPN) 連線內容 對話方塊。

9. 關閉 網路原則伺服器。

>[!NOTE]
>如果您有多部 NPS 伺服器，請在如此的 VPN 設定檔可以確認每個它們應完成這些步驟，每一項。

### <a name="configure-the-template-vpn-profile-on-a-domain-joined-client-computer"></a>已加入網域的用戶端電腦上設定 VPN 設定檔範本

既然您已加入網域的用戶端電腦上設定 VPN 設定檔範本的必要資訊。 您的使用者類型的帳戶使用 （也就是標準使用者或系統管理員） 的程序的這個部分並不重要。

不過，如果您還沒有設定憑證自動註冊之後，重新啟動電腦，這樣做之前設定範本來確保您有可用的憑證，在其上註冊的 VPN 連線。

>[!NOTE]
>沒有任何方法來手動新增任何進階的屬性，vpn、 Always On 的 NRPT 規則，例如受信任網路偵測等等。在下一個步驟中，建立測試 VPN 連線才能驗證 VPN 伺服器的設定，您可以建立 VPN 連線到伺服器。

**手動建立單一測試 VPN 連線**

1.  成員的身分登入已加入網域的用戶端電腦**VPN 使用者**群組。

2.  在 [開始] 功能表中，輸入**VPN**，然後按 Enter。

3.  在 [詳細資料] 窗格中，按一下**新增 VPN 連線**。

4.  在 [VPN 提供者] 清單中，按一下**Windows （內建）** 。

5.  在 連線名稱，輸入**範本**。

6.  在 伺服器名稱或位址，鍵入**外部**VPN 伺服器的 FQDN (例如 **vpn.contoso.com**)。

7.  按一下 [儲存]  。

8.  相關設定下，按一下**變更介面卡選項**。

9.  以滑鼠右鍵按一下**樣板**，然後按一下**屬性**。

10. 在上**安全性**索引標籤中，於**類型的 VPN**，按一下  **IKEv2**。

11. 在 資料加密，請按一下**最大長度加密**。

12. 按一下 **使用可延伸驗證通訊協定 (EAP)** ; 然後，在**使用可延伸驗證通訊協定 (EAP)** ，按一下  **Microsoft:受保護的 EAP (PEAP) （已啟用加密）** 。

13. 按一下 [**屬性**開啟受保護的 EAP 內容] 對話方塊中，並完成下列步驟：

    a. 在 **連線到這些伺服器**方塊中，輸入您稍早在這一節 (比方說，NPS01) 中的 NPS 伺服器驗證設定從擷取的 NPS 伺服器的名稱。

    >[!NOTE]
    >您所輸入的伺服器名稱必須符合憑證中的名稱。 您復原此稍早在本章節中的名稱。 如果名稱不符，連接將會失敗，指出，「 連線被禁止因為您 RAS/VPN 伺服器上設定的原則。 」

    b.  在 [受信任的根憑證授權單位] 下選取的根 CA 發給 NPS 伺服器的憑證 (例如，contoso-CA)。

    c.  在連線前的通知，按一下**不要求使用者授權新伺服器或受信任的 Ca**。

    d.  在選取驗證方法中，按一下**智慧卡或其他憑證**，然後按一下**設定**。 智慧卡或其他憑證內容 對話方塊隨即開啟。

    e.  按一下 **使用這台電腦上的憑證**。

    f.  在 [連接到這些伺服器] 方塊中，輸入您從 NPS 伺服器驗證設定，在先前步驟中擷取的 NPS 伺服器的名稱。

    g.  在 [受信任的根憑證授權單位] 下選取的根 CA 發給 NPS 伺服器的憑證。

    h.  選取 **不要提示使用者授權新伺服器或受信任的憑證授權單位**核取方塊。

    i.  按一下 [**確定**關閉智慧卡或其他憑證內容] 對話方塊。

    j.  按一下 **確定**以關閉 受保護的 EAP 內容 對話方塊。

14. 按一下 [**確定**關閉範本屬性] 對話方塊。

15. 關閉 [網路連線] 視窗。

16. 在設定中，按一下以測試 VPN**樣板**，然後按一下**Connect**。

>[!IMPORTANT]
>請確定範本至您的 VPN 伺服器的 VPN 連線會成功。 這麼做可確保 EAP 設定正確，才能在下一個範例中使用它們。 您必須連接至少一次，再繼續進行;否則，設定檔不會包含所有 vpn 連線所需的資訊。

## <a name="create-the-profilexml-configuration-files"></a>建立 ProfileXML 設定檔

在之前完成本節中，請確定您已建立並測試範本的 VPN 連線的一節[手動建立範本的連線設定檔](#manually-create-a-template-connection-profile)描述。 測試 VPN 連線是為了確保設定檔包含連接至 VPN 所需的所有資訊。

在 列表 1 中的 Windows PowerShell 指令碼會建立兩個檔案在桌面上，兩者都包含**EAPConfiguration**您先前建立的範本連線設定檔為基礎的標記：

- **VPN_Profile.xml.** 此檔案包含在 VPNv2 CSP 設定 ProfileXML 節點所需的 XML 標記。 OMA DM – 相容 MDM 服務，例如 Intune 中使用此檔案。

- **VPN_Profile.ps1.** 這個檔案是您可以執行的 Windows PowerShell 指令碼來設定 ProfileXML 節點 VPNv2 CSP 中的用戶端電腦上。 您也可以部署這個指令碼透過 System Center Configuration Manager 中設定的 CSP。 您無法在遠端桌面工作階段，包括 HYPER-V 增強的工作階段中執行此指令碼。

>[!IMPORTANT]
>下列範例命令需要 Windows 10 組建 1607年或更新版本。

**建立 VPN_Profile.xml 和 VPN_Proflie.ps1**

1. 登入包含具有相同使用者的 VPN 設定檔範本的已加入網域的用戶端電腦帳戶 區段[手動建立範本的連線設定檔](#manually-create-a-template-connection-profile)所述。

2. 列表 1 貼入 Windows PowerShell 整合式指令碼環境 (ISE) 中，並自訂參數的註解中所述。 這些是 $Template、 $ProfileName、 $Servers、 $DnsSuffix、 $DomainName、 $TrustedNetwork 和 $DNSServers。 每個設定的完整描述是註解中。

3. 執行指令碼來產生**VPN_Profile.xml**並**VPN_Profile.ps1**桌面上。

#### <a name="listing-1-understanding-makeprofileps1"></a>列表 1。 了解 MakeProfile.ps1

本節說明可用來了解如何建立 VPN 設定檔，專為設定 ProfileXML VPNv2 CSP 中的範例程式碼。

組合的指令碼從這個範例程式碼後，當您執行指令碼時，指令碼會產生兩個檔案：VPN_Profile.xml 和 VPN_Profile.ps1。 若要設定 ProfileXML OMA DM 標準 MDM 服務，例如 Microsoft Intune 中使用 VPN_Profile.xml。

使用**VPN_Profile.ps1** ProfileXML 設定 Windows 10 desktop 的 Windows PowerShell 或 System Center Configuration Manager 中的指令碼。

>[!NOTE]
>若要檢視完整的範例指令碼，請參閱下節[MakeProfile.ps1 完整的指令碼](#makeprofileps1-full-script)。

#### <a name="parameters"></a>參數

設定下列參數：

**$Template**。 要從中擷取 EAP 設定範本的名稱。

**$ProfileName**。 設定檔的唯一英數字元識別碼。 設定檔名稱不能包含斜線 （/）。 如果設定檔名稱包含空格或其他非英數字元，則必須正確逸出根據 URL 編碼標準。

**$Servers**。 公用或路由傳送 IP 位址或 VPN 閘道的 DNS 名稱。 它可以指向外部，閘道的 IP 或伺服器陣列的虛擬 IP。 範例，208.147.66.130 或 vpn.contoso.com。

**$DnsSuffix**. 指定一或多個逗號分隔的 DNS 尾碼。 在清單中的第一個也做主要的連線特定 DNS 尾碼的 VPN 介面。 整份清單也會加入到 SuffixSearchList。

**$DomainName**。 用來指示要套用原則的命名空間。 當發出名稱查詢時，DNS 用戶端就會比較 DomainNameInformationList 來尋找相符項目底下的命名空間的所有查詢中的名稱。 這個參數可以是下列類型之一：

- FQDN-完整的網域名稱
- 後置字元-將會附加至 DNS 解析的簡短名稱查詢的網域尾碼。 若要指定後置詞，在前面加上句號 （.） 的 DNS 尾碼。

**$DNSServers**。 清單的逗號分隔的 DNS 伺服器 IP 位址新增至使用命名空間。

**$TrustedNetwork**。 以逗號分隔字串來識別受信任的網路。 VPN 不會連線自動當使用者位於受保護的資源所在的裝置直接存取其公司無線網路。

以下是範例值，如下列命令中使用的參數。 請確定您變更這些值，為您的環境。

```xml
$TemplateName = 'Template'
$ProfileName = 'Contoso AlwaysOn VPN'
$Servers = 'vpn.contoso.com'
$DnsSuffix = 'corp.contoso.com'
$DomainName = '.corp.contoso.com'
$DNSServers = '10.10.0.2,10.10.0.3'
$TrustedNetwork = 'corp.contoso.com'
```

### <a name="prepare-and-create-the-profile-xml"></a>準備和建立設定檔 XML

下列範例命令會取得 EAP 設定，從範本設定檔：

```xml
$Connection = Get-VpnConnection -Name $TemplateName
if(!$Connection)
{
$Message = "Unable to get $TemplateName connection profile: $_"
Write-Host "$Message"
exit
}
$EAPSettings= $Connection.EapConfigXmlStream.InnerXml
```

### <a name="create-the-profile-xml"></a>建立設定檔 XML

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

```xml
$ProfileXML =
'<VPNProfile>
  <DnsSuffix>' + $DnsSuffix + '</DnsSuffix>
  <NativeProfile>
<Servers>' + $Servers + '</Servers>
<NativeProtocolType>IKEv2</NativeProtocolType>
<Authentication>
  <UserMethod>Eap</UserMethod>
  <Eap>
    <Configuration>
  '+ $EAPSettings + '
    </Configuration>
  </Eap>
</Authentication>
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>
  </NativeProfile>
  <AlwaysOn>true</AlwaysOn>
  <RememberCredentials>true</RememberCredentials>
  <TrustedNetworkDetection>' + $TrustedNetwork + '</TrustedNetworkDetection>
  <DomainNameInformation>
<DomainName>' + $DomainName + '</DomainName>
<DnsServers>' + $DNSServers + '</DnsServers>
</DomainNameInformation>
</VPNProfile>'
```

### <a name="output-vpnprofilexml-for-intune"></a>適用於 Intune 的輸出 VPN_Profile.xml

若要儲存設定檔 XML 檔案，您可以使用下列範例命令：

```xml
$ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
```

### <a name="output-vpnprofileps1-for-the-desktop-and-system-center-configuration-manager"></a>輸出 VPN_Profile.ps1 桌面和 System Center Configuration Manager

下列範例程式碼會藉由使用 ProfileXML 節點 VPNv2 CSP 中設定 AlwaysOn IKEv2 VPN 連線。

在 Windows 10 desktop 或 System Center Configuration Manager 中，您可以使用此指令碼。

### <a name="define-key-vpn-profile-parameters"></a>定義索引鍵的 VPN 設定檔參數

```xml
$Script = '$ProfileName = ''' + $ProfileName + '''
$ProfileNameEscaped = $ProfileName -replace ' ', '%20'
```

## <a name="define-vpn-profilexml"></a>定義 VPN ProfileXML

```xml
$ProfileXML = ''' + $ProfileXML + '''
```

### <a name="escape-special-characters-in-the-profile"></a>設定檔中的逸出特殊字元

```xml
$ProfileXML = $ProfileXML -replace '<', '&lt;'
$ProfileXML = $ProfileXML -replace '>', '&gt;'
$ProfileXML = $ProfileXML -replace '"', '&quot;'
```

### <a name="define-wmi-to-csp-bridge-properties"></a>定義 WMI 至 CSP 橋接器屬性

```xml
$nodeCSPURI = "./Vendor/MSFT/VPNv2"
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_VPNv2_01"
```

### <a name="determine-user-sid-for-vpn-profile"></a>決定 VPN 設定檔的使用者 SID:

```xml
try
{
$username = Gwmi -Class Win32_ComputerSystem | select username
$objuser = New-Object System.Security.Principal.NTAccount($username.username)
$sid = $objuser.Translate([System.Security.Principal.SecurityIdentifier])
$SidValue = $sid.Value
$Message = "User SID is $SidValue."
Write-Host "$Message"
}
catch [Exception]
{
$Message = "Unable to get user SID. User may be logged on over Remote Desktop: $_"
Write-Host "$Message"
exit
}
```

### <a name="define-wmi-session"></a>定義 WMI 工作階段：

```xml
$session = New-CimSession
$options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
$options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
$options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)
```

### <a name="detect-and-delete-previous-vpn-profile"></a>偵測並刪除先前的 VPN 設定檔：

```xml
try
{
  $deleteInstances = $session.EnumerateInstances($namespaceName, $className, $options)
  foreach ($deleteInstance in $deleteInstances)
  {
    $InstanceId = $deleteInstance.InstanceID
    if ("$InstanceId" -eq "$ProfileNameEscaped")
    {
        $session.DeleteInstance($namespaceName, $deleteInstance, $options)
        $Message = "Removed $ProfileName profile $InstanceId"
        Write-Host "$Message"
    } else {
        $Message = "Ignoring existing VPN profile $InstanceId"
        Write-Host "$Message"
    }
  }
}
catch [Exception]
{
  $Message = "Unable to remove existing outdated instance(s) of $ProfileName profile: $_"
  Write-Host "$Message"
  exit
}
```

### <a name="create-the-vpn-profile"></a>建立 VPN 設定檔：

```xml
try
{
  $newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
  $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", "String", "Key")
  $newInstance.CimInstanceProperties.Add($property)
  $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", "String", "Key")
  $newInstance.CimInstanceProperties.Add($property)
  $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", "String", "Property")
  $newInstance.CimInstanceProperties.Add($property)
  $session.CreateInstance($namespaceName, $newInstance, $options)
  $Message = "Created $ProfileName profile."


  Write-Host "$Message"
}
catch [Exception]
{
$Message = "Unable to create $ProfileName profile: $_"
Write-Host "$Message"
exit
}

$Message = "Script Complete"
Write-Host "$Message"'
```

### <a name="save-the-profile-xml-file"></a>儲存設定檔 XML 檔案

```xml
$Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')

$Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
Write-Host "$Message"
```

## <a name="makeprofileps1-full-script"></a>MakeProfile.ps1 Full Script

大部分的範例會使用 Set-wmiinstance Windows PowerShell cmdlet，將 ProfileXML 插入 MDM_VPNv2_01 WMI 類別的新執行個體。 

不過，這無法運作，在 System Center Configuration Manager 因為您無法在使用者的內容中執行封裝。 因此，此指令碼建立 WMI 的工作階段在使用者的情況下，使用通用訊息模型，然後再於該工作階段建立 MDM_VPNv2_01 WMI 類別的新執行個體。 此 WMI 類別會使用 WMI 至 CSP 的橋接器來設定 VPNv2 CSP。 因此，藉由新增的類別執行個體，您會設定 CSP。 

>[!IMPORTANT]
>WMI 至 CSP 橋接器會需要本機系統管理員權限，來設計。 每個使用者的 VPN 設定檔部署，您應該使用 SCCM 或 mdm。

>[!NOTE]
>VPN_Profile.ps1 指令碼會使用目前使用者的 SID 來識別使用者的內容。 因為沒有 SID 都是可用的遠端桌面工作階段中，指令碼無法遠端桌面工作階段中。 同樣地，它不適用於 HYPER-V 增強的工作階段。 如果您要在虛擬機器中測試遠端存取一律開啟 」 VPN，停用增強的工作階段在您的用戶端 Vm 上執行此指令碼之前。

下列範例指令碼包含所有的上一節中的程式碼範例。 請確定您將範例值變更為適合您環境的值。
    
   ```XML
    $TemplateName = 'Template'
    $ProfileName = 'Contoso AlwaysOn VPN'
    $Servers = 'vpn.contoso.com'
    $DnsSuffix = 'corp.contoso.com'
    $DomainName = '.corp.contoso.com'
    $DNSServers = '10.10.0.2,10.10.0.3'
    $TrustedNetwork = 'corp.contoso.com'

    
    $Connection = Get-VpnConnection -Name $TemplateName
    if(!$Connection)
    {
    $Message = "Unable to get $TemplateName connection profile: $_"
    Write-Host "$Message"
    exit
    }
    $EAPSettings= $Connection.EapConfigXmlStream.InnerXml
    
    $ProfileXML =
    '<VPNProfile>
      <DnsSuffix>' + $DnsSuffix + '</DnsSuffix>
      <NativeProfile>
    <Servers>' + $Servers + '</Servers>
    <NativeProtocolType>IKEv2</NativeProtocolType>
    <Authentication>
      <UserMethod>Eap</UserMethod>
      <Eap>
       <Configuration>
     '+ $EAPSettings + '
       </Configuration>
      </Eap>
    </Authentication>
    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>
      </NativeProfile>
    <AlwaysOn>true</AlwaysOn>
    <RememberCredentials>true</RememberCredentials>
    <TrustedNetworkDetection>' + $TrustedNetwork + '</TrustedNetworkDetection>
      <DomainNameInformation>
    <DomainName>' + $DomainName + '</DomainName>
    <DnsServers>' + $DNSServers + '</DnsServers>
    </DomainNameInformation>
    </VPNProfile>'
    
    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
    
    $Script = '$ProfileName = ''' + $ProfileName + '''
    $ProfileNameEscaped = $ProfileName -replace ' ', '%20'
    
    $ProfileXML = ''' + $ProfileXML + '''
    
    $ProfileXML = $ProfileXML -replace '<', '&lt;'
    $ProfileXML = $ProfileXML -replace '>', '&gt;'
    $ProfileXML = $ProfileXML -replace '"', '&quot;'
    
    $nodeCSPURI = "./Vendor/MSFT/VPNv2"
    $namespaceName = "root\cimv2\mdm\dmmap"
    $className = "MDM_VPNv2_01"
    
    try
    {
    $username = Gwmi -Class Win32_ComputerSystem | select username
    $objuser = New-Object System.Security.Principal.NTAccount($username.username)
    $sid = $objuser.Translate([System.Security.Principal.SecurityIdentifier])
    $SidValue = $sid.Value
    $Message = "User SID is $SidValue."
    Write-Host "$Message"
    }
    catch [Exception]
    {
    $Message = "Unable to get user SID. User may be logged on over Remote Desktop: $_"
    Write-Host "$Message"
    exit
    }
    
    $session = New-CimSession
    $options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)
    
        try
    {
        $deleteInstances = $session.EnumerateInstances($namespaceName, $className, $options)
        foreach ($deleteInstance in $deleteInstances)
        {
            $InstanceId = $deleteInstance.InstanceID
            if ("$InstanceId" -eq "$ProfileNameEscaped")
            {
                $session.DeleteInstance($namespaceName, $deleteInstance, $options)
                $Message = "Removed $ProfileName profile $InstanceId"
                Write-Host "$Message"
            } else {
                $Message = "Ignoring existing VPN profile $InstanceId"
                Write-Host "$Message"
            }
        }
    }
    catch [Exception]
    {
        $Message = "Unable to remove existing outdated instance(s) of $ProfileName profile: $_"
        Write-Host "$Message"
        exit
    }
    
    try
    {
        $newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", "String", "Key")
        $newInstance.CimInstanceProperties.Add($property)
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", "String", "Key")
        $newInstance.CimInstanceProperties.Add($property)
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", "String", "Property")
        $newInstance.CimInstanceProperties.Add($property)
        $session.CreateInstance($namespaceName, $newInstance, $options)
        $Message = "Created $ProfileName profile."

        Write-Host "$Message"
    }
    catch [Exception]
    {
        $Message = "Unable to create $ProfileName profile: $_"
        Write-Host "$Message"
        exit
    }
    
    $Message = "Script Complete"
    Write-Host "$Message"'
    
    $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"
   ```

## <a name="configure-the-vpn-client-by-using-windows-powershell"></a>使用 Windows PowerShell 設定 VPN 用戶端

若要設定 Windows 10 用戶端電腦上的 VPNv2 CSP，請執行 VPN_Profile.ps1 Windows PowerShell 指令碼中建立[建立設定檔 XML](#create-the-profile-xml)一節。 以系統管理員身分，開啟 Windows PowerShell否則，您會收到錯誤，指出此項目，_拒絕存取_。

執行 VPN_Profile.ps1 設定 VPN 設定檔之後, 您可以驗證在任何時間它已成功在 Windows PowerShell ISE 中執行下列命令：

```powershell
Get-WmiObject -Namespace root\cimv2\mdm\dmmap -Class MDM_VPNv2_01
```

**成功的結果從 Get-wmiobject cmdlet**

```powershell
__GENUS : 2
__CLASS : MDM_VPNv2_01
__SUPERCLASS:
__DYNASTY   : MDM_VPNv2_01
__RELPATH   : MDM_VPNv2_01.InstanceID="Contoso%20AlwaysOn%20VPN",ParentID
  ="./Vendor/MSFT/VPNv2"
__PROPERTY_COUNT: 10
__DERIVATION: {}
__SERVER: WIN01
__NAMESPACE : root\cimv2\mdm\dmmap
__PATH  : \\WIN01\root\cimv2\mdm\dmmap:MDM_VPNv2_01.InstanceID="Conto
  so%20AlwaysOn%20VPN",ParentID="./Vendor/MSFT/VPNv2"
AlwaysOn: True
ByPassForLocal  :
DnsSuffix   : corp.contoso.com
EdpModeId   :
InstanceID  : Contoso%20AlwaysOn%20VPN
LockDown:
ParentID: ./Vendor/MSFT/VPNv2
ProfileXML  : <VPNProfile><RememberCredentials>true</RememberCredentials>
  <AlwaysOn>true</AlwaysOn><DnsSuffix>corp.contoso.com</DnsSu
  ffix><TrustedNetworkDetection>corp.contoso.com</TrustedNetw
  orkDetection><NativeProfile><Servers>vpn.contoso.com;vpn.co
  ntoso.com</Servers><RoutingPolicyType>SplitTunnel</RoutingP
  olicyType><NativeProtocolType>Ikev2</NativeProtocolType><Au
  thentication><UserMethod>Eap</UserMethod><MachineMethod>Eap
  </MachineMethod><Eap><Configuration><EapHostConfig xmlns="h
  ttp://www.microsoft.com/provisioning/EapHostConfig"><EapMet
  hod><Type xmlns="https://www.microsoft.com/provisioning/EapC
  ommon">25</Type><VendorId xmlns="https://www.microsoft.com/p
  rovisioning/EapCommon">0</VendorId><VendorType xmlns="http:
  //www.microsoft.com/provisioning/EapCommon">0</VendorType><
  AuthorId xmlns="https://www.microsoft.com/provisioning/EapCo
  mmon">0</AuthorId></EapMethod><Config xmlns="https://www.mic
  rosoft.com/provisioning/EapHostConfig"><Eap xmlns="https://w
  ww.microsoft.com/provisioning/BaseEapConnectionPropertiesV1
  "><Type>25</Type><EapType xmlns="https://www.microsoft.com/p
  rovisioning/MsPeapConnectionPropertiesV1"><ServerValidation
  ><DisableUserPromptForServerValidation>true</DisableUserPro
  mptForServerValidation><ServerNames>NPS</ServerNames><Trust
  edRootCA>3f 07 88 e8 ac 00 32 e4 06 3f 30 f8 db 74 25 e1
  2e 5b 84 d1 </TrustedRootCA></ServerValidation><FastReconne
  ct>true</FastReconnect><InnerEapOptional>false</InnerEapOpt
  ional><Eap xmlns="https://www.microsoft.com/provisioning/Bas
  eEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="
  https://www.microsoft.com/provisioning/EapTlsConnectionPrope
  rtiesV1"><CredentialsSource><CertificateStore><SimpleCertSe
  lection>true</SimpleCertSelection></CertificateStore></Cred
  entialsSource><ServerValidation><DisableUserPromptForServer
  Validation>true</DisableUserPromptForServerValidation><Serv
  erNames>NPS</ServerNames><TrustedRootCA>3f 07 88 e8 ac 00
  32 e4 06 3f 30 f8 db 74 25 e1 2e 5b 84 d1 </TrustedRootCA><
  /ServerValidation><DifferentUsername>false</DifferentUserna
  me><PerformServerValidation xmlns="https://www.microsoft.com
  /provisioning/EapTlsConnectionPropertiesV2">true</PerformSe
  rverValidation><AcceptServerName xmlns="https://www.microsof
  t.com/provisioning/EapTlsConnectionPropertiesV2">true</Acce
  ptServerName></EapType></Eap><EnableQuarantineChecks>false<
  /EnableQuarantineChecks><RequireCryptoBinding>false</Requir
  eCryptoBinding><PeapExtensions><PerformServerValidation xml
  ns="https://www.microsoft.com/provisioning/MsPeapConnectionP
  ropertiesV2">true</PerformServerValidation><AcceptServerNam
  e xmlns="https://www.microsoft.com/provisioning/MsPeapConnec
  tionPropertiesV2">true</AcceptServerName></PeapExtensions><
  /EapType></Eap></Config></EapHostConfig></Configuration></E
  ap></Authentication></NativeProfile><DomainNameInformation>
  <DomainName>corp.contoso.com</DomainName><DnsServers>10.10.
      0.2,10.10.0.3</DnsServers><AutoTrigger>true</AutoTrigger></
  DomainNameInformation></VPNProfile>
RememberCredentials : True
TrustedNetworkDetection : corp.contoso.com
PSComputerName  : WIN01
```

ProfileXML 設定必須是正確的結構、 拼字檢查、 組態和有時字母大小寫。 如果您看到列表 1 不同結構中的項目，ProfileXML 標記可能會包含錯誤。

如果您需要進行疑難排解的標記，就將它放在 XML 編輯器比來疑難排解在 Windows PowerShell ISE 中的工作變得更容易。 在任一情況下，設定檔，最簡單版本開始，將元件新增回直到問題一次，就會發生一次。

## <a name="configure-the-vpn-client-by-using-system-center-configuration-manager"></a>使用 System Center Configuration Manager 中設定 VPN 用戶端

System Center Configuration Manager 中，可以使用 [ProfileXML CSP] 節點中，就像在 Windows PowerShell 來部署 VPN 設定檔。 這裡，您可以使用您在一節中建立的 VPN_Profile.ps1 Windows PowerShell 指令碼[建立 ProfileXML 設定檔](#create-the-profilexml-configuration-files)。

若要使用 System Center Configuration Manager 的遠端存取一律開啟 」 VPN 設定檔部署至 Windows 10 用戶端電腦，您必須啟動建立群組的電腦或使用者對您部署設定檔。 在此案例中，建立使用者群組來部署組態指令碼。

### <a name="create-a-user-group"></a>建立使用者群組

1.  在 Configuration Manager 主控台中，開啟 資產與相容性\\使用者集合。

2.  在 **首頁**功能區**建立**群組中，按一下**建立使用者集合**。

3.  在 [一般] 頁面上完成下列步驟：

    a. 在 **名稱**，型別**VPN 使用者**。

    b. 按一下 **瀏覽**，按一下**所有使用者**，按一下 **確定**。

    c. 按一下 [下一步]  。

4.  在成員資格規則 頁面上，完成下列步驟：

    a.  在 **成員資格規則**，按一下**新增規則**，然後按一下**直接規則**。 在此範例中，您要將個別的使用者新增至使用者集合。 不過，您可以使用查詢規則，將使用者加入此集合中以動態方式的大規模部署。

    b.  在 [歡迎]  頁面上，按 [下一步]  。

    c.  在 [搜尋資源] 頁面中，在**值**，輸入您想要新增之使用者的名稱。 資源名稱包含使用者的網域。 若要包含根據部分的比對的結果，請插入 **%** 字元任一端的搜尋準則。 例如，若要尋找包含字串"lori"的所有使用者，請輸入 **%lori%** 。 按一下 [下一步]  。

    d.  在選取資源 頁面上，選取您想要新增至群組，然後按一下 的使用者**下一步**。

    e.  在 [摘要] 頁面中，按一下 [**下一步]** 。

    f.  在 完成 頁面上，按一下 **關閉**。

6.  返回 [建立使用者集合精靈] 的 [成員資格規則] 頁面中，按一下 [**下一步]** 。

7.  在 [摘要] 頁面中，按一下 [**下一步]** 。

8.  在 完成 頁面上，按一下 **關閉**。

您建立使用者群組，以接收 VPN 設定檔之後，您可以建立套件和程式以部署一節建立的 Windows PowerShell 組態指令碼[建立 ProfileXML 設定檔](#create-the-profilexml-configuration-files)。

### <a name="create-a-package-containing-the-profilexml-configuration-script"></a>建立內含 ProfileXML 設定指令碼的封裝

1.  裝載站台伺服器電腦帳戶可以存取網路共用上的指令碼 VPN_Profile.ps1。

2.  在 Configuration Manager 主控台中，開啟**軟體程式庫\\應用程式管理\\封裝**。

3.  在上**首頁**功能區**建立**群組中，按一下**建立封裝**以啟動 [建立套件和程式精靈]。

4.  在 [套件] 頁面上完成下列步驟：

    a. 在 **名稱**，型別**Windows 10 Always On VPN 設定檔**。

    b. 選取 **此套件包含來源檔案**核取方塊，然後按一下**瀏覽**。

    c. 在 設定來源資料夾 對話方塊中，按一下**瀏覽**，選取 檔案共用，其中包含 VPN_Profile.ps1，然後按一下**確定**。
        請確定您選取的網路路徑，而不是本機路徑。 換句話說，路徑必須是類似 *\\fileserver\\vpnscript*，而非*c:\\vpnscript*。

1.  按一下 [下一步]  。

2.  在 [程式類型] 頁面中，按一下 [**下一步]** 。

3.  在標準程式 頁面上，完成下列步驟：

    a.  在 **名稱**，型別**VPN 設定檔指令碼**。

    b.  在 **命令列**，型別**PowerShell.exe-ExecutionPolicy 略過-檔案 「 VPN_Profile.ps1"** 。

    c.  在 **執行模式**，按一下**具有系統管理權限執行**。

    d.  按一下 [下一步]  。

4.  在 [需求] 頁面中，完成下列步驟：

    a.  選取 **只能在指定的平台上執行此程式可以**。

    b.  選取 **所有 Windows 10 （32 位元）** 並**所有 Windows 10 （64 位元）** 核取方塊。

    c.  在 **預估的磁碟空間**，型別**1**。

    d.  在 **允許的執行的時間 （分鐘） 上限**，型別**15**。

    e.  按一下 [下一步]  。

5.  在 [摘要] 頁面中，按一下 [**下一步]** 。

6.  在 完成 頁面上，按一下 **關閉**。

封裝及建立程式，您需要將其部署至**VPN 使用者**群組。

### <a name="deploy-the-profilexml-configuration-script"></a>部署 ProfileXML 設定指令碼

1.  在 Configuration Manager 主控台中，開啟 軟體程式庫\\應用程式管理\\封裝。

2.  在 **封裝**，按一下**Windows 10 Always On VPN 設定檔**。

3.  在 **程式**索引標籤上，底部的 詳細資料 窗格中，以滑鼠右鍵按一下**VPN 設定檔指令碼**，按一下 **屬性**，並完成下列步驟：

    a.  上**進階**索引標籤中，於**當此程式會指派給電腦**，按一下 **針對每個登入的使用者執行一次**。

    b.  按一下 [確定]  。

4.  以滑鼠右鍵按一下**VPN 設定檔指令碼**然後按一下**部署**以啟動 [部署軟體精靈]。

5.  在 [一般] 頁面上完成下列步驟：

    a.  旁邊**集合**，按一下**瀏覽**。

    b.  在 **集合型別**清單中 （左上方），按一下**使用者集合**。

    c.  按一下  **VPN 使用者**，然後按一下**確定**。

    d.  按一下 [下一步]  。

6.  在 [內容] 頁面中，完成下列步驟：

    a.  按一下 **新增**，然後按一下**發佈點**。

    b.  在 **可用的發佈點**，選取您想要散發 ProfileXML 設定指令碼，並按一下 發佈點**確定**。

    c.  按一下 [下一步]  。

7.  在 [部署設定] 頁面中，按一下 [**下一步]** 。

8.  在 [排程] 頁面中，完成下列步驟：

    a.  按一下 **新增**以開啟 作業排程 對話方塊。

    b.  按一下 **此事件後立即指派**，然後按一下**確定**。

    c.  按一下 [下一步]  。

9.  在使用者經驗 頁面上，完成下列步驟：

    1.  選取 **軟體安裝**核取方塊。

    2.  按一下 **摘要**。

10. 在 [摘要] 頁面中，按一下 [**下一步]** 。

11. 在 完成 頁面上，按一下 **關閉**。

部署 ProfileXML 設定指令碼，使用登入您所建立的使用者集合時所選取的使用者帳戶的 Windows 10 用戶端電腦。 確認 VPN 用戶端的組態。

>[!NOTE]
>指令碼 VPN_Profile.ps1 不適用於遠端桌面工作階段。 同樣地，它不適用於 HYPER-V 增強的工作階段。 如果您要在虛擬機器中測試遠端存取一律開啟 」 VPN，停用增強的工作階段在您的用戶端 Vm 再繼續進行。

### <a name="verify-the-configuration-of-the-vpn-client"></a>確認 VPN 用戶端的組態

1.  在 控制台底下**系統\\安全性**，按一下  **Configuration Manager**。 

2.  在 Configuration Manager 內容 對話方塊中，在**動作**索引標籤上，完成下列步驟：

    a.  按一下 **電腦原則抓取和評估週期**，按一下**立即執行**，然後按一下**確定**。

    b.  按一下 **使用者原則抓取和評估週期**，按一下**立即執行**，然後按一下**確定**。

    c.  按一下 [確定]  。

3.  關閉 控制台。

您應該很快就會看到新的 VPN 設定檔。

## <a name="configure-the-vpn-client-by-using-intune"></a>使用 Intune 設定 VPN 用戶端

若要使用 Intune 來部署 Windows 10 的遠端存取一律在 VPN 設定檔，您可以使用您建立一節中的 VPN 設定檔設定 ProfileXML CSP 節點[建立 ProfileXML 設定檔](#create-the-profilexml-configuration-files)，或者您可以使用基底的 EAP下面提供的 XML 範例。

>[!NOTE]
>Intune 現在會使用 Azure AD 群組。 如果 Azure AD Connect 同步處理至 Azure AD 中，從內部部署 VPN 使用者群組和使用者指派給 VPN 使用者群組，您已準備好繼續進行。

建立 VPN 裝置組態原則設定新增至群組的所有使用者的 Windows 10 用戶端電腦。 由於 Intune 範本提供 VPN 參數，只複製\<EapHostConfig > \</EapHostConfig > VPN_ProfileXML 檔案的一部分。

### <a name="create-the-always-on-vpn-configuration-policy"></a>建立一律開啟 」 VPN 設定原則

1.  登入 [Azure 入口網站](https://portal.azure.com/)。

2.  移至**Intune** > **裝置組態** > **設定檔**。

3.  按一下 **建立設定檔**以啟動 建立設定檔精靈。

4.  請輸入**名稱**VPN 設定檔及 （選擇性） 描述。

1.   底下**平台**，選取**Windows 10 或更新版本**，然後選擇**VPN**從設定檔類型 下拉式清單。

     >[!TIP]
     >如果您要建立自訂的 VPN profileXML，請參閱[使用 Intune 套用 ProfileXML](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune)如需相關指示。

2. 底下**基底 VPN**索引標籤上，確認或設定下列設定：

    - **連線名稱：** 輸入 VPN 連線的名稱，在用戶端電腦上所顯示的樣子**VPN**索引標籤下**設定**，例如_Contoso AutoVPN_。  
    
    - **伺服器：** 加入一或多個 VPN 伺服器，請按一下**新增。**
    
    - **描述**和**IP 位址或 FQDN:** 輸入描述及 IP 位址或 VPN 伺服器的 FQDN。 中的 VPN 伺服器驗證憑證的主體名稱必須符合這些值。 
    
    - **預設伺服器：** 如果這是預設 VPN 伺服器，設定為 **，則為 True**。 這麼做可讓此伺服器作為裝置要連線的預設伺服器。 
    
    - **連線類型：** 設定為**IKEv2**。  
    
    - **Always On:** 設定為**啟用**會自動在登入 vpn 連線，並保持連線，直到使用者以手動方式中斷連接。
    
    - **在每次登入時記住認證**:布林值 （true 或 false） 快取認證。 如果可能的話，會快取設為 true，認證。

3.  將下列 XML 字串複製到文字編輯器中：
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

4.  取代 **\<TrustedRootCA > 5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 是 4b <\/TrustedRootCA >** 在範例中，您在內部部署環境的根憑證授權單位的憑證指紋在兩個位置中。
  
    >[!Important]
    >請勿使用中的範例指紋\<TrustedRootCA >\</TrustedRootCA > 一節。  TrustedRootCA 必須發出 RRAS 和 NPS 伺服器的伺服器驗證憑證的內部部署根憑證授權單位的憑證指紋。 **這必須不是雲端的根憑證，也不中繼的發行 CA 的憑證指模**。

5.  取代 **\<ServerNames > NPS.contoso.com\</ServerNames >** 在範例 XML 的已加入網域的 NPS 驗證發生的其中的 fqdn。 

6.  複製修改過的 XML 字串並貼入**EAP Xml**下方 基底 VPN 索引標籤，然後按一下 **確定**。
    在 Intune 中，會建立使用 EAP 一律在 VPN 裝置組態原則。


### <a name="sync-the-always-on-vpn-configuration-policy-with-intune"></a>同步處理一律開啟 」 VPN 組態原則與 Intune

若要測試的設定原則，Windows 10 用戶端電腦使用者身分登入新增至您**一律在 VPN 使用者**群組，然後與 Intune 同步處理。

1.  在 開始 功能表中，按一下 **設定**。

2.  在設定中，按一下**帳戶**，然後按一下**存取工作或學校**。

3.  按一下 MDM 設定檔，然後按一下**資訊**。

4.  按一下 **同步**強制 Intune 原則的評估和擷取。

5.  關閉設定。 同步處理之後，您會看到可用的 VPN 設定檔在電腦上。

## <a name="next-steps"></a>後續步驟

您已完成部署一律開啟 」 VPN。  如需您可以設定其他功能，請參閱下表：

|如果您想要...  |然後請參閱...  |
|---------|---------|
|設定 VPN 的條件式存取    |[步驟 7.（選擇性）設定 VPN 連線使用 Azure AD 條件式存取](../../ad-ca-vpn-connectivity-windows10.md):在此步驟中，您可以微調授權的 VPN 使用者如何存取您使用的資源[Azure Active Directory (Azure AD) 條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。 使用 Azure AD 條件式存取虛擬私人網路 (VPN) 連線，您可以協助保護 VPN 連線。 「條件式存取」是原則型評估引擎，可讓您為任何 Azure Active Directory (Azure AD) 已連線的應用程式建立存取規則。         |
|深入了解的進階 VPN 功能  |[進階 VPN 功能](always-on-vpn-adv-options.md#advanced-vpn-features):此頁面會提供指引，了解如何啟用 VPN 流量篩選器、 如何設定自動 VPN 連線使用應用程式-觸發程序，以及如何設定 NPS 只允許來自用戶端使用 Azure AD 所簽發的憑證的 VPN 連線。        |
