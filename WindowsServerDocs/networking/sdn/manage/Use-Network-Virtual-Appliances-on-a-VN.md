---
title: 使用網路 Virtual 設備 Virtual 網路
description: 本主題是輔助的軟體定義網路上如何管理承租人工作負載和 Windows Server 2016 Virtual 網路的一部分。
manager: brianlic
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
ms.openlocfilehash: db46189931263d230f013431f319eb2497589dee
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>使用網路 Virtual 設備 Virtual 網路

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解如何部署承租人 Virtual 網路上的網路 virtual 裝置。

您可以新增網路 virtual 裝置執行使用者定義路由並連接埠鏡像函式的網路。

本主題包含下列各節。

- [網路 Virtual 裝置的類型](#bkmk_types)
- [部署的網路 Virtual 應用裝置](#bkmk_deploy)
- [範例： 使用者定義路由](#bkmk_routing)
- [範例： 連接埠鏡像](#bkmk_port)

## <a name="bkmk_types"></a>網路 Virtual 裝置的類型

有兩種類型的 virtual，您可以使用 virtual 網路上的裝置：

1. **使用者定義路由**。 使用者定義路由分散式的路由器 virtual 取代路由 virtual 應用裝置的功能。  使用使用者定義路由 virtual 應用裝置做為路由器之間 virtual 網路上的 virtual 子網路。
2. **連接埠鏡像**。 鏡像連接埠，輸入或離開受監視的連接埠所有網路流量是重複與傳送到分析 virtual 應用裝置。 
## <a name="bkmk_deploy"></a>部署的網路 Virtual 應用裝置

若要部署的 virtual 應用裝置，您必須先建立一樣 (VM) 包含應用裝置，，，然後將 VM 連接到適當的 virtual 網路子網路。

>[!NOTE]
>如需詳細資訊，請查看[建立承租人 VM 和承租人 Virtual 網路或 VLAN 連接](Create-a-Tenant-VM.md)

某些裝置需要多重 virtual 網路介面卡。 通常是一個網路介面卡專用應用裝置管理時處理流量使用其他的顯示卡。 

如果應用程式裝置需要多個網路介面卡，您必須建立每個網路介面 Network Controller 中。 

您也必須指派介面 ID 每個其他的顯示卡不同 virtual 子網路上的每個主機上。

您已完成網路 virtual 應用裝置部署之後，您可以使用應用裝置的使用者定義路由、 連接埠鏡像或兩者。

##<a name="bkmk_routing"></a>範例： 使用者定義路由

針對大部分的環境，您將只需要已經所定義之 virtual 網路分散式路由器系統路徑。 不過，您可能需要建立之前的路徑表並將一或多個路徑新增特定萬一，例如：

* 推動網際網路透過在場所網路的通道。
* 在您的環境中 virtual 裝置使用。

這些案例中，您必須建立之前的路徑表，並加入表格中的使用者定義路徑。 您有多個之前的路徑表格，相同路由表可以相關聯的一或多個子網路。 

每個子網路只能相關單一路由資料表。 子網路中的所有 Vm 都使用子網路相關聯的之前的路徑資料表。

子網路依賴之前的路徑表格子網路中相關系統路徑。 關聯存在之後，路由完成根據上最長前置詞相符項目 (LPM) 之間使用者定義路徑和系統路徑。 

如果有多個路由相同 LPM 相符項目，然後使用者定義的路由是第一次-前選取系統之前的路徑。 

###<a name="step-1-create-the-route-table-properties"></a>步驟 1： 建立路由表格屬性

本表路由會包含所有的使用者定義路徑。  系統路徑仍會套用根據定義上方的規則。

您可以使用下列命令範例建立之前的路徑表格屬性。

    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties

###<a name="step-2-add-a-route-to-the-route-table-properties"></a>步驟 2： 新增路由之前的路徑表格屬性

這顯示，取得傳送任何流量 12.0.0.0/8 子網路，在 192.168.1.10 路由傳送 virtual 應用裝置。  請務必應用裝置已連接到 virtual 網路的 ip 指派給網路介面 virtual 網路介面卡。

您可以使用下列命令範例路由加入之前的路徑表格屬性。

    $route = new-object Microsoft.Windows.NetworkController.Route
    $route.ResourceID = "0_0_0_0_0"
    $route.properties = new-object Microsoft.Windows.NetworkController.RouteProperties
    $route.properties.AddressPrefix = "0.0.0.0/0"
    $route.properties.nextHopType = "VirtualAppliance"
    $route.properties.nextHopIpAddress = "192.168.1.10"
    $routetableproperties.routes += $route

針對每個您想要定義的路徑重複此步驟，您可以新增額外的路徑。
s
###<a name="step-3-add-the-route-table-to-network-controller"></a>步驟 3： 新增 Network Controller 路由表
您可以使用下列命令範例加入 Network Controller 路由表。

    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties

###<a name="step-4-apply-the-route-table-to-the-virtual-subnet"></a>步驟 4： 適用於路由表 virtual 子網路
 
當您將路由表套用到 virtual 子網路時，Tenant1_Vnet1 網路中的第一個 virtual 子網路使用路由表。 您可以指定路由表最多的子網路中 virtual 網路您想要。

將之前的路徑表套用到 virtual 子網路，您可以使用下列命令範例。

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid

在您之前的路徑表套用到 virtual 網路，資料傳輸轉寄給 virtual 應用裝置。 您必須設定路由表 virtual 應用裝置轉送流量，方式是適用於您的環境中。

##<a name="bkmk_port"></a>範例： 連接埠鏡像

此範例中，可讓您設定 MyVM_Ethernet1 的資料傳輸到 Appliance_Ethernet1 鏡像流量。

假設您已經已經部署兩個 Vm 一個為應用裝置，使用鏡像監視 VM 為一個。

請務必應用裝置，鏡像可以為上 Appliance_Ethernet1 目的地之後，它不會收到流量 IP 介面有的設定，因為有第二個網路介面管理。

###<a name="step-1-get-the-virtual-network-on-which-your-vms-are-located"></a>步驟 1： 取得您的 Vm 位於 virtual 網路
若要取得 virtual 網路，您可以使用下列命令的範例。

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"

###<a name="step-2-get-the-network-controller-network-interfaces-for-the-mirroring-source-and-destination"></a>步驟 2： 鏡像來源和目的地取得 Network Controller 的網路介面
您可以使用下列命令範例取得 Network Controller 的網路介面鏡像來源和目的地資訊。

    $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
    $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-3-create-a-serviceinsertionproperties-object-to-contain-the-port-mirroring-rules-and-the-element-which-represents-the-destination-interface"></a>步驟 3： 建立包含連接埠規則及的項目，表示介面目的地鏡像 serviceinsertionproperties 物件
您可以使用下列命令範例建立目的地 serviceinsertionproperties 物件。

    $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
    $portMirror.Priority = 1

###<a name="step-4-create-a-serviceinsertionrules-object-to-contain-the-rules-that-must-be-matched-in-order-for-the-traffic-to-be-sent-to-the-appliance"></a>步驟 4： 建立包含必須符合傳送到應用裝置流量順序規則 serviceinsertionrules 物件

下列符合所有流量，同時輸入 / 輸出，表示傳統鏡像都定義規則。  如果您感興趣鏡像特定的連接埠或特定的來源目的地，您可以調整本規則。

您可以使用下列命令範例建立 serviceinsertionproperties 物件。

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

###<a name="step-5-create-a-serviceinsertionelements-object-to-contain-the-network-interface-of-the-appliance-you-are-mirroring-to"></a>步驟 5： 建立 serviceinsertionelements 物件包含您要的鏡像應用裝置的網路介面
您可以使用下列命令範例建立網路介面 serviceinsertionelements 物件。

    $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

    $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
    $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
    $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

    $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
    $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
    $portmirror.ServiceInsertionElements[0].Properties.Order = 1

###<a name="step-6-add-the-service-insertion-object-in-network-controller"></a>步驟 6: Network Controller 中新增服務插入物件
當您發出這個命令時，將會停止所有的資料傳輸到上一個步驟中所指定應用裝置網路介面。

您可以使用下列命令範例 Network Controller 中新增服務插入物件。

    $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"

###<a name="step-7-update-the-network-interface-of-the-source-to-be-mirrored"></a>步驟 7： 更新的來源鏡像的網路介面
更新網路介面，您可以使用下列命令範例。

    $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
    $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId

當您完成下列步驟執行時，從 MyVM_Ethernet1 介面流量被鏡像 Appliance_Ethernet1 介面。
 
