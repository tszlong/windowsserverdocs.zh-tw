---
title: 設定虛擬網路對等互連
description: 設定虛擬網路對等互連，牽涉到建立取得對等互連的兩個虛擬網路。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: 3ef3db879080e3372e7b287dcc55ae052c1fe109
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816389"
---
# <a name="configure-virtual-network-peering"></a>設定虛擬網路對等互連

>適用於：Windows Server

在此程序中，您可以使用 Windows PowerShell 來建立兩個虛擬網路，每個都有一個子網路。 然後，您會設定它們之間啟用連線的兩個虛擬網路之間對等互連。

- [步驟 1。建立第一個虛擬網路](#step-1-create-the-first-virtual-network)

- [步驟 2。建立第二個虛擬網路](#step-2-create-the-second-virtual-network)

- [步驟 3。設定從第一個虛擬網路到第二個虛擬網路對等互連](#step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network)

- [步驟 4。設定從第二個虛擬網路到第一個虛擬網路對等互連](#step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network)


>[!IMPORTANT]
>請記得更新您的環境的屬性。

## <a name="step-1-create-the-first-virtual-network"></a>步驟 1. 建立第一個虛擬網路

在此步驟中，您使用 Windows PowerShell 尋找 HNV 提供者邏輯網路具有一個子網路中建立第一個虛擬網路。 下列範例指令碼會建立 Contoso 虛擬網路具有一個子網路。

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

在此步驟中，您可以建立第二個虛擬網路具有一個子網路。 下列範例指令碼會建立 Woodgrove 的虛擬網路，具有一個子網路。

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

## <a name="step-3-configure-peering-from-the-first-virtual-network-to-the-second-virtual-network"></a>步驟 3。 設定從第一個虛擬網路到第二個虛擬網路對等互連

在此步驟中，您可以設定第一個虛擬網路與您在前兩個步驟中建立第二個虛擬網路之間對等互連。 下列範例指令碼會建立從虛擬網路對等互連**Contoso_vnet1**要**Woodgrove_vnet1**。

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
>Vnet 狀態會顯示建立此對等互連之後,**初始化**。

## <a name="step-4-configure-peering-from-the-second-virtual-network-to-the-first-virtual-network"></a>步驟 4. 設定從第二個虛擬網路到第一個虛擬網路對等互連

在此步驟中，您可以設定第二個虛擬網路與您在步驟 1 和 2 上述步驟中建立第一個虛擬網路之間對等互連。 下列範例指令碼會建立從虛擬網路對等互連**Woodgrove_vnet1**要**Contoso_vnet1**。

```PowerShell
$peeringProperties = New-Object Microsoft.Windows.NetworkController.VirtualNetworkPeeringProperties 
$vnet2=Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Contoso_VNet1"
$peeringProperties.remoteVirtualNetwork = $vnet2 

# Indicates whether communication between the two virtual networks is allowed 
$peeringProperties.allowVirtualnetworkAccess = $true 

# Indicates whether forwarded traffic will be allowed across the vnets
$peeringProperties.allowForwardedTraffic = $true 

# Indicates whether the peer virtual network can access this virtual network’s gateway
$peeringProperties.allowGatewayTransit = $false 

# Indicates whether this virtual network will use peer virtual network’s gateway
$peeringProperties.useRemoteGateways =$false 

New-NetworkControllerVirtualNetworkPeering -ConnectionUri $uri -VirtualNetworkId “Woodgrove_vnet1” -ResourceId “WoodgrovetoContoso” -Properties $peeringProperties 

```

建立此對等互連，vnet 對等互連狀態之後顯示**Connected**這兩個對等電腦。 現在，一個虛擬網路中的虛擬機器可以與對等互連的虛擬網路中的虛擬機器通訊。

---