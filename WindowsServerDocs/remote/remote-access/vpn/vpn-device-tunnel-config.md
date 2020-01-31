---
title: 在 Windows 10 中設定 VPN 裝置通道
description: 瞭解如何在 Windows 10 中建立 VPN 裝置通道。
ms.prod: windows-server
ms.date: 11/05/2018
ms.technology: networking-ras
ms.topic: article
ms.assetid: 158b7a62-2c52-448b-9467-c00d5018f65b
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: b5be8827cee22b35fb31bf08d1c960b150dcad84
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822651"
---
# <a name="configure-vpn-device-tunnels-in-windows-10"></a>在 Windows 10 中設定 VPN 裝置通道

>適用于： Windows 10 版本1709

Always On VPN 可讓您為裝置或電腦建立專用的 VPN 設定檔。 Always On VPN 連接包含兩種類型的通道： 

- _裝置_通道會在使用者登入裝置之前連線到指定的 VPN 伺服器。 登入前的連線案例和裝置管理用途是使用裝置通道。

- 只有在使用者登入裝置之後，_使用者_通道才會連接。 使用者通道可讓使用者透過 VPN 伺服器存取組織資源。

不同_于使用者通道_，只有在使用者登入裝置或電腦之後才會連線，_裝置_通道可讓 VPN 在使用者登入之前，先建立連線能力。 _裝置_通道和_使用者_通道都是獨立運作的 VPN 設定檔，可以同時連線，並且可以適當地使用不同的驗證方法和其他 VPN 設定。 使用者通道支援 SSTP 和 IKEv2，而裝置通道僅支援 IKEv2，而不支援 SSTP 回溯。

已加入網域、未加入網域（工作組）的使用者通道，或已加入 Azure AD 的裝置，都支援企業和 BYOD 案例。 它適用于所有 Windows 版本，而平臺功能則可透過 UWP VPN 外掛程式支援提供給協力廠商使用。

裝置通道只能在執行 Windows 10 企業版或教育版1709或更新版本的已加入網域裝置上設定。 不支援裝置通道的協力廠商控制。


## <a name="device-tunnel-requirements-and-features"></a>裝置通道需求和功能
您必須為 VPN 連線啟用電腦憑證驗證，並定義用來驗證傳入 VPN 連線的根憑證授權單位。 

```PowerShell
$VPNRootCertAuthority = “Common Name of trusted root certification authority”
$RootCACert = (Get-ChildItem -Path cert:LocalMachine\root | Where-Object {$_.Subject -Like “*$VPNRootCertAuthority*” })
Set-VpnAuthProtocol -UserAuthProtocolAccepted Certificate, EAP -RootCertificateNameToAccept $RootCACert -PassThru
```

![裝置通道功能和需求](../../media/device-tunnel-feature-and-requirements.png)

## <a name="vpn-device-tunnel-configuration"></a>VPN 裝置通道設定

下面的範例設定檔 XML 提供良好的指引，適用于只需要透過裝置通道進行用戶端起始提取的案例。  流量篩選器會用來將裝置通道限制為僅限管理流量。  此設定適用于 Windows Update、一般群組原則（GP）和 Microsoft 端點 Configuration Manager 更新案例，以及用於第一次登入但沒有快取認證或密碼重設案例的 VPN 連線能力。 

針對伺服器起始的推送案例（例如 Windows 遠端管理（WinRM）、遠端 GPUpdate 和遠端 Configuration Manager 更新案例），您必須允許裝置通道上的輸入流量，因此無法使用流量篩選器。  如果在裝置通道設定檔中，您開啟流量篩選器，則裝置通道會拒絕輸入流量。  未來的版本將會移除這項限制。


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

視每個特定部署案例的需求而定，另一個可使用裝置通道設定的 VPN 功能就是[受信任的網路偵測](https://social.technet.microsoft.com/wiki/contents/articles/38546.new-features-for-vpn-in-windows-10-and-windows-server-2016.aspx#Trusted_Network_Detection)。

```
 <!-- inside/outside detection -->
  <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

## <a name="deployment-and-testing"></a>部署與測試

您可以使用 Windows PowerShell 腳本並使用 Windows Management Instrumentation （WMI）橋接器來設定裝置通道。 Always On VPN 裝置通道必須在**本機系統**帳戶的內容中設定。 若要完成這項操作，必須使用[PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec)，這是[Sysinternals](https://docs.microsoft.com/sysinternals/)的公用程式套件中所包含的其中一個[PsTools](https://docs.microsoft.com/sysinternals/downloads/pstools) 。

如需有關如何在每個裝置上部署 `(.\Device)` 與每個使用者 `(.\User)` 設定檔的指導方針，請參閱搭配[使用 PowerShell 腳本與 WMI 橋接器提供者](https://docs.microsoft.com/windows/client-management/mdm/using-powershell-scripting-with-the-wmi-bridge-provider)。

執行下列 Windows PowerShell 命令，以確認您已成功部署裝置設定檔：

  ```powershell
  Get-VpnConnection -AllUserConnection
  ```

輸出會顯示裝置上所部署\-wide VPN 設定檔的清單。

### <a name="example-windows-powershell-script"></a>範例 Windows PowerShell 腳本

您可以使用下列 Windows PowerShell 腳本，協助建立自己的腳本來建立設定檔。

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

以下是可協助您進行 VPN 部署的其他資源。

### <a name="vpn-client-configuration-resources"></a>VPN 用戶端設定資源

以下是 VPN 用戶端設定資源。

- [如何在 Configuration Manager 中建立 VPN 設定檔](https://docs.microsoft.com/configmgr/protect/deploy-use/create-vpn-profiles)
- [設定 Windows 10 用戶端 Always On VPN 連線](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [VPN 設定檔選項](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)

### <a name="remote-access-server-gateway-resources"></a>遠端存取服務器閘道資源

以下是遠端存取服務器（RAS）閘道資源。

- [使用電腦驗證憑證設定 RRAS](https://technet.microsoft.com/library/dd458982.aspx)
- [針對 IKEv2 VPN 連線進行疑難排解](https://technet.microsoft.com/library/dd941612.aspx)
- [設定以 IKEv2 為基礎的遠端存取](https://technet.microsoft.com/library/ff687731.aspx)

>[!IMPORTANT]
>搭配使用裝置通道與 Microsoft RAS 閘道時，您必須設定 RRAS 伺服器以支援 IKEv2 電腦憑證驗證，方法是啟用 [**允許對 ikev2 驗證方法進行電腦憑證驗證**]，如[這裡](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee922682%28v=ws.10%29)所述。 一旦啟用此設定，強烈建議使用**VpnAuthProtocol** PowerShell Cmdlet 搭配**RootCertificateNameToAccept**選擇性參數，以確保只有連結至明確定義的內部/私用根憑證授權單位的 VPN 用戶端憑證，才允許 RRAS IKEv2 連接。 或者，應該修改 RRAS 伺服器上的「**受信任的根憑證授權**單位」存放區，以確保它不包含如[這裡](https://blogs.technet.microsoft.com/rrasblog/2009/06/10/what-type-of-certificate-to-install-on-the-vpn-server/)所討論的公開憑證授權單位單位。 類似的方法也可能需要考慮其他 VPN 閘道。

