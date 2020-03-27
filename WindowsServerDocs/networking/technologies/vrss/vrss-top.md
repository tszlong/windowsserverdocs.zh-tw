---
title: 虛擬接收端調整 (vRSS)
description: 瞭解 Windows Server 中的虛擬接收端調整（vRSS），以及如何設定虛擬網路介面卡，以對 VM 中多個邏輯處理器核心的傳入網路流量進行負載平衡。 您也可以為主機虛擬網路介面卡（vNIC）設定數個實體核心。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 9be477b3-f81d-4e84-a6b0-ac4c1ea97715
ms.date: 09/05/2018
ms.localizationpriority: medium
manager: dougkim
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8841e0e5b33df6b44d63598ebf1f29caf89e1f3f
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315250"
---
# <a name="virtual-receive-side-scaling-vrss"></a>虛擬接收端調整 \(vRSS\)

>適用於：Windows Server (半年通道)、Windows Server 2016

在本主題中，您將瞭解虛擬接收端調整（vRSS），以及如何設定虛擬網路介面卡，以對 VM 中多個邏輯處理器核心的傳入網路流量進行負載平衡。 您也可以使用 vRSS 來設定主機虛擬網路介面卡 \(vNIC\)的多個實體核心。

此設定可讓虛擬網路介面卡的負載分散到虛擬機器 \(VM\)中的多個虛擬處理器，讓 VM 能夠更快處理更多的網路流量，而不會超過單一邏輯處理器的速度。

>[!TIP]
>您可以在具有多個處理器、單一多核心處理器，或已安裝並設定多個多核心處理器的\-Hyper-v 主機上的 Vm 中，使用 vRSS 來進行 VM 的使用。

vRSS 與其他所有的\-Hyper-v 網路技術都相容。 vRSS 相依于\-Hyper-v 中的虛擬機器佇列 \(VMQ\)，以及 VM 或主機 vNIC 上的 RSS。

根據預設，Windows Server 會啟用 vRSS，但是您可以使用 Windows PowerShell 在 VM 中停用它。 如需詳細資訊， [Manage vRSS](vrss-manage.md)請參閱管理[適用于 RSS 和 vRSS 的 VRSS 和 Windows PowerShell 命令](vrss-wps.md)。



## <a name="operating-system-compatibility"></a>作業系統相容性

您可以在執行 Windows Server 2016 的任何多重處理器或多核心 VM 上，對任何多處理器或多核心電腦或 vRSS 使用 RSS。

執行下列 Microsoft 作業系統的多處理器或多核心 Vm 也支援 vRSS。

- Windows Server 2016
- Windows 10 專業版或企業版
- Windows Server 2012 R2
- Windows 8.1 Pro 或企業
- 已安裝 Windows Server 2012 R2 整合元件的 windows Server 2012。
- 已安裝 Windows Server 2012 R2 整合元件的 windows 8。

如需在 Hyper-v 上執行 FreeBSD 或 Linux 作為客體作業系統之 Vm 的 vRSS 支援的相關資訊，請參閱[支援的 Linux 和 FreeBSD 虛擬機器（適用于 Windows 上的 hyper-v](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows)）。
  
## <a name="hardware-requirements"></a>硬體需求

以下是 vRSS 的硬體需求。
 
- 實體網路介面卡必須支援 \(VMQ\)虛擬機器佇列。 如果 VMQ 已停用或不受支援，則會針對\-Hyper-v 主機和主機上設定的任何 Vm 停用 vRSS。
- 網路介面卡的連結速度必須為 10 Gbps 或以上。
- \-hyper-v 主機必須設定多個處理器或至少一個多\-核心處理器，才能使用 vRSS。
- 虛擬機器 \(Vm\) 必須設定為使用兩個或多個邏輯處理器。


## <a name="use-case-scenarios"></a>使用案例

下列兩個使用案例說明了用於處理器負載平衡和軟體負載平衡之 vRSS 的一般用法。

### <a name="processor-load-balancing"></a>處理器負載平衡
  
Anthony 是網路系統管理員，是使用兩張網路介面卡來設定新的 Hyper-v 主機，以支援單一根目錄輸入-輸出虛擬化 \(SR\-SR-IOV\)。 他會部署 Windows Server 2016 來裝載 VM 檔案伺服器。

安裝硬體和軟體之後，Anthony 會將 VM 設定為使用8個虛擬處理器和 4096 MB 的記憶體。 可惜的是，Anthony 沒有開啟 SR\-SR-IOV 的選項，因為其 Vm 透過他使用超\-V 虛擬交換器管理員所建立的虛擬交換器來依賴原則強制執行。 因此，Anthony 決定使用 vRSS，而不是 SR-IOV\-SR-IOV。

一開始，Anthony 會指派四個虛擬處理器，方法是使用 Windows PowerShell 來與 vRSS 搭配使用。 在一周後使用檔案伺服器似乎相當受歡迎，因此 Anthony 會檢查 VM 的效能。  他會探索四個虛擬處理器的完整使用率。

因此，Anthony 決定將處理器新增至 VM，以供 vRSS 使用。  他會將兩個以上的虛擬處理器指派給 VM，其會自動提供給 vRSS，以協助處理大型網路負載。 他的努力為 VM 檔案伺服器帶來較佳的效能，讓六個處理器有效率地處理網路流量負載。


### <a name="software-load-balancing"></a>軟體負載平衡

Sandra 是網路系統管理員，它會在其中一個系統上設定單一高效能的 VM，做為軟體負載平衡器。 她已將所有可用的邏輯處理器指派給這個單一 VM。

安裝 Windows Server 之後，她會使用 vRSS 來取得 VM 中多個邏輯處理器的平行網路流量處理。 由於 Windows Server 會啟用 vRSS，因此 Sandra 不需要進行任何設定變更。


## <a name="related-topics"></a>相關主題

- [規劃使用 vRSS](vrss-plan.md)
- [在虛擬網路介面卡上啟用 vRSS](vrss-enable.md)
- [管理 vRSS](vrss-manage.md)
- [vRSS 常見問題](vrss-faq.md)
- [適用于 RSS 和 vRSS 的 Windows PowerShell 命令](vrss-wps.md)

---
