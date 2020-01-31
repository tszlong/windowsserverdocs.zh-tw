---
title: 設定 Windows 10 用戶端 Always On VPN 連線
description: 在此步驟中，您將瞭解 ProfileXML 選項和架構，並將 Windows 10 用戶端電腦設定為使用 VPN 連線與該基礎結構進行通訊。
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 05/29/2018
ms.assetid: d165822d-b65c-40a2-b440-af495ad22f42
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.reviewer: deverette
ms.openlocfilehash: c35cc44e0a52596fd9fa2ba5b2c01727c11b834c
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822404"
---
# <a name="step-6-configure-windows-10-client-always-on-vpn-connections"></a>步驟 6. 設定 Windows 10 用戶端 Always On VPN 連線

>適用于： Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows 10

- [**上一步：** 步驟5。設定 DNS 和防火牆設定](vpn-deploy-dns-firewall.md)<br>
- [**下一步：** 步驟7：選擇性使用 Azure AD 進行 VPN 連線的條件式存取](../../ad-ca-vpn-connectivity-windows10.md)

在此步驟中，您將瞭解 ProfileXML 選項和架構，並將 Windows 10 用戶端電腦設定為使用 VPN 連線與該基礎結構進行通訊。

您可以透過 PowerShell、Microsoft Endpoint Configuration Manager 或 Intune 來設定 Always On VPN 用戶端。 這三個都需要 XML VPN 設定檔來設定適當的 VPN 設定。 不需要 Configuration Manager 或 Intune 即可自動化組織的 PowerShell 註冊。

>[!NOTE]
>群組原則不包含系統管理範本來設定 Windows 10 遠端存取 Always On VPN 用戶端。  不過，您可以使用登入腳本。

## <a name="profilexml-overview"></a>ProfileXML 總覽

ProfileXML 是 VPNv2 CSP 內的 URI 節點。 並不是個別設定每個 VPNv2 CSP 節點（例如觸發程式、路由清單和驗證通訊協定），而是使用此節點來設定 Windows 10 VPN 用戶端，方法是將所有設定當做單一的 XML 區塊傳遞至單一 CSP 節點。 ProfileXML 架構比對 VPNv2 CSP 節點的架構幾乎完全相同，但有些詞彙則稍有不同。

您會在此部署描述的所有傳遞方法中使用 ProfileXML，包括 Windows PowerShell、Microsoft Endpoint Configuration Manager 和 Intune。 有兩種方式可以在此部署中設定 ProfileXML VPNv2 CSP 節點：

- **OMA DM**。 其中一種方式是使用 OMA-URI 提供者，如稍早在[VPNV2 CSP 節點](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes)一節中所述。 使用此方法時，您可以輕鬆地在使用 Intune 時，將 VPN 設定檔設定 XML 標記插入 ProfileXML CSP 節點。

- **Windows Management Instrumentation （WMI）到 CSP 橋接器**。 設定 ProfileXML CSP 節點的第二個方法是使用 WMI 對 CSP 橋接器（稱為**MDM_VPNv2_01**的 wmi 類別），其可存取 VPNv2 CSP 和 ProfileXML 節點。 當您建立該 WMI 類別的新實例時，WMI 會在使用 Windows PowerShell 和 Configuration Manager 時，使用 CSP 來建立 VPN 設定檔。

雖然這些設定方法不同，但兩者都需要格式正確的 XML VPN 設定檔。 若要使用 ProfileXML VPNv2 CSP 設定，您可以使用 ProfileXML 架構來設定簡單部署案例所需的標記，以建立 XML。 如需詳細資訊，請參閱[PROFILEXML XSD](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd)。

在下方，您會找到每個必要的設定及其對應的 ProfileXML 標記。 您可以在 ProfileXML 架構內的特定標籤中設定每個設定，而不是在原生設定檔下找到所有設定。 如需其他標記位置，請參閱 ProfileXML 架構。

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**連線類型：** 原生 IKEv2

ProfileXML 元素：

```xml
<NativeProtocolType>IKEv2</NativeProtocolType>
```

**路由：** 分割通道

ProfileXML 元素：

```xml
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>
```

**名稱解析：** 功能變數名稱資訊清單和 DNS 尾碼

ProfileXML 元素：

```xml
<DomainNameInformation>
<DomainName>.corp.contoso.com</DomainName>
<DnsServers>10.10.1.10,10.10.1.50</DnsServers>
</DomainNameInformation>

<DnsSuffix>corp.contoso.com</DnsSuffix>
```

**觸發：** Always On 和信任的網路偵測

ProfileXML 元素：

```xml
<AlwaysOn>true</AlwaysOn>
<TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

**驗證：** PEAP-TLS 與 TPM 保護的使用者憑證

ProfileXML 元素：

```xml
<Authentication>
<UserMethod>Eap</UserMethod>
<Eap>
<Configuration>...</Configuration>
</Eap>
</Authentication>
```

您可以使用簡單標記來設定某些 VPN 驗證機制。 不過，EAP 和 PEAP 更牽涉到。 建立 XML 標記最簡單的方式，就是使用其 EAP 設定來設定 VPN 用戶端，然後將該設定匯出至 XML。

如需有關 EAP 設定的詳細資訊，請參閱[eap configuration](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/eap-configuration)。

## <a name="manually-create-a-template-connection-profile"></a>手動建立範本連線設定檔

在此步驟中，您會使用受保護的可延伸驗證通訊協定（PEAP）來保護用戶端與伺服器之間的通訊。 與簡單的使用者名稱和密碼不同的是，此連線需要 VPN 設定檔中唯一的 New-eapconfiguration 區段才能正常執行。

您不需要描述如何從頭開始建立 XML 標記，而是使用 Windows 中的設定來建立範本 VPN 設定檔。 建立範本 VPN 設定檔之後，您可以使用 Windows PowerShell 來取用該範本中的 New-eapconfiguration 部分，以建立您稍後在部署中部署的最終 ProfileXML。

### <a name="record-nps-certificate-settings"></a>記錄 NPS 憑證設定

建立範本之前，請記下伺服器憑證中 NPS 伺服器的主機名稱或完整功能變數名稱（FQDN），以及頒發證書的 CA 名稱。

**步**

1. 在 NPS 伺服器上，開啟 [網路原則伺服器]。

2. 在 NPS 主控台的 [原則] 底下，按一下 [**網路原則**]。

3. 以滑鼠右鍵按一下 **[虛擬私人網路（VPN）連接**]，然後按一下 [**屬性**]。

4. 按一下 [**條件約束**] 索引標籤，然後按一下 [**驗證方法**]。

5. 在 [EAP 類型] 中，按一下 [ **Microsoft：受保護的 EAP （PEAP）** ]，然後按一下 [**編輯**]。

6. 記錄發給和**簽發者**之**憑證**的值。

    您會在即將推出的 VPN 範本設定中使用這些值。 例如，如果伺服器的 FQDN 是 nps01.corp.contoso.com，而主機名稱是 NPS01，則憑證名稱是以伺服器的 FQDN 或 DNS 名稱為基礎，例如 nps01.corp.contoso.com。

7. 取消 [編輯受保護的 EAP 內容] 對話方塊。

8. 取消 [虛擬私人網路（VPN）連線屬性] 對話方塊。

9. 關閉 [網路原則伺服器]。

>[!NOTE]
>如果您有多部 NPS 伺服器，請在每個伺服器上完成這些步驟，如此一來，VPN 設定檔就可以驗證它們的用途。

### <a name="configure-the-template-vpn-profile-on-a-domain-joined-client-computer"></a>在已加入網域的用戶端電腦上設定範本 VPN 設定檔

現在您已有必要的資訊，請在已加入網域的用戶端電腦上設定範本 VPN 設定檔。 在此過程中，您所使用的使用者帳戶類型（也就是標準使用者或系統管理員）並不重要。

不過，如果您在設定憑證自動註冊後尚未重新開機電腦，請在設定範本 VPN 連線之前執行此動作，以確保您已在其上註冊可用的憑證。

>[!NOTE]
>沒有任何方法可以手動新增 VPN 的任何 advanced 屬性，例如 NRPT 規則、Always On、受信任的網路偵測等等。在下一個步驟中，您會建立測試 VPN 連線來驗證 VPN 伺服器的設定，而且您可以建立與伺服器的 VPN 連接。

**手動建立單一測試 VPN 連線**

1.  以**VPN 使用者**群組的成員身分登入加入網域的用戶端電腦。

2.  在 [開始] 功能表上，輸入**VPN**，然後按 enter。

3.  在詳細資料窗格中，按一下 [**新增 VPN**連線]。

4.  在 [VPN 提供者] 清單中，按一下 [ **Windows （內建）** ]。

5.  在 [連接名稱] 中，輸入**Template**。

6.  在 [伺服器名稱或位址] 中，輸入 VPN 伺服器的**外部**FQDN （例如， **vpn.contoso.com**）。

7.  按一下 **\[儲存\]** 。

8.  在 [相關設定] 底下，按一下 [**變更介面卡選項**]。

9.  以滑鼠右鍵按一下 [**範本**]，然後按一下 [**屬性**]。

10. 在 [**安全性**] 索引標籤的 [ **VPN 類型**] 中，按一下 [ **IKEv2**]。

11. 在 [資料加密] 中，按一下 [**最大強度加密**]。

12. 按一下 **[使用可延伸的驗證通訊協定（EAP）** ];然後，在 [使用可延伸的**驗證通訊協定（EAP）** ] 中，按一下 **[Microsoft：受保護的 EAP （PEAP）（啟用加密）** ]。

13. 按一下 **[** 內容] 以開啟 [受保護的 EAP 內容] 對話方塊，並完成下列步驟：

    a. 在 [連線**到這些伺服器]** 方塊中，輸入您在本節稍早從 nps 伺服器驗證設定中抓取的 nps 伺服器名稱（例如，NPS01）。

    >[!NOTE]
    >您輸入的伺服器名稱必須符合憑證中的名稱。 您先前在本節中已復原此名稱。 如果名稱不相符，連接將會失敗，並指出「因為 RAS/VPN 伺服器上設定的原則而導致連線被禁止。」

    b。  在 [信任的根憑證授權單位] 底下，選取發出 NPS 伺服器憑證的根 CA （例如 contoso-CA）。

    c.  在 [連接前的通知] 中，按一下 [**不要求使用者授權新伺服器或信任的 ca**]。

    d.  在 [選取驗證方法] 中，按一下 [**智慧卡或其他憑證**]，然後按一下 [**設定**]。 [智慧卡或其他憑證內容] 對話方塊隨即開啟。

    e.  按一下 [**在這部電腦上使用憑證**]。

    f.  在 [連線到這些伺服器] 方塊中，輸入您在先前步驟中從 NPS 伺服器驗證設定取得的 NPS 伺服器名稱。

    g.  在 [信任的根憑證授權單位] 底下，選取發出 NPS 伺服器憑證的根 CA。

    h.  選取 [**不要提示使用者授權新伺服器或信任的憑證授權單位**單位] 核取方塊。

    i.  按一下 **[確定]** 以關閉 [智慧卡或其他憑證內容] 對話方塊。

    j.  按一下 **[確定]** 以關閉 [受保護的 EAP 內容] 對話方塊。

14. 按一下 **[確定]** 以關閉 [範本內容] 對話方塊。

15. 關閉 [網路連線] 視窗。

16. 在 [設定] 中，按一下 [**範本**]，然後按一下 **[連線]** ，以測試 VPN。

>[!IMPORTANT]
>請確定 VPN 伺服器的範本 VPN 連線成功。 這麼做可確保 EAP 設定正確，然後再于下一個範例中使用它們。 您至少必須連接一次，才能繼續進行;否則，設定檔將不會包含連接到 VPN 所需的所有資訊。

## <a name="create-the-profilexml-configuration-files"></a>建立 ProfileXML 設定檔

完成本節之前，請確定您已建立並測試「[手動建立範本連線設定檔](#manually-create-a-template-connection-profile)」一節所述的範本 VPN 連線。 測試 VPN 連線是必要的，以確保設定檔包含連接到 VPN 所需的所有資訊。

[清單 1] 中的 Windows PowerShell 腳本會在桌面上建立兩個檔案，這兩個檔案都包含以您先前建立的範本連線設定檔為基礎的**new-eapconfiguration**標記：

- **VPN_Profile .xml。** 此檔案包含在 VPNv2 CSP 中設定 ProfileXML 節點所需的 XML 標記。 將此檔案用於與 OMA DM 相容的 MDM 服務，例如 Intune。

- **VPN_Profile. ps1。** 此檔案是您可以在用戶端電腦上執行的 Windows PowerShell 腳本，以設定 VPNv2 CSP 中的 ProfileXML 節點。 您也可以透過 Configuration Manager 部署此腳本來設定 CSP。 您無法在遠端桌面會話中執行此腳本，包括 Hyper-v 增強的會話。

>[!IMPORTANT]
>以下的範例命令需要 Windows 10 組建1607或更新版本。

**建立 VPN_Profile .xml 和 VPN_Proflie. ps1**

1. 以[手動建立範本連線設定檔](#manually-create-a-template-connection-profile)一節所述的相同使用者帳戶，登入加入網域的用戶端電腦（包含範本 VPN 設定檔）。

2. 將清單1貼入 Windows PowerShell 整合式腳本環境（ISE），並自訂批註中所述的參數。 這些是 $Template、$ProfileName、$Servers、$DnsSuffix、$DomainName、$TrustedNetwork 和 $DNSServers。 每個設定的完整描述都在批註中。

3. 執行腳本，以在桌面上產生**VPN_Profile .xml**和**VPN_Profile ps1** 。

#### <a name="listing-1-understanding-makeprofileps1"></a>[清單 1]。 瞭解 MakeProfile

本節說明您可以用來瞭解如何建立 VPN 設定檔的範例程式碼，特別是在 VPNv2 CSP 中設定 ProfileXML。

當您從這個範例程式碼組合腳本並執行腳本之後，腳本會產生兩個檔案： VPN_Profile .xml 和 VPN_Profile. ps1。 使用 VPN_Profile 在 OMA-URI 相容的 MDM 服務（例如 Microsoft Intune）中設定 ProfileXML。

使用 Windows PowerShell 或 Microsoft Endpoint Configuration Manager 中的**VPN_Profile ps1**腳本，在 windows 10 桌上型電腦上設定 ProfileXML。

>[!NOTE]
>若要查看完整的範例腳本，請參閱[MakeProfile 完整腳本](#makeprofileps1-full-script)一節。

#### <a name="parameters"></a>Parameters

設定下列參數：

**$Template**。 要從中取得 EAP 設定的範本名稱。

**$ProfileName**。 設定檔的唯一英數位元識別碼。 設定檔名稱不能包含正斜線（/）。 如果設定檔名稱包含空格或其他非英數位元，則必須根據 URL 編碼標準適當地將其轉義。

**$Servers**。 VPN 閘道的公用或可路由傳送 IP 位址或 DNS 名稱。 它可以指向閘道的外部 IP 或伺服器陣列的虛擬 IP。 範例：208.147.66.130 或 vpn.contoso.com。

**$DnsSuffix**。 指定一或多個逗號分隔的 DNS 尾碼。 清單中的第一個也用來做為 VPN 介面的主要連線特定 DNS 尾碼。 整個清單也會加入至 SuffixSearchList。

**$DomainName**。 用來表示套用原則的命名空間。 當發出名稱查詢時，DNS 用戶端會比較查詢中的名稱與 DomainNameInformationList 下的所有命名空間，以尋找相符的。 這個參數可以是下列其中一種類型：

- FQDN-完整功能變數名稱
- 尾碼-將附加至簡短名稱查詢以進行 DNS 解析的網域尾碼。 若要指定尾碼，請在 DNS 尾碼前面加上句號（.）。

**$DNSServers**。 要用於命名空間的逗點分隔 DNS 伺服器 IP 位址清單。

**$TrustedNetwork**。 以逗號分隔的字串，用來識別受信任的網路。 當使用者位於其公司無線網路上，而裝置可直接存取受保護的資源時，VPN 不會自動連接。

以下是下列命令中使用之參數的範例值。 請確定您已針對您的環境變更這些值。

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

下列範例命令會從範本設定檔取得 EAP 設定：

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
$ProfileXML = @("
<VPNProfile>
  <DnsSuffix>$DnsSuffix</DnsSuffix>
  <NativeProfile>
<Servers>$Servers</Servers>
<NativeProtocolType>IKEv2</NativeProtocolType>
<Authentication>
  <UserMethod>Eap</UserMethod>
  <Eap>
    <Configuration>
     $EAPSettings
    </Configuration>
  </Eap>
</Authentication>
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>
  </NativeProfile>
  <AlwaysOn>true</AlwaysOn>
  <RememberCredentials>true</RememberCredentials>
  <TrustedNetworkDetection>$TrustedNetwork</TrustedNetworkDetection>
  <DomainNameInformation>
<DomainName>$DomainName</DomainName>
<DnsServers>$DNSServers</DnsServers>
</DomainNameInformation>
</VPNProfile>
")
```

### <a name="output-vpn_profilexml-for-intune"></a>適用于 Intune 的輸出 VPN_Profile .xml

您可以使用下列範例命令來儲存設定檔 XML 檔案：

```xml
$ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
```

### <a name="output-vpn_profileps1-for-the-desktop-and-configuration-manager"></a>桌上型電腦和 Configuration Manager 的輸出 VPN_Profile

下列範例程式碼會使用 VPNv2 CSP 中的 ProfileXML 節點來設定 AlwaysOn IKEv2 VPN 連線。

您可以在 Windows 10 desktop 或 Configuration Manager 中使用此腳本。

### <a name="define-key-vpn-profile-parameters"></a>定義金鑰 VPN 設定檔參數

```xml
$Script = '$ProfileName = ''' + $ProfileName + ''''
$ProfileNameEscaped = $ProfileName -replace ' ', '%20'
```

### <a name="escape-special-characters-in-the-profile"></a>在設定檔中換用特殊字元

```xml
$ProfileXML = $ProfileXML -replace '<', '&lt;'
$ProfileXML = $ProfileXML -replace '>', '&gt;'
$ProfileXML = $ProfileXML -replace '"', '&quot;'
```

### <a name="define-wmi-to-csp-bridge-properties"></a>定義 WMI 對 CSP 橋接器屬性

```xml
$nodeCSPURI = "./Vendor/MSFT/VPNv2"
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_VPNv2_01"
```

### <a name="determine-user-sid-for-vpn-profile"></a>判斷 VPN 設定檔的使用者 SID：

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

### <a name="define-wmi-session"></a>定義 WMI 會話：

```xml
$session = New-CimSession
$options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
$options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
$options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)
```

### <a name="detect-and-delete-previous-vpn-profile"></a>偵測及刪除先前的 VPN 設定檔：

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
Write-Host "$Message"
```

### <a name="save-the-profile-xml-file"></a>儲存設定檔 XML 檔案

```xml
$Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')

$Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
Write-Host "$Message"
```

## <a name="makeprofileps1-full-script"></a>MakeProfile. ps1 完整腳本

大部分的範例會使用 Set-wmiinstance Windows PowerShell Cmdlet，將 ProfileXML 插入 MDM_VPNv2_01 WMI 類別的新實例中。 

不過，這不會在 Configuration Manager 中運作，因為您無法在使用者的內容中執行封裝。 因此，此腳本會使用通用訊息模型在使用者的內容中建立 WMI 會話，然後在該會話中建立 MDM_VPNv2_01 WMI 類別的新實例。 此 WMI 類別會使用 WMI 對 CSP 橋接器來設定 VPNv2 CSP。 因此，藉由新增類別實例，您可以設定 CSP。 

>[!IMPORTANT]
>WMI 到 CSP 橋接器需要本機系統管理許可權（依設計）。 若要部署每個使用者的 VPN 設定檔，您應該使用 Configuration Manager 或 MDM。

>[!NOTE]
>腳本 VPN_Profile. ps1 會使用目前使用者的 SID 來識別使用者的內容。 因為遠端桌面會話中沒有可用的 SID，所以腳本無法在遠端桌面會話中運作。 同樣地，它無法在 Hyper-v 增強的會話中運作。 如果您要在虛擬機器中測試 Always On VPN 的遠端存取，請先停用用戶端 Vm 上的增強會話，再執行此腳本。

下列範例腳本包含先前章節中的所有程式碼範例。 請確定您將範例值變更為適合您環境的值。
    
   ```makeProfile.ps1
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
    
    $ProfileXML = @("
    <VPNProfile>
      <DnsSuffix>$DnsSuffix</DnsSuffix>
      <NativeProfile>
    <Servers>$Servers</Servers>
    <NativeProtocolType>IKEv2</NativeProtocolType>
    <Authentication>
      <UserMethod>Eap</UserMethod>
      <Eap>
       <Configuration>
        $EAPSettings
       </Configuration>
      </Eap>
    </Authentication>
    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>
      </NativeProfile>
    <AlwaysOn>true</AlwaysOn>
    <RememberCredentials>true</RememberCredentials>
    <TrustedNetworkDetection>$TrustedNetwork</TrustedNetworkDetection>
      <DomainNameInformation>
    <DomainName>$DomainName</DomainName>
    <DnsServers>$DNSServers</DnsServers>
    </DomainNameInformation>
    </VPNProfile>
    ")
    
    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
    
     $Script = @("
      `$ProfileName = '$ProfileName'
      `$ProfileNameEscaped = `$ProfileName -replace ' ', '%20'
 
      `$ProfileXML = '$ProfileXML'
 
      `$ProfileXML = `$ProfileXML -replace '<', '&lt;'
      `$ProfileXML = `$ProfileXML -replace '>', '&gt;'
      `$ProfileXML = `$ProfileXML -replace '`"', '&quot;'
 
      `$nodeCSPURI = `"./Vendor/MSFT/VPNv2`"
      `$namespaceName = `"root\cimv2\mdm\dmmap`"
      `$className = `"MDM_VPNv2_01`"
 
      try
      {
      `$username = Gwmi -Class Win32_ComputerSystem | select username
      `$objuser = New-Object System.Security.Principal.NTAccount(`$username.username)
      `$sid = `$objuser.Translate([System.Security.Principal.SecurityIdentifier])
      `$SidValue = `$sid.Value
      `$Message = `"User SID is `$SidValue.`"
      Write-Host `"`$Message`"
      }
      catch [Exception]
      {
      `$Message = `"Unable to get user SID. User may be logged on over Remote Desktop: `$_`"
      Write-Host `"`$Message`"
      exit
      }
 
      `$session = New-CimSession
      `$options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
      `$options.SetCustomOption(`"PolicyPlatformContext_PrincipalContext_Type`", `"PolicyPlatform_UserContext`", `$false)
      `$options.SetCustomOption(`"PolicyPlatformContext_PrincipalContext_Id`", `"`$SidValue`", `$false)
 
      try
      {
    `$deleteInstances = `$session.EnumerateInstances(`$namespaceName, `$className, `$options)
    foreach (`$deleteInstance in `$deleteInstances)
    {
        `$InstanceId = `$deleteInstance.InstanceID
        if (`"`$InstanceId`" -eq `"`$ProfileNameEscaped`")
        {
            `$session.DeleteInstance(`$namespaceName, `$deleteInstance, `$options)
            `$Message = `"Removed `$ProfileName profile `$InstanceId`"
            Write-Host `"`$Message`"
        } else {
            `$Message = `"Ignoring existing VPN profile `$InstanceId`"
            Write-Host `"`$Message`"
        }
    }
      }
      catch [Exception]
      {
    `$Message = `"Unable to remove existing outdated instance(s) of `$ProfileName profile: `$_`"
    Write-Host `"`$Message`"
    exit
      }
 
      try
      {
    `$newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance `$className, `$namespaceName
    `$property = [Microsoft.Management.Infrastructure.CimProperty]::Create(`"ParentID`", `"`$nodeCSPURI`", `"String`", `"Key`")
    `$newInstance.CimInstanceProperties.Add(`$property)
    `$property = [Microsoft.Management.Infrastructure.CimProperty]::Create(`"InstanceID`", `"`$ProfileNameEscaped`", `"String`",      `"Key`")
    `$newInstance.CimInstanceProperties.Add(`$property)
    `$property = [Microsoft.Management.Infrastructure.CimProperty]::Create(`"ProfileXML`", `"`$ProfileXML`", `"String`", `"Property`")
    `$newInstance.CimInstanceProperties.Add(`$property)
    `$session.CreateInstance(`$namespaceName, `$newInstance, `$options)
    `$Message = `"Created `$ProfileName profile.`"

    Write-Host `"`$Message`"
      }
      catch [Exception]
      {
    `$Message = `"Unable to create `$ProfileName profile: `$_`"
    Write-Host `"`$Message`"
    exit
      }
 
      `$Message = `"Script Complete`"
      Write-Host `"`$Message`"
      ")
 
      $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"
   ```

## <a name="configure-the-vpn-client-by-using-windows-powershell"></a>使用 Windows PowerShell 設定 VPN 用戶端

若要在 Windows 10 用戶端電腦上設定 VPNv2 CSP，請執行您在[建立設定檔 XML](#create-the-profile-xml)一節中建立的 VPN_Profile. Ps1 Windows PowerShell 腳本。 以系統管理員身分開啟 Windows PowerShell;否則，您將會收到錯誤，指出「_拒絕存取_」。

在執行 VPN_Profile ps1 來設定 VPN 設定檔之後，您可以在 Windows PowerShell ISE 中執行下列命令，以確認是否有任何一次成功的狀態：

```powershell
Get-WmiObject -Namespace root\cimv2\mdm\dmmap -Class MDM_VPNv2_01
```

**WmiObject Cmdlet 的成功結果**

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

ProfileXML 設定在結構、拼寫、設定和有時候字母大小寫中必須正確。 如果您在 [清單 1] 的結構中看到不同的內容，ProfileXML 標記可能會包含錯誤。

如果您需要針對標記進行疑難排解，將它放在 XML 編輯器中比在 Windows PowerShell ISE 中進行疑難排解更為容易。 不論是哪一種情況，請從最簡單的設定檔版本開始，然後一次新增一個元件，直到問題再次發生為止。

## <a name="configure-the-vpn-client-by-using-configuration-manager"></a>使用 Configuration Manager 設定 VPN 用戶端

在 Configuration Manager 中，您可以使用 ProfileXML CSP 節點來部署 VPN 設定檔，就像您在 Windows PowerShell 中所做的一樣。 在這裡，您會使用您在[建立 ProfileXML 設定檔](#create-the-profilexml-configuration-files)一節中建立的 VPN_Profile. Ps1 Windows PowerShell 腳本。

若要使用 Configuration Manager 將遠端存取 Always On VPN 設定檔部署到 Windows 10 用戶端電腦，您必須從建立您要部署設定檔的一組電腦或使用者開始。 在此案例中，請建立使用者群組來部署設定腳本。

### <a name="create-a-user-group"></a>建立使用者群組

1.  在 Configuration Manager 主控台中，開啟 [資產與合規性] [\\使用者集合]。

2.  在 [**首頁**] 功能區的 [**建立**] 群組中，按一下 [**建立使用者集合**]。

3.  在 [一般] 頁面上，完成下列步驟：

    a. 在 [**名稱**] 中輸入**VPN 使用者**。

    b。 按一下 **[流覽]** ，按一下 [**所有使用者**]，然後按一下 **[確定]** 。

    c. 按一下 **\[下一步\]** 。

4.  在 [成員資格規則] 頁面上，完成下列步驟：

    a.  在 [**成員資格規則**] 中，按一下 [**新增規則**]，然後按一下 [**直接規則**]。 在此範例中，您會將個別使用者新增至使用者集合。 不過，您可以使用查詢規則，以動態方式將使用者新增至這個集合，以進行更大規模的部署。

    b。  在 **[歡迎使用]** 頁面上，按 **[下一步]** 。

    c.  在 [搜尋資源] 頁面的 [**值**] 中，輸入您想要新增的使用者名稱。 資源名稱包含使用者的網域。 若要包含以部分相符項為基礎的結果，請在搜尋條件的任一結尾插入 **%** 字元。 例如，若要尋找所有包含字串 "lori" 的使用者，請輸入 **% lori%** 。 按一下 **\[下一步\]** 。

    d.  在 [選取資源] 頁面上，選取您要新增至群組的使用者，然後按 **[下一步]** 。

    e.  在 [摘要] 頁面上，按 **[下一步]** 。

    f.  在 [完成] 頁面上，按一下 [**關閉**]。

6.  回到 [建立使用者集合] 嚮導的 [成員資格規則] 頁面上，按 **[下一步]** 。

7.  在 [摘要] 頁面上，按 **[下一步]** 。

8.  在 [完成] 頁面上，按一下 [**關閉**]。

建立要接收 VPN 設定檔的使用者群組之後，您可以建立套件和程式來部署您在[建立 ProfileXML 設定檔](#create-the-profilexml-configuration-files)一節中建立的 Windows PowerShell 設定腳本。

### <a name="create-a-package-containing-the-profilexml-configuration-script"></a>建立包含 ProfileXML 設定腳本的封裝

1.  在網站伺服器電腦帳戶可以存取的網路共用上，裝載腳本 VPN_Profile. ps1。

2.  在 Configuration Manager 主控台中，開啟 [**軟體程式庫]\\[應用程式管理] [\\套件**]。

3.  在 [常用 **] 功能區的 [** **建立**] 群組中，按一下 [**建立封裝**] 啟動 [建立套件和程式嚮導]。

4.  在 [封裝] 頁面上，完成下列步驟：

    a. 在 [**名稱**] 中，輸入**WINDOWS 10 Always On VPN 設定檔**。

    b。 選取 [**此套件包含來源**檔案] 核取方塊，然後按一下 **[流覽]** 。

    c. 在 [設定源資料夾] 對話方塊中，按一下 **[流覽]** ，選取包含 VPN_Profile 的檔案共用，然後按一下 **[確定]** 。
        請確定您選取的是網路路徑，而不是本機路徑。 換句話說，路徑應該類似\\檔案伺服器 *\\vpnscript*，而不是*c：\\vpnscript*。

1.  按一下 **\[下一步\]** 。

2.  在 [程式類型] 頁面上，按 **[下一步]** 。

3.  在 [標準程式] 頁面上，完成下列步驟：

    a.  在 [**名稱**] 中，輸入**VPN 設定檔腳本**。

    b。  在 [**命令列**] 中，輸入**ExecutionPolicy 略過檔案 "VPN_Profile. ps1"** 。

    c.  在 [**執行模式]** 中，按一下 [以系統**管理許可權執行**]。

    d.  按一下 **\[下一步\]** 。

4.  在 [需求] 頁面上，完成下列步驟：

    a.  選取 [**此程式僅能在指定的平臺上執行**]。

    b。  選取 [**所有 windows 10 （32位）** ] 和 [**所有 windows 10 （64位）** ] 核取方塊。

    c.  在 [**估計的磁碟空間**] 中，輸入**1**。

    d.  在 **[允許的執行時間上限（分鐘）** ] 中，輸入**15**。

    e.  按一下 **\[下一步\]** 。

5.  在 [摘要] 頁面上，按 **[下一步]** 。

6.  在 [完成] 頁面上，按一下 [**關閉**]。

建立套件和程式之後，您必須將它部署到**VPN 使用者**群組。

### <a name="deploy-the-profilexml-configuration-script"></a>部署 ProfileXML 設定腳本

1.  在 Configuration Manager 主控台中，開啟 [軟體程式庫]\\[應用程式管理] [\\套件]。

2.  在 [**套件**] 中，按一下 [ **WINDOWS 10 Always On VPN 設定檔**]。

3.  在詳細資料窗格底部的 [**程式**] 索引標籤上，以滑鼠右鍵按一下 [ **VPN 設定檔腳本**] **，按一下 [** 內容]，然後完成下列步驟：

    a.  在 [ **Advanced** ] 索引標籤上的 [**將此程式指派給電腦時**]，**針對登入的每個使用者按一下 [一次**]。

    b。  按一下 **\[確定\]** 。

4.  以滑鼠右鍵按一下 [ **VPN 設定檔腳本**]，然後按一下 [**部署**] 啟動 [部署軟體嚮導]。

5.  在 [一般] 頁面上，完成下列步驟：

    a.  在 [**集合**] 旁，按一下 **[流覽]** 。

    b。  在 [**集合類型**] 清單（左上方）中，按一下 [**使用者集合**]。

    c.  按一下 [ **VPN 使用者**]，然後按一下 **[確定]** 。

    d.  按一下 **\[下一步\]** 。

6.  在 [內容] 頁面上，完成下列步驟：

    a.  按一下 [**新增**]，然後按一下 [**發佈點**]。

    b。  在 [**可用的發佈點**] 中，選取您要發佈 ProfileXML 設定腳本的發佈點，然後按一下 **[確定]** 。

    c.  按一下 **\[下一步\]** 。

7.  在 [部署設定] 頁面上，按 **[下一步]** 。

8.  在 [排程] 頁面上，完成下列步驟：

    a.  按一下 [**新增**] 以開啟 [指派排程] 對話方塊。

    b。  按一下**此事件後立即指派**，然後按一下 **[確定]** 。

    c.  按一下 **\[下一步\]** 。

9.  在 [使用者經驗] 頁面上，完成下列步驟：

    1.  選取 [**軟體安裝**] 核取方塊。

    2.  按一下 [**摘要**]。

10. 在 [摘要] 頁面上，按 **[下一步]** 。

11. 在 [完成] 頁面上，按一下 [**關閉**]。

部署 ProfileXML 設定腳本後，使用您在建立使用者集合時選取的使用者帳戶登入 Windows 10 用戶端電腦。 確認 VPN 用戶端的設定。

>[!NOTE]
>腳本 VPN_Profile. ps1 無法在遠端桌面會話中運作。 同樣地，它無法在 Hyper-v 增強的會話中運作。 如果您要在虛擬機器中測試 Always On VPN 的遠端存取，請先停用用戶端 Vm 上的增強會話，再繼續進行。

### <a name="verify-the-configuration-of-the-vpn-client"></a>確認 VPN 用戶端的設定

1.  在 [控制台] 的 [**系統\\安全性**] 底下，按一下 [ **Configuration Manager**]。 

2.  在 [Configuration Manager 屬性] 對話方塊的 [**動作**] 索引標籤上，完成下列步驟：

    a.  按一下 [**電腦原則抓取 & 評估週期**]，按一下 [**立即執行**]，然後按一下 **[確定]** 。

    b。  按一下 [**使用者原則抓取 & 評估週期**]，按一下 [**立即執行**]，然後按一下 **[確定]** 。

    c.  按一下 **\[確定\]** 。

3.  關閉 [控制台]。

您應該很快就會看到新的 VPN 設定檔。

## <a name="configure-the-vpn-client-by-using-intune"></a>使用 Intune 設定 VPN 用戶端

若要使用 Intune 來部署 Windows 10 遠端存取 Always On VPN 設定檔，您可以使用您在[建立 ProfileXML 設定檔](#create-the-profilexml-configuration-files)一節中建立的 VPN 設定檔來設定 ProfileXML CSP 節點，或使用下面提供的基本 EAP XML 範例。

>[!NOTE]
>Intune 現在會使用 Azure AD 群組。 如果 Azure AD Connect 將 VPN 使用者群組從內部部署同步至 Azure AD，而且使用者已指派給 VPN 使用者群組，則您已準備好繼續進行。

建立 VPN 裝置設定原則，為新增至群組的所有使用者設定 Windows 10 用戶端電腦。 因為 Intune 範本提供 VPN 參數，所以只會將 \<EapHostConfig > \</EapHostConfig > 部分的 VPN_ProfileXML 檔案。

### <a name="create-the-always-on-vpn-configuration-policy"></a>建立 Always On VPN 設定原則

1.  登入 [Azure 入口網站](https://portal.azure.com/)。

2.  移至**Intune** > [**裝置**設定] [ > **設定檔**]。

3.  按一下 [**建立設定檔**]，啟動 [建立設定檔嚮導]。

4.  輸入 VPN 設定檔的**名稱**，以及（選擇性） [描述]。

1.   在 [**平臺**] 底下選取 [ **Windows 10 或更新版本**]，然後從 [配置檔案類型] 下拉式選單選擇 [ **VPN** ]。

     >[!TIP]
     >如果您要建立自訂 VPN profileXML，請參閱[使用 Intune 套用 profileXML](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune)以取得指示。

2. 在 [**基底 VPN** ] 索引標籤底下，確認或設定下列設定：

    - **連接名稱：** 在 [**設定**] 底下的 [ **vpn** ] 索引標籤中，輸入出現在用戶端電腦上的 VPN 連線名稱，例如_Contoso AutoVPN_。  
    
    - **伺服器：** 按一下 [新增] 以新增一或多個 VPN 伺服器 **。**
    
    - **描述**和**IP 位址或 FQDN：** 輸入 VPN 伺服器的描述和 IP 位址或 fqdn。 這些值必須與 VPN 伺服器驗證憑證中的主體名稱一致。 
    
    - **預設伺服器：** 如果這是預設的 VPN 伺服器，請將設定為**True**。 這麼做可讓此伺服器做為裝置用來建立連線的預設伺服器。 
    
    - **連線類型：** 設定為**IKEv2**。  
    
    - **Always On：** 設定為 [**啟用**] 會在登入時自動連線至 VPN，並保持連接，直到使用者手動中斷連接為止。
    
    - **在每次登入時記住認證**：快取認證的布林值（true 或 false）。 如果設定為 true，則會盡可能快取認證。

3.  將下列 XML 字串複製到文字編輯器：
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

4.  取代 **\<TrustedRootCA > 5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7-c2 68 是**在範例中使用內部部署根憑證授權單位之憑證指紋的 4b <\/TrustedRootCA >。
  
    >[!Important]
    >請勿使用下列 \<TrustedRootCA >\</TrustedRootCA > 一節中的範例指紋。  TrustedRootCA 必須是為 RRAS 和 NPS 伺服器發出伺服器驗證憑證的內部部署根憑證授權單位的憑證指紋。 **這不能是雲端根憑證，也不得為中繼發行 CA 憑證指紋**。

5.  以進行驗證之已加入網域的 NPS 的 FQDN 取代範例 XML 中的 **\<ServerNames > nps. .com\</ServerNames >** 。 

6.  複製修改過的 XML 字串，並貼入 [基底 VPN] 索引標籤底下的 [ **EAP XML** ] 方塊，然後按一下 **[確定**]。
    使用 EAP 的 Always On VPN 裝置設定原則會在 Intune 中建立。


### <a name="sync-the-always-on-vpn-configuration-policy-with-intune"></a>使用 Intune 同步處理 Always On VPN 設定原則

若要測試設定原則，請以您新增至**ALWAYS ON VPN 使用者**群組的使用者身分登入 Windows 10 用戶端電腦，然後與 Intune 同步處理。

1.  在 [開始] 功能表上，按一下 [**設定**]。

2.  在 [設定] 中，按一下 [**帳戶**]，然後按一下 [**存取公司或學校**]。

3.  按一下 [MDM 設定檔]，然後按一下 [**資訊**]。

4.  按一下 [**同步**] 強制執行 Intune 原則評估和抓取。

5.  關閉 [設定]。 同步處理之後，您會在電腦上看到可用的 VPN 設定檔。

## <a name="next-steps"></a>接下來的步驟

您已完成 Always On VPN 部署。  如需您可以設定的其他功能，請參閱下表：

|如果您要...  |然後查看 。  |
|---------|---------|
|設定 VPN 的條件式存取    |[步驟7：選擇性使用 Azure AD 設定 VPN 連線的條件式存取](../../ad-ca-vpn-connectivity-windows10.md)：在此步驟中，您可以使用[Azure Active Directory （Azure AD）條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)，微調授權的 vpn 使用者如何存取您的資源。 透過 Azure AD 虛擬私人網路（VPN）連線的條件式存取，您可以協助保護 VPN 連接。 「條件式存取」是原則型評估引擎，可讓您為任何 Azure Active Directory (Azure AD) 已連線的應用程式建立存取規則。         |
|深入瞭解 advanced VPN 功能  |[ADVANCED VPN 功能](always-on-vpn-adv-options.md#advanced-vpn-features)：此頁面提供如何啟用 Vpn 流量篩選器、如何使用應用程式觸發程式來設定自動 VPN 連線，以及如何設定 NPS 只允許使用 Azure AD 所發行之憑證的 vpn 連線的指引。        |
