---
title: 設定資料中心防火牆存取控制清單 (ACL)
description: 您可以將特定的 Acl 套用至網路介面。  如果 Acl 也會設定虛擬網路介面所連接的子網路上，同時套用 Acl，但網路介面的 Acl 會列為優先順序，高於 Acl 的虛擬子網路。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 25f18927-a63e-44f3-b02a-81ed51933187
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 77a7706e39da265eedd65342a0ccf2174ab050ea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853399"
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>設定存取控制清單 (Acl) 的資料中心防火牆

>適用於：Windows Server （半年通道），Windows Server 2016

一旦您已建立 ACL 並將它指派給虛擬子網路，您可能想要覆寫個別網路介面的特定 ACL 的虛擬子網路上的預設 ACL。  在此情況下，您特定的 Acl 直接套用到連接到 Vlan，而不是虛擬網路的網路介面。 如果您有連線到網路介面的虛擬子網路上設定的 Acl，這兩種 Acl 會套用，並排定優先順序的網路介面上的虛擬子網路 Acl 的 Acl。

>[!IMPORTANT]
>如果您未建立 ACL 並指派至虛擬網路，請參閱[使用存取控制清單 (Acl) 來管理資料中心網路流量流動](Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md)建立 ACL，並將它指派給虛擬子網路。  

在本主題中，我們告訴您如何將 ACL 新增到網路介面。 我們也示範如何從使用 Windows PowerShell 和網路控制站的 REST API 的網路介面移除 ACL。

- [範例：將 ACL 新增到網路介面](#example-add-an-acl-to-a-network-interface)
- [範例：網路介面移除 ACL，請使用 Windows Powershell 和網路控制站的 REST API](#example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api)


## <a name="example-add-an-acl-to-a-network-interface"></a>範例：將 ACL 新增到網路介面
在此範例中，我們會示範如何將 ACL 新增到虛擬網路。 

>[!TIP]
>它也可建立網路介面的同時加入 ACL。

1. 取得或建立要在其中加入 ACL 的網路介面。
 
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. 取得或建立您將新增到網路介面的 ACL。
 
   ```PowerShell
   $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"
   ```
 
3. 將 ACL 指派給網路介面的 AccessControlList 屬性
 
   ```PowerShell
    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl
   ```
 
4. 網路控制卡中新增網路介面
 
   ```
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
 
## <a name="example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api"></a>範例：網路介面移除 ACL，請使用 Windows Powershell 和網路控制站的 REST API
在此範例中，我們說明如何移除 ACL。 移除 ACL 的網路介面套用一組預設的規則。 一組預設規則允許所有輸出流量，但會封鎖所有輸入的流量。

>[!NOTE]
>如果您想要允許所有的輸入的流量，您必須遵循先前[範例](#example-add-an-acl-to-a-network-interface)將允許所有輸入和所有輸出流量的 ACL。


1. 取得網路介面，您將從中移除 ACL。<br>
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. 將 $NULL 指派至 ipConfiguration AccessControlList 屬性。<br>
   ```PowerShell
   $nic.properties.ipconfigurations[0].properties.AccessControlList = $null
   ```
 
3. 加入網路控制卡中的網路介面物件。<br>
   ```PowerShell
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
