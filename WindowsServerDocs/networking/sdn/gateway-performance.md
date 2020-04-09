---
title: Windows Server 2019 閘道效能
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/22/2018
ms.openlocfilehash: 34890a5d93d6e2e214e401f5566cbb0ffde37508
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855701"
---
# <a name="windows-server-2019-gateway-performance"></a>Windows Server 2019 閘道效能

>適用于： Windows Server


在 Windows Server 2016 中，客戶關心的其中一個問題是，SDN 閘道無法符合新式網路的輸送量需求。 IPsec 和 GRE 通道的網路輸送量具有單一連線輸送量的限制，IPsec 連線大約是 300 Mbps，而 GRE 連線則是 2.5 Gbps。

我們已大幅改善 Windows Server 2019，分別 soaring 為 1.8 Gbps 的數位，以及適用于 IPsec 和 GRE 連線的 15 Gbps。 如此一來，每個位元組就能大幅減少 CPU 週期，進而提供具有較少 CPU 使用率的超高效能輸送量。

## <a name="enable-high-performance-with-gateways-in-windows-server-2019"></a>在 Windows Server 2019 中使用閘道啟用高效能

針對**GRE**連線，一旦您部署/升級至閘道 vm 上的 Windows Server 2019 之後，您應該會自動看到改善的效能。 不涉及任何手動步驟。

針對**IPsec**連線，根據預設，當您建立虛擬網路的連線時，您會取得 Windows Server 2016 資料路徑和效能數位。 若要啟用 Windows Server 2019 資料路徑，請執行下列動作：

   1. 在 SDN 閘道 VM 上，移至 [**服務**] [主控台] （services.msc）。
   2. 尋找名為 [ **Azure 閘道服務**] 的服務，並將 [啟動類型] 設定為 [**自動**]。
   3. 重新開機閘道 VM。
      此閘道上的作用中連接會容錯移轉至多餘的閘道 VM。
   4. 針對其餘的閘道 Vm 重複上述步驟。

>[!TIP]
>為了獲得最佳效能結果，請確定 IPsec 連線的快速配置設定中的 cipherTransformationConstant 和 authenticationTransformConstant 使用**GCMAES256**加密套件。
>
>為了達到最大效能，閘道主機硬體必須支援 AES-NI 和 PCLMULQDQ CPU 指令集。 這些在任何 Westmere （32nm）和更新版本的 Intel CPU 上都有提供，但已停用 AES-NI 的模型除外。 您可以查看硬體廠商的檔，以查看 CPU 是否支援 AES-NI 和 PCLMULQDQ CPU 指令集。

以下是具有最佳安全性演算法的 IPsec 連線的 REST 範例：

```PowerShell
# NOTE: The virtual gateway must be created before creating the IPsec connection. More details here.
# Create a new object for Tenant Network IPsec Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   

# Update the common object properties  
$nwConnectionProperties.ConnectionType = "IPSec"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 2000000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 2000000  

# Update specific properties depending on the Connection Type  
$nwConnectionProperties.IpSecConfiguration = New-Object Microsoft.Windows.NetworkController.IpSecConfiguration   
$nwConnectionProperties.IpSecConfiguration.AuthenticationMethod = "PSK"   
$nwConnectionProperties.IpSecConfiguration.SharedSecret = "111_aaa"   

$nwConnectionProperties.IpSecConfiguration.QuickMode = New-Object Microsoft.Windows.NetworkController.QuickMode   
$nwConnectionProperties.IpSecConfiguration.QuickMode.PerfectForwardSecrecy = "PFS2048"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.AuthenticationTransformationConstant = "GCMAES256"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.CipherTransformationConstant = "GCMAES256"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeSeconds = 3600   
$nwConnectionProperties.IpSecConfiguration.QuickMode.IdleDisconnectSeconds = 500   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeKiloBytes = 2000   

$nwConnectionProperties.IpSecConfiguration.MainMode = New-Object Microsoft.Windows.NetworkController.MainMode   
$nwConnectionProperties.IpSecConfiguration.MainMode.DiffieHellmanGroup = "Group2"   
$nwConnectionProperties.IpSecConfiguration.MainMode.IntegrityAlgorithm = "SHA256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.EncryptionAlgorithm = "AES256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeSeconds = 28800
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeKiloBytes = 2000   

# L3 specific configuration (leave blank for IPSec)  
$nwConnectionProperties.IPAddresses = @()   
$nwConnectionProperties.PeerIPAddresses = @()   

# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "<<On premise subnet that must be reachable over the VPN tunnel. Ex: 10.0.0.0/24>>"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   

# Tunnel Destination (Remote Endpoint) Address  
$nwConnectionProperties.DestinationIPAddress = "<<Public IP address of the On-Premise VPN gateway. Ex: 192.168.3.4>>"   

# Add the new Network Connection for the tenant. Note that the virtual gateway must be created before creating the IPsec connection. $uri is the REST URI of your deployment and must be in the form of “https://<REST URI>”  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_IPSecGW" -Properties $nwConnectionProperties -Force
```

## <a name="testing-results"></a>測試結果

我們已對測試實驗室中的 SDN 閘道進行大量的效能測試。 在測試中，我們已比較 SDN 案例和非 SDN 案例中的閘道網路效能與 Windows Server 2019。 您可以在[這裡](https://blogs.technet.microsoft.com/networking/2018/08/15/high-performance-gateways/)的 blog 文章中找到所捕獲的結果和測試設定詳細資料。

---