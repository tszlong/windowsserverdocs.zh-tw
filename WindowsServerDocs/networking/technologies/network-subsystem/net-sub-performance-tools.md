---
title: 網路工作負載的效能工具
description: 本主題是 Windows Server 2016 的網路子系統效能調整指南的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 07/16/2018
ms.openlocfilehash: e71c5f34041145907c30b279dc91a94c03c2abed
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824929"
---
# <a name="performance-tools-for-network-workloads"></a>網路工作負載的效能工具

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題以了解效能工具。

本主題包含有關用戶端伺服器流量工具、 TCP/IP 視窗大小，以及 Microsoft Server Performance Advisor 的章節。

##  <a name="bkmk_tuning"></a> 用戶端至伺服器的資料傳輸工具

用戶端到伺服器的流量\(ctsTraffic\)工具可讓您能夠建立並驗證網路流量。

如需詳細資訊，以及下載工具，請參閱[ctsTraffic （用戶端對伺服器流量）](https://github.com/Microsoft/ctsTraffic)。
  
##  <a name="bkmk_size"></a> TCP/IP 視窗大小

1 GB 介面卡，如上表所示的設定應該提供良好的輸送量因為 NTttcp 透過特定的邏輯處理器選項將預設的 TCP 視窗大小設定為 64k \(SO_RCVBUF\)連接。 這是低延遲網路上提供良好的效能。  

相反地，對於高延遲網路或 10 GB 介面卡，NTttcp 預設 TCP 視窗大小的值會產生比最佳效能少。 在這兩種情況下，您必須調整 TCP 視窗大小，以允許較大的頻寬延遲乘積。  

您以靜態方式可以設定為大數值的 TCP 視窗大小，使用 **-rb**選項。 此選項會停用 TCP 視窗自動微調，我們建議使用者完全了解 TCP/IP 行為的結果變更時，才使用它。 根據預設，TCP 視窗大小會設定為足夠的值，並調整只有在高載量下或透過高延遲連結。  

##  <a name="bkmk_advisor"></a> Microsoft Server Performance Advisor

Microsoft Server Performance Advisor \(SPA\)可協助 IT 系統管理員收集計量，以識別、 比較及診斷潛在的效能問題，在 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 WindowsServer 2008 R2 或 Windows Server 2008 部署。 

SPA 會產生完整的診斷報表和圖表，並提供建議來協助您快速分析問題，以及開發的修正動作。  
  
 如需詳細資訊，以及下載 advisor，請參閱[Microsoft Server Performance Advisor](https://msdn.microsoft.com/library/windows/hardware/dn481522.aspx) Windows 硬體開發人員中心。

本指南中的所有主題的連結，請參閱[網路子系統效能調整](net-sub-performance-top.md)。