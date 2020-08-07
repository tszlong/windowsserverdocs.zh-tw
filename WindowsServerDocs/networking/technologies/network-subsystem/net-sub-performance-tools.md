---
title: 網路工作負載的效能工具
description: 本主題是 Windows Server 2016 的網路子系統效能微調指南的一部分。
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.date: 07/16/2018
ms.openlocfilehash: 3e08541b1bfd6dd07d134560c9d03306566b18db
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953914"
---
# <a name="performance-tools-for-network-workloads"></a>網路工作負載的效能工具

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解效能工具。

本主題包含有關用戶端到伺服器流量工具、TCP/IP 視窗大小和 Microsoft Server Performance Advisor 的各節。

##  <a name="client-to-server-traffic-tool"></a><a name="bkmk_tuning"></a>用戶端對伺服器流量工具

[用戶端到伺服器流量 \( ctsTraffic] \) 工具可讓您建立及驗證網路流量。

如需詳細資訊及下載此工具，請參閱[ctsTraffic (用戶端對伺服器流量) ](https://github.com/Microsoft/ctsTraffic)。

##  <a name="tcpip-window-size"></a><a name="bkmk_size"></a>TCP/IP 視窗大小

針對 1 GB 介面卡，上表中顯示的設定應該提供良好的輸送量，因為 NTttcp 會透過連接的特定邏輯處理器 SO_RCVBUF 選項，將預設的 TCP 視窗大小設定為 64 K \( \) 。 這會在低延遲的網路上提供良好的效能。

相反地，針對高延遲網路或 10 GB 介面卡，NTttcp 的預設 TCP 視窗大小值會產生低於最佳效能。 在這兩種情況下，您都必須調整 TCP 視窗大小，以允許較大的頻寬延遲產品。

您可以使用 **-rb**選項，以靜態方式將 TCP 視窗大小設定為較大的值。 此選項會停用 TCP 視窗自動調整，而且只有在使用者完全瞭解 TCP/IP 行為中的結果變更時，才建議使用。 根據預設，TCP 視窗大小會設定為足夠的值，且只會在大量負載或高延遲的連結上調整。

##  <a name="microsoft-server-performance-advisor"></a><a name="bkmk_advisor"></a>Microsoft Server Performance Advisor

Microsoft Server Performance Advisor \( SPA \) 可協助 IT 系統管理員收集計量，以識別、比較及診斷 windows server 2016、windows Server 2012 R2、windows server 2012、windows Server 2008 R2 或 windows server 2008 部署中的潛在效能問題。

SPA 會產生完整的診斷報告和圖表，並提供建議，協助您快速分析問題並開發更正動作。

 如需詳細資訊及下載 advisor，請參閱 Windows 硬體開發人員中心的[Microsoft Server Performance advisor](https://msdn.microsoft.com/library/windows/hardware/dn481522.aspx) 。

如需本指南中所有主題的連結，請參閱[網路子系統效能調整](net-sub-performance-top.md)。