---
title: 虛擬網路中的輸出計量
description: 雲端網路營收的基本層面是網路頻寬輸出。 例如，Microsoft Azure 商務模型中的輸出資料傳輸。 輸出資料的收費依據是在給定的計費週期內，透過網際網路從 Azure 資料中心移出的總數據量。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: e68a3889867b75152ea941ac1d8eb113b9acd3cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406003"
---
# <a name="egress-metering-in-a-virtual-network"></a>虛擬網路中的輸出計量

>適用於：Windows Server Standard 2012 R2


雲端網路營收的基本層面是能夠依網路頻寬使用量來計費。 輸出資料的收費依據是在給定的計費週期內，透過網際網路從資料中心移出資料的總金額。

Windows Server 2019 中 SDN 網路流量的輸出計量可讓您提供輸出資料傳輸的使用量計量。 離開每個虛擬網路但仍留在資料中心內的網路流量，可以個別追蹤，使其可以從計費計算中排除。 針對未包含在其中一個未開立帳單位址範圍內的目的地 IP 位址所系結的封包，會以計費的輸出資料傳輸進行追蹤。

## <a name="virtual-network-unbilled-address-ranges-whitelist-of-ip-ranges"></a>虛擬網路未開立帳單位址範圍（IP 範圍的白名單）

您可以在現有虛擬網路的 [ **UnbilledAddressRanges** ] 屬性下找到未開立帳單位址範圍。 根據預設，不會新增任何位址範圍。

   ```PowerShell
   import-module NetworkController
   $uri = "https://sdn.contoso.com"

   (Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties
   ```

您的輸出看起來會像這樣：
   ```
    AddressSpace           : Microsoft.Windows.NetworkController.AddressSpace
    DhcpOptions            :
    UnbilledAddressRanges  :
    ConfigurationState     :
    ProvisioningState      : Succeeded
    Subnets                : {21e71701-9f59-4ee5-b798-2a9d8c2762f0, 5f4758ef-9f96-40ca-a389-35c414e996cc,
                         29fe67b8-6f7b-486c-973b-8b9b987ec8b3}
    VirtualNetworkPeerings :
    EncryptionCredential   :
    LogicalNetwork         : Microsoft.Windows.NetworkController.LogicalNetwork
   ```


## <a name="example-manage-the-unbilled-address-ranges-of-a-virtual-network"></a>範例：管理虛擬網路的未開立帳單位址範圍

您可以藉由設定虛擬網路的**UnbilledAddressRange**屬性，管理要從計費輸出計量排除的 IP 子網首碼集合。  虛擬網路上網路介面所傳送的任何流量若目的地 IP 位址符合其中一個前置詞，將不會包含在 BilledEgressBytes 屬性中。

1.  更新**UnbilledAddressRanges**屬性，使其包含不會針對存取計費的子網。

    ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1"
    $vnet.Properties.UnbilledAddressRanges = "10.10.2.0/24,10.10.3.0/24"
    ```

    >[!TIP]
    >如果新增多個 IP 子網，請在每個 IP 子網之間使用逗號。  請不要在逗號前後包含任何空格。

2.  使用修改過的**UnbilledAddressRanges**屬性來更新虛擬網路資源。

    ```PowerShell
    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "VNet1" -Properties $unbilled.Properties -PassInnerException
    ```

    您的輸出看起來會像這樣：
      ```
         Confirm
         Performing the operation 'New-NetworkControllerVirtualNetwork' on entities of type
         'Microsoft.Windows.NetworkController.VirtualNetwork' via
         'https://sdn.contoso.com/networking/v3/virtualNetworks/VNet1'. Are you sure you want to continue?
         [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y


         Tags             :
         ResourceRef      : /virtualNetworks/VNet1
         InstanceId       : 29654b0b-9091-4bed-ab01-e172225dc02d
         Etag             : W/"6970d0a3-3444-41d7-bbe4-36327968d853"
         ResourceMetadata :
         ResourceId       : VNet1
         Properties       : Microsoft.Windows.NetworkController.VirtualNetworkProperties
      ```


3. 檢查虛擬網路以查看已設定的**UnbilledAddressRanges**。

   ```PowerShell
   (Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1").properties
   ```

   您的輸出現在看起來會像這樣：
   ```
   AddressSpace           : Microsoft.Windows.NetworkController.AddressSpace
   DhcpOptions            :
   UnbilledAddressRanges  : 10.10.2.0/24,192.168.2.0/24
   ConfigurationState     :
   ProvisioningState      : Succeeded
   Subnets                : {21e71701-9f59-4ee5-b798-2a9d8c2762f0, 5f4758ef-9f96-40ca-a389-35c414e996cc,
                        29fe67b8-6f7b-486c-973b-8b9b987ec8b3}
   VirtualNetworkPeerings :
   EncryptionCredential   :
   LogicalNetwork         : Microsoft.Windows.NetworkController.LogicalNetwork
   ```

## <a name="check-the-billed-the-unbilled-egress-usage-of-a-virtual-network"></a>檢查虛擬網路的未開立帳單輸出使用量計費

設定**UnbilledAddressRanges**屬性之後，您可以檢查虛擬網路中每個子網的計費和未開立帳單輸出使用量。 輸出流量會每四分鐘更新一次，並加上計費和未開立帳單範圍的總位元組數。

下列屬性適用于每個虛擬子網：

-   **UnbilledEgressBytes**顯示連線到此虛擬子網的網路介面所傳送的未開立帳單位元組數目。 未開立帳單 bytes 是傳送到位址範圍的位元組，屬於父虛擬網路的**UnbilledAddressRanges**屬性。

-   **BilledEgressBytes**會顯示連線到此虛擬子網的網路介面所傳送的計費位元組數。 計費的位元組是傳送給不屬於父虛擬網路**UnbilledAddressRanges**屬性之位址範圍的位元組。

使用下列範例來查詢輸出使用方式：

```PowerShell
(Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties.subnets.properties | ft AddressPrefix,BilledEgressBytes,UnbilledEgressBytes
```

您的輸出看起來會像這樣：
```
AddressPrefix BilledEgressBytes UnbilledEgressBytes
------------- ----------------- -------------------
10.0.255.8/29          16827067                   0
10.0.2.0/24           781733019                   0
10.0.4.0/24                   0                   0
```


---
