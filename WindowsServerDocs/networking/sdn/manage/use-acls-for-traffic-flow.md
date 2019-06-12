---
title: 使用存取控制清單 (Acl) 來管理資料中心網路流量
description: 本主題中，您已了解如何設定存取控制清單 (Acl) 來管理虛擬子網路上使用資料中心防火牆和 Acl 的資料流量。 您啟用並設定資料中心防火牆建立套用至虛擬子網路或網路介面的 Acl。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a7ac5af-85e9-4440-a631-6a3a38e9015d
ms.author: pashort
author: shortpatti
ms.date: 08/27/2018
ms.openlocfilehash: 7bfb74e0964735d357226ab1e5af826796c48d81
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446299"
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>使用存取控制清單 (Acl) 來管理資料中心網路流量

>適用於：Windows Server （半年通道），Windows Server 2016

本主題中，您已了解如何設定存取控制清單 (Acl) 來管理虛擬子網路上使用資料中心防火牆和 Acl 的資料流量。 您啟用並設定資料中心防火牆建立套用至虛擬子網路或網路介面的 Acl。   

本主題中的下列範例將示範如何使用 Windows PowerShell 來建立這些 Acl。  

## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>設定資料中心防火牆以允許所有流量  

一旦您部署 SDN，您應該在新環境中測試的基本網路連線。  若要達成此目的，建立可讓所有的網路流量，不受限制的資料中心防火牆的規則。   

使用下表中的項目，來建立一組允許所有的輸入和輸出網路流量的規則。


| 來源 IP | 目的地 IP | Protocol | 來源連接埠 | 目的地連接埠 | Direction | 動作 | Priority |
|:---------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|    \*     |       \*       |   全部    |     \*      |        \*        |  輸入  | 允許  |   100    |
|    \*     |       \*       |   全部    |     \*      |        \*        | 輸出  | 允許  |   110    |

---       

### <a name="example-create-an-acl"></a>範例：建立 ACL 
在此範例中，您會建立 ACL 含有兩個規則：

1. **AllowAll_Inbound** -可讓所有的網路流量進入這個 ACL 設定所在的網路介面。    
2. **AllowAllOutbound** -允許所有流量流到從網路介面。 此資源識別碼"AllowAll-1"所識別的 ACL 現在已準備好用於虛擬子網路和網路介面的。  

下列範例指令碼會使用從匯出的 Windows PowerShell 命令**NetworkController**模組來建立這個 ACL。  


```PowerShell
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "100"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  
$aclrule1 = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule1.Properties = $ruleproperties  
$aclrule1.ResourceId = "AllowAll_Inbound"  
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "110"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  
$aclrule2 = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule2.Properties = $ruleproperties  
$aclrule2.ResourceId = "AllowAll_Outbound"  
$acllistproperties = new-object Microsoft.Windows.NetworkController.AccessControlListProperties  
$acllistproperties.AclRules = @($aclrule1, $aclrule2)  
New-NetworkControllerAccessControlList -ResourceId "AllowAll" -Properties $acllistproperties -ConnectionUri <NC REST FQDN>  
```  

>[!NOTE]  
>網路控制站的 Windows PowerShell 命令參考位於本主題[網路控制站 Cmdlet](https://technet.microsoft.com/library/mt576401.aspx)。  

## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>使用 Acl 來限制在子網路上的流量  
在此範例中，您會建立 ACL 以防止 192.168.0.0/24 子網路內 Vm 彼此通訊。 這種類型的 ACL 可用於限制攻擊者的能力，同時仍然允許 Vm 接收來自外部的子網路，以及與其他子網路上其他服務通訊的子網路內分散橫向。   


|   來源 IP    | 目的地 IP | Protocol | 來源連接埠 | 目的地連接埠 | Direction | 動作 | Priority |
|:--------------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|  192.168.0.1   |       \*       |   全部    |     \*      |        \*        |  輸入  | 允許  |   100    |
|       \*       |  192.168.0.1   |   全部    |     \*      |        \*        | 輸出  | 允許  |   101    |
| 192.168.0.0/24 |       \*       |   全部    |     \*      |        \*        |  輸入  | 封鎖  |   102    |
|       \*       | 192.168.0.0/24 |   全部    |     \*      |        \*        | 輸出  | 封鎖  |   103    |
|       \*       |       \*       |   全部    |     \*      |        \*        |  輸入  | 允許  |   104    |
|       \*       |       \*       |   全部    |     \*      |        \*        | 輸出  | 允許  |   105    |

--- 

ACL 的範例指令碼建立，資源識別碼所識別**子網路-192 168-0-0**，現在可套用到使用"192.168.0.0/24 」 子網路位址的虛擬網路子網路。  自動附加到該虛擬網路子網路的任何網路介面取得套用上述的 ACL 規則。  

以下是使用 Windows Powershell 命令來建立使用網路控制站的 REST API ACL 的範例指令碼：  

```PowerShell  
import-module networkcontroller  
$ncURI = "https://mync.contoso.local"  
$aclrules = @()  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "192.168.0.1"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "100"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowRouter_Inbound"  
$aclrules += $aclrule  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "192.168.0.1"  
$ruleproperties.Priority = "101"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowRouter_Outbound"  
$aclrules += $aclrule  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Deny"  
$ruleproperties.SourceAddressPrefix = "192.168.0.0/24"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "102"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "DenySubnet_Inbound"  
$aclrules += $aclrule  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Deny"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "192.168.0.0/24"  
$ruleproperties.Priority = "103"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "DenySubnet_Outbound"  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "104"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowAll_Inbound"  
$aclrules += $aclrule  

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "105"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowAll_Outbound"  
$aclrules += $aclrule  

$acllistproperties = new-object Microsoft.Windows.NetworkController.AccessControlListProperties  
$acllistproperties.AclRules = $aclrules  

New-NetworkControllerAccessControlList -ResourceId "Subnet-192-168-0-0" -Properties $acllistproperties -ConnectionUri $ncURI   
```  

