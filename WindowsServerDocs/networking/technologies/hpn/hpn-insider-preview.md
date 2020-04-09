---
title: Windows Server 2019 中適用于 HPN 功能的 Insider Preview
description: 瞭解 Windows Server 2019 中的新高效能網路功能。
manager: dougkim
author: eross-msft
ms.author: lizross
ms.date: 09/12/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: f6403cd9787ccdd4f50eb08ebd7723d2de789ebb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819641"
---
# <a name="new-hpn-features-in-windows-server-2019"></a>Windows Server 2019 中的新 HPN 功能


## <a name="dynamic-vrss-and-vmmq"></a>動態 vRSS 和 VMMQ

>適用于： Windows Server 2019

在過去，虛擬機器佇列和虛擬機器多佇列已啟用個別 Vm 的輸送量更高，因為網路輸送量第一次達到10GbE 標記和更遠。 可惜的是，成功所需的規劃、基準化、微調和監視變得很大，通常不只是 IT 系統管理員想要花費的。 

Windows Server 2019 藉由動態散佈和微調網路工作負載的處理，來改善這些優化。 Windows Server 2019 可確保尖峰效率，並移除 IT 系統管理員的設定負擔。

如需詳細資訊，請參閱：

-   [公告 blog](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-   [IT 專業人員的驗證指南](https://aka.ms/DVMMQ-Validation)

## <a name="receive-segment-coalescing-rsc-in-the-vswitch"></a>在 vSwitch 中接收區段聯合 (RSC)

>適用于： Windows Server 2019 和 Windows 10，版本1809

VSwitch 中的接收區段聯合（RSC）是一項增強功能，可在資料通過 vSwitch 之前，將多個 TCP 區段合併成較大的區段。 大型區段可改善虛擬工作負載的網路效能。

先前，這是 NIC 所執行的卸載。 可惜的是，當您將介面卡連接至虛擬交換器時，這是停用的。 Windows Server 2019 上的 vSwitch 和 Windows 10 2018 年10月更新中的 RSC 會移除這項限制。

根據預設，會在外部虛擬交換器上啟用 vSwitch 中的 RSC。

如需詳細資訊，請參閱：

-  [公告 blog](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-  [IT 專業人員的驗證指南](https://aka.ms/RSC-Validation)
