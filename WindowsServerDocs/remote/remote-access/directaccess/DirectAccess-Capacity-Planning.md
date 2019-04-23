---
title: DirectAccess 容量規劃
description: 您可以使用本主題的 Windows Server 2012 DirectAccess 伺服器的效能上的報表，可協助您進行 Windows Server 2016 中的 DirectAccess 容量規劃。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 456e5971-3aa7-4a24-bc5d-0c21fec7687e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 37fd165b5a943be15eda007bf22c3081e27b799a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843619"
---
# <a name="directaccess-capacity-planning"></a>DirectAccess 容量規劃

>適用於：Windows Server （半年通道），Windows Server 2016

本文件是有關 Windows Server 2012 DirectAccess 伺服器效能的報告。 執行測試是為了判斷使用高階電腦硬體和低階電腦硬體的輸送量容量。 高階和低階 CPU 效能取決於網路流量輸送量和使用的用戶端類型。 一般 DirectAccess 部署 (以及這些測試的基礎) 是由 1/3 (30%) IPHTTPS 用戶端和 2/3 (70%) Teredo 用戶端組成。 Teredo 用戶端的部分效能比 IPHTTPS 用戶端高，因為 Windows Server 2012 利用接收端調整 (RSS) 而允許使用所有 CPU 核心。 在這些測試中，由於已啟用 RSS，因此停用了超執行緒。 此外，Windows Server 2012 中的 TCP/IP 支援 UDP 流量，允許 Teredo 用戶端跨 CPU 的負載平衡。  
  
收集的資料來自於低階 (4 核心、4GB) 伺服器和高階 (8 核心、8GB) 伺服器中較為典型的硬體。  以下是 750 個用戶端 （562 個 Teredo，188 個 IPHTTPS） 的低階硬體上新的 Windows 8 工作管理員的螢幕擷取畫面執行 ~ 77 Mbits/sec。這是模擬沒有智慧卡認證的使用者。  
  
這些測試結果指出 Windows 8 中 Teredo 的執行效能優於 IPHTTPS，但是與 Windows 7 相較，Teredo 與 IPHTTPS 頻寬使用量都顯著提升。  
  
![測試結果](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning1.gif)  
  
## <a name="high-end-hardware-test-environment"></a>高階硬體測試環境  
以下圖表顯示高階硬體效能測試環境的結果。 本文件會詳細說明所有測試結果和分析。  
  
||||  
|-|-|-|  
|設定-硬體|低階硬體 (4GB RAM、4 核心)|高階硬體 (8 GB、8 核心)|  
|雙通道<br /><br />-   PKI<br /><br />-   Including DNS64/NAT64|750 個並行連線，使用 50% CPU、50 % 記憶體，Corpnet NIC 輸送量 75 Mbps。 延伸目標是 1000 位使用者使用 50% CPU。|1500 個並行連線，在 50%的 CPU，50%記憶體，Corpnet NIC 輸送量 150 Mbps。|  
## <a name="test-environment"></a>測試環境

**效能測試拓撲**  
  
![測試環境](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning2.gif)  
  
效能測試環境是以 5 部電腦為基準。 在低階測試中，使用一部 4 核心 4 GB DirectAccess 伺服器；在高階硬體測試中，使用一部 8 核心 16 GB DirectAccess 伺服器。 低階和高階測試環境都使用一部後端伺服器 (傳送者) 和兩部用戶端電腦 (接收者)。  接收者分割為兩部用戶端電腦。 否則，接收者會與 CPU 繫結且限制用戶端的數量和頻寬。 在接收端，模擬器會模擬數百個用戶端 (模擬 HTTPS 或 Teredo 用戶端)。 IPsec、DOSp 兩者都已設定。 DirectAccess 伺服器上已啟用 RSS。 RSS 佇列大小設定為 8。  如果不設定 RSS，單一處理器就會一直處於高使用率，而不會用到其他核心。 另外也要注意，DirectAccess 伺服器是 4 核心電腦且已關閉超執行緒。  超執行緒關閉的原因是 RSS 只能在實體核心上運作，使用超執行緒會使結果失真 (這表示不會統一載入所有核心)。  
  
## <a name="testing-results-for-low-end-hardware"></a>低階硬體的測試結果：

測試是在 1000 和 750 個用戶端執行。  在所有情況下，流量都分割為 70% Teredo 和 30% IPHTTPS。  所有測試都包含 Nat64 上的 TCP 流量，每個用戶端使用 2 個 IPsec 通道。  在所有測試中，記憶體使用率很少量，且 CPU 使用率在可接受範圍內。  
  
**個別的測試結果：**  
  
下列各節說明個別測試。 每節的標題會突顯每個測試的主要元素，後面接著結果的摘要描述，然後是顯示詳細結果資料的圖表。  
  
**低階效能：750 個用戶端、 70/30 分割、 84.17 Mbits/sec 輸送量：**  
  
以下三個測試表示低階硬體。  在以下的測試執行中，有 750 個用戶端，輸送量為 84.17 Mbits/sec，流量分割為 562 個 Teredo 和 188 個 IPHTTPS。 Teredo MTU 設定為 1472 且已啟用 Teredo 分流器。 三個測試的平均 CPU 使用率為 46.42%，平均記憶體使用率以總可用記憶體 4GB 的認可位元組百分比表示，為 25.95%。  
  
||||||||  
|-|-|-|-|-|-|-|  
|**案例**|**CPUAvg （來自計數器）**|**Mbit/s （公司端）**|**Mbit/s (網際網路端)**|**Active QMSA**|**Active MMSA**|**記憶體使用率 （4 Gb 系統）**|  
|**低階硬體。562 個 Teredo 用戶端。188 個 IPHTTPS 用戶端中。**|47.7472542|84.3|119.13|1502.05|1502.1|26.27%|  
|**低階硬體。562 個 Teredo 用戶端。188 個 IPHTTPS 用戶端中。**|46.3889778|84.146|118.73|1501.25|1501.2|25.90%|  
|**低階硬體。562 個 Teredo 用戶端。188 個 IPHTTPS 用戶端中。**|45.113082|84.0494|118.43|1546.14|1546.1|25.68%|  
  
**1000 個用戶端、 70/30 分割、 78 Mbits/sec 輸送量：**  
  
以下三個測試表示低階硬體上的效能。 在以下的測試執行中，有 1000 個用戶端，平均輸送量為 ~78.64 Mbits/sec，流量分割為 700 個 Teredo 和 300 個 IPHTTPS。  Teredo MTU 設定為 1472 且已啟用 Teredo 分流器。  平均 CPU 使用率為 ~50.7%，平均記憶體使用率以總可用記憶體 4GB 的認可位元組百分比表示，為 ~28.7%。  
  
||||||||  
|-|-|-|-|-|-|-|  
|**案例**|**CPUAvg （來自計數器）**|**Mbit/s （公司端）**|**Mbit/s (網際網路端)**|**Active QMSA**|**Active MMSA**|**記憶體使用率 （4 Gb 系統）**|  
|**低階硬體。700 的 teredo 用戶端。300 個 IPHTTPS 用戶端中。**|51.28406247|78.6432|113.19|2002.42|1502.1|25.59%|  
|**低階硬體。700 的 teredo 用戶端。300 個 IPHTTPS 用戶端中。**|51.06993128|78.6402|113.22|2001.4|1501.2|30.87%|  
|**低階硬體。700 的 teredo 用戶端。300 個 IPHTTPS 用戶端中。**|49.75235617|78.6387|113.2|2002.6|1546.1|30.66%|  
  
**1000 個用戶端、 70/30 分割、 109 Mbits/sec 輸送量：**  
  
在以下的三個測試執行中，有 1000 個用戶端，平均輸送量為 ~109.2 Mbits/sec，流量分割為 700 個 Teredo 和 300 個 IPHTTPS。 Teredo MTU 設定為 1472 且已啟用 Teredo 分流器。 三個測試的平均 CPU 使用率為 ~59.06%，平均記憶體使用率以總可用記憶體 4GB 的認可位元組百分比表示，為 ~27.34%。  
  
||||||||  
|-|-|-|-|-|-|-|  
|**案例**|**CPUAvg （來自計數器）**|**Mbit/s （公司端）**|**Mbit/s (網際網路端)**|**Active QMSA**|**Active MMSA**|**記憶體使用率 （4 Gb 系統）**|  
|**低階硬體。700 的 teredo 用戶端。300 個 IPHTTPS 用戶端中。**|59.81640675|108.305|153.14|2001.64|2001.6|24.38%|  
|**低階硬體。700 的 teredo 用戶端。300 個 IPHTTPS 用戶端中。**|59.46473798|110.969|157.53|2005.22|2005.2|28.72%|  
|**低階硬體。700 的 teredo 用戶端。300 個 IPHTTPS 用戶端中。**|57.89089768|108.305|153.14|1999.53|2018.3|24.38%|  
  
## <a name="testing-results-for-high-end-hardware"></a>高階硬體的測試結果：  
測試是在 1500 個用戶端執行。 流量分割為 70% Teredo 和 30% IPHTTPS。 所有測試都包含 Nat64 上的 TCP 流量，每個用戶端使用 2 個 IPsec 通道。 在所有測試中，記憶體使用率很少量，且 CPU 使用率在可接受範圍內。  
  
**個別的測試結果：**  
  
下列各節說明個別測試。 每節的標題會突顯每個測試的主要元素，後面接著結果的摘要描述，然後是包含詳細結果資料的圖表。  
  
**1500 個用戶端、 70/30 分割、 153.2 Mbits/sec 輸送量**  
  
以下五個測試表示高階硬體。 在以下的測試執行中，有 1500 個用戶端，平均輸送量為 153.2 Mbits/sec，流量分割為 1050 個 Teredo 和 450 個 IPHTTPS。 五個測試的平均 CPU 使用率為 50.68%，平均記憶體使用率以總可用記憶體 8GB 的認可位元組百分比表示，為 22.25%。  
  
||||||||  
|-|-|-|-|-|-|-|  
|**案例**|**CPUAvg （來自計數器）**|**Mbit/s （公司端）**|**Mbit/s (網際網路端)**|**Active QMSA**|**Active MMSA**|**記憶體使用率 （4 Gb 系統）**|  
|**高階硬體。1050 teredo 用戶端。450 個 IPHTTPS 用戶端中。**|51.712437|157.029|216.29|3000.31|3046|21.58%|  
|**高階硬體。1050 teredo 用戶端。450 個 IPHTTPS 用戶端中。**|48.86020205|151.012|206.53|3002.86|3045.3|21.15%|  
|**高階硬體。1050 teredo 用戶端。450 個 IPHTTPS 用戶端中。**|52.23979519|155.511|213.45|3001.15|3002.9|22.90%|  
|**高階硬體。1050 teredo 用戶端。450 個 IPHTTPS 用戶端中。**|51.26269767|155.09|212.92|3000.74|3002.4|22.91%|  
|**高階硬體。1050 teredo 用戶端。450 個 IPHTTPS 用戶端中。**|50.15751307|154.772|211.92|3000.9|3002.1|22.93%|  
|**高階硬體。1050 teredo 用戶端。450 個 IPHTTPS 用戶端中。**|49.83665607|145.994|201.92|3000.51|3006|22.03%|  
  
![高階硬體測試結果](../../media/DirectAccess-Capacity-Planning/DACapacityPlanning3.gif)  
  


