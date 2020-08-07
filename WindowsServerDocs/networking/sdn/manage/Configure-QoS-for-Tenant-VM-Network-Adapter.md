---
title: '設定租使用者 VM 網路介面卡的服務品質 (QoS) '
description: 當您設定租使用者 VM 網路介面卡的 QoS 時，您可以選擇 [資料中心橋接] \( DCB \) 或 [軟體定義的網路] \( SDN \) QoS。
manager: grcusanz
ms.topic: article
ms.assetid: 6d783ff6-7dd5-496c-9ed9-5c36612c6859
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/23/2018
ms.openlocfilehash: 95307dafd077f1efc3b44131494ced709334a2d8
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962223"
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>設定租使用者 VM 網路介面卡的服務品質 (QoS) 

>適用於：Windows Server (半年度管道)、Windows Server 2016

當您設定租使用者 VM 網路介面卡的 QoS 時，您可以選擇 [資料中心橋接] \( DCB \) 或 [軟體定義的網路] \( SDN \) QoS。

1.    **DCB**。 您可以使用 Windows PowerShell NetQoS Cmdlet 來設定 DCB。 如需範例，請參閱[遠端直接記憶體存取 (RDMA) 和切換內嵌小組 (設定) ](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)主題中的「啟用資料中心橋接」一節。

2.    **SDN QoS**。 您可以使用網路控制卡來啟用 SDN QoS，這可設定為限制虛擬介面上的頻寬，以防止高流量的 VM 封鎖其他使用者。  您也可以設定 SDN QoS 來保留 VM 的特定頻寬量，以確保不論網路流量的數量為何，VM 皆可供存取。

透過網路介面內容的埠設定，套用所有 SDN QoS 設定。 如需詳細資訊，請參閱下表。

|元素名稱|描述|
|------------|-----------|
|macSpoofing| 允許 Vm 將傳出封包中的來源媒體存取控制 \( MAC \) 位址變更為未指派給 VM 的 MAC 位址。<p>允許的值：<ul><li>[已啟用] –使用不同的 MAC 位址。</li><li>已停用-僅使用指派給它的 MAC 位址。</li></ul>|
|arpGuard| 只允許 ArpFilter 中指定的 ARP 防護位址通過埠。<p>允許的值：<ul><li>已啟用-允許</li><li>Disabled –不允許</li></ul>|
|dhcpGuard| 允許或卸載宣告為 DHCP 伺服器之 VM 中的任何 DHCP 訊息。 <p>允許的值：<ul><li>[已啟用] –丟棄 DHCP 訊息，因為虛擬化的 DHCP 伺服器被視為不受信任。</li><li>Disabled –允許接收訊息，因為虛擬化的 DHCP 伺服器被視為值得信任。</li></ul>|
|stormLimit| 每秒允許 VM 透過虛擬網路介面卡傳送的封包數 (廣播、多播及未知的單播) 。 一秒內的封包會被捨棄超過限制。 值為 \( 0 \) 時，表示沒有限制。。|
|portFlowLimit| 允許為埠執行的流程數目上限。 空白或零0的值 \( \) 表示沒有任何限制。 |
|vmqWeight| 相對權數描述虛擬網路介面卡的親和性，以使用虛擬機器佇列 (VMQ) 。 值的範圍是0到100。<p>允許的值：<ul><li>0–停用虛擬網路介面卡上的 VMQ。</li><li>1-100 –在虛擬網路介面卡上啟用 VMQ。</li></ul>|
|iovWeight| 相對權數會將虛擬網路介面卡的親和性設定為指派的單一根目錄 i/o 虛擬化 \( sr-iov 虛擬函式 \) 。 <p>允許的值：<ul><li>0-停用虛擬網路介面卡上的 SR-IOV。</li><li>1-100 –在虛擬網路介面卡上啟用 SR-IOV。</li></ul>|
|iovInterruptModeration|<p>允許的值：<ul><li>預設–實體網路介面卡廠商的設定會決定此值。</li><li>性 </li><li>關 </li><li>low</li><li>中</li><li>high</li></ul><p>如果您選擇 [**預設**]，實體網路介面卡廠商的設定會決定值。  如果您選擇 [彈性 **]，運行**時間流量模式就會 determins 中斷仲裁速率。|
|iovQueuePairsRequested| 配置給 SR-IOV 虛擬函式的硬體佇列配對數目。 如果需要接收端調整 \( RSS \) ，而且系結至虛擬交換器的實體網路介面卡支援 sr-iov 虛擬函式上的 rss，則需要一組以上的佇列。 <p>允許的值：1到4294967295。|
|QosSettings| 設定下列 Qos 設定，全都是選擇性的： <ul><li>**outboundReservedValue** -如果 outboundReservedMode 為「絕對」，則值表示保證虛擬埠傳輸 (輸出) 的頻寬（以 Mbps 為單位）。 如果 outboundReservedMode 為「權數」，則此值會指出所保證之頻寬的加權部分。</li><li>**outboundMaximumMbps** -指出虛擬埠 (輸出) 的最大允許傳送端頻寬（以 Mbps 為單位）。</li><li>**InboundMaximumMbps** -指出虛擬通訊埠 (輸入) 的最大允許接收端頻寬（以 Mbps 為單位）。</li></ul> |

---