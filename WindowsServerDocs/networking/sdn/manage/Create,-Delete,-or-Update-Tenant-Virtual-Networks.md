---
title: 建立、Delete，或更新承租人 Virtual 網路
description: 本主題是輔助的軟體定義網路上如何管理承租人工作負載和 Windows Server 2016 Virtual 網路的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a820826-e829-4ef2-9a20-f74235f8c25b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6ef30dcc31593e15c36f846cf6d64afcd4b85f19
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>建立、Delete，或更新承租人 Virtual 網路

>適用於：Windows Server（以每年次管道）、Windows Server 2016

若要了解如何建立、delete，以及更新 HYPER-V 網路模擬 Virtual 網路部署軟體定義網路 (SDN) 之後，您可以使用此主題。  
  
使用 HYPER-V 網路模擬，您可以找出承租人網路，使每個承租人網路的完全獨立實體不跨連接可能使用除非您設定的工作負載公用存取。  
  
建立 tenants Virtual 新的網路，您可以修改現有 Virtual 網路，並如果房客不再需要特定資源，或是承租人不再是您客戶，您可以 delete 承租人 Virtual 網路。  
  
## <a name="create-a-new-virtual-network"></a>建立新的 Virtual 網路  
  
當您建立的房客 Virtual 網路時，它放 HYPER-V 主機上的唯一路由網域中。  
  
以下是步驟建立新的 Virtual 網路。  
  
1. 找出您要建立的 virtual 子網路的 IP 位址前置詞。   
2. 找出邏輯提供者網路承租人流量使用的通道。   
3. 針對每個 IP 首碼所定義步驟 1 中建立至少一 virtual 子網路。   
  
>[!NOTE]  
>每個 virtual 網路下方還有至少一個 virtual 子網路。 IP 首碼所定義 virtual 子網路，以及參考預先定義的存取控制清單。  
  
（選擇性）完成這些步驟之後，您也可以將先前建立的存取控制清單新增至 virtual 子網路，或新增 tenants 閘道器連接。    
  
下表包含兩種虛構 tenants 範例子網路 Id 和前置詞。 承租人 Fabrikam 有兩個 virtual 子網路，以 Contoso 承租人有三種 virtual 子網路時。  
  
  
  
承租人名稱  |子網路 virtual ID  |Virtual 子網路首碼    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Contoso    |6001         |  24.30.1.0/24         
Contoso    | 6002        |  24.30.2.0/24         
Contoso     | 6003        | 24.30.3.0/24          
  
下列範例指令碼使用 Windows PowerShell 命令匯出從**NetworkController**模組建立 Contoso virtual 網路和一個子網路：   
  
```  
import-module networkcontroller  
$URI = "https://ncrest.contoso.local"  
  
#Find the HNV Provider Logical Network  
  
$logicalnetworks = Get-NetworkControllerLogicalNetwork -ConnectionUri $uri  
foreach ($ln in $logicalnetworks) {  
   if ($ln.Properties.NetworkVirtualizationEnabled -eq "True") {  
      $HNVProviderLogicalNetwork = $ln  
   }  
}   
  
#Find the Access Control List to user per virtual subnet  
  
$acllist = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"  
  
#Create the Virtual Subnet  
  
$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Contoso_WebTier"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AccessControlList = $acllist  
$vsubnet.Properties.AddressPrefix = "24.30.1.0/24"  
  
#Create the Virtual Network  
  
$vnetproperties = new-object Microsoft.Windows.NetworkController.VirtualNetworkProperties  
$vnetproperties.AddressSpace = new-object Microsoft.Windows.NetworkController.AddressSpace  
$vnetproperties.AddressSpace.AddressPrefixes = @("24.30.1.0/24")  
$vnetproperties.LogicalNetwork = $HNVProviderLogicalNetwork  
$vnetproperties.Subnets = @($vsubnet)  
New-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri -Properties $vnetproperties  
  
  
```  
  
## <a name="modify-an-existing-virtual-network"></a>修改現有 Virtual 網路  
您可以使用 Windows PowerShell 來更新現有虛擬子網路。   
  
已更新的資源當您執行以下的範例指令碼時，只是要使用相同的資源收到 Network Controller 如果您承租人 Contoso 想要新增新 virtual 子網路 (24.30.2.0 月 24）他們 virtual 的網路，您或 Contoso 系統管理員可以使用下列的指令碼。  
  
```  
$acllist = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"  
  
$vnet = Get-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri  
  
$vnet.properties.AddressSpace.AddressPrefixes += "24.30.2.0/24"  
  
$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Contoso_DBTier"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AccessControlList = $acllist  
$vsubnet.Properties.AddressPrefix = "24.30.2.0/24"  
  
$vnet.properties.Subnets += $vsubnet  
  
New-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri -properties $vnet.properties  
  
```  
  
## <a name="delete-a-virtual-network"></a>Delete Virtual 網路  
  
您可以使用 Windows PowerShell 來 delete Virtual 網路。  
  
Windows PowerShell 下例 HTTP delete 發出的資源 ID uri 移除房客 Virtual 網路  
  
    Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  


