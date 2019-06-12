---
title: 在 Windows 10 中設定 VPN 裝置通道
description: 了解如何在 Windows 10 中建立的 VPN 裝置通道。
ms.prod: windows-server-threshold
ms.date: 11/05/2018
ms.technology: networking-ras
ms.topic: article
ms.assetid: 158b7a62-2c52-448b-9467-c00d5018f65b
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 989216f90e78689b464240cff957bab1d9c1229b
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749564"
---
# <a name="configure-vpn-device-tunnels-in-windows-10"></a>在 Windows 10 中設定 VPN 裝置通道

>適用於：Windows 10 1709年版

一律開啟 」 VPN 可讓您能夠建立專用的 VPN 設定檔針對裝置或電腦。 一律開啟 」 VPN 連線中包含兩種類型的通道： 

- _裝置通道_連接到指定的 VPN 伺服器，然後再使用者登入裝置。 登入前連線案例和裝置管理，請使用 裝置通道。

- _使用者通道_使用者登入裝置之後，才會連線。 使用者通道可讓使用者透過 VPN 伺服器存取組織資源。

不同於_使用者通道_，這只會連接使用者登入裝置或電腦之後,_裝置通道_可讓使用者登入之前建立的連接 VPN。 兩者_裝置通道_並_使用者通道_獨立運作，使用自己的 VPN 設定檔，可以連線在此同時，也可以使用不同的驗證方法和其他 VPN 組態設定視需要。 使用者通道支援 SSTP 和 IKEv2，而且裝置通道只提供任何支援 SSTP 遞補支援 IKEv2。

使用者通道支援加入網域，非網域聯結 （工作群組） 或以供企業和 BYOD 案例的 Azure AD – 已加入裝置。 它是用於所有 Windows 版本，和的平台功能都適用於 UWP VPN 外掛程式支援透過第三方。

只能在執行 Windows 10 企業版或教育版本 1709年或更新版本的已加入網域的裝置上設定裝置通道。 沒有支援的裝置通道的協力廠商控制項。


## <a name="device-tunnel-requirements-and-features"></a>裝置通道需求和功能
您必須啟用 VPN 連線的電腦憑證驗證，並定義根憑證授權單位來驗證連入的 VPN 連線。 

```PowerShell
$VPNRootCertAuthority = “Common Name of trusted root certification authority”
$RootCACert = (Get-ChildItem -Path cert:LocalMachine\root | Where-Object {$_.Subject -Like “*$VPNRootCertAuthority*” })
Set-VpnAuthProtocol -UserAuthProtocolAccepted Certificate, EAP -RootCertificateNameToAccept $RootCACert -PassThru
```

![裝置通道功能和需求](../../media/device-tunnel-feature-and-requirements.png)

## <a name="vpn-device-tunnel-configuration"></a>設定 VPN 裝置通道

透過裝置通道，則需要下列的 XML 提供正確的指引，其中只有用戶端起始提取案例的範例設定檔。  流量篩選器可用來管理流量只以限制裝置通道。  這項設定適用於 Windows 更新，一般群組原則 (GP) 和 System Center Configuration Manager (SCCM) 更新案例，以及 VPN 連線能力，而不需要快取的認證，第一次登入或密碼重設案例。 

伺服器起始的推播的情況下，Windows 遠端管理 (WinRM)、 Remote GPUpdate，等遠端 SCCM 更新案例 – 您必須在裝置通道，允許輸入的流量，因此不能用於流量篩選器。  裝置通道設定檔在您開啟流量篩選器，如果裝置通道就會拒絕傳入的流量。  這項限制會在未來版本中移除。


### <a name="sample-vpn-profilexml"></a>範例 VPN profileXML

以下是範例 VPN profileXML。

``` xml
<VPNProfile>  
  <NativeProfile>  
<Servers>vpn.contoso.com</Servers>  
<NativeProtocolType>IKEv2</NativeProtocolType>  
<Authentication>  
  <MachineMethod>Certificate</MachineMethod>  
</Authentication>  
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>  
 <!-- disable the addition of a class based route for the assigned IP address on the VPN interface -->
<DisableClassBasedDefaultRoute>true</DisableClassBasedDefaultRoute>  
  </NativeProfile> 
  <!-- use host routes(/32) to prevent routing conflicts -->  
  <Route>  
<Address>10.10.0.2</Address>  
<PrefixSize>32</PrefixSize>  
  </Route>  
  <Route>  
<Address>10.10.0.3</Address>  
<PrefixSize>32</PrefixSize>  
  </Route>  
<!-- traffic filters for the routes specified above so that only this traffic can go over the device tunnel --> 
  <TrafficFilter>  
<RemoteAddressRanges>10.10.0.2, 10.10.0.3</RemoteAddressRanges>  
  </TrafficFilter>
<!-- need to specify always on = true --> 
  <AlwaysOn>true</AlwaysOn> 
<!-- new node to specify that this is a device tunnel -->  
 <DeviceTunnel>true</DeviceTunnel>
<!--new node to register client IP address in DNS to enable manage out -->
<RegisterDNS>true</RegisterDNS>
</VPNProfile>
```

根據每個特定部署案例的需求，是另一個 VPN 功能，可設定裝置通道[受信任網路偵測](https://social.technet.microsoft.com/wiki/contents/articles/38546.new-features-for-vpn-in-windows-10-and-windows-server-2016.aspx#Trusted_Network_Detection)。

```
 <!-- inside/outside detection -->
  <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

## <a name="deployment-and-testing"></a>部署和測試

您可以使用 Windows PowerShell 指令碼，並使用 Windows Management Instrumentation (WMI) 的橋接器來設定裝置通道。 中的內容，就必須設定一律開啟 」 VPN 裝置通道**本機系統**帳戶。 若要這麼做，它會使用所需[PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec)、 其中的[PsTools](https://docs.microsoft.com/sysinternals/downloads/pstools)納入[Sysinternals](https://docs.microsoft.com/sysinternals/)公用程式的套件。

如需有關如何部署的指導方針每個裝置`(.\Device)`與每位使用者`(.\User)`設定檔，請參閱[使用 PowerShell 指令碼和 WMI 橋接器提供者](https://docs.microsoft.com/windows/client-management/mdm/using-powershell-scripting-with-the-wmi-bridge-provider)。

執行下列 Windows PowerShell 命令來確認您已成功部署的裝置設定檔：

  ```powershell
  Get-VpnConnection -AllUserConnection
  ```

輸出會顯示一份裝置\-各種裝置部署的 VPN 設定檔。

### <a name="example-windows-powershell-script"></a>Windows PowerShell 指令碼範例

您可以使用下列 Windows PowerShell 指令碼，可協助建立您自己的設定檔建立的指令碼。

```PowerShell
Param(
[string]$xmlFilePath,
[string]$ProfileName
)

$a = Test-Path $xmlFilePath
echo $a

$ProfileXML = Get-Content $xmlFilePath

echo $XML

$ProfileNameEscaped = $ProfileName -replace ' ', '%20'

$Version = 201606090004

$ProfileXML = $ProfileXML -replace '<', '&lt;'
$ProfileXML = $ProfileXML -replace '>', '&gt;'
$ProfileXML = $ProfileXML -replace '"', '&quot;'

$nodeCSPURI = './Vendor/MSFT/VPNv2'
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_VPNv2_01"

$session = New-CimSession

try
{
$newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", 'String', 'Key')
$newInstance.CimInstanceProperties.Add($property)
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", 'String', 'Key')
$newInstance.CimInstanceProperties.Add($property)
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", 'String', 'Property')
$newInstance.CimInstanceProperties.Add($property)

$session.CreateInstance($namespaceName, $newInstance)
$Message = "Created $ProfileName profile."
Write-Host "$Message"
}
catch [Exception]
{
$Message = "Unable to create $ProfileName profile: $_"
Write-Host "$Message"
exit
}
$Message = "Complete."
Write-Host "$Message"
```

## <a name="additional-resources"></a>其他資源

以下是可協助您的 VPN 部署的其他資源。

### <a name="vpn-client-configuration-resources"></a>VPN 用戶端設定資源

以下是 VPN 用戶端設定的資源。

- [如何建立 VPN 設定檔在 System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles)
- [設定 Windows 10 用戶端一律開啟 VPN 連線](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [VPN 設定檔選項](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)

### <a name="remote-access-server-gateway-resources"></a>遠端存取伺服器閘道資源

以下是遠端存取伺服器 (RAS) 閘道資源。

- [設定 RRAS 電腦驗證憑證](https://technet.microsoft.com/library/dd458982.aspx)
- [IKEv2 VPN 連線疑難排解](https://technet.microsoft.com/library/dd941612.aspx)
- [設定 IKEv2 型遠端存取](https://technet.microsoft.com/library/ff687731.aspx)

>[!IMPORTANT]
>搭配使用時裝置通道 Microsoft RAS 閘道，您必須設定 RRAS 伺服器支援 IKEv2 機器憑證驗證，進而**IKEv2 允許機器憑證驗證**如所述的驗證方法[此處](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee922682%28v=ws.10%29)。 一旦啟用此設定時，強烈建議**組 VpnAuthProtocol** PowerShell cmdlet，連同**RootCertificateNameToAccept**選擇性參數，用來確保RRAS IKEv2 連線只能用於 VPN 用戶端憑證鏈結至明確定義內部/私用根憑證授權單位。 或者，**受信任的根憑證授權單位**RRAS 伺服器上的存放區應該加以修改以確保它不包含公用憑證授權單位，如所述[這裡](https://blogs.technet.microsoft.com/rrasblog/2009/06/10/what-type-of-certificate-to-install-on-the-vpn-server/)。 其他 VPN 閘道的考量，可能也需要類似的方法。

