---
ms.assetid: ''
title: 如需 Windows Server 2019 HPN 功能 insider Preview
description: 深入了解 Windows Server 2019 的新高效能的網路功能。
manager: dougkim
author: shortpatti
ms.author: pashort
ms.date: 09/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 3d3e974472f28c30d093fbd1094ef3693d984d19
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886269"
---
# <a name="insider-preview"></a>Insider Preview


## <a name="dynamic-vrss-and-vmmq"></a>動態 vRSS 和 VMMQ

>適用於：Windows Server 2019

在過去，虛擬機器佇列和虛擬機器多佇列啟用到個別 VMs 的輸送量為先達到 10GbE 標示的網路輸送量和更新版本。 不幸的是，規劃、 設定基準中、 調整和監視所需成功變成大型的工作;多個 IT 系統管理員通常要花費。 

Windows Server 2019 藉由動態分配和微調處理所需的網路工作負載，改善這些最佳化。 Windows Server 2019 可確保尖峰效率，並為 IT 系統管理員移除組態負擔。

如需詳細資訊，請參閱：

-   [公告部落格](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-   [驗證指南適用於 IT 專業人員](https://aka.ms/DVMMQ-Validation)

## <a name="receive-segment-coalescing-rsc-in-the-vswitch"></a>在 vSwitch 中接收區段聯合 (RSC)

>適用於：Windows Server 2019 和 Windows 10 版本 1809

接收區段聯合 (RSC) 中的 vSwitch 是聯合成較大的區段資料周遊 vSwitch 之前的多個 TCP 區段的增強功能。 大型的區段可改善網路效能的虛擬工作負載。

先前，這是藉由將 NIC 卸載 不幸的是，這已停用您連接到虛擬交換器，配接器的時刻。 在 Windows Server 2019 和 Windows 上 vSwitch 的 RSC 10 年 10 月 2018年更新會移除這項限制。

根據預設，RSC 在 vSwitch 是外部虛擬交換器上啟用。

如需詳細資訊，請參閱：

-  [公告部落格](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-  [驗證指南適用於 IT 專業人員](https://aka.ms/RSC-Validation)
