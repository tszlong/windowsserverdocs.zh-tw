---
title: 將容器端點連線至租用戶虛擬網路
description: 在本主題中，我們會示範如何將容器端點連線到透過 SDN 建立的現有租使用者虛擬網路。 您可以使用 l2bridge (，並選擇性地 l2tunnel 適用于 Docker 的 Windows libnetwork 外掛程式所提供的) 網路驅動程式，以在租使用者 VM 上建立容器網路。
manager: grcusanz
ms.topic: article
ms.assetid: f7af1eb6-d035-4f74-a25b-d4b7e4ea9329
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/24/2018
ms.openlocfilehash: a09d3f7002ffd0bb73d39a57caba4e497eafd17a
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716804"
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>將容器端點連線至租用戶虛擬網路

>適用於：Windows Server 2019、Windows Server 2016

在本主題中，我們會示範如何將容器端點連線到透過 SDN 建立的現有租使用者虛擬網路。 您可以使用 *l2bridge* (，並選擇性地 *L2tunnel* 適用于 Docker 的 Windows libnetwork 外掛程式所提供的) 網路驅動程式，以在租使用者 VM 上建立容器網路。

在 [容器網路驅動程式](/virtualization/windowscontainers/container-networking/network-drivers-topologies) 主題中，我們討論了多個網路驅動程式可透過 Windows 上的 Docker 取得。 若為 SDN，請使用 *l2bridge* 和 *l2tunnel* 驅動程式。 針對這兩個驅動程式，每個容器端點都位於與容器主機相同的虛擬子網中， (租使用者) 虛擬機器。

主機網路服務 (HNS) 透過私人雲端外掛程式，可動態指派容器端點的 IP 位址。 容器端點具有唯一的 IP 位址，但因為第2層位址轉譯，所以會將相同的容器主機 MAC 位址 (租使用者) 虛擬機器。

系統會在實體 Hyper-v 主機中強制執行這些容器端點的網路原則 (Acl、封裝和 QoS) ，其會在網路控制站接收的實體 Hyper-v 主機中強制執行，並定義于上層管理系統中。

*L2bridge* 與 *l2tunnel* 驅動程式之間的差異如下：


|                                                                                                                                                                                                                                                                            l2bridge                                                                                                                                                                                                                                                                            |                                                                                                 l2tunnel                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 容器端點位於： <ul><li>相同的容器主機虛擬機器和相同的子網中的所有網路流量都在 Hyper-v 虛擬交換器內橋接。 </li><li>不同的容器主機 Vm 或不同的子網會將其流量轉送至實體 Hyper-v 主機。 </li></ul>因為相同主機和相同子網中容器之間的網路流量不會流向實體主機，所以不會強制執行網路原則。 網路原則僅適用于跨主機或跨子網容器網路流量。 | 無論主機或子網為何，在兩個容器端點之間的 *所有* 網路流量都會轉送到實體 hyper-v 主機。 網路原則同時適用于跨子網和跨主機網路流量。 |

---

>[!NOTE]
>這些網路功能模式不適用於將 windows 容器端點連接至 Azure 公用雲端中的租使用者虛擬網路。


## <a name="prerequisites"></a>Prerequisites
-  具有網路控制站的已部署 SDN 基礎結構。
-  已建立租使用者虛擬網路。
-  已啟用 Windows 容器功能、已安裝 Docker 和 Hyper-v 功能的已部署租使用者虛擬機器。 需要 Hyper-v 功能才能為 l2bridge 和 l2tunnel 網路安裝多個二進位檔。

   ```powershell
   # To install HyperV feature without checks for nested virtualization
   dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All
   ```

>[!Note]
>除非使用 Hyper-v 容器，否則不需要[嵌套的虛擬化](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization)和公開虛擬化延伸模組。


## <a name="workflow"></a>工作流程

[1. 透過網路控制站，將多個 IP 設定新增至現有的 VM NIC 資源， (Hyper-v 主機) ](#1-add-multiple-ip-configurations) 
 [2。在主機上啟用網路 proxy，以針對 (hyper-v 主機) 3 的容器端點配置 CA IP 位址](#2-enable-the-network-proxy) 
 [。安裝私人雲端外掛程式，以將 CA IP 位址指派給容器主機 VM 的容器端點 () ](#3-install-the-private-cloud-plug-in) 
 [4。使用 docker (容器主機 VM 來建立 *l2bridge* 或 *l2tunnel* 網路) ](#4-create-an-l2bridge-container-network)

>[!NOTE]
>透過 System Center Virtual Machine Manager 建立的 VM NIC 資源不支援多個 IP 設定。 建議您針對這些部署類型，使用網路控制站 PowerShell 來建立頻外的 VM NIC 資源。

### <a name="1-add-multiple-ip-configurations"></a>1. 新增多個 IP 設定
在此步驟中，我們假設租使用者虛擬機器的 VM NIC 有一個 IP 設定的 ip 位址為192.168.1.9，並且已連接至 192.168.1.0/24 IP 子網中 ' VNet1 ' 的 VNet 資源識別碼和 ' Subnet1 ' 的 VM 子網資源。 我們會從 192.168.1.101-192.168.1.110 新增容器的10個 IP 位址。

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

### <a name="2-enable-the-network-proxy"></a>2. 啟用網路 proxy
在此步驟中，您會啟用網路 proxy 以配置容器主機虛擬機器的多個 IP 位址。

若要啟用網路 proxy，請在裝載容器主機 (租使用者) 虛擬機器的 **Hyper-v 主機** 上執行 [ConfigureMCNP.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1)腳本。

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="3-install-the-private-cloud-plug-in"></a>3. 安裝私用雲端外掛程式
在此步驟中，您會安裝外掛程式，以允許 HNS 與 Hyper-v 主機上的網路 proxy 進行通訊。

若要安裝外掛程式，請在 **容器主機 (租使用者) 虛擬機器** 中執行 [InstallPrivateCloudPlugin.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1)腳本。


```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="4-create-an-l2bridge-container-network"></a>4. 建立 *l2bridge* 容器網路
在此步驟中，您會使用 `docker network create` 容器主機上的命令 **(租使用者) 虛擬機器** 來建立 l2bridge 網路。

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>搭配 Microsoft SDN 堆疊使用 *l2bridge* 或 *l2tunnel* 容器網路時，不支援靜態 IP 指派。

## <a name="more-information"></a>詳細資訊
如需部署 SDN 基礎結構的詳細資訊，請參閱 [部署軟體定義的網路基礎結構](../deploy/deploy-a-software-defined-network-infrastructure.md)。