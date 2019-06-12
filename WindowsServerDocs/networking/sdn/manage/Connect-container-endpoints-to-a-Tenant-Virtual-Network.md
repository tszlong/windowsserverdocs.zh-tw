---
title: 將容器端點連線至租用戶虛擬網路
description: 本主題中，我們示範您如何將容器端點連線至 SDN 透過所建立的現有租用戶虛擬網路。 您可以使用 l2 橋接 （及選擇性地 l2tunnel） 適用於 Windows libnetwork 外掛程式來建立容器網路上的租用戶 VM 的 docker 網路驅動程式。
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
ms.date: 08/24/2018
ms.openlocfilehash: cb9c7157ffb07233e41e1c933f6775f1cd0766a9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446356"
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>將容器端點連線至租用戶虛擬網路

>適用於：Windows Server （半年通道），Windows Server 2016

本主題中，我們示範您如何將容器端點連線至 SDN 透過所建立的現有租用戶虛擬網路。 您使用*l2 橋接*(並選擇性地*l2tunnel*) 適用於 Windows libnetwork 外掛程式來建立容器網路上的租用戶 VM 的 docker 網路驅動程式。

在 [容器的網路驅動程式](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/network-drivers-topologies)我們所討論的主題中，多個網路驅動程式都是透過在 Windows 上的 Docker。 適用於 SDN，使用*l2 橋接*並*l2tunnel*驅動程式。 這兩個驅動程式，每個容器端點會是容器主機 （租用戶） 的虛擬機器位於相同虛擬子網路中。 

主機網路服務 (HNS)，透過私用雲端外掛程式，以動態方式將容器端點的 IP 位址。 容器端點會有唯一的 IP 位址，但共用相同的 MAC 位址的容器主機 （租用戶） 的虛擬機器，因為第 2 層位址轉譯。 

這些容器端點 （Acl、 封裝，以及 QoS） 的網路原則是所接收的網路控制站，在實體 HYPER-V 主機中強制執行，且定義在上層管理系統。 

之間的差異*l2 橋接*並*l2tunnel*驅動程式：


|                                                                                                                                                                                                                                                                            l2bridge                                                                                                                                                                                                                                                                            |                                                                                                 l2tunnel                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 容器上的端點： <ul><li>相同的容器裝載的虛擬機器，並在相同的子網路上已在 HYPER-V 虛擬交換器橋接所有的網路流量。 </li><li>不同的容器裝載虛擬機器，或在不同的子網路上有其轉送到實體 HYPER-V 主機的流量。 </li></ul>因為在相同主機上和在相同的子網路中的容器之間的網路流量不會流動至實體主機，不會不取得強制執行網路原則。 網路原則適用於僅為跨主應用程式或跨子網路的容器的網路流量。 | *所有*兩個容器端點之間的網路流量轉送到實體的 HYPER-V 主機，無論主機或子網路。 網路原則套用到跨子網路和跨主應用程式網路流量。 |

---

>[!NOTE]
>這些網路模式不適用於 Azure 公用雲端中的租用戶虛擬網路連接的 windows 容器端點。


## <a name="prerequisites"></a>先決條件
-  與網路控制卡部署的 SDN 基礎結構。
-  已建立租用戶虛擬網路。
-  Docker 安裝，以及啟用 HYPER-V 功能啟用了 Windows 容器功能的部署的租用戶虛擬機器。 HYPER-V 功能，才能安裝 l2 橋接和 l2tunnel 網路的數個二進位檔。

   ```powershell
   # To install HyperV feature without checks for nested virtualization
   dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All 
   ```

>[!Note]
>[巢狀虛擬化](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/nesting)公開虛擬化延伸模組則不需要除非使用 HYPER-V 容器。 


## <a name="workflow"></a>工作流程

[1.將多個 IP 組態新增至現有 VM NIC 資源透過網路控制站 （HYPER-V 主機）](#1-add-multiple-ip-configurations)
[2。啟用要配置的容器端點 （HYPER-V 主機） 的 CA IP 位址的主機上的網路 proxy](#2-enable-the-network-proxy)
[3。安裝外掛程式，以便在將 CA IP 位址指派給容器端點 (容器主機 VM) 的私人雲端](#3-install-the-private-cloud-plug-in)
[4。建立*l2 橋接*或是*l2tunnel*使用 docker (容器主機 VM) 網路](#4-create-an-l2bridge-container-network)

>[!NOTE]
>透過 System Center Virtual Machine Manager 所建立的 VM NIC 資源不支援多個 IP 組態。 建議您使用這些部署類型，您會建立超出範圍使用網路控制站 PowerShell 的 VM NIC 資源。

### <a name="1-add-multiple-ip-configurations"></a>1.新增多個 IP 組態
在此步驟中，我們假設 VM NIC 的租用戶虛擬機器有一個 IP 組態與 IP 位址的 192.168.1.9 和 192.168.1.0/24 IP 子網路中附加至 'VNet1' VNet 資源識別碼和 「 Subnet1 」 的 VM 子網路資源。 我們會新增適用於容器的 10 個 IP 位址 192.168.1.101-從 192.168.1.110。

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

### <a name="2-enable-the-network-proxy"></a>2.啟用的網路 proxy
在此步驟中，您可以啟用為容器主機的虛擬機器配置多個 IP 位址的網路 proxy。 

若要啟用的網路 proxy，請執行[ConfigureMCNP.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1)上的指令碼**HYPER-V 主機**裝載容器主機 （租用戶） 的虛擬機器。

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="3-install-the-private-cloud-plug-in"></a>3.安裝外掛程式的私用雲端
在此步驟中，您安裝外掛程式，以允許與 HYPER-V 主機上的網路 proxy 通訊 HNS 項目。

若要安裝外掛程式，請執行[InstallPrivateCloudPlugin.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1)指令碼內**容器主機 （租用戶） 虛擬機器**。


```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="4-create-an-l2bridge-container-network"></a>4.建立*l2 橋接*容器網路
在此步驟中，您可以使用`docker network create`命令**容器主機 （租用戶） 虛擬機器**建立 l2 橋接網路。 

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork 
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>不支援靜態 IP 指派*l2 橋接*或是*l2tunnel*容器網路時搭配 Microsoft SDN 堆疊。

## <a name="more-information"></a>詳細資訊
如需進一步瞭解部署 SDN 基礎結構的詳細資訊，請參閱[部署軟體定義網路基礎結構](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure)。

