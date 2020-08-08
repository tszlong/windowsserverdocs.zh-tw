---
title: 僅限軟體 (SO) 功能和技術
description: 這些功能會實作為作業系統的一部分，並獨立于基礎 NIC (的) 。 有時候，這些功能需要對 NIC 進行一些調整，以進行最佳操作。 其中的範例包括 Hyper-v 功能，例如虛擬機器服務品質 (vmQoS) 、存取控制清單 (Acl) 和非 Hyper-v 功能（例如 NIC 小組）。
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/20/2018
ms.openlocfilehash: 94b7a4b87a74c24acc97289516f6835be9b90ef7
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990316"
---
# <a name="software-only-so-features-and-technologies"></a>僅限軟體 (SO) 功能和技術
僅限軟體的功能會實作為作業系統的一部分，並獨立于基礎 NIC (的) 。 有時候，這些功能需要對 NIC 進行一些調整，以進行最佳操作。 其中的範例包括 Hyper-v 功能，例如虛擬機器服務品質 (vmQoS) 、存取控制清單 (Acl) 和非 Hyper-v 功能（例如 NIC 小組）。

## <a name="access-control-lists-acls"></a>存取控制清單 (ACL)

用來管理 VM 安全性的 Hyper-v 和 SDNv1 功能。 這項功能適用于非虛擬化的 Hyper-v 堆疊和 HVNv1 堆疊。 您可以透過[get-vmnetworkadapteracl](/powershell/module/hyper-v/add-vmnetworkadapteracl?view=win10-ps)和[Remove-get-vmnetworkadapteracl](/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) PowerShell Cmdlet 來管理 hyper-v 交換器 acl。

## <a name="extended-acls"></a>延伸的 Acl

Hyper-v 虛擬交換器延伸 Acl 可讓您設定 Hyper-v 虛擬交換器擴充通訊埠 Acl，以提供防火牆保護，並為資料中心內的租使用者 Vm 強制執行安全性原則。 由於通訊埠 Acl 是在 Hyper-v 虛擬交換器上設定，而不是在 Vm 內，因此系統管理員可以管理多租使用者環境中所有租使用者的安全性原則。

您可以透過[add-vmnetworkadapterextendedacl](/powershell/module/hyper-v/add-vmnetworkadapterextendedacl?view=win10-ps)和[add-vmnetworkadapterextendedacl](/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) PowerShell Cmdlet 來管理 Hyper-v 交換器擴充的 acl。

>[!TIP]
>這項功能適用于 HNVv1 堆疊。 針對 SDN 堆疊中的 Acl，請參閱下面的軟體定義網路 SDN) Acl。

如需有關此程式庫中擴充埠存取控制清單的詳細資訊，請參閱[使用延伸埠存取控制清單建立安全性原則](../../../virtualization/hyper-v-virtual-switch/create-security-policies-with-extended-port-access-control-lists.md)。

## <a name="nic-teaming"></a>NIC 小組

NIC 小組（也稱為 NIC 結合）是將多個 NIC 埠匯總到主機做為單一 NIC 埠的實體。 NIC 小組可以防止單一 NIC 埠 (或連線到它的纜線) 的失敗。 它也會匯總網路流量，以提高輸送量。 如需詳細資訊，請參閱[NIC](../nic-teaming/nic-teaming.md)小組。

有了 Windows Server 2016，您有兩種方式可以進行小組：

1.  Windows Server 2012 團隊解決方案

2.  Windows Server 2016 交換器內嵌小組 (設定) 


## <a name="rsc-in-the-vswitch"></a>VSwitch 中的 RSC

VSwitch 中 (RSC) 的接收區段聯合是一項功能，它會採用屬於相同資料流程的封包並抵達網路中斷，然後將它們合併成單一封包，然後再將它們傳遞給作業系統。 Windows Server 2019 中的虛擬交換器具有這項功能。 如需這項功能的詳細資訊，請參閱[vSwitch 中的接收區段](./rsc-in-the-vswitch.md)聯合。

## <a name="software-defined-networking-sdn-acls"></a>軟體定義網路 (SDN) Acl

Windows Server 2016 中的 SDN 延伸模組改良了支援 Acl 的方式。 在 Windows Server 2016 SDN v2 堆疊中，會使用 SDN Acl，而不是 Acl 和擴充的 Acl。 您可以使用網路控制卡來管理 SDN Acl。

## <a name="sdn-quality-of-service-qos"></a>SDN 服務品質 (QoS)

Windows Server 2016 中的 SDN 延伸模組改良了提供頻寬控制 (的輸出保留、輸出限制和輸入限制，) 以5個元組為基礎。 這些原則通常會套用在 vNIC 或 vmNIC 層級，但您可以讓它們更明確。 在 Windows Server 2016 SDN v2 堆疊中，會使用 SDN QoS，而不是 vmQoS。 您可以使用網路控制卡來管理 SDN QoS。

## <a name="switch-embedded-teaming-set"></a>切換內嵌小組 (設定) 

SET 是替代的 NIC 小組解決方案，可用於包含 Hyper-v 的環境，以及在 Windows Server 2016 中 (SDN) 堆疊的軟體定義網路功能。 將一些 NIC 小組功能整合到 Hyper-v 虛擬交換器。 如需有關在此程式庫中切換內嵌小組的詳細資訊，請參閱[ (RDMA 的遠端直接記憶體存取) 和切換內嵌小組 (設定) ](../../../virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md)。

## <a name="virtual-receive-side-scaling-vrss"></a>虛擬接收端調整 (vRSS)

軟體 vRSS 用來將以 VM 為目標的連入流量分散到多個邏輯處理器， (LPs) 的 VM 上。 軟體 vRSS 可讓 VM 處理比單一 LP 能夠處理更多網路流量的能力。 如需詳細資訊，請參閱[虛擬接收端調整 (vRSS) ](../vrss/vrss-top.md)。

## <a name="virtual-machine-quality-of-service-vmqos"></a>虛擬機器服務品質 (vmQoS) 

「虛擬機器服務品質」是一項 Hyper-v 功能，可讓交換器設定每個 VM 所產生的流量限制。 此外，它也可讓 VM 保留外部網路連線的頻寬量，讓一個 VM 無法使另一個 VM 以提供頻寬。 在 Windows Server 2016 SDN v2 堆疊中，SDN QoS 會取代 vmQoS。

vmQoS 可以設定輸出限制和輸出保留專案。 在建立 Hyper-v 交換器之前，您必須先決定輸出保留模式 (相對權數或絕對頻寬) 。

-  使用新的 VMSwitch PowerShell Cmdlet 的– MinimumBandwidthMode 參數來決定輸出保留模式。

-  使用 VMNetworkAdapter PowerShell Cmdlet 上的– Maximumbandwidth 值參數來設定輸出限制的值。

-  使用 Set VMNetworkAdapter PowerShell Cmdlet 的下列其中一個參數來設定輸出保留的值：

   -  如果新的-VMSwitch Cmdlet 上的– MinimumBandwidthMode 參數是絕對的，則在 Set VMNetworkAdapter Cmdlet 上設定– MinimumBandwidthAbsolute 參數。

   -  如果新的-VMSwitch Cmdlet 上的– MinimumBandwidthMode 參數是權數，則在 Set VMNetworkAdapter Cmdlet 上設定– MinimumBandwidthWeight 參數。

由於這項功能所使用的演算法有限制，因此，我們建議最高權數或絕對頻寬不能超過最低權數或絕對頻寬的20倍以上。 如果需要更多控制，請考慮使用 SDN 堆疊和 SDN QoS 功能。


---