---
title: 建立、刪除或更新租使用者虛擬網路
description: 在本主題中，您將瞭解如何在部署軟體定義的網路功能（SDN）後，建立、刪除和更新 Hyper-v 網路虛擬化虛擬網路。 Hyper-v 網路虛擬化可協助您隔離租使用者網路，讓每個租使用者網路都是個別的實體。 除非您設定公用存取工作負載，否則每個實體都沒有跨連接的可能性。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a820826-e829-4ef2-9a20-f74235f8c25b
ms.author: lizross
author: eross-msft
ms.date: 08/24/2018
ms.openlocfilehash: f85f593ec3dca33c5b35fb065c7d84ed12ea9af2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309827"
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>建立、刪除或更新租用戶虛擬網路

>適用於：Windows Server (半年通道)、Windows Server 2016

在本主題中，您將瞭解如何在部署軟體定義的網路功能（SDN）後，建立、刪除和更新 Hyper-v 網路虛擬化虛擬網路。 Hyper-v 網路虛擬化可協助您隔離租使用者網路，讓每個租使用者網路都是個別的實體。 除非您設定公用存取工作負載，否則每個實體都沒有跨連接的可能性。   
  
## <a name="create-a-new-virtual-network"></a>建立新的虛擬網路  
為租使用者建立虛擬網路時，會將它放置在 Hyper-v 主機上唯一的路由網域中。 在每個虛擬網路底下，至少有一個虛擬子網。 虛擬子網是由 IP 首碼所定義，並參考先前定義的 ACL。  

建立新虛擬網路的步驟如下：

1. 識別您想要從中建立虛擬子網的 IP 位址首碼。   
2. 識別用來傳送租使用者流量的邏輯提供者網路。   
3. 針對您在步驟1中識別的每個 IP 首碼，至少建立一個虛擬子網。 
4. 選擇性將先前建立的 Acl 新增至虛擬子網，或為租使用者新增閘道連線能力。 

下表包含兩個虛構租使用者的範例子網識別碼和首碼。 租使用者 Fabrikam 有兩個虛擬子網，而 Contoso 租使用者有三個虛擬子網。  
 
  
租使用者名稱  |虛擬子網識別碼  |虛擬子網首碼    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Contoso    |6001         |  24.30.1.0/24         
Contoso    | 6002        |  24.30.2.0/24         
Contoso     | 6003        | 24.30.3.0/24          
  
下列範例腳本會使用從**NetworkController**模組匯出的 Windows PowerShell 命令，來建立 Contoso 的虛擬網路和一個子網：   
  
```Powershell  
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
  
## <a name="modify-an-existing-virtual-network"></a>修改現有的虛擬網路  
您可以使用 Windows PowerShell 來更新現有的虛擬子網或網路。   
  
當您執行下列範例腳本時，已更新的資源會直接放在具有相同資源識別碼的網路控制卡。 如果您的租使用者想要將新的虛擬子網（24.30.2.0/24）新增至其虛擬網路，您或 Contoso 系統管理員可以使用下列腳本。  
  
```PowerShell  
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
  
## <a name="delete-a-virtual-network"></a>刪除虛擬網路  
  
您可以使用 Windows PowerShell 來刪除虛擬網路。  
  
下列 Windows PowerShell 範例會藉由發出 HTTP delete 到資源識別碼的 URI，來刪除租使用者虛擬網路。  

```PowerShell  
Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  
```

