---
title: 建立、 刪除或更新租用戶虛擬網路
description: 本主題中，您將了解如何建立、 刪除和更新 HYPER-V 網路虛擬化的虛擬網路，在您部署軟體定義網路 (SDN) 之後。 HYPER-V 網路虛擬化可協助您隔離租用戶網路，讓每個租用戶網路是個別的實體。 除非您設定公用存取的工作負載，每個實體會有不跨連線可能會發生。
manager: dougkim
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
ms.date: 08/24/2018
ms.openlocfilehash: a125ec220b4769a57a6be30f1425283afb7f0fe6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838349"
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>建立、刪除或更新租用戶虛擬網路

>適用於：Windows Server （半年通道），Windows Server 2016

本主題中，您將了解如何建立、 刪除和更新 HYPER-V 網路虛擬化的虛擬網路，在您部署軟體定義網路 (SDN) 之後。 HYPER-V 網路虛擬化可協助您隔離租用戶網路，讓每個租用戶網路是個別的實體。 除非您設定公用存取的工作負載，每個實體會有不跨連線可能會發生。   
  
## <a name="create-a-new-virtual-network"></a>建立新的虛擬網路  
為租用戶中建立虛擬網路將將它放在 HYPER-V 主機上的唯一路由網域。 每個虛擬網路，下方沒有至少一個虛擬子網路。 虛擬子網路取得 IP 前置詞所定義，並參考先前定義的 ACL。  

若要建立新的虛擬網路的步驟如下：

1. 找出您要建立的虛擬子網路的 IP 位址首碼。   
2. 識別租用戶流量建立通道的提供者邏輯網路。   
3. 在步驟 1 中建立至少一個虛擬子網路，您識別每個 IP 前置詞。 
4. （選擇性）將先前建立的 Acl 新增至虛擬子網路，或新增租用戶的閘道連線。 

下表包含兩個虛構的租用戶範例中的子網路識別碼和前置詞。 Fabrikam 的租用戶會有兩個虛擬子網路，而 Contoso 租用戶有三個虛擬子網路。  
 
  
租用戶名稱  |虛擬子網路識別碼  |虛擬子網路首碼    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Contoso    |6001         |  24.30.1.0/24         
Contoso    | 6002        |  24.30.2.0/24         
Contoso     | 6003        | 24.30.3.0/24          
  
下列範例指令碼會使用從匯出的 Windows PowerShell 命令**NetworkController**模組來建立 Contoso 的虛擬網路和一個子網路：   
  
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
您可以使用 Windows PowerShell 來更新現有的虛擬子網路。   
  
當您執行下列的範例指令碼時，更新的資源會簡單地說到網路控制站，並提供相同的資源識別碼。 如果您的租用戶 Contoso 想要將新的虛擬子網路 (24.30.2.0/24) 新增至他們的虛擬網路，您或 Contoso 系統管理員可以使用下列指令碼。  
  
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
  
您可以使用 Windows PowerShell 刪除虛擬網路。  
  
下列 Windows PowerShell 範例會刪除租用戶虛擬網路的資源識別碼 URI 發出 HTTP delete  

```PowerShell  
Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  
```

