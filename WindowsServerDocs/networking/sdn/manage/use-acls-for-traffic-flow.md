---
title: 使用存取控制清單 (Acl) 管理資料中心網路流量
description: 在本主題中，您將瞭解如何 (Acl 設定存取控制清單，) 使用虛擬子網上的資料中心防火牆和 Acl 來管理資料流量流程。 您可以藉由建立可套用至虛擬子網或網路介面的 Acl 來啟用和設定資料中心防火牆。
manager: grcusanz
ms.topic: article
ms.assetid: 6a7ac5af-85e9-4440-a631-6a3a38e9015d
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/27/2018
ms.openlocfilehash: f96f41f18b57e53ebe01c70b6775d3608d06b28c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953994"
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>使用存取控制清單 (Acl) 管理資料中心網路流量

>適用於：Windows Server (半年度管道)、Windows Server 2016

在本主題中，您將瞭解如何 (Acl 設定存取控制清單，) 使用虛擬子網上的資料中心防火牆和 Acl 來管理資料流量流程。 您可以藉由建立可套用至虛擬子網或網路介面的 Acl 來啟用和設定資料中心防火牆。

本主題中的下列範例將示範如何使用 Windows PowerShell 來建立這些 Acl。

## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>設定資料中心防火牆以允許所有流量

部署 SDN 之後，您應該在新的環境中測試基本的網路連線能力。  若要達成此目的，請建立資料中心防火牆的規則，以允許所有網路流量，而不受限制。

使用下表中的專案來建立一組規則，以允許所有輸入和輸出網路流量。


| 來源 IP | 目的地 IP | 通訊協定 | 來源連接埠 | 目的地連接埠 | 方向 | 動作 | 優先順序 |
|:---------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|    \*     |       \*       |   全部    |     \*      |        \*        |  輸入  | 允許  |   100    |
|    \*     |       \*       |   全部    |     \*      |        \*        | 輸出  | 允許  |   110    |

---

### <a name="example-create-an-acl"></a>範例：建立 ACL
在此範例中，您會建立具有兩個規則的 ACL：

1. **AllowAll_Inbound** -允許所有網路流量傳入設定此 ACL 的網路介面。
2. **AllowAllOutbound** -允許所有流量傳入網路介面。 此 ACL （由資源識別碼 "AllowAll-1" 識別）現在已準備好用於虛擬子網和網路介面。

下列範例腳本會使用從**NetworkController**模組匯出的 Windows PowerShell 命令來建立此 ACL。


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
>網路控制卡的 Windows PowerShell 命令參考位於[網路控制站 Cmdlet](https://technet.microsoft.com/library/mt576401.aspx)主題。

## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>使用 Acl 來限制子網上的流量
在此範例中，您會建立 ACL，以防止 192.168.0.0/24 子網中的 Vm 彼此通訊。 這種類型的 ACL 有助於限制攻擊者在子網內散佈橫向的能力，同時仍然允許 Vm 接收來自子網外部的要求，以及與其他子網上的其他服務通訊。


|   來源 IP    | 目的地 IP | 通訊協定 | 來源連接埠 | 目的地連接埠 | 方向 | 動作 | 優先順序 |
|:--------------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|  192.168.0.1   |       \*       |   全部    |     \*      |        \*        |  輸入  | 允許  |   100    |
|       \*       |  192.168.0.1   |   全部    |     \*      |        \*        | 輸出  | 允許  |   101    |
| 192.168.0.0/24 |       \*       |   全部    |     \*      |        \*        |  輸入  | 封鎖  |   102    |
|       \*       | 192.168.0.0/24 |   全部    |     \*      |        \*        | 輸出  | 封鎖  |   103    |
|       \*       |       \*       |   全部    |     \*      |        \*        |  輸入  | 允許  |   104    |
|       \*       |       \*       |   全部    |     \*      |        \*        | 輸出  | 允許  |   105    |

---

下面的範例腳本所建立的 ACL （由資源識別碼**子網-192-168-0-0**識別）現在可以套用至使用 "192.168.0.0/24" 子網位址的虛擬網路子網。  任何連接至該虛擬網路子網的網路介面，都會自動取得上述已套用的 ACL 規則。

以下範例腳本使用 Windows Powershell 命令，以使用網路控制卡 REST API 建立此 ACL：

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

