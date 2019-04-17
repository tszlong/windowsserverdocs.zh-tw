---
title: 設定 Windows 10 用戶端 Always On VPN 連線
description: 在此步驟，您可以深入了解 ProfileXML 選項與結構描述，並設定 Windows 10 用戶端電腦與該基礎結構的 VPN 連線的通訊。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 05/29/2018
ms.assetid: d165822d-b65c-40a2-b440-af495ad22f42
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.reviewer: deverette
ms.openlocfilehash: 6b383076686092e20448977bed3766f7d7d1c2b8
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031312"
---
# 步驟 6. 設定 Windows 10 用戶端 Always On VPN 連線

>適用於： Windows Server （半年度管道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10


& #171; [**先前：** 步驟 5。設定 DNS 和防火牆設定](vpn-deploy-dns-firewall.md)<br>
& #187;[**下一步：** 步驟 7。（選擇性）使用 Azure AD 的 VPN 連線的條件式存取](../../ad-ca-vpn-connectivity-windows10.md)

在此步驟，您可以深入了解 ProfileXML 選項與結構描述，並設定 Windows 10 用戶端電腦與該基礎結構的 VPN 連線的通訊。 

您可以設定 Always On VPN 用戶端透過 PowerShell、 SCCM 或 Intune。 所有三個需要設定適當的 VPN 設定 XML VPN 設定檔。 自動化 PowerShell 註冊，而無須 SCCM 或 Intune 的組織可能會。

>[!NOTE]
>群組原則中不包含系統管理範本 \] 來設定 windows 10 遠端存取 Always On VPN 用戶端。  不過，您可以使用登入指令碼。

## ProfileXML 概觀

ProfileXML 是 VPNv2 CSP 中的 [URI] 節點。 而不是個別設定每個 VPNv2 CSP 節點 — 例如觸發程序，路由清單和驗證通訊協定 — 使用此節點來設定 windows 10 VPN 用戶端藉由為單一 XML 區塊至單一雲端解決方案提供者節點傳遞的所有設定。 ProfileXML 結構描述符合 VPNv2 CSP 節點的結構描述幾乎完全相同，但有些許不同的詞彙。

您可以使用 ProfileXML 說明此部署，包括 Windows PowerShell、 System Center Configuration Manager 和 Intune 的所有傳遞方法中。 有兩種方式，來設定此部署中的 ProfileXML VPNv2 CSP 節點：

- **OMA DM**。 一種方式是使用 MDM 提供者使用 OMA DM，如前面所討論的區段[VPNv2 CSP 節點](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes)。 使用此方法，您可以輕鬆地將 VPN 設定檔設定 XML 標記插入 ProfileXML 雲端解決方案提供者節點時使用 Intune。

- **Windows Management Instrumentation (WMI)-至-CSP 橋接器**。 設定 ProfileXML 雲端解決方案提供者節點的第二種方法是使用 WMI 至 CSP 橋接器 — WMI 類別稱為**MDM_VPNv2_01**—，可以存取 VPNv2 CSP 和 ProfileXML 節點。 當您建立的 WMI 類別的新執行個體時，WMI 使用雲端解決方案提供者時使用 Windows PowerShell 和 System Center Configuration Manager 建立 VPN 設定檔。

雖然這些組態方法之間的差異，兩者都需要格式正確的 XML VPN 設定檔。 若要使用 ProfileXML VPNv2 CSP 設定，您可以建構 XML 設定簡單的部署案例所需的標記中使用 ProfileXML 結構描述。 如需詳細資訊，請參閱[ProfileXML XSD](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd)。

下方您會找到每個必要的設定，以及其對應的 ProfileXML 標記。 您可以設定每個設定特定 ProfileXML 結構描述，在標記中，並非所有的原生的設定檔中找到。 其他標籤位置，請參閱 ProfileXML 結構描述。

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**連線類型：** 原生 IKEv2

ProfileXML 項目：

    <NativeProtocolType>IKEv2</NativeProtocolType>

**路由：** 分割通道

ProfileXML 項目：

    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>

**名稱解析：** 網域名稱的資訊清單和 DNS 尾碼

ProfileXML 元素：

    <DomainNameInformation>
    <DomainName>.corp.contoso.com</DomainName>
    <DnsServers>10.10.1.10,10.10.1.50</DnsServers>
    </DomainNameInformation>
    
    <DnsSuffix>corp.contoso.com</DnsSuffix>


**觸發：** 一律開啟且受信任的網路偵測

ProfileXML 元素：

    <AlwaysOn>true</AlwaysOn>
    <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>


**驗證：** PEAP TLS 使用 TPM 保護的使用者憑證

ProfileXML 元素：

    <Authentication>
    <UserMethod>Eap</UserMethod>
    <Eap>
    <Configuration>...</Configuration>
    </Eap>
    </Authentication>

您可以使用簡單的標記來設定一些 VPN 驗證機制。 不過，EAP 和 PEAP 都更深入參與。 若要建立的 XML 標記的最簡單方式是使用它的 EAP 設定設定 VPN 用戶端，然後將該組態匯出為 XML。 

如需有關 EAP 設定的詳細資訊，請參閱 < <b0>EAP 設定</b0>。 

## <a name="bkmk_profile"></a>手動建立範本連線設定檔

在此步驟中，您可以使用受保護的可延伸驗證通訊協定 \(PEAP\) 安全用戶端和伺服器之間的通訊。 不同於簡單的使用者名稱和密碼，此連線需要專用的 EAPConfiguration 區段中的 VPN 設定檔，才能運作。 

而不是描述如何從頭開始建立在 XML 標記時，您使用設定 Windows 中來建立範本 VPN 設定檔。 建立範本 VPN 設定檔之後, 您可以使用 Windows PowerShell 以取用從該範本來建立您部署最終 ProfileXML 稍後部署中的 EAPConfiguration 部分。

### 錄製 NPS 憑證設定

之前建立範本時，請注意，主機名稱或 NPS 伺服器的伺服器憑證的完整的網域名稱 (FQDN) 和發行憑證的 CA 的名稱。

**程序：**

1.  在 NPS 伺服器上，開啟網路原則伺服器。

2.  在 NPS 主控台中，依據原則，按一下 [**網路原則**。

3.  以滑鼠右鍵按一下**虛擬私人網路 \(VPN\) 連線**，並按一下 \ [**內容**。

4.  按一下 [**限制**] 索引標籤，然後按一下 [**驗證方法**。

5.  在 EAP 類型中，按一下**Microsoft： 受保護的 EAP (PEAP)**，然後按一下 [**編輯**。

6.  適用於**憑證發給**和**簽發者**記錄的值。<p>您可以使用這些值中即將推出的 VPN 範本設定。 例如，如果伺服器的 FQDN 為 nps01.corp.contoso.com，主機名稱是 NPS01 的憑證名稱根據 FQDN 或 DNS 伺服器的名稱-例如，nps01.corp.contoso.com。

7.  取消編輯受保護的 EAP 內容] 對話方塊。

8.  取消虛擬私人網路 (VPN) 連線內容] 對話方塊。

9.  關閉網路原則伺服器。

>[!NOTE]
>如果您有多個 NPS 伺服器，完成這些步驟，在每一個，以便在 VPN 設定檔可以驗證每個它們應該使用它們。

### Domain\ 加入用戶端電腦上設定 VPN 設定檔的範本

現在，您已加入網域的用戶端電腦上設定 VPN 設定檔的範本所需的資訊。 您使用的使用者帳戶類型 \ （也就是標準使用者或 administrator\） 的程序的此部分並不重要。 

不過，如果您還沒有重新啟動電腦，因為設定憑證自動註冊，這樣做之前設定 VPN 連線，以確保您有註冊在其上的可用憑證範本。

>[!NOTE]
>沒有任何方式手動新增任何進階的屬性，例如 NRPT 規則，Always On VPN 的受信任的網路偵測等等。在下一個步驟中，建立測試來驗證設定 VPN 伺服器的 VPN 連線，您可以建立 VPN 連線到伺服器。

**手動建立單一測試 VPN 連線**

1.  **VPN 使用者**群組的成員登入加入網域的用戶端電腦。

2.  在 [開始] 功能表中，輸入**VPN**，然後按 Enter。

3.  在詳細資料窗格中，按一下 [**新增 VPN 連線**。

4.  在 VPN 提供者清單中，按一下**Windows （內建）**。

5.  在 [連線的名稱，輸入**範本**。

6.  在伺服器名稱或位址，輸入**外部**您 VPN 伺服器的 FQDN \ (例如，**vpn.contoso.com**\)。

7.  按一下 **\[儲存\]**。

8.  相關設定下，按一下 [**變更介面卡的選項**。

9.  以滑鼠右鍵按一下**範本**，並按一下 \ [**內容**。

10. 在 [**安全性**] 索引標籤中**的 VPN 類型**，按一下 [ **IKEv2**]。

11. 在 [資料加密，按一下**最大長度加密**。

12. 按一下 [**使用 「 可延伸驗證通訊協定 (EAP)**;然後，在**使用 「 可延伸驗證通訊協定 (EAP)**，按一下**Microsoft： 受保護的 EAP (PEAP) （啟用加密）**。

13. 按一下 [**屬性**] 以開啟受保護的 EAP 內容] 對話方塊中，，請完成下列步驟：

    a. 在**連線到這些伺服器**] 方塊中輸入您擷取自稍早在本區段 (例如，NPS01) 中的 NPS 伺服器驗證設定 NPS 伺服器的名稱。

    >[!NOTE]
    >您輸入的伺服器名稱必須符合憑證中的名稱。 您已復原稍早在本區段中的這個名稱。 如果名稱不相符，連線將會失敗，指出，「 連線，所以無法因為 RAS/VPN 伺服器上設定的原則。 」

    b。  受信任的根憑證授權單位，底下選取根憑證授權單位發出 NPS 伺服器憑證 (例如，contoso-CA)。

    c.  在通知連線之前，請按一下 [**不要求使用者授權的新伺服器或受信任的 Ca**。

    d.  在選取的驗證方法，按一下 [**智慧卡或其他憑證**，然後按一下**設定**。 智慧卡或其他憑證屬性對話方塊隨即開啟。

    e.  按一下 [**使用此電腦上的憑證**。

    f.  在 [連線至這些伺服器 \] 方塊中，輸入您在先前步驟中的 NPS 伺服器驗證設定從擷取 NPS 伺服器的名稱。

    g.  受信任的根憑證授權單位，底下選取根發出 NPS 伺服器憑證的 CA。

    h.  選取**不提示使用者授權新伺服器或受信任的憑證授權單位**核取方塊。

    i.  按一下 **[確定]** 以關閉智慧卡或其他憑證內容] 對話方塊。

    j。  按一下 **[確定]** 以關閉的受保護的 EAP 內容對話框。

10. 按一下 **[確定]** 以關閉範本內容] 對話方塊。

11. 關閉網路連線] 視窗。

12. 在 [設定]，測試 VPN 按一下**範本**，然後再按一下**連線**。

>[!IMPORTANT]
>請確定範本 VPN 連線到 VPN 伺服器會成功。 這樣可確保 EAP 設定正確，才能在下一個範例中使用它們。 您必須至少一次，再繼續進行; 連接否則，在設定檔不會包含所有連線至 VPN 所需的資訊。

## <a name="bkmk_ProfileXML"></a>建立 ProfileXML 組態檔

之前提到這一節，請確定您已建立和測試的範本[以手動方式建立範本連線設定檔](#bkmk_profile)的一節描述的 VPN 連線。 測試 VPN 連線時，需要以確保在設定檔包含連線到 VPN 所需的所有資訊。

在列表 1 中的 Windows PowerShell 指令碼會建立兩個檔案在桌面上，兩者都包含**EAPConfiguration**標籤，根據您先前建立的範本連線設定檔：

-   **VPN_Profile.xml。** 這個檔案包含 XML 標記中的 VPNv2 CSP 設定 ProfileXML 節點所需。 使用 OMA DM 相容 MDM 服務，例如 Intune 中使用這個檔案。

-   **VPN_Profile.ps1。** 這個檔案是您可以執行的 Windows PowerShell 指令碼來設定 ProfileXML 節點 VPNv2 CSP 中的用戶端電腦上。 您也可以藉由部署此指令碼透過 System Center Configuration Manager 設定雲端解決方案提供者。 您無法在遠端桌面工作階段，包括 HYPER-V 加強的工作階段中執行此指令碼。

>[!IMPORTANT]
>以下範例命令需要 1607年或更新版本的 Windows 10 組建。

**建立 VPN_Profile.xml 和 VPN_Proflie.ps1**

1. 登入加入網域的用戶端電腦包含範本 VPN 設定檔具有相同的使用者帳戶的 [] 區段中 [手動建立範本連線設定檔 \] 所述。

2. 貼上至 Windows PowerShell 整合式指令碼環境的列表 1 \(ISE\)，以及自訂註解中所述的參數。 這些是 $Template、 $ProfileName、 $Servers、 $DnsSuffix、 $DomainName、 $TrustedNetwork，以及 $DNSServers。 每個設定的完整描述是註解中。

3.  執行指令碼來產生**VPN_Profile.xml**和**VPN_Profile.ps1**桌面上。

#### 清單 1。 了解 MakeProfile.ps1

本節說明您可用來深入了解如何建立 VPN 設定檔，專為設定 ProfileXML VPNv2 CSP 中的範例程式碼。

您在這個範例程式碼從組合指令碼，並執行指令碼，指令碼會產生兩個檔案之後： VPN_Profile.xml 和 VPN_Profile.ps1。 若要設定 OMA DM 相容 MDM 服務，例如 Microsoft Intune 中的 ProfileXML 使用 VPN_Profile.xml。

使用 Windows PowerShell 或 System Center Configuration Manager 中**VPN_Profile.ps1**指令碼來設定 ProfileXML Windows 10 desktop 上。

>[!NOTE]
>若要檢視完整的範例指令碼，請參閱[MakeProfile.ps1 完整的指令碼](#bkmk_fullscript)。

#### Parameters

設定下列參數：

**$Template**。 要從中擷取 EAP 設定範本的名稱。

**$ProfileName**。 設定檔的唯一英數字元的識別碼。 設定檔名稱不能包含正斜線 （/）。 如果設定檔名稱有空格或其他非英數字元，它必須正確逸出根據 URL 編碼標準。

**$Servers**。 公用或是可路由選擇的 IP 位址或 DNS 名稱針對 VPN 閘道。 這可以指向外部的閘道 IP 或伺服器陣列的虛擬 IP。 範例，208.147.66.130 或 vpn.contoso.com。

**$DnsSuffix**。 指定一或多個逗號分隔的 DNS 尾碼。 在清單中的第一個也能用做為主要連線特定 DNS 尾碼 VPN 介面。 整個清單也會加入到 SuffixSearchList。

**$DomainName**。 用來指出要套用原則的命名空間。 當發出名稱查詢時，DNS 用戶端會比較查詢所有的下方尋找相符項目來設定命名空間中的名稱。 這個參數可以是下列類型的其中一個：

- FQDN-完整的網域名稱
- 尾碼-要附加到 DNS 解析 shortname 查詢的網域尾碼。 若要指定尾碼，前面加上句號 \ （.） 的 DNS 尾碼。

**$DNSServers**。 逗號分隔的 DNS 伺服器 IP 位址清單用於命名空間。

**$TrustedNetwork**。 以逗號分隔字串以識別受信任的網路。 VPN 不是不自動連線當使用者在公司無線網路受保護的資源可直接存取裝置的位置。

以下是範例以下命令中使用的參數值。 請確定您將這些值變更為您的環境。

    $TemplateName = 'Template'
    $ProfileName = 'Contoso AlwaysOn VPN'
    $Servers = 'vpn.contoso.com'
    $DnsSuffix = 'corp.contoso.com'
    $DomainName = '.corp.contoso.com'
    $DNSServers = '10.10.0.2,10.10.0.3'
    $TrustedNetwork = 'corp.contoso.com'


### 準備與建立設定檔 XML

下列範例命令會取得 EAP 設定從範本設定檔：


    $Connection = Get-VpnConnection -Name $TemplateName
    if(!$Connection)
    {
    $Message = "Unable to get $TemplateName connection profile: $_"
    Write-Host "$Message"
    exit
    }
    $EAPSettings= $Connection.EapConfigXmlStream.InnerXml


### 建立設定檔 XML

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

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


### 輸出 VPN_Profile.xml intune

您可以使用下列範例命令，以儲存設定檔 XML 檔案：

    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')

### 輸出 VPN_Profile.ps1 桌面與 System Center Configuration Manager

下列範例程式碼會使用 ProfileXML 節點 VPNv2 CSP 中設定 AlwaysOn IKEv2 VPN 連線。

在 Windows 10 desktop 上或 System Center Configuration Manager 中，您可以使用這個指令碼。

### 定義索引鍵的 VPN 設定檔參數

    $Script = '$ProfileName = ''' + $ProfileName + '''
    $ProfileNameEscaped = $ProfileName -replace ' ', '%20'

## 定義 VPN ProfileXML

    $ProfileXML = ''' + $ProfileXML + '''
    
### 在設定檔中逸出特殊字元

    $ProfileXML = $ProfileXML -replace '<', '&lt;'
    $ProfileXML = $ProfileXML -replace '>', '&gt;'
    $ProfileXML = $ProfileXML -replace '"', '&quot;'
    
### 定義 WMI 至 CSP 橋接器屬性

    $nodeCSPURI = "./Vendor/MSFT/VPNv2"
    $namespaceName = "root\cimv2\mdm\dmmap"
    $className = "MDM_VPNv2_01"

### 判斷使用者 SID 的 VPN 設定檔：

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
    

### 定義 WMI 工作階段：

    $session = New-CimSession
    $options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)


### 偵測及刪除先前的 VPN 設定檔：

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
    

### 建立 VPN 設定檔：

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

### 儲存設定檔 XML 檔案

    $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"

## <a name="bkmk_fullscript"></a>MakeProfile.ps1 完整的指令碼

大部分的範例會使用組 Set-wmiinstance Windows PowerShell cmdlet，將 ProfileXML 插入 MDM_VPNv2_01 WMI 類別的新執行個體。 

不過，這並不搭配 System Center Configuration Manager 中因為您無法在使用者的內容中執行的套件。 因此，此指令碼建立 WMI 工作階段，在使用者的內容中，使用常見的資訊模型，然後在該工作階段中建立 MDM_VPNv2_01 WMI 類別的新執行個體。 此 WMI 類別會使用 WMI 至 CSP 橋接器來設定 VPNv2 CSP。 因此，藉由新增類別執行個體，您會設定雲端解決方案提供者。 

>[!IMPORTANT]
>WMI 至 CSP 橋接器需要本機系統管理員權限，原本設計的做法。 每個使用者 VPN 設定檔部署您應該使用 SCCM 或 MDM

>[!NOTE]
>VPN_Profile.ps1 指令碼會使用目前使用者 SID 來識別使用者的內容。 因為沒有 SID 使用遠端桌面工作階段中，無法在遠端桌面工作階段中運作指令碼。 同樣地，它無法運作的 HYPER-V 加強的工作階段中。 如果您要在虛擬機器中測試遠端存取 Always On VPN，停用加強的工作階段在您的用戶端 Vm 上執行此指令碼之前。

以下範例指令碼會包含所有的前面小節中的程式碼範例。 請確定您將範例值變更為適合您環境的值。
    
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

## 使用 Windows PowerShell 來設定 VPN 用戶端

若要設定 Windows 10 用戶端電腦上的 VPNv2 CSP，執行您在[建立設定檔 XML](#create-the-profile-xml)一節中建立的 VPN_Profile.ps1 Windows PowerShell 指令碼。 開啟 Windows PowerShell 以系統管理員身分;否則，您會收到錯誤訊息指出_拒絕存取_。

執行 VPN_Profile.ps1 來設定 VPN 設定檔之後, 您可以隨時確認它成功的 Windows PowerShell ISE 中執行下列命令：

    Get-WmiObject -Namespace root\cimv2\mdm\dmmap -Class MDM_VPNv2_01

**成功從 Get-wmiobject cmdlet 的結果**


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

ProfileXML 設定必須是正確的結構、 拼字檢查、 設定及有時字母大小寫。 如果您看到清單 1 不同結構中的項目，可能會在 ProfileXML 標記包含錯誤。

如果您需要進行疑難排解的標記，它是更輕鬆地將它放到疑難排解它在 Windows PowerShell ISE 比 XML 編輯器中。 在任一情況下，開頭的設定檔，最簡單的版本，並新增的元件回問題直到一次，就會發生一次。

## <a name="vpn-deploy-client-sccm"></a>使用 System Center Configuration Manager 設定 VPN 用戶端

System Center Configuration Manager 中，您可以使用 ProfileXML 雲端解決方案提供者] 節點中，就如同在 Windows PowerShell 部署 VPN 設定檔。 在這裡，您可以使用您在[建立 ProfileXML 設定檔](#bkmk_ProfileXML)的一節中建立的 VPN_Profile.ps1 Windows PowerShell 指令碼。

若要使用 System Center Configuration Manager 部署 Windows 10 用戶端電腦的遠端存取 Always On VPN 設定檔，您必須藉由建立一組電腦或使用者對象您部署的設定檔開始。 在此案例中，建立部署設定指令碼的使用者群組。

### 建立使用者群組

1.  在 Configuration Manager 主控台中，開啟 [資產和 Compliance\\User 集合。

2.  **家用版**功能區上，在**建立**群組中，按一下 [**建立使用者的集合**。

3.  在 [一般 \] 頁面上完成下列步驟：

    a. 在**名稱**中，輸入**VPN 使用者**。

    b。 按一下 [**瀏覽**、 按一下 [**所有使用者**，並都按一下 **[確定]**。

    c. 按 **[下一步]**。

4.  在成員資格規則頁面上，請完成下列步驟：

    a.  在**成員資格規則**，按一下 [**新增規則**，然後按一下**直接規則**。 在此範例中，您要新增個別使用者，到使用者集合。 不過，您可能會使用查詢規則，將使用者新增到動態的大規模部署這個集合。

    b。  在**歡迎**頁面上，按一下 [**下一步**。

    c.  在 [搜尋資源] 頁面的**值**，輸入您想要新增之使用者的名稱。 資源名稱包含使用者的網域。 若要包含根據部分相符的結果，請插入**%** 您的搜尋條件結尾處的字元。 例如，若要尋找包含字串 「 lori 」 的所有使用者，輸入 **%lori%**。 按 **[下一步]**。

    d.  在選取的資源頁面上，選取您想要新增到群組中，的使用者，並按一下 [**下一步**。

    e.  在摘要頁面上，按一下 [**下一步**。

    f.  在 [完成] 頁面上，按一下 [**關閉**]。

6.  重新開啟 \ [建立使用者集合精靈的成員資格規則頁面，按一下 [**下一步**。

7.  在摘要頁面上，按一下 [**下一步**。

8.  在 [完成] 頁面上，按一下 [**關閉**]。

建立使用者群組，以接收 VPN 設定檔之後，您可以建立套件和程式來部署您在[建立 ProfileXML 設定檔](#bkmk_ProfileXML)的一節中建立 Windows PowerShell 設定指令碼。

### 建立包含 ProfileXML 設定指令碼的套件

1.  主控站台伺服器的電腦帳戶可存取的網路共用上的指令碼 VPN_Profile.ps1。

2.  在 Configuration Manager 主控台中，開啟**軟體 Library\\Application Management\\Packages**。

3.  **家用版**功能區上，在**建立**群組中，按一下要開始建立套件和程式精靈**建立的套件**。

4.  在封裝頁面上，請完成下列步驟：

    a. 在**名稱**中，輸入**Windows 10 一律開啟 VPN 設定檔**。

    b。 選取**此套件包含來源檔案**] 核取方塊，然後按一下 [**瀏覽**。

    c. 在設定來源資料夾] 對話方塊中，按一下 [**瀏覽**、 選取的檔案共用，包含 VPN_Profile.ps1，然後按一下 **[確定]**。<p>請確定您選取的網路路徑，而不是本機路徑。 換句話說，路徑應該*\\fileserver\\vpnscript*， *c:\\vpnscript*類似。

1.  按 **[下一步]**。

2.  在計畫類型頁面上，按一下 [**下一步**。

3.  在標準程式頁面上，請完成下列步驟：

    a.  在**名稱**中，輸入 [ **VPN 設定檔的指令碼**。

    b。  在**命令列**中，輸入**PowerShell.exe ExecutionPolicy 略過-檔案 」 VPN_Profile.ps1 」**。

    c.  在**執行模式**中，按一下 [**使用管理權限執行**。

    d.  按 **[下一步]**。

4.  在需求頁面上，請完成下列步驟：

    a.  選取**此計畫可以只在指定的平台上執行**。

    b。  選取**所有的 Windows 10 （32 位元）** 及**所有 Windows 10 （64 位元）** 核取方塊。

    c.  在**估計磁碟空間**中，輸入**1**。

    d.  在**最多允許執行的時間 （分鐘）**，輸入**15**。

    e.  按 **[下一步]**。

5.  在摘要頁面上，按一下 [**下一步**。

6.  在 [完成] 頁面上，按一下 [**關閉**]。

套件和建立計畫，您需要將它部署到**VPN 使用者**群組。

### 部署 ProfileXML 設定指令碼

1.  在 Configuration Manager 主控台中，開啟軟體 Library\\Application Management\\Packages。

2.  在**套件**中，按一下 [ **Windows 10 一律開啟 VPN 設定檔**。

3.  在 [**程式**] 索引標籤的底部的詳細資料窗格中，以滑鼠右鍵按一下 [ **VPN 設定檔的指令碼**、 按一下 [**屬性**]，請完成下列步驟：

    a.  在 [**進階**] 索引標籤中，**此計畫被指派至電腦時**，按一下 [**一次用於每一位登入的使用者**。

    b。  按一下 **\[確定\]**。

4.  以滑鼠右鍵按一下 [ **VPN 設定檔的指令碼**，然後按一下 [啟動 \ [部署軟體精靈**部署**。

5.  在 [一般 \] 頁面上完成下列步驟：

    a.  旁邊**集合**，按一下 [**瀏覽**。

    b。  在**集合類型**清單 （左上角） 中，按一下**使用者集合**。

    c.  按一下**VPN 使用者**，然後按一下 **[確定]**。

    d.  按 **[下一步]**。

6.  在內容的頁面上，請完成下列步驟：

    a.  按一下 [**新增**]，然後按一下 [**發佈點**。

    b。  **可用的發佈點**，在選取的發佈點，您要發佈 ProfileXML 設定指令碼，並按一下 **[確定]**。

    c.  按 **[下一步]**。

7.  在部署的設定頁面上，按一下 [**下一步**。

8.  排程在頁面上，請完成下列步驟：

    a.  按一下 \ [**新增]** 以開啟 [指派排程] 對話方塊。

    b。  按一下 [**在這個事件後立即指派**，然後按一下 **[確定]**。

    c.  按 **[下一步]**。

9.  在使用者體驗頁面上，請完成下列步驟：

    1.  選取 [**軟體安裝**核取方塊。

    2.  按一下 [**摘要**]。

10. 在摘要頁面上，按一下 [**下一步**。

11. 在 [完成] 頁面上，按一下 [**關閉**]。

ProfileXML 設定指令碼，部署，Windows 10 用戶端電腦以建置使用者集合時，您選取的使用者帳戶登入。 確認 VPN 用戶端的設定。

>[!NOTE]
>指令碼 VPN_Profile.ps1 無法在遠端桌面工作階段中運作。 同樣地，它無法運作的 HYPER-V 加強的工作階段中。 如果您要在虛擬機器中測試遠端存取 Always On VPN，停用加強的工作階段在您的用戶端 Vm 上再繼續進行。

### 確認 VPN 用戶端的設定

1.  在 [控制台**System\\Security**下的 [**組態管理員**。 

2.  在 Configuration Manager 屬性對話方塊中，在 [**動作**] 索引標籤，請完成下列步驟：

    a.  按一下**電腦原則抓取 & 和評估週期**，按一下 \ [**立即執行**，然後按一下 **[確定]**。

    b。  按一下**使用者原則抓取 & 和評估週期**，按一下 \ [**立即執行**，然後按一下 **[確定]**。

    c.  按一下 **\[確定\]**。

3.  關閉在控制台中。

稍後您應該會看到新的 VPN 設定檔。

## 使用 Intune 來設定 VPN 用戶端

若要使用 Intune 部署 Windows 10 遠端存取 Always On VPN 設定檔，您可以使用您在[建立 ProfileXML 組態檔](#bkmk_ProfileXML)，一節中建立 VPN 設定檔設定 ProfileXML 雲端解決方案提供者節點或您可以使用提供的基底 EAP XML 範例下方。

>[!NOTE]
>Intune 現在會使用 Azure AD 群組。 Azure AD Connect 同步至 Azure AD，從內部部署 VPN 使用者群組，且使用者已指派給 VPN 使用者群組，如果您準備好進行。

建立 VPN 裝置設定原則來設定 Windows 10 用戶端電腦新增至群組的所有使用者。 由於 Intune 範本提供 VPN 參數，只會複製 VPN_ProfileXML 檔案的 \<EapHostConfig> \</EapHostConfig> 部分。 


### 建立 Always On VPN 設定原則

1.  登入[Azure 入口網站](https://portal.azure.com/)。

2.  移至**Intune** > **裝置設定** > **設定檔**。

3.  點選以開始精靈建立設定檔的**建立設定檔**。

4.  VPN 設定檔和 （選擇性） 描述中輸入**名稱**。

5.   **平台**底下選取**Windows 10 或更新版本**，並選擇**VPN**設定檔類型下拉式從。

     >[!TIP]
     >如果您要建立自訂的 VPN profileXML，請參閱[套用 ProfileXML 使用 Intune](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune)的指示。

6. 在**基底 VPN** ] 索引標籤中，確認或設定下列設定：

    - **連線名稱：** 它會出現在用戶端電腦**設定**] 下的 [ **VPN** ] 索引標籤中，例如_Contoso AutoVPN_輸入 VPN 連線的名稱。  
    
    - **伺服器：** 新增一或多個 VPN 伺服器，按一下 [**新增]。**
    
    - **描述**以及**IP 位址或 FQDN:** 輸入的描述和 IP 位址或 VPN 伺服器的 FQDN。 中的 VPN 伺服器驗證憑證的主體名稱必須符合這些值。 
    
    - **預設的伺服器：** 如果這是預設的 VPN 伺服器時，設為**True**。 執行此動作會啟用此伺服器為預設的伺服器，用來建立連線的裝置。 
    
    - **連線類型：** 設為**IKEv2**。  
    
    - **Always On:** 設為 「**啟用**」，以連線至 VPN，自動在登入時，並保持連線，直到使用者手動中斷。
    
    - **記住認證，每次登入時**： 布林值 （true 或 false），適用於快取的認證。 如果設定為 true，認證儘可能會快取。

7.  將下列 XML 字串複製到文字編輯器中：<p>
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    <p>
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

8.  取代**\<TrustedRootCA>5a 89 中文 cb 5b\ 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 會 4b<\/TrustedRootCA>** 在此範例使用兩個地方您內部部署根憑證授權單位的憑證指紋。
  
    >[!Important]
    >請勿使用下方的 \<TrustedRootCA>\</TrustedRootCA> 區段中的範例憑證指紋。  TrustedRootCA 必須是在內部根憑證授權單位發出 RRAS 及 NPS 伺服器的伺服器驗證憑證的憑證指紋。 **這不能雲端根憑證，也不中繼發行的 CA 憑證指紋**。

10. **\_LT_ServerNames>NPS.contoso.com\</ServerNames>** 中的範例 XML 取代的驗證會在已加入網域的 NPS 的 FQDN。 

11. 將修改過的 XML 字串複製和貼到基底 VPN] 索引標籤下的**EAP Xml**方塊並按一下 **[確定]**。<p>在 Intune 中，會建立使用 EAP 一律在 VPN 裝置設定原則。


### 同步處理的 Intune Always On VPN 設定原則

若要測試設定原則，您新增至**一律在 VPN 使用者**群組中，在使用者登入 windows 10 用戶端電腦，然後再同步與 Intune。

1.  在 [開始] 功能表中，按一下 [**設定**]。

2.  在設定中，按一下 [**帳戶**]，並按一下 [**存取公司或學校資源**。

3.  按一下 MDM 設定檔，然後按一下**資訊**。

4.  按一下 [強制 Intune 原則評估和擷取**同步**。

5.  關閉設定。 同步之後，您會看到可用的 VPN 設定檔的電腦上。

## 後續步驟
您完成部署 Always On VPN。  您可以設定其他功能，請參閱下表：

|如果您想要...  |然後查看...  |
|---------|---------|
|設定 vpn 的條件式存取    |[步驟 7。（選擇性）設定適用於使用 Azure AD 的 VPN 連線性條件式存取](../../ad-ca-vpn-connectivity-windows10.md)： 在此步驟中，您可以微調如何已獲授權的 VPN 使用者存取您使用[Azure Active Directory (Azure AD) 的條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)的資源。 使用 Azure AD 的虛擬私人網路 (VPN) 連線的條件式存取，您可以協助保護 VPN 連線。 「條件式存取」是原則型評估引擎，可讓您為任何 Azure Active Directory (Azure AD) 已連線的應用程式建立存取規則。         |
|深入了解進階 VPN 功能  |[進階 VPN 功能](always-on-vpn-adv-options.md#advanced-vpn-features)： 此頁面提供關於如何啟用 VPN 流量篩選器、 如何設定自動 VPN 連線使用的應用程式-觸發程序，以及如何設定 NPS 只允許來自用戶端使用 Azure 所發行的憑證的 VPN 連線的指導方針廣告。        |


---
