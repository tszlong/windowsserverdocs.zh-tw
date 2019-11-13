---
title: 設定資料中心防火牆存取控制清單 (ACL)
description: 您可以將特定的 Acl 套用到網路介面。  如果在網路介面所連線的虛擬子網上也設定 Acl，則會同時套用這兩個 Acl，但網路介面 Acl 的優先順序高於虛擬子網 Acl。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 25f18927-a63e-44f3-b02a-81ed51933187
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 2e3f365a820de67dec87ea209cbfeb22d9091616
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406108"
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>設定資料中心防火牆存取控制清單（Acl）

>適用於：Windows Server (半年通道)、Windows Server 2016

建立 ACL 並將它指派給虛擬子網之後，您可能會想要使用個別網路介面的特定 ACL，覆寫虛擬子網上的預設 ACL。  在此情況下，您會將特定的 Acl 直接套用到連結至 Vlan 的網路介面，而不是虛擬網路。 如果您已在連線到網路介面的虛擬子網上設定 Acl，則會套用這兩個 Acl，並將虛擬子網 Acl 之上的網路介面 Acl 排定優先順序。

>[!IMPORTANT]
>如果您尚未建立 ACL，並將它指派給虛擬網路，請參閱[使用存取控制清單（acl）來管理資料中心網路流量](Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md)，以建立 acl 並將它指派給虛擬子網。  

在本主題中，我們將示範如何將 ACL 新增至網路介面。 我們也會示範如何使用 Windows PowerShell 和網路控制卡 REST API 從網路介面移除 ACL。

- [範例：將 ACL 新增至網路介面](#example-add-an-acl-to-a-network-interface)
- [範例：使用 Windows Powershell 和網路控制卡，從網路介面移除 ACL REST API](#example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api)


## <a name="example-add-an-acl-to-a-network-interface"></a>範例：將 ACL 新增至網路介面
在此範例中，我們會示範如何將 ACL 新增至虛擬網路。 

>[!TIP]
>您也可以在建立網路介面時，同時新增 ACL。

1. 取得或建立您將在其中新增 ACL 的網路介面。
 
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. 取得或建立您將新增至網路介面的 ACL。
 
   ```PowerShell
   $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"
   ```
 
3. 將 ACL 指派給網路介面的 AccessControlList 屬性
 
   ```PowerShell
    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl
   ```
 
4. 在網路控制卡中新增網路介面
 
   ```
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
 
## <a name="example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api"></a>範例：使用 Windows Powershell 和網路控制卡，從網路介面移除 ACL REST API
在此範例中，我們會示範如何移除 ACL。 移除 ACL 會將一組預設的規則套用至網路介面。 預設的規則集允許所有的輸出流量，但會封鎖所有輸入流量。

>[!NOTE]
>如果您想要允許所有輸入流量，您必須遵循先前的[範例](#example-add-an-acl-to-a-network-interface)來新增允許所有輸入和所有輸出流量的 ACL。


1. 取得您將從中移除 ACL 的網路介面。<br>
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. 將 $Null 指派給 ipConfiguration 的 AccessControlList 屬性。<br>
   ```PowerShell
   $nic.properties.ipconfigurations[0].properties.AccessControlList = $null
   ```
 
3. 在網路控制卡中新增網路介面物件。<br>
   ```PowerShell
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
