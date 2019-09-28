---
title: 使用虛擬網路上的網路虛擬設備
description: 在本主題中，您將瞭解如何在租使用者虛擬網路上部署網路虛擬裝置。 您可以將網路虛擬裝置新增至執行使用者定義路由和埠鏡像功能的網路。
manager: dougkim
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.prod: windows-server
ms.technology: networking-sdn
ms.assetid: 3c361575-1050-46f4-ac94-fa42102f83c1
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: 158183bab74e6e45c36c579f3259fc2095a939b5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406051"
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>使用虛擬網路上的網路虛擬設備

>適用於：Windows Server (半年度管道)、Windows Server 2016

在本主題中，您將瞭解如何在租使用者虛擬網路上部署網路虛擬裝置。 您可以將網路虛擬裝置新增至執行使用者定義路由和埠鏡像功能的網路。

## <a name="types-of-network-virtual-appliances"></a>網路虛擬裝置的類型

您可以使用這兩種虛擬裝置類型的其中一種：

1. **使用者定義的路由**-以虛擬裝置的路由功能取代虛擬網路上的分散式路由器。  使用使用者定義的路由，虛擬裝置會當做虛擬網路上虛擬子網之間的路由器使用。

2. **埠鏡像**-所有進入或離開受監視埠的網路流量都會複製並傳送至虛擬裝置進行分析。 


## <a name="deploying-a-network-virtual-appliance"></a>部署網路虛擬裝置

若要部署網路虛擬裝置，您必須先建立包含設備的 VM，然後將 VM 連線至適當的虛擬網路子網。 如需詳細資訊，請參閱[建立租使用者 VM 並聯機至租使用者虛擬網路或 VLAN](Create-a-Tenant-VM.md)。

某些設備需要多個虛擬網路介面卡。 通常會有一張網路介面卡專用於裝置管理，而其他介面卡則會處理流量。  如果您的設備需要多個網路介面卡，您必須在網路控制卡中建立每個網路介面。 您也必須為每個位於不同虛擬子網的其他介面卡，在每部主機上指派介面識別碼。

部署網路虛擬裝置之後，您可以使用設備來定義路由、移植鏡像或兩者。 


## <a name="example-user-defined-routing"></a>範例：使用者定義的路由

在大部分的環境中，您只需要已由虛擬網路的分散式路由器定義的系統路由。 不過，您可能需要建立路由表，並在特定案例中新增一或多個路由，例如：

- 透過您的內部部署網路強制通道傳送至網際網路。
- 在您的環境中使用虛擬應用裝置。

在這些情況下，您必須建立路由表，並將使用者定義的路由新增至資料表。 您可以有多個路由表，而且可以將相同的路由表與一或多個子網建立關聯。 您只能將每個子網與單一路由表產生關聯。 子網中的所有 Vm 都會使用與子網相關聯的路由表。

子網會依賴系統路由，直到路由表與子網相關聯為止。 關聯存在之後，就會根據使用者定義的路由和系統路由中最長的前置詞比對（LPM）來完成路由。 如果有多個路由具有相同的 LPM 相符項，則會先選取使用者定義的路由（在系統路由之前）。
 
**步**

1. 建立路由表屬性，其中包含所有使用者定義的路由。<p>系統路由仍會根據上面定義的規則套用。

   ```PowerShell
    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties
   ```

2. 將路由新增至路由表屬性。<p>任何目的地為 12.0.0.0/8 子網的路由，會在192.168.1.10 路由傳送至虛擬裝置。 設備必須將虛擬網路介面卡連接到虛擬網路，並將該 IP 指派給網路介面。

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

3. 將路由表新增至網路控制卡。

   ```PowerShell
    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties
   ```

4. 將路由表套用至虛擬子網。<p>當您將路由表套用至虛擬子網時，Tenant1_Vnet1 網路中的第一個虛擬子網會使用路由表。 您可以視需要將路由表指派給虛擬網路中的多個子網。

   ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid
   ```

將路由表套用至虛擬網路之後，流量就會轉送到虛擬應用裝置。 您必須將虛擬裝置中的路由表設定為以適合您環境的方式轉送流量。

## <a name="example-port-mirroring"></a>範例：埠鏡像

在此範例中，您會設定 MyVM_Ethernet1 至鏡像 Appliance_Ethernet1 的流量。  我們假設您已部署兩個 Vm，一個做為應用裝置，另一個作為要以鏡像監視的 VM。 

設備必須有第二個網路介面來進行管理。 在 Appliciance_Ethernet1 上啟用鏡像做為目的地之後，它就不會再接收到目的地設定之 IP 介面的流量。


**步**

1. 取得 Vm 所在的虛擬網路。

   ```PowerShell
   $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
   ```

2. 取得鏡像來源和目的地的網路控制站網路介面。

   ```PowerShell
   $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
   $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```

3. 建立 serviceinsertionproperties 物件以包含埠鏡像規則，以及代表目的地介面的元素。

   ```PowerShell
   $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
   $portMirror.Priority = 1
   ```

4. 建立 serviceinsertionrules 物件，以包含必須符合才能將流量傳送到設備的規則。<p>以下定義的規則會比對代表傳統鏡像的所有流量，包括輸入和輸出。  如果您想要鏡像特定的埠或特定的來源/目的地，您可以調整這些規則。

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

5. 建立 serviceinsertionelements 物件以包含鏡像設備的網路介面。

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

完成這些步驟之後，Appliance_Ethernet1 介面會反映來自 MyVM_Ethernet1 介面的流量。
 
---