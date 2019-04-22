---
title: 網路相關的效能計數器
description: 本主題是 Windows Server 2016 的網路子系統效能調整指南的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e5e8abbc19482bcd0dd5670065cde59d5be3169a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824349"
---
# <a name="network-related-performance-counters"></a>網路相關的效能計數器

>適用於：Windows Server （半年通道），Windows Server 2016

本主題會列出與管理網路效能，且包含下列各節的計數器。  
  
-   [資源使用率](#bkmk_ru)  
  
-   [潛在網路問題](#bkmk_np)  
  
-   [接收端聯合 (RSC) 效能](#bkmk_rsc)  
  
##  <a name="bkmk_ru"></a> 資源使用率  

下列效能計數器會與網路資源使用率。  
  
-   IPv4, IPv6  
  
    -   接收資料包數/秒  
  
    -   傳送的資料包/秒  
  
-   TCPv4, TCPv6  
  
    -   Segments Received/sec  
  
    -   傳送的區段/秒  
  
    -   Segments Retransmitted/sec  
  
-   網路 Interface(*)，網路介面卡 (\*)  
  
    -   Bytes Received/sec  
  
    -   傳送的位元組/秒  
  
    -   Packets Received/sec  
  
    -   傳送的封包/秒  
  
    -   輸出佇列長度  
  
     此計數器是輸出封包佇列長度\(封包中\)。 如果值大於 2，就會發生延遲。 您應該找出瓶頸，以及如果您可以將它消除。 NDIS 排入佇列的要求，因為這個長度應該一律是 0。  
  
-   處理器資訊  
  
    -   % Processor Time  
  
    -   Interrupts/sec  
  
    -   DPCs Queued/sec  
  
     此計數器是在 Dpc 已加入邏輯處理器 DPC 佇列的平均速率。 每個邏輯處理器有它自己 DPC 佇列。 此計數器會測量 Dpc 加入至佇列，而不是 Dpc 佇列的數目的速率。 它會顯示在最後兩個範例中，除以樣本間隔的持續時間中未觀察到的值之間的差異。  
  
##  <a name="bkmk_np"></a> 潛在網路問題  

下列效能計數器是潛在的網路問題相關。  
  
-   網路 Interface(*)，網路介面卡 (\*)  
  
    -   已丟棄接收封包  
  
    -   收到的封包錯誤  
  
    -   輸出封包捨棄  
  
    -   輸出封包錯誤  
  
-   WFPv4, WFPv6  
  
    -   捨棄的封包/秒

-   UDPv4, UDPv6

    -   Datagrams Received 錯誤  
  
-   TCPv4, TCPv6  
  
    -   連線失敗  
  
    -   連線重設  
  
-   網路 QoS 原則  
  
    -   捨棄的封包  
  
    -   卸除的封包/秒  
  
-   每個處理器網路介面卡活動  
  
    -   低資源接收指示數/秒  
  
    -   低資源接收的封包/秒  
  
-   Microsoft Winsock BSP  
  
    -   已捨棄的資料包  
  
    -   已捨棄的資料包/sec  
  
    -   拒絕的連線  
  
    -   已拒絕的連線數/秒  
  
##  <a name="bkmk_rsc"></a> 接收端聯合 (RSC) 效能  

下列效能計數器會與 RSC 的效能相關的。  
  
-   網路 Adapter(*)  
  
    -   作用中的 RSC 的 TCP 連線  
  
    -   TCP RSC 平均封包大小  
  
    -   TCP RSC 聯合封包/秒  
  
    -   TCP RSC Exceptions/sec

本指南中的所有主題的連結，請參閱[網路子系統效能調整](net-sub-performance-top.md)。