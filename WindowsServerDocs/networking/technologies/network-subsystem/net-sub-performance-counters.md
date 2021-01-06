---
title: 網路相關的效能計數器
description: 本主題是 Windows Server 2016 的網路子系統效能微調指南的一部分。
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.date: 08/07/2020
ms.openlocfilehash: df57714980a6dce5187cd01d1da74e703d6cefca
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949024"
---
# <a name="network-related-performance-counters"></a>網路相關的效能計數器

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題列出與管理網路效能相關的計數器，並包含下列各節。

-   [資源使用率](#bkmk_ru)

-   [潛在的網路問題](#bkmk_np)

-   [ (RSC) 效能的接收端聯合](#bkmk_rsc)

##  <a name="resource-utilization"></a><a name="bkmk_ru"></a> 資源使用率

以下是與網路資源使用率相關的效能計數器。

- IPv4、IPv6

  -   接收的資料包/秒

  -   傳送的資料包/sec

- TCPv4, TCPv6

  -   接收的區段數/秒

  -   Segments Sent/sec

  -   重新傳輸的區段數/秒

- 網路介面 ( * ) ，網路介面卡 (\*) 

  - Bytes Received/sec

  - Bytes Sent/sec

  - Packets Received/sec

  - Packets Sent/sec

  - 輸出佇列長度

    此計數器是封包中輸出封包佇列的長度 \( \) 。 如果超過2，就會發生延遲。 如果有的話，您應該會發現瓶頸並消除瓶頸。 因為 NDIS 會將要求排在佇列中，所以此長度應該一律為0。

- 處理器資訊

  - % Processor Time

  - 中斷數/秒

  - Dpc 已排入佇列/秒

    此計數器是將 Dpc 新增至邏輯處理器 DPC 佇列的平均速率。 每個邏輯處理器都有自己的 DPC 佇列。 此計數器會測量將 Dpc 新增至佇列的速率，而不是佇列中的 Dpc 數目。 它會顯示最後兩個樣本中觀察到的值之間的差異，並除以樣本間隔的持續時間。

##  <a name="potential-network-problems"></a><a name="bkmk_np"></a> 潛在的網路問題

下列效能計數器與潛在的網路問題有關。

-   網路介面 ( * ) ，網路介面卡 (\*) 

    -   已丟棄接收封包

    -   已收到封包錯誤

    -   已丟棄輸出封包

    -   輸出封包錯誤

-   WFPv4, WFPv6

    -   捨棄的封包數/秒

-   UDPv4, UDPv6

    -   收到的資料包錯誤

-   TCPv4, TCPv6

    -   連接失敗

    -   Connections Reset

-   網路 QoS 原則

    -   丟棄的封包

    -   丟棄的封包數/秒

-   每個處理器網路介面卡活動

    -   資源接收指示下限/秒

    -   資源接收的低封包數/秒

-   Microsoft Winsock BSP

    -   丟棄的資料包

    -   丟棄的資料包/sec

    -   拒絕的連線

    -   拒絕的連線數/秒

##  <a name="receive-side-coalescing-rsc-performance"></a><a name="bkmk_rsc"></a> (RSC) 效能的接收端聯合

下列效能計數器與 RSC 效能相關。

-   網路介面卡 ( * ) 

    -   TCP Active RSC 連接

    -   TCP RSC 平均封包大小

    -   TCP RSC 合併的封包數/秒

    -   TCP RSC 例外狀況/秒

如需本指南中所有主題的連結，請參閱 [網路子系統效能微調](net-sub-performance-top.md)。