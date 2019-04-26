---
title: 租用戶 VM 網路介面卡設定服務品質 (QoS)
description: 當您的租用戶 VM 網路介面卡設定 QoS 時，您可以選擇資料中心橋接\(DCB\)或軟體定義網路\(SDN\) QoS。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d783ff6-7dd5-496c-9ed9-5c36612c6859
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 0b9ce318c3d249b23d7560e0b6bb90a83e60d64d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880599"
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>租用戶 VM 網路介面卡設定服務品質 (QoS)

>適用於：Windows Server （半年通道），Windows Server 2016

當您的租用戶 VM 網路介面卡設定 QoS 時，您可以選擇資料中心橋接\(DCB\)或軟體定義網路\(SDN\) QoS。

1.  **DCB**。 您可以使用 Windows PowerShell NetQoS cmdlet 設定 DCB。 如需範例，請參閱主題中的 「 啟用資料中心橋接 」 章節[遠端直接記憶體存取 (RDMA) 和 Switch Embedded Teaming (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

2.  **SDN QoS**。 您可以使用網路控制站，可以設定來限制頻寬，以避免高流量 VM 封鎖其他使用者的虛擬介面上，以啟用 SDN QoS。  您也可以設定 SDN QoS，來保留一段特定的 VM，以確保 VM 處於可供存取，不論網路流量的頻寬。  

適用於透過網路介面屬性的連接埠設定所有 SDN QoS 設定。 請參閱下表，如需詳細資訊。

|元素名稱|描述|
|------------|-----------| 
|macSpoofing| 可讓 Vm 變更原始檔的媒體存取控制\(MAC\)與未指派給 VM 的 MAC 位址的傳出封包中的位址。<p>允許的值：<ul><li>已啟用-使用不同的 MAC 位址。</li><li>停用 – 使用只能指派給它的 MAC 位址。</li></ul>|
|arpGuard| 可讓 ARP guard ArpFilter 通過連接埠中指定唯一的位址。<p>允許的值：<ul><li>已啟用-允許</li><li>已停用-不允許</li></ul>|
|dhcpGuard| 允許或卸除任何 DHCP 訊息從宣告為 DHCP 伺服器的 VM。 <p>允許的值：<ul><li>已啟用-卸除 DHCP 訊息，因為虛擬化的 DHCP 伺服器會被視為未受信任的。</li><li>停用-可讓虛擬化的 DHCP 伺服器會被視為高可信度電腦接收訊息。</li></ul>|
|stormLimit| 允許透過虛擬網路介面卡傳送封包 （廣播、 多點傳送及未知單點傳播） 每秒的 VM 數目。 超過限制的一秒的間隔期間的封包遭到捨棄。 值為零\(0\)表示沒有限制...|
|portFlowLimit| 流程可以執行的連接埠的數目上限。 值為空白或零\(0\)表示沒有限制。 |
|vmqWeight| 相對權數描述要使用虛擬機器佇列 (VMQ) 的虛擬網路介面卡親和性。 值的範圍是 0 到 100 之間。<p>允許的值：<ul><li>0 – 停用 VMQ，虛擬網路介面卡。</li><li>1-100 – 可讓虛擬網路介面卡的 VMQ。</li></ul>|
|iovWeight| 相對權數指派的單一根目錄 I/O 虛擬化設定的虛擬網路介面卡親和性\(SR-IOV\)虛擬函式。 <p>允許的值：<ul><li>0 – 停用 SR-IOV 的虛擬網路介面卡。</li><li>1-100 – 啟用 SR-IOV 的虛擬網路介面卡。</li></ul>|
|iovInterruptModeration|<p>允許的值：<ul><li>預設值 – 實體網路介面卡廠商的設定會決定值。</li><li>adaptive - </li><li>off </li><li>低</li><li>中型</li><li>高</li></ul><p>如果您選擇**預設**，實體網路介面卡廠商的設定會決定值。  如果您選擇，**調適性**，執行階段流量模式決定插斷仲裁速率。|
|iovQueuePairsRequested| 配置給 SR-IOV 虛擬函式的硬體佇列對數目。 如果接收端調整\(RSS\)是必要的且如果實體網路介面卡繫結至虛擬交換器支援 RSS 在 SR-IOV 虛擬函式，則需要一個以上的佇列對。 <p>允許的值：1 到 4294967295。|
|QosSettings| 設定下列的 Qos 設定，這些都是選擇性： <ul><li>**outboundReservedValue** -如果 outboundReservedMode 是 「 絕對 」，則這個值指出的頻寬，以 mbps 為單位，保證傳輸 （輸出） 的虛擬連接埠。 如果 outboundReservedMode 是 「 權重 」 值會表示保證頻寬的權重的部分。</li><li>**outboundMaximumMbps** -表示最大虛擬通訊埠 （輸出） 以 mbps 為單位，允許傳送端頻寬。</li><li>**InboundMaximumMbps** -表示最大值，允許接收端的頻寬，讓虛擬通訊埠 （輸入） 以 mbps 為單位。</li></ul> |

---