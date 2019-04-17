---
title: 在 Windows 10 中設定 VPN 裝置通道
description: 了解如何在 Windows 10 中建立 VPN 裝置通道。
ms.prod: windows-server-threshold
ms.date: 11/05/2018
ms.technology: networking-ras
ms.topic: article
ms.assetid: 158b7a62-2c52-448b-9467-c00d5018f65b
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 005721873ad3a0df942bc9e23eba13728965ccba
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067125"
---
# 在 Windows 10 中設定 VPN 裝置通道

>適用於： Windows 10 版本 1709

Always On VPN 可讓您能夠建立專用的 VPN 設定檔的裝置或電腦。 Always On VPN 連線包括兩種類型的通道： 

- _裝置通道_連線到指定的 VPN 伺服器之前使用者身分登入裝置。 預先登入的連線案例和裝置管理目的使用裝置通道。

- 只有在使用者登入裝置之後，_使用者通道_連線。 使用者通道可讓使用者透過 VPN 伺服器存取組織資源。

不同於_使用者通道_，這只連接之後有使用者登入的裝置或電腦時，_裝置通道_可讓使用者登入之前建立連線 VPN。 _裝置通道_和_使用者通道_都獨立運作以其 VPN 設定檔、 可在此同時，連線，可以使用不同的驗證方法及其他 VPN 組態設定適當。 使用者通道支援 SSTP 和 IKEv2，而裝置通道支援 IKEv2 只能在採用 SSTP 後援不支援。

使用者通道上支援已加入網域，未加入網域的 （工作群組） 或加入 Azure AD – 裝置允許的企業與 BYOD 案例。 它是適用於所有 Windows 版本，也可用來透過支援 UWP VPN 外掛程式的協力廠商平台功能。

裝置通道只能在執行 Windows 10 企業版或教育版版本 1709年或更新版本的加入網域的裝置上進行設定。 沒有支援的裝置通道的第三方控制項。


## 裝置通道需求和功能
您必須啟用適用於 VPN 連線的電腦憑證驗證，並定義用於驗證連入的 VPN 連線的根憑證授權單位。 

```PowerShell
$VPNRootCertAuthority = “Common Name of trusted root certification authority”
$RootCACert = (Get-ChildItem -Path cert:LocalMachine\root | Where-Object {$_.Subject -Like “*$VPNRootCertAuthority*” })
Set-VpnAuthProtocol -UserAuthProtocolAccepted Certificate, EAP -RootCertificateNameToAccept $RootCACert -PassThru
```

![裝置通道功能和需求](../../media/device-tunnel-feature-and-requirements.png)

## VPN 裝置通道設定

以下的 XML 提供良好的指導方針案例，其中僅用戶端起始拖動範例設定檔是必要項目透過裝置通道。  若要將裝置通道限制為僅限管理流量，運用了流量篩選器。  此設定適用於 Windows Update、 典型的群組原則 (GP) 和 System Center Configuration Manager (SCCM) 更新案例，以及 VPN 連線，而快取的認證，第一次登入或密碼重設案例。 

針對伺服器起始推播的情況下，例如 Windows 遠端管理 (WinRM)、 遠端 GPUpdate，以及遠端 SCCM 更新案例 – 您必須允許輸入的流量裝置通道中，因此無法用於流量篩選器。  如果裝置通道設定檔中您開啟流量篩選器，裝置通道拒絕輸入的流量。  這項限制前往未來版本中移除。


### 範例 VPN profileXML

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

根據每個特定部署案例的需求，另一個可以使用裝置通道設定的 VPN 功能是[受信任的網路偵測](https://social.technet.microsoft.com/wiki/contents/articles/38546.new-features-for-vpn-in-windows-10-and-windows-server-2016.aspx#Trusted_Network_Detection)。

```
 <!-- inside/outside detection --> 
  <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection> 
```

## 部署和測試

您可以使用 Windows PowerShell 指令碼，並使用 Windows Management Instrumentation \(WMI\) 橋接器設定裝置通道。 Always On VPN 裝置通道必須設定的**本機系統**帳戶內容中。 若要這樣做，您必須使用[PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec)， [PsTools](https://docs.microsoft.com/sysinternals/downloads/pstools)的公用程式[Sysinternals](https://docs.microsoft.com/sysinternals/)套件中包含的其中一個。

如需如何部署指導方針每個裝置`(.\Device)`與每個使用者`(.\User)`設定檔，請參閱[使用 PowerShell 指令碼搭配 WMI 橋接器提供者](https://docs.microsoft.com/windows/client-management/mdm/using-powershell-scripting-with-the-wmi-bridge-provider)。 

執行下列 Windows PowerShell 命令，以確認您已成功部署的裝置設定檔：

    `Get-VpnConnection -AllUserConnection`

輸出會顯示在裝置上部署的全 device\ 的 VPN 設定檔清單。


### 範例 Windows PowerShell 指令碼

您可以使用下列 Windows PowerShell 指令碼來協助建立您自己的設定檔建立指令碼。

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

## 其他資源

以下是其他資源來協助您的 VPN 部署。

### VPN 用戶端設定資源

這些是 VPN 用戶端設定資源。

- [如何建立 VPN 設定檔中 System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles)
- [設定 Windows 10 用戶端 Always On VPN 連線](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [VPN 設定檔選項](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)

### 遠端存取伺服器 \(RAS\) 閘道資源

下列是 RAS 閘道的資源。

- [使用電腦驗證憑證設定 RRAS](https://technet.microsoft.com/library/dd458982.aspx)
- [疑難排解 IKEv2 VPN 連線](https://technet.microsoft.com/library/dd941612.aspx)
- [設定 IKEv2 型遠端存取](https://technet.microsoft.com/library/ff687731.aspx)

>[!IMPORTANT]
>當使用裝置通道與 Microsoft RAS 閘道，您將需要設定 RRAS 伺服器所述方式啟用**IKEv2 允許電腦憑證驗證**的驗證方法支援 IKEv2 電腦憑證驗證[以下](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee922682%28v=ws.10%29)。 一旦啟用這項設定，強烈建議**組 VpnAuthProtocol** PowerShell cmdlet，以及**RootCertificateNameToAccept**選擇性參數，用來確保 RRAS IKEv2 連線，只允許適用於VPN 用戶端鏈結來明確定義內部/私密金鑰根憑證授權單位的憑證。 或者，以確保它不包含公開憑證授權單位如下所討論[這裡](https://blogs.technet.microsoft.com/rrasblog/2009/06/10/what-type-of-certificate-to-install-on-the-vpn-server/)應該要修改 RRAS 伺服器上**受信任的根憑證授權單位**存放區。 類似的方法可能也需要考慮其他 VPN 閘道。

---
