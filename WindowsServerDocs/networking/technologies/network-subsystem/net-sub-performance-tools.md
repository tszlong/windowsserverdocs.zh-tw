---
title: 效能網路工作負載的工具
description: 本主題是部分的 Windows Server 2016 的網路效能子系統調整節目表。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c7789781-87e8-464e-981b-af887d01badd
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 81c31871b3dfa4644690fe074ae15eaaa680d7f2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="performance-tools-for-network-workloads"></a>效能網路工作負載的工具

>適用於：Windows Server（以每年次管道）、Windows Server 2016

若要深入了解效能工具，您可以使用此主題。

本主題包含有關伺服器流量工具，TCP/IP 視窗大小和 Microsoft 伺服器效能建議 Client 的各節。

##  <a name="bkmk_tuning"></a>Client 伺服器流量工具

伺服器流量 \(ctsTraffic\) 工具 Client 提供您建立和驗證網路流量的能力。

如需詳細資訊，並下載工具，請查看[（Client 對伺服器流量）ctsTraffic](http://ctstraffic.codeplex.com/)。
  
##  <a name="bkmk_size"></a>TCP/IP 視窗大小

1 GB 介面卡] 設定中的上一個表格應該提供的好輸送量因為 NTttcp 特定的邏輯處理器透過 64 K 來設定預設 TCP 的視窗大小選項 \(SO_RCVBUF\) 連接。 這低延遲網路上提供最佳效能。  

相較之下，高延遲網路或的 10 GB 的介面卡，NTttcp 預設 TCP 視窗大小值會小於最佳效能。 這兩個萬一您必須調整較大的頻寬延遲 product 允許的 TCP 視窗大小。  

您可以靜態為 TCP 視窗大小較大的值，使用**-rb**選項。 這個選項會停用 TCP 視窗自動調整和建議才使用者完全了結果變更 TCP/IP 行為中的使用。 根據預設，TCP 視窗大小設定，不足的值，並調整只在重裝載入，或透過高延遲連結。  

##  <a name="bkmk_advisor"></a>Microsoft 伺服器效能建議

Microsoft 伺服器效能建議 \(SPA\) 協助 IT 系統管理員會收集計量以找出、比較和診斷在 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 部署潛在的效能問題。 

選項產生完整的診斷報告和圖表一起，並提供建議，可協助您快速分析問題並修正動作的開發。  
  
 如需詳細資訊，並下載的建議，請查看[Microsoft 伺服器效能建議](https://msdn.microsoft.com/library/windows/hardware/dn481522.aspx)Windows 硬體開發人員中心。

本指南所有主題的連結，會看到[的網路效能子系統調整](net-sub-performance-top.md)。