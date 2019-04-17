---
title: 容器連接到 Virtual 網路
description: 本主題是輔助的軟體定義網路上如何管理承租人工作負載和 Windows Server 2016 Virtual 網路的一部分。
manager: ravirao
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f7af1eb6-d035-4f74-a25b-d4b7e4ea9329
ms.author: pashort
author: jmesser81
ms.openlocfilehash: 801cf4b8f71935eb72d820d47e523a310fa64562
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>連接到承租人 virtual 網路的容器端點

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題您顯示如何在連接的容器端點建立透過 Microsoft 軟體定義網路 (SDN) 堆疊現有承租人 virtual 網路。 我們將使用*l2bridge* (也可以*l2tunnel*) 的 Docker 建立容器網路上的容器主機（承租人）一樣的 Windows libnetwork 增益集可用的網路驅動程式。

如中所述[容器網路](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/management/container_networking)多個網路驅動程式都可以透過 windows Docker msdn 主題。 最適合 SDN 的驅動程式是*l2bridge*和*l2tunnel*。 這兩個驅動程式，每個容器端點會在與容器主機（承租人）一樣相同 virtual 子網路中。 IP 位址的容器端點會動態指派來主機網路服務 (HNS) 透過私人雲端增益集。 容器端點具有獨特的 IP 位址，但分享的容器主機（承租人）一樣層級 2 位址轉譯因為相同的 MAC 地址。 網路原則 (例如：Acl、封裝，並 QoS) 這些容器端點會執行實體 HYPER-V 主機時收到 Network Controller 的及上層管理系統所述。 有會稍微不同*l2bridge*和*l2tunnel*下面會解釋驅動程式。

- **L2 橋接器**-上相同的容器主機一樣，並在相同的子網路中的容器端點能所有的網路流量橋接 HYPER-V virtual 開關切換至中。 容器端點的 Vm 的不同的容器主機或哪些是不同的子網路中有他們流量轉寄給實體 HYPER-V 主機。 因為容器主機上並在相同的子網路中間網路流量執行流向實體主機，會不執行任何網路原則。 跨主機或跨子網路容器網路流量僅會套用原則。  
 
- **L2 通道** - *所有*兩個容器端點間網路流量轉寄給實體 HYPER-V 主機無論主機或子網路。 跨子網路和跨主機的網路流量被執行網路原則。   

>[!NOTE]
>這些網路功能模式連接 windows 容器端點承租人 virtual 網路 Azure 公用雲端中無法運作

## <a name="prerequistes"></a>Prerequistes
 * 使用網路控制器 SDN 基礎結構部署
 * 已建立承租人 virtual 網路
 * Windows 容器功能、與安裝 Docker HYPER-V 功能部署承租人一樣

>[!Note]
>[巢的模擬](https://msdn.microsoft.com/en-us/virtualization/hyperv_on_windows/user_guide/nesting)和公開模擬延伸模組並不需要使用 HYPER-V 容器 HyperV 功能不需要安裝數個二進位檔 l2bridge 和 l2tunnel 網路

```powershell
# To install HyperV feature without checks for nested virtualization
dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All 
```

 

## <a name="workflow"></a>工作流程

1. [加入現有的 VM NIC 資源 Network Controller 透過多個 IP 設定](#Add)（HYPER-V 主機）
2. [讓上的容器端點配置 CA IP 位址主機的網路 proxy](#Enable)（HYPER-V 主機） 
3. [安裝私人雲端插件，以將 CA IP 位址指派給容器端點](#Install)(容器主機 VM) 
4. [建立*l2bridge*或*l2tunnel*網路使用 docker](#Create) (容器主機 VM) 
 
>[!NOTE]
>多個 IP 設定不支援透過 System Center 一樣管理員建立 VM NIC 資源。 建議的類部署建立 VM NIC 退出 band 使用網路控制器 PowerShell 資源。

### <a name="Add"></a>1.新增多個 IP 設定

針對此範例，我們假設 VM 而承租人一樣的已經有一 IP 設定的 IP 位址的 192.168.1.9 和 192.168.1.0 24 IP 子網路中的 'VNet1' VNet 資源 ID 和 VM 子網路資源的 'Subnet1' 已連接。 我們會將 192.168.1.101-192.168.1.110 10 容器 IP 位址。

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

### <a name="Enable"></a>2.可讓網路 Proxy

[ConfigureMCNP.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1>)

此指令碼執行**HYPER-V 主機**，裝載容器主機（承租人）一樣，以便網路 proxy 配置多個 IP 位址的容器主機一樣。

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="Install"></a>3.安裝外掛程式的私人雲端

[InstallPrivateCloudPlugin.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1)

執行中的指令碼**容器主機（承租人）一樣**允許主機網路服務 (HNS) 以與網路 proxy HYPER-V 主機上。

```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="Create"></a>4。建立*l2bridge*容器網路 4。

在**容器主機（承租人）一樣**使用`docker network create`以建立 l2bridge 網路的命令

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork 
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>靜態 IP 指派不支援的*l2bridge*或*l2tunnel*容器網路時搭配 Microsoft SDN 堆疊。

## <a name="more-information"></a>更多的資訊
如需有關部署 SDN 基礎結構 infortation，[部署軟體定義網路基礎架構](https://technet.microsoft.com/en-us/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure)。

