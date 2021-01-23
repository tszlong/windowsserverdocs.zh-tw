---
title: 使用虛擬網路上的網路虛擬設備
description: 在本主題中，您將瞭解如何在租使用者虛擬網路上部署網路虛擬裝置。 您可以將網路虛擬裝置新增至執行使用者定義的路由和埠鏡像功能的網路。
manager: grcusanz
ms.topic: article
ms.assetid: 3c361575-1050-46f4-ac94-fa42102f83c1
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/30/2018
ms.openlocfilehash: fc263ea778209da699cc7c78fd6111f512760951
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716894"
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>使用虛擬網路上的網路虛擬設備

>適用於：Windows Server 2019、Windows Server 2016

在本主題中，您將瞭解如何在租使用者虛擬網路上部署網路虛擬裝置。 您可以將網路虛擬裝置新增至執行使用者定義的路由和埠鏡像功能的網路。

## <a name="types-of-network-virtual-appliances"></a>網路虛擬裝置的類型

您可以使用這兩種類型的虛擬裝置之一：

1. **使用者定義的路由** -將虛擬網路上的分散式路由器取代為虛擬裝置的路由功能。  使用使用者定義的路由，虛擬裝置可作為虛擬網路上虛擬子網之間的路由器。

2. **埠鏡像** -進入或離開受監視埠的所有網路流量都會複製並傳送至虛擬裝置以進行分析。


## <a name="deploying-a-network-virtual-appliance"></a>部署網路虛擬裝置

若要部署網路虛擬裝置，您必須先建立包含設備的 VM，然後將 VM 連線到適當的虛擬網路子網。 如需詳細資訊，請參閱 [建立租使用者 VM 並聯機至租使用者虛擬網路或 VLAN](Create-a-Tenant-VM.md)。

某些設備需要多個虛擬網路介面卡。 通常，一個專門用於裝置管理的網路介面卡，而額外的介面卡會處理流量。  如果您的設備需要多張網路介面卡，您必須在網路控制站中建立每個網路介面。 您也必須針對不同虛擬子網上的每個額外介面卡，在每部主機上指派介面識別碼。

部署網路虛擬裝置之後，您可以使用設備來定義路由、移植鏡像或兩者。


## <a name="example-user-defined-routing"></a>範例：使用者定義的路由

在大部分的環境中，您只需要虛擬網路的分散式路由器已定義的系統路由。 不過，在特定情況下，您可能需要建立路由表並新增一或多個路由，例如：

- 透過內部部署網路到網際網路的強制通道。
- 在您的環境中使用虛擬應用裝置。

在這些情況下，您必須建立路由表，並將使用者定義的路由加入至資料表。 您可以有多個路由表，而且您可以將相同的路由表與一或多個子網產生關聯。 您只能將每個子網與單一路由表建立關聯。 子網中的所有 Vm 都會使用與子網相關聯的路由表。

子網會依賴系統路由，直到路由表與子網產生關聯。 關聯存在之後，就會根據最長的前置詞比對，在使用者定義的路由和系統路由之間 (LPM) 進行路由。 如果有多個符合相同 LPM 的路由，則會先選取使用者定義的路由，再選取系統路由。

**程式：**

1. 建立路由表屬性，其中包含所有使用者定義的路由。<p>系統路由仍會根據上述定義的規則套用。

   ```PowerShell
    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties
   ```

2. 將路由新增至路由表屬性。<p>目的地為 12.0.0.0/8 子網的任何路由都會路由傳送至位於192.168.1.10 的虛擬裝置。 設備必須有連接至虛擬網路的虛擬網路介面卡，並將該 IP 指派給網路介面。

   ```PowerShell
    $route = new-object Microsoft.Windows.NetworkController.Route
    $route.ResourceID = "0_0_0_0_0"
    $route.properties = new-object Microsoft.Windows.NetworkController.RouteProperties
    $route.properties.AddressPrefix = "0.0.0.0/0"
    $route.properties.nextHopType = "VirtualAppliance"
    $route.properties.nextHopIpAddress = "192.168.1.10"
    $routetableproperties.routes += $route
   ```
   >[!TIP]
   >如果您想要新增更多路由，請針對您要定義的每個路由重複此步驟。

3. 將路由表新增至網路控制站。

   ```PowerShell
    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties
   ```

4. 將路由表套用至虛擬子網。<p>當您將路由表套用至虛擬子網時，Tenant1_Vnet1 網路中的第一個虛擬子網會使用路由表。 您可以依照您的需要，將路由表指派給虛擬網路中的多個子網。

   ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid
   ```

一旦您將路由表套用至虛擬網路，就會將流量轉送到虛擬應用裝置。 您必須在虛擬裝置中設定路由表，以將流量轉送到適合您環境的方式。

## <a name="example-port-mirroring"></a>範例：埠鏡像

在此範例中，您會設定 MyVM_Ethernet1 到鏡像 Appliance_Ethernet1 的流量。  我們假設您已部署兩個 Vm，一個做為設備，另一個作為要使用鏡像監視的 VM。

設備必須有第二個網路介面可進行管理。 當您在 Appliciance_Ethernet1 上啟用鏡像作為目的地之後，它就不會再收到目的地設定 IP 介面的流量。


**程式：**

1. 取得您的 Vm 所在的虛擬網路。

   ```PowerShell
   $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
   ```

2. 取得鏡像來源和目的地的網路控制站網路介面。

   ```PowerShell
   $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
   $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```

3. 建立 serviceinsertionproperties 物件，以包含埠鏡像規則和代表目的介面的元素。

   ```PowerShell
   $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
   $portMirror.Priority = 1
   ```

4. 建立 serviceinsertionrules 物件，以包含必須符合才能將流量傳送到設備的規則。<p>以下定義的規則符合代表傳統鏡像的所有流量，包括輸入和輸出。  如果您想要鏡像特定埠或特定來源/目的地，可以調整這些規則。

   ```PowerShell
   $portmirror.ServiceInsertionRules = [Microsoft.Windows.NetworkController.ServiceInsertionRule[]]::new(1)

   $portmirror.ServiceInsertionRules[0] = [Microsoft.Windows.NetworkController.ServiceInsertionRule]::new()
   $portmirror.ServiceInsertionRules[0].ResourceId = "Rule1"
   $portmirror.ServiceInsertionRules[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionRuleProperties]::new()

   $portmirror.ServiceInsertionRules[0].Properties.Description = "Port Mirror Rule"
   $portmirror.ServiceInsertionRules[0].Properties.Protocol = "All"
   $portmirror.ServiceInsertionRules[0].Properties.SourcePortRangeStart = "0"
   $portmirror.ServiceInsertionRules[0].Properties.SourcePortRangeEnd = "65535"
   $portmirror.ServiceInsertionRules[0].Properties.DestinationPortRangeStart = "0"
   $portmirror.ServiceInsertionRules[0].Properties.DestinationPortRangeEnd = "65535"
   $portmirror.ServiceInsertionRules[0].Properties.SourceSubnets = "*"
   $portmirror.ServiceInsertionRules[0].Properties.DestinationSubnets = "*"
   ```

5. 建立 serviceinsertionelements 物件，以包含鏡像設備的網路介面。

   ```PowerShell
   $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

   $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
   $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
   $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

   $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
   $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
   $portmirror.ServiceInsertionElements[0].Properties.Order = 1
   ```

6. 在網路控制卡中新增服務插入物件。<p>當您發出此命令時，在上一個步驟中指定的設備網路介面的所有流量都會停止。

   ```PowerShell
   $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"
   ```

7. 更新要鏡像之來源的網路介面。

   ```PowerShell
   $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
   $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId
   ```

完成這些步驟之後，Appliance_Ethernet1 介面會從 MyVM_Ethernet1 介面鏡像流量。

---