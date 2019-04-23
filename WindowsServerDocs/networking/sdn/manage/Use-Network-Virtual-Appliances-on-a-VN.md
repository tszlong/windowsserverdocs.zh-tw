---
title: 使用虛擬網路上的網路虛擬設備
description: 本主題中，您已了解如何部署租用戶虛擬網路上的網路虛擬設備。 您可以新增網路虛擬設備來執行使用者定義路由和連接埠鏡像函式的網路。
manager: dougkim
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.assetid: 3c361575-1050-46f4-ac94-fa42102f83c1
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: e715a782651a5b9867f3b45251fd6ea6e4a9e4f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847369"
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>使用虛擬網路上的網路虛擬設備

>適用於：Windows Server （半年通道），Windows Server 2016

本主題中，您已了解如何部署租用戶虛擬網路上的網路虛擬設備。 您可以新增網路虛擬設備來執行使用者定義路由和連接埠鏡像函式的網路。

## <a name="types-of-network-virtual-appliances"></a>類型的網路虛擬設備

您可以使用其中一個虛擬設備的兩種類型：

1. **使用者定義路由**-虛擬網路上的分散式的路由器取代為虛擬設備的路由功能。  使用者定義路由中，虛擬應用裝置取得作為虛擬網路上的虛擬子網路之間的路由器。

2. **連接埠鏡像**-所有的網路流量進入或離開受監視的連接埠會複製並傳送至虛擬設備進行分析。 


## <a name="deploying-a-network-virtual-appliance"></a>部署網路虛擬設備

若要部署網路虛擬設備，您必須先建立 包含設備，VM，然後將 VM 連接至適當的虛擬網路子網路。 如需詳細資訊，請參閱 <<c0> [ 建立租用戶 VM 及連線到租用戶虛擬網路或 VLAN](Create-a-Tenant-VM.md)。

某些裝置需要多個虛擬網路介面卡。 通常，一張網路介面卡專用於設備管理而其他配接器會處理流量。  如果您的應用裝置需要多個網路介面卡，您必須建立網路控制卡中的每個網路介面。 您也必須指派每個額外的介面卡不同的虛擬子網路上的每個主機上的介面識別碼。

一旦您部署網路虛擬設備，您可以使用的設備定義的路由、 移植的鏡像，或兩者。 


## <a name="example-user-defined-routing"></a>範例：使用者定義的路由

對於大部分的環境，您只需要虛擬網路的分散式路由器已定義的系統路由。 不過，您可能需要建立路由表，並在特定情況下，新增一或多個路由，例如：

- 強制通道網際網路透過您的內部部署網路。
- 使用您的環境中的虛擬應用裝置。

針對這些案例中，您必須建立路由表，並將使用者定義的路由新增至資料表。 您可以有多個路由表，以及您可以將相同的路由表，以一個或多個子網路產生關聯。 您只可以關聯到單一的路由表的每個子網路。 子網路中的所有 Vm 都使用的子網路相關聯的路由表。

子網路會依賴系統路由，直到路由表取得子網路產生關聯。 關聯存在之後，路由是根據最長首碼比對 (LPM) 使用者定義的路由和系統路由之間。 如果有多個符合相同 LPM 的路由，則使用者定義的路由之前就已選取 first-系統路由。
 
**程序：**

1. 建立路由資料表屬性，其中包含所有使用者定義的路由。<p>根據以上定義的規則仍然適用的系統路由。

   ```PowerShell
    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties
   ```

2. 新增路由到路由表內容。<p>針對 12.0.0.0/8 子網路的任何路由路由傳送至虛擬設備在 192.168.1.10。 設備必須附加至虛擬網路指派給網路介面的 IP 的虛擬網路介面卡。

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
   >如果您想要新增更多的路由，請針對每個您想要定義的路由中重複此步驟。

3. 將路由表新增至網路控制卡中。

   ```PowerShell
    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties
   ```

4. 適用於虛擬子網路的路由表。<p>當您將路由表套用至的虛擬子網路時，Tenant1_Vnet1 網路中的第一個虛擬子網路會使用路由表。 視需要，您可以將路由表指派給最多的虛擬網路中的子網路中。

   ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid
   ```

當您將路由表套用至虛擬網路時，流量取得轉送至虛擬設備。 您必須在虛擬設備轉送流量，以適合您環境的方式來設定路由表。

## <a name="example-port-mirroring"></a>範例：連接埠鏡像

在此範例中，您設定流量的 MyVM_Ethernet1 鏡像 Appliance_Ethernet1。  我們假設您已部署兩個 Vm，另一個做為設備，而另一個則為要使用鏡像監視的 VM。 

設備必須管理的第二個網路介面。 啟用鏡像做為目的地 Appliciance_Ethernet1 上之後，它不會再接收那里設定的 IP 介面的流量。


**程序：**

1. 取得您的 Vm 位於虛擬網路。

   ```PowerShell
   $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
   ```

2. 取得鏡像來源和目的地網路控制卡網路介面。

   ```PowerShell
   $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
   $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```

3. 建立包含連接埠鏡像的規則和項目代表目的地介面 serviceinsertionproperties 物件。

   ```PowerShell
   $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
   $portMirror.Priority = 1
   ```

4. 建立 serviceinsertionrules 物件包含為了讓流量傳送到設備必須符合的規則。<p>規則會定義下列比對所有流量，輸入和輸出，其代表傳統的鏡像。  如果您想要在鏡像中特定的連接埠或特定的來源/目的地，您可以調整這些規則。

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

5. 建立 serviceinsertionelements 物件來包含鏡像的應用裝置的網路介面。

   ```PowerShell
   $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

   $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
   $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
   $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

   $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
   $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
   $portmirror.ServiceInsertionElements[0].Properties.Order = 1
   ```

6. 將服務插入物件加入網路控制卡中。<p>當您發出此命令時，應用裝置的所有流量的都網路中前一個步驟就會停止指定的介面。

   ```PowerShell
   $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"
   ```

7. 更新來源，以進行鏡像的網路介面。

   ```PowerShell
   $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
   $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId
   ```

完成這些步驟之後，Appliance_Ethernet1 介面會反映來自 MyVM_Ethernet1 介面的流量。
 
---