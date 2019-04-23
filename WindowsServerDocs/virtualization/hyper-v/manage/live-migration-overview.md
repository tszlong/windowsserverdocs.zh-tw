---
title: 即時移轉概觀
description: 在 Windows Server 2016 中提供的即時移轉功能的概觀。
ms.prod: windows-server-threshold
ms.service: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5cc875ab-05c4-439e-b27d-6bfc77054660
author: johncslack
ms.author: joslack
ms.date: 06/27/2017
ms.openlocfilehash: 2bbe897ffb8b200a72fac5a662e518d4a4be1131
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887849"
---
# <a name="live-migration-overview"></a>即時移轉概觀

即時移轉是在 Windows Server 中的 HYPER-V 功能。  它可讓您無障礙地移動 執行中虛擬機器從一部 HYPER-V 主機之間沒有感受到停機。  即時移轉的主要優點是彈性;執行中虛擬機器不會繫結至單一主機電腦。  這可讓像清空特定主機的虛擬機器，然後才能解除委任或升級它的動作。  與 Windows 容錯移轉叢集搭配時，即時移轉可讓建立高可用性和錯誤容錯系統。 

## <a name="related-technologies-and-documentation"></a>相關的技術和文件

即時移轉通常用於搭配幾個相關的技術，例如容錯移轉叢集和 System Center Virtual Machine Manager。  如果您使用即時移轉，透過這些技術，以下是其最新的文件的指標：
* [容錯移轉叢集](../../../failover-clustering/failover-clustering-overview.md)(Windows Server 2016) 
* [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/) (System Center 2016) 

如果您使用舊版的 Windows Server，或需要的 Windows Server 較舊版本中引進的功能的詳細資料，以下是過去的文件的指標： 
* [即時移轉](https://technet.microsoft.com/library/ee815293(v=ws.10).aspx)(Windows Server 2008 R2)  
* [即時移轉](https://technet.microsoft.com/library/hh831435(v=ws.11).aspx)(Windows Server 2012 R2) 
* [容錯移轉叢集](https://technet.microsoft.com/library/hh831579(v=ws.11).aspx)(Windows Server 2012 R2)
* [容錯移轉叢集](https://technet.microsoft.com/library/ff182338(v=ws.10).aspx)(Windows Server 2008 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/gg610610.aspx) (System Center 2012 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/cc917964.aspx) (System Center 2008 R2)

## <a name="live-migration-in-windows-server-2016"></a>Windows Server 2016 中的即時移轉

在 Windows Server 2016 中，有較少的限制，即時移轉的部署。  它現在就不會容錯移轉叢集。  其他功能會保持不變的即時移轉的舊版項目。  如需設定和使用即時移轉，而不容錯移轉叢集的詳細資訊： 
* [設定即時移轉，而不容錯移轉叢集的主機](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)
* [使用沒有容錯移轉叢集的即時移轉來移動虛擬機器](use-live-migration-without-failover-clustering-to-move-a-virtual-machine.md)