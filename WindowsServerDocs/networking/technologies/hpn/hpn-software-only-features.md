---
title: 僅限軟體 (SO) 功能和技術
description: 這些功能會實作為作業系統的一部分，而且與基礎 NIC (s) 無關。 這些功能有時需要調整 NIC 以進行最佳操作。 其中的範例包括 Hyper-v 功能，例如虛擬機器服務品質 (vmQoS) 、存取控制清單 (Acl) 和非 Hyper-v 功能（例如 NIC 小組）。
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/20/2018
ms.openlocfilehash: ec4867b0c2ca760babd2b07062c9b8ee3d73e8f7
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96864877"
---
# <a name="software-only-so-features-and-technologies"></a>僅限軟體 (SO) 功能和技術
軟體專用功能會實作為作業系統的一部分，而且與基礎 NIC (s) 無關。 這些功能有時需要調整 NIC 以進行最佳操作。 其中的範例包括 Hyper-v 功能，例如虛擬機器服務品質 (vmQoS) 、存取控制清單 (Acl) 和非 Hyper-v 功能（例如 NIC 小組）。

## <a name="access-control-lists-acls"></a>存取控制清單 (ACL)

用於管理 VM 安全性的 Hyper-v 和 SDNv1 功能。 這項功能適用于非虛擬化的 Hyper-v 堆疊和 HVNv1 堆疊。 您可以透過 [get-vmnetworkadapteracl](/powershell/module/hyper-v/add-vmnetworkadapteracl) 和 [移除 get-vmnetworkadapteracl](/powershell/module/hyper-v/remove-vmnetworkadapteracl) PowerShell Cmdlet 來管理 hyper-v 交換器 acl。

## <a name="extended-acls"></a>延伸的 Acl

Hyper-v 虛擬交換器延伸 Acl 可讓您設定 Hyper-v 虛擬交換器擴充的埠 Acl，以提供防火牆保護，並為資料中心的租使用者 Vm 強制執行安全性原則。 由於埠 Acl 是在 Hyper-v 虛擬交換器上設定，而不是在 Vm 內進行設定，因此系統管理員可以管理多租使用者環境中所有租使用者的安全性原則。

您可以透過 [get-vmnetworkadapterextendedacl](/powershell/module/hyper-v/add-vmnetworkadapterextendedacl) 和 [移除 get-vmnetworkadapterextendedacl](/powershell/module/hyper-v/remove-vmnetworkadapteracl) PowerShell Cmdlet 來管理 Hyper-v 交換器擴充的 acl。

>[!TIP]
>這項功能適用于 HNVv1 堆疊。 若為 SDN 堆疊中的 Acl，請參閱下面的軟體定義網路 SDN) Acl。

如需此程式庫中延伸埠存取控制清單的詳細資訊，請參閱 [使用擴充通訊埠存取控制清單建立安全性原則](../../../virtualization/hyper-v-virtual-switch/create-security-policies-with-extended-port-access-control-lists.md)。

## <a name="nic-teaming"></a>NIC 小組

NIC 小組（也稱為 NIC 組合）是將多個 NIC 埠匯總到主機作為單一 NIC 埠的實體。 NIC 小組可以防止單一 NIC 埠 (失敗或連接到該埠) 的纜線。 它也會匯總網路流量，以加快輸送量。 如需詳細資訊，請參閱 [NIC](../nic-teaming/nic-teaming.md)小組。

使用 Windows Server 2016，您有兩種方式可以進行小組作業：

1.  Windows Server 2012 團隊解決方案

2.  Windows Server 2016 交換器 Embedded 組合 (設定) 


## <a name="rsc-in-the-vswitch"></a>VSwitch 中的 RSC

在 vSwitch 中 (RSC) 的接收區段聯合是一項功能，它會取得屬於相同串流的封包，並在網路中斷之間抵達，然後將它們合併成單一封包，然後再傳遞至作業系統。 Windows Server 2019 中的虛擬交換器具有這項功能。 如需這項功能的詳細資訊，請參閱 [vSwitch 中的接收區段](./rsc-in-the-vswitch.md)聯合。

## <a name="software-defined-networking-sdn-acls"></a>軟體定義的網路 (SDN) Acl

Windows Server 2016 中的 SDN 擴充功能改進了支援 Acl 的方式。 在 Windows Server 2016 SDN v2 堆疊中，會使用 SDN Acl，而不是 Acl 和延伸的 Acl。 您可以使用網路控制站來管理 SDN Acl。

## <a name="sdn-quality-of-service-qos"></a>SDN 服務品質 (QoS)

Windows Server 2016 中的 SDN 擴充功能改進了提供頻寬控制的方式， (輸出保留、輸出限制和輸入限制，) 以5個元組為基礎。 一般而言，這些原則會套用至 vNIC 或 vmNIC 層級，但您可以讓它們更明確。 在 Windows Server 2016 SDN v2 堆疊中，會使用 SDN QoS 來取代 vmQoS。 您可以使用網路控制站來管理 SDN QoS。

## <a name="switch-embedded-teaming-set"></a>切換內嵌團隊 (設定) 

SET 是替代的 NIC 小組解決方案，您可以在 Windows Server 2016 中包含 Hyper-v 和軟體定義網路 (SDN) 堆疊的環境中使用。 SET 可將某些 NIC 小組功能整合到 Hyper-v 虛擬交換器。 如需此程式庫中的 Switch Embedded 小組的詳細資訊，請參閱 [ (RDMA 的遠端直接記憶體存取) 和切換內嵌小組 (設定) ](../../../virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md)。

## <a name="virtual-receive-side-scaling-vrss"></a>虛擬接收端調整 (vRSS)

軟體 vRSS 可用來將目的地為 VM 的連入流量分散到多個邏輯處理器 (VM 的 LPs) 。 軟體 vRSS 可讓 VM 處理比單一 LP 可以處理更多的網路流量。 如需詳細資訊，請參閱 [ (vRSS 的虛擬接收端調整) ](../vrss/vrss-top.md)。

## <a name="virtual-machine-quality-of-service-vmqos"></a>虛擬機器服務品質 (vmQoS) 

虛擬機器服務品質是一項 Hyper-v 功能，可讓交換器設定每個 VM 所產生之流量的限制。 它也可讓 VM 保留外部網路連線上的頻寬量，讓一個 VM 無法使另一個 VM 的頻寬。 在 Windows Server 2016 SDN v2 堆疊中，SDN QoS 會取代 vmQoS。

vmQoS 可以設定輸出限制和輸出保留。 建立 Hyper-v 交換器之前，您必須先決定輸出保留模式 (相對權數或絕對頻寬) 。

-  使用 New-VMSwitch PowerShell Cmdlet 的– MinimumBandwidthMode 參數，判斷輸出保留模式。

-  使用 Set-VMNetworkAdapter PowerShell Cmdlet 上的– Maximumbandwidth 值參數，設定輸出限制的值。

-  使用 Set VMNetworkAdapter PowerShell Cmdlet 的下列任一參數，設定輸出保留的值：

   -  如果 New-VMSwitch Cmdlet 上的– MinimumBandwidthMode 參數是絕對的，則在 Set VMNetworkAdapter Cmdlet 上設定– MinimumBandwidthAbsolute 參數。

   -  如果 New-VMSwitch Cmdlet 上的– MinimumBandwidthMode 參數為權數，請在 Set VMNetworkAdapter Cmdlet 上設定– MinimumBandwidthWeight 參數。

由於這項功能使用的演算法有所限制，因此建議最高的權數或絕對頻寬不超過最小權數或絕對頻寬的20倍。 如果需要更多控制，請考慮使用 SDN 堆疊和 SDN-QoS 功能。


---
