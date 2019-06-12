---
title: 虛擬網路中的輸出計量
description: 雲端網路的營收的重要層面是網路頻寬的輸出。 例如，輸出資料傳輸在 Microsoft Azure 中的商務模型。 輸出資料會負責根據給定的計費週期中移出 Azure 資料中心經由網際網路的資料總量。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: 50aee16b0b5797f28ebcdf61494b09669699873f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446327"
---
# <a name="egress-metering-in-a-virtual-network"></a>虛擬網路中的輸出計量

>適用於：Windows Server 2019


雲端網路的營收的重要層面能夠透過網路頻寬使用量計費。 輸出資料會負責根據給定的計費週期中移出資料中心經由網際網路的資料總量。

在 Windows Server 2019 的 SDN 網路流量的計量的輸出可讓您為輸出資料傳輸提供使用量計量。 會保留每個虛擬網路，但會維持在資料中心內的網路流量可以藉由個別追蹤讓它可以從計費計算中排除。 繫結不包含在其中一個未開立帳單的位址範圍的目的地 IP 位址的封包會追蹤因為計費的輸出資料傳輸。

## <a name="virtual-network-unbilled-address-ranges-whitelist-of-ip-ranges"></a>未開立帳單的虛擬網路位址範圍 （IP 範圍列入允許清單）

您可以找到下未開立帳單的位址範圍**UnbilledAddressRanges**的現有虛擬網路的屬性。 根據預設，沒有新增位址範圍。

   ```PowerShell
   import-module NetworkController
   $uri = "https://sdn.contoso.com"

   (Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties
   ```

您的輸出會看起來像這樣：
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


## <a name="example-manage-the-unbilled-address-ranges-of-a-virtual-network"></a>範例：管理虛擬網路的未開立帳單的位址範圍

您可以管理從藉由設定計量的計費的輸出中排除的 IP 子網路首碼的組合**UnbilledAddressRange**的虛擬網路的屬性。  BilledEgressBytes 屬性中，將不會包含任何符合的前置詞的其中一個目的地 IP 位址與虛擬網路上的網路介面傳送的流量。

1.  更新**UnbilledAddressRanges**內容包含不會收取存取的子網路。

    ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1"
    $vnet.Properties.UnbilledAddressRanges = "10.10.2.0/24,10.10.3.0/24"
    ```

    >[!TIP]
    >如果新增多個 IP 子網路，請使用每個 IP 子網路之間有一個逗號。  在逗號前後不包含任何空格。

2.  更新虛擬網路資源，使用修改後**UnbilledAddressRanges**屬性。

    ```PowerShell
    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "VNet1" -Properties $unbilled.Properties -PassInnerException
    ```

    您的輸出會看起來像這樣：
    ```
    Confirm
    Performing the operation 'New-NetworkControllerVirtualNetwork' on entities of type
    'Microsoft.Windows.NetworkController.VirtualNetwork' via
    'https://sdn.contoso.com/networking/v3/virtualNetworks/VNet1'. Are you sure you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y


~~~
Tags             :
ResourceRef      : /virtualNetworks/VNet1
InstanceId       : 29654b0b-9091-4bed-ab01-e172225dc02d
Etag             : W/"6970d0a3-3444-41d7-bbe4-36327968d853"
ResourceMetadata :
ResourceId       : VNet1
Properties       : Microsoft.Windows.NetworkController.VirtualNetworkProperties
```
~~~


3. Check the Virtual Network to see the configured **UnbilledAddressRanges**.

   ```PowerShell
   (Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1").properties
   ```

   Your output will now look similar to this:
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

## Check the billed the unbilled egress usage of a virtual network

After you configure the **UnbilledAddressRanges** property, you can check the billed and unbilled egress usage of each subnet within a virtual network. Egress traffic updates every four minutes with the total bytes of the billed and unbilled ranges.

The following properties are available for each virtual subnet:

-   **UnbilledEgressBytes** shows the number of unbilled bytes sent by network interfaces connected to this virtual subnet. Unbilled bytes are bytes sent to address ranges that are part of the **UnbilledAddressRanges** property of the parent virtual network.

-   **BilledEgressBytes** shows Number of billed bytes sent by network interfaces connected to this virtual subnet. Billed bytes are bytes sent to address ranges that are not part of the **UnbilledAddressRanges** property of the parent virtual network.

Use the following example to query egress usage:

```PowerShell
(Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties.subnets.properties | ft AddressPrefix,BilledEgressBytes,UnbilledEgressBytes
```

Your output will look similar to this:
```
AddressPrefix BilledEgressBytes UnbilledEgressBytes
------------- ----------------- -------------------
10.0.255.8/29          16827067                   0
10.0.2.0/24           781733019                   0
10.0.4.0/24                   0                   0
```


---