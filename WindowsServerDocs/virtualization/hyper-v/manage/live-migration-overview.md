---
title: 即時移轉概觀
description: 提供 Windows Server 2016 中即時移轉功能的總覽。
ms.topic: article
ms.assetid: 5cc875ab-05c4-439e-b27d-6bfc77054660
author: johncslack
ms.author: joslack
ms.date: 06/27/2017
ms.openlocfilehash: 442e546983d1758dbe4d658eba29fca917e6225c
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994088"
---
# <a name="live-migration-overview"></a>即時移轉概觀

「即時移轉」是 Windows Server 中的一項 Hyper-v 功能。  它可讓您以透明方式將執行中的虛擬機器從一部 Hyper-v 主機移到另一部，而不會察覺到停機  即時移轉的主要優點是彈性;執行虛擬機器不會系結至單一主機電腦。  這可讓您在解除委任或升級之前，先清空虛擬機器的特定主機之類的動作。  與 Windows 容錯移轉叢集配對時，即時移轉可讓您建立高可用性和容錯系統。

## <a name="related-technologies-and-documentation"></a>相關技術和檔

即時移轉通常與一些相關技術搭配使用，例如容錯移轉叢集和 System Center Virtual Machine Manager。  如果您是透過這些技術來使用即時移轉，以下是其最新檔的指標：
*  (Windows Server 2016) 的[容錯移轉](../../../failover-clustering/failover-clustering-overview.md)叢集
* [System Center Virtual Machine Manager](/system-center/vmm/) (System Center 2016) 

如果您使用較舊版本的 Windows Server，或需要舊版 Windows Server 中引進的功能詳細資料，以下是歷程記錄檔的指標：
* Windows Server 2008 R2[即時移轉](/previous-versions/windows/it-pro/microsoft-hyper-v-server-2008-R2/ee815293(v=ws.10)) () 
* Windows Server 2012 R2[即時移轉](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831435(v=ws.11)) () 
*  (Windows Server 2012 R2) 的[容錯移轉](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831579(v=ws.11))叢集
*  (Windows Server 2008 R2) 的[容錯移轉](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff182338(v=ws.10))叢集
* [System Center Virtual Machine Manager](/previous-versions/system-center/system-center-2012-R2/gg610610(v=sc.12)) (System Center 2012 R2) 
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/cc917964.aspx) (System Center 2008 R2) 

## <a name="live-migration-in-windows-server-2016"></a>Windows Server 2016 中的即時移轉

在 Windows Server 2016 中，即時移轉部署的限制較少。  它現在不需要容錯移轉叢集即可運作。  與舊版即時移轉的其他功能保持不變。  如需設定和使用即時移轉而不搭配容錯移轉叢集的詳細資訊：
* [設定主機以進行即時移轉而不需要容錯移轉叢集](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)
* [在沒有容錯移轉叢集的情況下使用即時移轉來移動虛擬機器](use-live-migration-without-failover-clustering-to-move-a-virtual-machine.md)