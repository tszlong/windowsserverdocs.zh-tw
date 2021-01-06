---
title: 虛擬網路中的輸出計量
description: 雲端網路營收的基本層面是網路頻寬輸出。 例如，Microsoft Azure 商務模型中的輸出資料傳輸。 輸出資料的收費依據是在給定的計費週期內，透過網際網路從 Azure 資料中心移出的資料總量。
manager: grcusanz
ms.topic: how-to
ms.author: anpaul
author: AnirbanPaul
ms.date: 10/02/2018
ms.openlocfilehash: 821adcc4ae3ac99a92de8b0b57e3248be1786ce3
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947274"
---
# <a name="egress-metering-in-a-virtual-network"></a>虛擬網路中的輸出計量

>適用於：Windows Server 2019


雲端網路營收的基本層面是能夠依網路頻寬使用量來計費。 輸出資料的收費依據是在給定的計費週期內，透過網際網路從資料中心移出資料的總金額。

Windows Server 2019 中 SDN 網路流量的輸出計量能夠提供輸出資料傳輸的使用計量。 離開每個虛擬網路，但保留在資料中心內的網路流量可以個別追蹤，讓它可以從計費計算中排除。 針對未包含在其中一個未開立帳單位址範圍內的目的地 IP 位址所系結的封包，會追蹤為計費的輸出資料傳輸。

## <a name="virtual-network-unbilled-address-ranges-allowlist-of-ip-ranges"></a>虛擬網路未開立帳單位址範圍 (IP 範圍的允許清單) 

您可以在現有虛擬網路的 **UnbilledAddressRanges** 屬性下找到未開立帳單位址範圍。 依預設，不會新增任何位址範圍。

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

您可以藉由設定虛擬網路的 **UnbilledAddressRange** 屬性，來管理一組 IP 子網首碼，以排除在計費輸出計量之外。  使用符合其中一個首碼的目的地 IP 位址，在虛擬網路上由網路介面傳送的任何流量，將不會包含在 BilledEgressBytes 屬性中。

1.  更新 **UnbilledAddressRanges** 屬性，以包含不需要支付存取權的子網。

    ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1"
    $vnet.Properties.UnbilledAddressRanges = "10.10.2.0/24,10.10.3.0/24"
    ```

    >[!TIP]
    >如果新增多個 IP 子網，請在每個 IP 子網之間使用逗號。  不要在逗號之前或之後包含任何空格。

2.  使用已修改的 **UnbilledAddressRanges** 屬性更新虛擬網路資源。

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


3. 檢查虛擬網路以查看已設定的 **UnbilledAddressRanges**。

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

## <a name="check-the-billed-the-unbilled-egress-usage-of-a-virtual-network"></a>檢查虛擬網路的計費未開立帳單輸出使用量

設定 **UnbilledAddressRanges** 屬性之後，您可以在虛擬網路內檢查每個子網的計費和未開立帳單輸出使用量。 輸出流量會每四分鐘更新一次，包含計費和未開立帳單範圍的總位元組數。

以下是每個虛擬子網的可用屬性：

-   **UnbilledEgressBytes** 會顯示連線到此虛擬子網的網路介面所傳送的未開立帳單位元組數目。 未開立帳單 bytes 是傳送至位址範圍的位元組，這些範圍屬於父虛擬網路的 **UnbilledAddressRanges** 屬性。

-   **BilledEgressBytes** 會顯示連線到此虛擬子網的網路介面所傳送的計費位元組數目。 計費的位元組是傳送至位址範圍的位元組，不是父虛擬網路 **UnbilledAddressRanges** 屬性的一部分。

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
