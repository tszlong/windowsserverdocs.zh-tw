---
title: 設定服務 (QoS) 品質承租人 VM 網路介面卡
description: 本主題是輔助的軟體定義網路上如何管理承租人工作負載和 Windows Server 2016 Virtual 網路的一部分。
manager: brianlic
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
ms.openlocfilehash: cde4e21beaec58a98a5d5fbe5c4631e2f113dbf7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>設定服務 (QoS) 品質承租人 VM 網路介面卡

>適用於：Windows Server（以每年次管道）、Windows Server 2016

當您設定 QoS 承租人 VM 網路介面卡時，您可以選擇資料中心橋接 \ (DCB\) 或軟體所定義網路 \(SDN\) QoS。

1.  **DCB**。 您可以使用 Windows PowerShell NetQoS cmdlet 設定 DCB。 範例看到「讓資料中心橋接「主題中的區段[遠端直接記憶體存取 (RDMA) 和切換 Embedded 小組（設定）](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

2.  **SDN QoS**。 您可以使用 Network Controller，可以設定限制以防止高流量 VM 封鎖其他使用者介面 virtual 上的頻寬，來讓 SDN QoS。  您也可以設定 SDN QoS 預約特定的頻寬，來確保 VM 的網路流量無論無障礙 VM 的間距。  

所有 SDN Qos 設定的都套用透過連接埠設定，如下表的網路介面屬性。

|項目名稱|描述|
|------------|-----------| 
|macSpoofing|指定 Vm 是否可以變更來源媒體存取控制 \(MAC\) 中的位址輸出封包未指派給 VM 的 MAC 位址。 允許的值」功能的「\（允許使用不同的 MAC address\ VM）和「停用「\（允許使用僅限指派給 it\ 的 MAC 位址 VM）。|
|arpGuard|指定 ARP guard 是否已支援。  ARP guard 可透過連接埠傳遞 ArpFilter 中指定的地址。  」功能的「或者」已停用 [允許的值。
|dhcpGuard|指定要放 DHCP 訊息會 DHCP 伺服器宣告 VM 的。 允許的值是」功能的「，，會捨棄 DHCP 訊息因為模擬的 DHCP 伺服器被視為不受信任，或「已停用「，可模擬的 DHCP 伺服器會被視為可靠，因為接收的訊息。
|stormLimit|指定的秒允許透過指定 virtual 網路介面卡，將 VM 廣播、多點，並未知單封包。 會卸除超過限制該一個第二個間隔期間廣播、多點，並未知單封包。 值零 \(0\) 表示無限制。
|portFlowLimit|指定流程連接埠，可執行的最大。  值空白或零 \(0\) 表示無限制。
|vmqWeight|指定是否一樣佇列 (VMQ) virtual 網路介面卡的支援。 相對減重描述 virtual 網路介面卡用於 VMQ 相關性。 值的範圍是 0 到 100。 指定要停用 virtual 網路介面卡上的 VMQ 0。
|iovWeight|指定單一根 I/O 模擬 \(SR-IOV\) 是否支援這 virtual 網路介面卡上。 相對減重設定 virtual 網路介面卡相關性指派 SR-IOV virtual 函式。 值的範圍是 0 到 100。 指定要停用 virtual 網路介面卡上的 SR-IOV 0。 
|iovInterruptModeration|指定單一根 I/O 模擬 \(SR-IOV\) virtual 函式指派給 virtual 網路介面卡的中斷仲裁值。 允許的值為」預設」、「自動調整]、[關閉]，」低」、「中] 和 [高]。   選擇預設，如果是由實體網路介面卡廠商設定判斷的值。  如果選擇彈性，中斷仲裁速率根據執行階段流量模式。 
|iovQueuePairsRequested|指定的硬體佇列配對 SR-IOV virtual 函式的配置。 如果需要，接收端的縮放比例 \(RSS\) 與繫結至 virtual 切換實體網路介面卡上 SR IOV virtual 支援 RSS 是必要的功能，再高於一個佇列配對。 允許的值範圍是 1 到 4294967295。 
|QosSettings|您可以設定下列 Qos 設定，都是選擇性：  <br/><br />**outboundReservedValue:**<br/>如果 outboundReservedMode」絕對」值表示的頻寬，以 Mbps，保證 virtual 的連接埠，傳輸（輸出）。 如果 outboundReservedMode > 重量「值表示保證頻寬加權的部分。 <br/><br />**outboundMaximumMbps:**  <br/>表示最多允許傳送端的頻寬，在 Mbps，virtual 連接埠（輸出）。 <br/><br/>**InboundMaximumMbps:**  <br/>最多允許接收端頻寬 virtual 連接埠（輸入）Mbps 中的指示。 |