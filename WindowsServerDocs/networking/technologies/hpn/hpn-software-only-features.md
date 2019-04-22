---
title: 僅限軟體 (SO) 功能和技術
description: 這些功能會實作為作業系統的一部分，而且是獨立的基礎 NIC。 有時這些功能都需要一些調整的 nic，最佳的作業。 這些範例包括 hyper-v 功能，例如虛擬機器的服務品質 (vmQoS)、 存取控制清單 (Acl)，以及非 HYPER-V 功能，例如 NIC 小組。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: 504bc92971e778b468812dc4064fa6f0afff87ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823779"
---
# <a name="software-only-so-features-and-technologies"></a>僅限軟體 (SO) 功能和技術
軟體功能會實作為作業系統的一部分，而且無關基礎 NIC。 有時這些功能都需要一些調整的 nic，最佳的作業。 這些範例包括 hyper-v 功能，例如虛擬機器的服務品質 (vmQoS)、 存取控制清單 (Acl)，以及非 HYPER-V 功能，例如 NIC 小組。

## <a name="access-control-lists-acls"></a>存取控制清單 (Acl)

HYPER-V 和 SDNv1 管理 vm 的安全性功能。 這項功能適用於非虛擬化 HYPER-V 堆疊和 HVNv1 堆疊。 您可以管理 HYPER-V 切換透過 Acl [Add-vmnetworkadapteracl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapteracl?view=win10-ps)並[Remove-vmnetworkadapteracl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) PowerShell cmdlet。

## <a name="extended-acls"></a>延伸的 Acl

HYPER-V 虛擬交換器延伸 Acl 可讓您設定 HYPER-V 虛擬交換器延伸連接埠 Acl 來提供防火牆保護，並在資料中心內的租用戶 Vm 強制執行安全性原則。 因為連接埠 Acl 會設定為 HYPER-V 虛擬交換器上，而不是 Vm 內，系統管理員可以在多租用戶環境中的所有租用戶管理安全性原則。

您可以管理 HYPER-V 交換器延伸 Acl，透過[Add-vmnetworkadapterextendedacl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapterextendedacl?view=win10-ps)並[Remove-vmnetworkadapterextendedacl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) PowerShell cmdlet。

>[!TIP] 
>這項功能適用於 HNVv1 堆疊。 SDN 堆疊中的 Acl，請參閱軟體定義網路 SDN） 下的 Acl。

如需有關擴充通訊埠存取控制清單中此程式庫的詳細資訊，請參閱[使用擴充通訊埠存取控制清單建立安全性原則](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/Create-Security-Policies-with-Extended-Port-Access-Control-Lists)。

## <a name="nic-teaming"></a>NIC 小組

NIC 小組，也稱為 NIC bonding 是彙總的多個 NIC 連接埠至實體主機視為單一的 NIC 連接埠。 NIC 小組可防止失敗的單一 NIC 連接埠 （或纜線連接到它）。 它也會彙總更快速的輸送量的網路流量。 如需詳細資訊，請參閱 < [NIC 小組](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)。

Windows Server 2016 中，您有兩種方式可進行小組：

1.  Windows Server 2012 小組解決方案

2.  Windows Server 2016 交換器內嵌小組 (SET)


## <a name="rsc-in-the-vswitch"></a>RSC 在 vSwitch

接收區段聯合 (RSC) 中的 vSwitch 是一項功能，會採用封包會在同一個資料流，且到達之間網路中斷和交給作業系統之前將它們聯合成單一的封包。 在 Windows Server 2019 虛擬交換器有這項功能。 如需進一步瞭解這項功能的詳細資訊，請參閱[接收區段聯合中 vSwitch](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch)。

## <a name="software-defined-networking-sdn-acls"></a>軟體定義網路 (SDN) 的 Acl

Windows Server 2016 中的 SDN 擴充功能改善方法來支援 Acl。 在 Windows Server 2016 SDN v2 堆疊中，會使用 SDN Acl，而非 Acl，並將延伸 Acl。 您可以使用網路控制卡管理 SDN Acl。 

## <a name="sdn-quality-of-service-qos"></a>SDN 的服務品質 (QoS)

Windows Server 2016 中的 SDN 擴充功能改進的 5 個 tuple 的基礎上提供 （輸出保留項目、 輸出限制，以及輸入） 的頻寬控制的方式。 一般而言，這些原則會套用 vNIC 或 vmNIC 層級，但您可以使它們更特定的。 在 Windows Server 2016 SDN v2 堆疊中，會使用 SDN QoS，而不是 vmQoS。 您可以使用網路控制卡管理 SDN QoS。

## <a name="switch-embedded-teaming-set"></a>交換器內嵌小組 (SET)

集合是替代的 NIC 小組解決方案，您可以使用包含 Windows Server 2016 中的 HYPER-V 和軟體定義網路 (SDN) 堆疊的環境中。 設定會將部分 NIC 小組功能整合到 HYPER-V 虛擬交換器。 如需交換器內嵌小組在此文件庫中的資訊，請參閱[遠端直接記憶體存取 (RDMA) 和 Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)。

## <a name="virtual-receive-side-scaling-vrss"></a>虛擬接收端調整 (vRSS)

軟體 vRSS 用來分散到多個邏輯處理器 (LPs) VM 的目的地 vm 的連入流量。 軟體 vRSS 可讓的 VM 能夠處理更多的網路流量，比單一 LP 就能夠處理。 如需詳細資訊，請參閱 <<c0> [ 虛擬接收端調整 (vRSS)](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top)。

## <a name="virtual-machine-quality-of-service-vmqos"></a>虛擬機器的服務品質 (vmQoS)

虛擬機器服務品質是 HYPER-V 功能，可讓切換到每個 VM 所產生的流量設定限制。 它也可讓要保留的頻寬量，在外部網路連線，讓一部 VM，不能佔用頻寬的另一個 VM 的 VM。 在 Windows Server 2016 SDN v2 堆疊，SDN QoS 來取代 vmQoS。

vmQoS 可以設定輸出限制和輸出保留項目。 建立 HYPER-V 交換器之前，您必須決定輸出保留模式 （相對權數或絕對的頻寬）。

-  判斷 New-vmswitch PowerShell cmdlet 的 – MinimumBandwidthMode 參數的輸出保留模式。

-  Set-vmnetworkadapter PowerShell cmdlet 將輸出限制 – maximumbandwidth 的值參數的值。

-  設定輸出的保留項目使用下列參數設定 VMNetworkAdapter PowerShell cmdlet 的值：

   -  如果絕對 New-vmswitch cmdlet 的 – MinimumBandwidthMode 參數，然後將 – MinimumBandwidthAbsolute 參數上設定 VMNetworkAdapter cmdlet。

   -  如果需 New-vmswitch cmdlet 的 – MinimumBandwidthMode 參數是加權，然後將 – MinimumBandwidthWeight 參數上設定 VMNetworkAdapter cmdlet。

因為此功能所使用的演算法中的限制，我們建議的最高的權數或絕對頻寬不完全次數超過 20 次的最低的權數或絕對的頻寬。 如果需要更多的控制，請考慮使用 SDN 堆疊和 SDN QoS 功能。


---