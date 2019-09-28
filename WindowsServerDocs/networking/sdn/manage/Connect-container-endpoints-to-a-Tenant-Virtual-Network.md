---
title: 將容器端點連線至租用戶虛擬網路
description: 在本主題中，我們將示範如何將容器端點連線至透過 SDN 建立的現有租使用者虛擬網路。 您可以使用適用于 Docker 的 Windows libnetwork 外掛程式所提供的 l2bridge （並選擇性 l2tunnel）網路驅動程式，在租使用者 VM 上建立容器網路。
manager: ravirao
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f7af1eb6-d035-4f74-a25b-d4b7e4ea9329
ms.author: pashort
author: jmesser81
ms.date: 08/24/2018
ms.openlocfilehash: 83996f7ffb82d01c9f36945efa022f0dd0b9825b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355815"
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>將容器端點連線至租用戶虛擬網路

>適用於：Windows Server (半年度管道)、Windows Server 2016

在本主題中，我們將示範如何將容器端點連線至透過 SDN 建立的現有租使用者虛擬網路。 您可以使用適用于 Docker 的 Windows libnetwork 外掛程式所提供的*l2bridge* （並選擇性*l2tunnel*）網路驅動程式，在租使用者 VM 上建立容器網路。

在[容器網路驅動程式](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/network-drivers-topologies)主題中，我們討論了透過 Windows 上的 Docker 提供的多個網路驅動程式。 針對 SDN，請使用*l2bridge*和*l2tunnel*驅動程式。 對於這兩個驅動程式，每個容器端點都位於與容器主機（租使用者）虛擬機器相同的虛擬子網中。 

主機網路服務（HNS）透過私用雲端外掛程式，以動態方式指派容器端點的 IP 位址。 容器端點具有唯一的 IP 位址，但會因為第2層位址轉譯而共用容器主機（租使用者）虛擬機器的相同 MAC 位址。 

這些容器端點的網路原則（Acl、封裝和 QoS）會在網路控制站所接收並定義于上層管理系統中的實體 Hyper-v 主機中強制執行。 

*L2bridge*和*l2tunnel*驅動程式之間的差異如下：


|                                                                                                                                                                                                                                                                            l2bridge                                                                                                                                                                                                                                                                            |                                                                                                 l2tunnel                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 位於的容器端點： <ul><li>相同的容器主機虛擬機器和相同的子網上，所有的網路流量都會在 Hyper-v 虛擬交換器中橋接。 </li><li>不同的容器主機 Vm 或不同的子網，其流量會轉送至實體 Hyper-v 主機。 </li></ul>網路原則不會強制執行，因為相同主機和相同子網中容器間的網路流量不會流向實體主機。 網路原則只適用于跨主機或跨子網容器網路流量。 | 不論主機或子網為何，兩個容器端點之間的*所有*網路流量都會轉送至實體 hyper-v 主機。 網路原則同時適用于跨子網和跨主機網路流量。 |

---

>[!NOTE]
>這些網路模式無法用於將 windows 容器端點連線至 Azure 公用雲端中的租使用者虛擬網路。


## <a name="prerequisites"></a>必要條件
-  具有網路控制卡的已部署 SDN 基礎結構。
-  已建立租使用者虛擬網路。
-  已部署的租使用者虛擬機器，且已啟用 Windows 容器功能、已安裝 Docker 和 Hyper-v 功能。 安裝 l2bridge 和 l2tunnel 網路的數個二進位檔時，需要 Hyper-v 功能。

   ```powershell
   # To install HyperV feature without checks for nested virtualization
   dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All 
   ```

>[!Note]
>除非使用 Hyper-v 容器，否則不需要[嵌套虛擬化](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/nesting)和公開虛擬化延伸模組。 


## <a name="workflow"></a>工作流程

[1.透過網路控制站（Hyper-v 主機） ](#1-add-multiple-ip-configurations) @ no__t-1 @ no__t-22，將多個 IP 設定新增至現有的 VM NIC 資源。啟用主機上的網路 proxy，以配置容器端點（Hyper-v 主機）的 CA IP 位址，](#2-enable-the-network-proxy) @ no__t-1 @ no__t-23。安裝私人雲端外掛程式，將 CA IP 位址指派給容器端點（容器主機 VM） ](#3-install-the-private-cloud-plug-in) @ no__t-1 @ no__t-24。使用 docker 建立*l2bridge*或*L2tunnel*網路（容器主機 VM） ](#4-create-an-l2bridge-container-network)

>[!NOTE]
>透過 System Center Virtual Machine Manager 建立的 VM NIC 資源不支援多個 IP 設定。 針對這些部署類型，建議您使用網路控制站 PowerShell 來建立頻外的 VM NIC 資源。

### <a name="1-add-multiple-ip-configurations"></a>1.新增多個 IP 設定
在此步驟中，我們假設租使用者虛擬機器的 VM NIC 有一個 IP 設定，其 IP 位址為192.168.1.9，且已連結至 192.168.1.0/24 IP 子網中 ' Subnet1 ' 的 VNet 資源識別碼 ' VNet1 ' 和 VM 子網資源。 我們會從192.168.1.101 新增10個容器的 IP 位址-192.168.1.110。

```powershell
Import-Module NetworkController

# Specify Network Controller REST IP or FQDN
$uri = "<NC REST IP or FQDN>"
$vnetResourceId = "VNet1"
$vsubnetResourceId = "Subnet1"

$vmnic= Get-NetworkControllerNetworkInterface -ConnectionUri $uri | where {$_.properties.IpConfigurations.Properties.PrivateIPAddress -eq "192.168.1.9" }
$vmsubnet = Get-NetworkControllerVirtualSubnet -VirtualNetworkId $vnetResourceId -ResourceId $vsubnetResourceId -ConnectionUri $uri

# For this demo, we will assume an ACL has already been defined; any ACL can be applied here
$allowallacl = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"


foreach ($i in 1..10)
{
    $newipconfig = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
    $props = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties

    $resourceid = "IP_192_168_1_1"
    if ($i -eq 10) 
    {
        $resourceid += "10"
        $ipstr = "192.168.1.110"
    }
    else
    {
        $resourceid += "0$i"
        $ipstr = "192.168.1.10$i"
    }

    $newipconfig.ResourceId = $resourceid
    $props.PrivateIPAddress = $ipstr    

    $props.PrivateIPAllocationMethod = "Static"
    $props.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $props.Subnet.ResourceRef = $vmsubnet.ResourceRef
    $props.AccessControlList = new-object Microsoft.Windows.NetworkController.AccessControlList
    $props.AccessControlList.ResourceRef = $allowallacl.ResourceRef

    $newipconfig.Properties = $props
    $vmnic.Properties.IpConfigurations += $newipconfig
}

New-NetworkControllerNetworkInterface -ResourceId $vmnic.ResourceId -Properties $vmnic.Properties -ConnectionUri $uri
```

### <a name="2-enable-the-network-proxy"></a>2.啟用網路 proxy
在此步驟中，您會啟用網路 proxy，為容器主機虛擬機器配置多個 IP 位址。 

若要啟用網路 proxy，請在裝載容器主機（租使用者）虛擬機器的**Hyper-v 主機**上執行[ConfigureMCNP](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1)腳本。

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="3-install-the-private-cloud-plug-in"></a>3.安裝私用雲端外掛程式
在此步驟中，您會安裝外掛程式，讓 HNS 能夠與 Hyper-v 主機上的網路 proxy 進行通訊。

若要安裝外掛程式，請在**容器主機（租使用者）虛擬機器**內執行[InstallPrivateCloudPlugin](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1)腳本。


```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="4-create-an-l2bridge-container-network"></a>4.建立*l2bridge*容器網路
在此步驟中，您會在**容器主機（租使用者）虛擬機器**上使用 `docker network create` 命令，以建立 l2bridge 網路。 

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork 
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>搭配 Microsoft SDN 堆疊使用時， *l2bridge*或*l2tunnel*容器網路不支援靜態 IP 指派。

## <a name="more-information"></a>詳細資訊
如需部署 SDN 基礎結構的詳細資訊，請參閱[部署軟體定義的網路基礎結構](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure)。

