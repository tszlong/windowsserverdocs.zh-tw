---
title: 網路相關的效能計數器
description: 本主題是部分的 Windows Server 2016 的網路效能子系統調整節目表。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 7ebaa271-2557-4c24-a679-c3d863e6bf9e
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 33551dfd4f76bc13ba69863b782ddae279e0ad16
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="network-related-performance-counters"></a>網路相關的效能計數器

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題列出的計數器相關管理網路效能，並包含下列各節。  
  
-   [資源使用量](#bkmk_ru)  
  
-   [潛在網路的問題](#bkmk_np)  
  
-   [接收效能側邊聯合 (RSC)](#bkmk_rsc)  
  
##  <a name="bkmk_ru"></a>資源使用量  

下列效能計數器是與網路資源使用量。  
  
-   IPv4，請 IPv6  
  
    -   資料包收到秒  
  
    -   傳送資料包日秒  
  
-   TCPv4 TCPv6  
  
    -   區段收到秒  
  
    -   區段會傳送秒  
  
    -   區段秒傳輸  
  
-   網路 Interface(*)、網路 Adapter(\*)  
  
    -   位元組收到秒  
  
    -   傳送位元組日秒  
  
    -   封包收到秒  
  
    -   傳送封包日秒  
  
    -   輸出佇列長度  
  
     這個計數器是輸出封包佇列 \(in packets\) 長度。 如果這是超過 2，就會發生延遲。 您應該找出瓶頸，如果您可以將它移除。 NDIS 佇列要求，因為此長度一律應為 0。  
  
-   處理器資訊  
  
    -   %處理器時間  
  
    -   中斷秒  
  
    -   Dpc 佇列秒  
  
     這個計數器是 Dpc 已加入到邏輯處理器 DPC 佇列平均比例。 每個邏輯處理器都有它自己 DPC 佇列。 這個計數器測量 Dpc 加入至佇列，不 Dpc 佇列中數目的速度。 它會顯示發現上次兩個樣本，樣本間隔持續值之間的人而有所不同。  
  
##  <a name="bkmk_np"></a>潛在網路的問題  

下列效能計數器的相關潛在網路問題。  
  
-   網路 Interface(*)、網路 Adapter(\*)  
  
    -   捨棄收到的封包  
  
    -   收到錯誤封包。  
  
    -   封包輸出捨棄  
  
    -   封包輸出錯誤  
  
-   WFPv4 WFPv6  
  
    -   捨棄的封包日秒

-   UDPv4 UDPv6

    -   資料包收到錯誤  
  
-   TCPv4 TCPv6  
  
    -   連接失敗  
  
    -   連接重設  
  
-   網路 QoS 原則  
  
    -   捨棄的封包  
  
    -   卸除封包日秒  
  
-   每個處理器網路介面卡活動  
  
    -   低資源收到指示秒  
  
    -   低資源收到封包秒  
  
-   Microsoft Winsock BSP  
  
    -   卸除的資料包  
  
    -   卸除的資料包秒  
  
    -   拒絕連接情形  
  
    -   連接拒絕的秒  
  
##  <a name="bkmk_rsc"></a>接收效能側邊聯合 (RSC)  

下列效能計數器的相關 RSC 效能。  
  
-   網路 Adapter(*)  
  
    -   TCP 使用 RSC 連接情形  
  
    -   TCP RSC 平均封包大小  
  
    -   TCP RSC 聯合封包秒  
  
    -   TCP RSC 例外秒

本指南所有主題的連結，會看到[的網路效能子系統調整](net-sub-performance-top.md)。