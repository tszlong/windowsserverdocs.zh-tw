---
title: 用於管理 Datacenter 網路流量存取控制清單 (Acl)
description: 本主題是輔助的軟體定義網路上如何管理承租人工作負載和 Windows Server 2016 Virtual 網路的一部分。
manager: brianlic
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
ms.openlocfilehash: 64b7e1abf1ddb8132a8c6692fe82521c589f32df
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>用於管理 Datacenter 網路流量存取控制清單 (Acl)

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解如何設定來管理使用 Datacenter 防火牆和 Acl virtual 子網路上的資料流量存取控制清單。  
  
您可以讓和 Datacenter 防火牆設定來建立套用到 virtual 子網路或網路介面 Acl。  
  
下列範例示範如何使用 Windows PowerShell 來建立這些 Acl。  
  
## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>設定資料中心防火牆允許所有的資料傳輸  
  
之後部署 SDN，建議您新增環境中，測試基本網路連接。  
  
若要完成此動作，您可以建立規則 Datacenter 防火牆，可讓所有的網路流量無限制。   
  
您可以使用下表中的項目建立規則允許所有輸入 / 輸出網路流量的一組。  
  
  
  
來源 IP|目的地 IP|通訊協定|來源連接埠|目的地連接埠|方向|控制項目|高優先順序   
---------|---------|---------|---------|---------|---------|---------|---------  
*    |   *      |   所有      |    *     |      *   |     輸入    |   允許      |   100        
*    |     *    |     所有    |     *    |     *    |   輸出      |  允許       |  110         
  
  
此範例指令碼建立包含兩規則 ACL:    
- 第一個規則 」 AllowAll_Inbound 」 可讓所有的網路流量傳遞到網路介面已此 ACL。    
- 第二個規則，「 AllowAllOutbound 」 可讓所有流量通過退出的網路介面。  
由資源 id 「 「 全部允許 」 1 」，此 ACL 現在已準備好 virtual 子網路和網路介面中使用。  
  
下列範例指令碼使用 Windows PowerShell 命令匯出從**NetworkController**來建立此 ACL 模組。  
  
  
```  
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
>Network Controller 的 Windows PowerShell 命令參照位於主題中的[網路控制器 Cmdlet](https://technet.microsoft.com/library/mt576401.aspx)。  
  
## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>使用 Acl 限制子網路流量  
  
您可以使用此範例中建立 ACL，以避免虛擬電腦 (Vm)，從互相通訊 192.168.0.0/24 子網路中。   
  
這種類型的 ACL 適合用於限制的攻擊但仍然允許 Vm 收到來自要求以外子網路，以及與其他子網路上的其他服務通訊橫向分散子網路中的功能。  
  
  
來源 IP|目的地 IP|通訊協定|來源連接埠|目的地連接埠|方向|控制項目|高優先順序   
---------|---------|---------|---------|---------|---------|---------|---------  
192.168.0.1    | * | 所有 | * | * | 輸入|   允許      |   100        
* |192.168.0.1 | 所有 | * | * | 輸出|  允許       |  101         
192.168.0.0/24 | * | 所有 | * | * | 輸入| 封鎖 | 102  
* | 192.168.0.0/24 |所有| * | * | 輸出 | 封鎖 |103  
* | *  |所有| * | * | 輸入 | 允許 |104  
* | *  |所有| * | * | 輸出 | 允許 |105  
  
ACL 建立以下的範例指令碼由的資源 id**子網路-192-168-0-0**，現在可套用至子 virtual 網路使用 「 192.168.0.0/24 [子網路位址。  會自動附加至該 virtual 網路子網路的任何網路介面取得套用上述 ACL 規則。  
  
以下是使用 Windows Powershell 命令來建立使用網路控制器 REST API 此 ACL 範例指令碼：  
  
```  
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
  
