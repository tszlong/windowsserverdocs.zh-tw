---
title: Windows Server 2019 閘道效能
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: a6530b29ce7ffb0d18e0266e70cb2ca45188915c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845629"
---
# <a name="windows-server-2019-gateway-performance"></a>Windows Server 2019 閘道效能

>適用於：Windows Server


在 Windows Server 2016 中，客戶考量之一會是 SDN 閘道無法符合輸送量需求的最新的網路。 IPsec 與 GRE 通道的網路輸送量會有與正在大約 300 Mbps 的 IPsec 連線和 GRE 連線正在約 2.5 Gbps 的單一連線輸送量的限制。

使用分別 soaring 1.8 Gbps 和 IPsec 與 GRE 連線，為 15 Gbps 的數字的在 Windows Server 2019，我們已大幅改善。 這一切，搭配 CPU 循環中 / 每個位元組，進而提供較少的 CPU 使用率的 ultra 高效能輸送量大幅縮減。

## <a name="enable-high-performance-with-gateways-in-windows-server-2019"></a>啟用高效能與在 Windows Server 2019 的閘道

針對**GRE 連線**，一旦您部署/升級到 Windows Server 2019 閘道 Vm 上的組建，您應該會自動看到改善的效能。 不涉及任何手動步驟。

針對**IPsec 連線**，根據預設，當您建立連線時為您的虛擬網路，您取得 Windows Server 2016 資料路徑和效能數字。 若要啟用 Windows Server 2019 資料路徑，請執行下列作業：

   1. 在 SDN 閘道 VM，請移至**Services**主控台 (services.msc)。
   2. 尋找名為的服務**Azure 閘道服務**，並將啟動類型設定為**自動**。
   3. 重新啟動閘道 VM。
      在此的閘道容錯移轉至備援的閘道 VM 上使用中的連線。
   4. 針對其餘的閘道 Vm 重複先前的步驟。

>[!TIP]
>為了獲得最佳的效能結果，請確認 cipherTransformationConstant 和快速模式設定的 IPsec 連接使用中的 authenticationTransformConstant **GCMAES256**加密套件。
>
>為達最佳效能，閘道主機硬體必須支援 AES NI 和 PCLMULQDQ CPU 指令集。 這些是適用於任何 Westmere (32nm) 和除了更新版本的 Intel CPU，其中已停用 AES NI 的模型。 您可以查看硬體廠商的文件的 CPU 是否支援 AES NI 和 PCLMULQDQ CPU 指令設定。

以下是最佳的安全性演算法與 IPsec 連線的 REST 範例：

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

我們已經廣泛測試我們的測試實驗室中的 SDN 閘道的效能。 在測試中，我們有比較與 Windows Server 2019 在 SDN 案例和非 SDN 案例中的閘道網路效能。 您可以找到結果和測試部落格文章中所擷取的設定詳細資料[此處](https://blogs.technet.microsoft.com/networking/2018/08/15/high-performance-gateways/)。

---