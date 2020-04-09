---
title: 設定虛擬網路對等互連
description: 設定虛擬網路對等互連牽涉到建立兩個可取得對等互連的虛擬網路。
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/08/2018
ms.openlocfilehash: ede13fd47c32b2d75ec71ad7c7bf7eb50c269c82
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853561"
---
# <a name="configure-virtual-network-peering"></a>設定虛擬網路對等互連

>適用于： Windows Server

在此程式中，您會使用 Windows PowerShell 來建立兩個虛擬網路，每個都有一個子網。 然後，您可以設定兩個虛擬網路之間的對等互連，以啟用兩者之間的連線。

- [步驟1。建立第一個虛擬網路](#step-1-create-the-first-virtual-network)

- [步驟2。建立第二個虛擬網路](#step-2-create-the-second-virtual-network)

- [步驟3。設定從第一個虛擬網路到第二個虛擬網路的對等互連](#step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network)

- [步驟4。設定從第二個虛擬網路到第一個虛擬網路的對等互連](#step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network)


>[!IMPORTANT]
>請記得更新您環境的屬性。

## <a name="step-1-create-the-first-virtual-network"></a>步驟 1. 建立第一個虛擬網路

在此步驟中，您會使用 Windows PowerShell 尋找 HNV 提供者邏輯網路，以建立具有一個子網的第一個虛擬網路。 下列範例腳本會建立具有一個子網的 Contoso 虛擬網路。

``` PowerShell
#Find the HNV Provider Logical Network  

$logicalnetworks = Get-NetworkControllerLogicalNetwork -ConnectionUri $uri  
foreach ($ln in $logicalnetworks) {  
   if ($ln.Properties.NetworkVirtualizationEnabled -eq "True") {  
      $HNVProviderLogicalNetwork = $ln  
   }  
}   

#Create the Virtual Subnet  

$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Contoso"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AddressPrefix = "24.30.1.0/24"
$uri=”https://restserver”  

#Create the Virtual Network  

$vnetproperties = new-object Microsoft.Windows.NetworkController.VirtualNetworkProperties  
$vnetproperties.AddressSpace = new-object Microsoft.Windows.NetworkController.AddressSpace  
$vnetproperties.AddressSpace.AddressPrefixes = @("24.30.1.0/24")  
$vnetproperties.LogicalNetwork = $HNVProviderLogicalNetwork  
$vnetproperties.Subnets = @($vsubnet)  
New-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri -Properties $vnetproperties
```

## <a name="step-2-create-the-second-virtual-network"></a>步驟 2. 建立第二個虛擬網路

在此步驟中，您會建立具有一個子網的第二個虛擬網路。 下列範例腳本會建立具有一個子網的 Woodgrove 虛擬網路。

``` PowerShell

#Create the Virtual Subnet  

$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Woodgrove"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AddressPrefix = "24.30.2.0/24"  
$uri=”https://restserver”

#Create the Virtual Network  

$vnetproperties = new-object Microsoft.Windows.NetworkController.VirtualNetworkProperties  
$vnetproperties.AddressSpace = new-object Microsoft.Windows.NetworkController.AddressSpace  
$vnetproperties.AddressSpace.AddressPrefixes = @("24.30.2.0/24")  
$vnetproperties.LogicalNetwork = $HNVProviderLogicalNetwork  
$vnetproperties.Subnets = @($vsubnet)  
New-NetworkControllerVirtualNetwork -ResourceId "Woodgrove_VNet1" -ConnectionUri $uri -Properties $vnetproperties
```

## <a name="step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network"></a>步驟 3： 設定從第一個虛擬網路到第二個虛擬網路的對等互連

在此步驟中，您會設定第一個虛擬網路與您在前兩個步驟中建立的第二個虛擬網路之間的對等互連。 下列範例腳本會建立從**Contoso_vnet1**到**Woodgrove_vnet1**的虛擬網路對等互連。

```PowerShell
$peeringProperties = New-Object Microsoft.Windows.NetworkController.VirtualNetworkPeeringProperties
$vnet2 = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Woodgrove_VNet1"
$peeringProperties.remoteVirtualNetwork = $vnet2

#Indicate whether communication between the two virtual networks
$peeringProperties.allowVirtualnetworkAccess = $true

#Indicates whether forwarded traffic is allowed across the vnets
$peeringProperties.allowForwardedTraffic = $true

#Indicates whether the peer virtual network can access this virtual networks gateway
$peeringProperties.allowGatewayTransit = $false

#Indicates whether this virtual network uses peer virtual networks gateway
$peeringProperties.useRemoteGateways =$false

New-NetworkControllerVirtualNetworkPeering -ConnectionUri $uri -VirtualNetworkId “Contoso_vnet1” -ResourceId “ContosotoWoodgrove” -Properties $peeringProperties

```

>[!IMPORTANT]
>建立此對等互連之後，vnet 狀態會顯示 [已**起始**]。

## <a name="step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network"></a>步驟 4. 設定從第二個虛擬網路到第一個虛擬網路的對等互連

在此步驟中，您會設定第二個虛擬網路與您在上述步驟1和2中建立的第一個虛擬網路之間的對等互連。 下列範例腳本會建立從**Woodgrove_vnet1**到**Contoso_vnet1**的虛擬網路對等互連。

```PowerShell
$peeringProperties = New-Object Microsoft.Windows.NetworkController.VirtualNetworkPeeringProperties 
$vnet2=Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Contoso_VNet1"
$peeringProperties.remoteVirtualNetwork = $vnet2 

# Indicates whether communication between the two virtual networks is allowed 
$peeringProperties.allowVirtualnetworkAccess = $true 

# Indicates whether forwarded traffic will be allowed across the vnets
$peeringProperties.allowForwardedTraffic = $true 

# Indicates whether the peer virtual network can access this virtual network's gateway
$peeringProperties.allowGatewayTransit = $false 

# Indicates whether this virtual network will use peer virtual network's gateway
$peeringProperties.useRemoteGateways =$false 

New-NetworkControllerVirtualNetworkPeering -ConnectionUri $uri -VirtualNetworkId “Woodgrove_vnet1” -ResourceId “WoodgrovetoContoso” -Properties $peeringProperties 

```

建立此對等互連之後，vnet 對等互連狀態會顯示兩個對等的 [**已連線**]。 現在，一個虛擬網路中的虛擬機器可以與對等互連虛擬網路中的虛擬機器進行通訊。

---