---
title: 設定資料中心防火牆存取控制清單 (Acl)
description: 本主題是輔助的軟體定義網路上如何管理承租人工作負載和 Windows Server 2016 Virtual 網路的一部分。
manager: brianlic
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
ms.openlocfilehash: d5f7c4baad24a720e073857cb6c835167e5b419b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>設定資料中心防火牆存取控制清單 (Acl)

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以將特定 Acl 套用到網路介面。  Acl 也會在連接的網路介面 virtual 子網路設定，如果同時套用 Acl，但網路介面 Acl 的上述 Acl virtual 子網路的優先順序。

本主題包含下列各節。

- [範例︰ 將 ACL 新增到網路介面](#bkmk_addacl)
- [範例： 移除 ACL 網路介面使用 Windows Powershell 與網路控制器 REST API](#bkmk_removeacl)

##<a name="bkmk_addacl"></a>範例︰ 將 ACL 新增到網路介面

主題中的[使用存取控制清單 (Acl) 來管理 Datacenter 網路流量 Flow](Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md)您如何建立 ACL 並指派給 virtual 子網路。  有時候，但是，您可能想要覆寫特定 ACL 個人網路介面 virtaul 子網路上的預設 ACL。  您必須將 Acl 直接套用至已連接到 Vlan 而不是 virtual 網路的網路介面。

此範例中示範如何將 ACL virtual 網路。 

>[!NOTE]
>它也可在此同時，您建立網路介面新增 ACL。

###<a name="step-1-get-or-create-the-network-interface-to-which-you-will-add-the-acl"></a>步驟 1： 取得或建立，您將會新增 ACL 網路介面

    $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-2-get-or-create-the-acl-you-will-add-to-the-network-interface"></a>步驟 2： 取得或建立的 ACL 您將會新增到網路介面
您可以使用下列命令的範例，以取得或建立 ACL。 

    $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"

###<a name="step-3-assign-the-acl-to-the-accesscontrollist-property-of-the-network-interface"></a>步驟 3： 指定 ACL 的 AccessControlList 屬性的網路介面
您可以使用下列命令範例 ACL 指派給 AccessControlList 屬性。

    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl

###<a name="step-4-add-the-network-interface-in-network-controller"></a>步驟 4: Network Controller 中新增的網路介面
您可以使用下列命令範例 Network Controller 中新增的網路介面。

    new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid


##<a name="bkmk_removeacl"></a>範例： 移除 ACL 網路介面使用 Windows Powershell 與網路控制器 REST API
您可以使用此範例中，如果您想要移除 ACL。 當您移除 ACL 時，預設一組規則適用於的網路介面。

預設一組規則允許傳出所有的資料，但封鎖所有輸入的流量。

>[!NOTE]
>如果您想要讓所有輸入的流量，您必須遵循加入 [允許所有輸入 / 輸出所有流量 ACL 前一個範例。

###<a name="step-1-get-the-network-interface-from-which-you-will-remove-the-acl"></a>步驟 1： 取得，您將會移除 ACL 網路介面
您可以使用下列命令範例擷取的網路介面。

    $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-2-assign-null-to-the-accesscontrollist-property-of-the-ipconfiguration"></a>步驟 2: $NULL 指定 ipConfiguration AccessControlList 屬性
您可以使用下列命令範例 $NULL 指派給 AccessControlList 屬性。

    $nic.properties.ipconfigurations[0].properties.AccessControlList = $null

###<a name="step-3-add-the-network-interface-object-in-network-controller"></a>步驟 3: Network Controller 中新增的網路介面物件
您可以使用下列命令範例新增網路介面物件網路控制器，請在

    new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid

