---
title: 虛擬接收端調整 (vRSS)
description: 在 Windows Server 以及如何設定負載平衡連入網路流量分散到多個邏輯處理器核心 VM 虛擬網路介面卡，以了解虛擬接收端調整 (vRSS)。 您也可以設定多個實體核心的主機虛擬網路介面卡 (vNIC)。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9be477b3-f81d-4e84-a6b0-ac4c1ea97715
ms.date: 09/05/2018
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0c1cb11cb8ce69463a31cfa5061290f79d8dda91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875229"
---
# <a name="virtual-receive-side-scaling-vrss"></a>虛擬接收端調整\(vRSS\)

>適用於：Windows Server （半年通道），Windows Server 2016

本主題中，您會了解虛擬接收端調整 (vRSS)，以及如何設定負載平衡連入網路流量分散到多個邏輯處理器核心 VM 虛擬網路介面卡。 您也可以使用 vRSS，若要設定多個實體核心的主機虛擬網路介面卡\(vNIC\)。

此設定可將負載分散到多個虛擬處理器，在虛擬機器的虛擬網路介面卡從\(VM\)，以便讓處理更多的網路流量速度，比它可以與單一 VM邏輯處理器。

>[!TIP]
>您可以在 Hyper-v 上的 Vm 中使用 vRSS\-之有多個處理器，單一多核心處理器，HYPER-V 主機或多個多核心處理器針對安裝和設定 VM 使用。

vRSS 適用於所有其他 Hyper-v\-V 網路技術。 vRSS 是取決於虛擬機器佇列\(VMQ\)中 Hyper-v\-主機和 VM 中或在主機 vNIC 上的 RSS。

根據預設，Windows Server 啟用 vRSS，但是您可以停用它在 VM 中使用 Windows PowerShell。 如需詳細資訊，請參閱 <<c0> [ 管理 vRSS](vrss-manage.md)並[Windows PowerShell 命令，RSS 和 vRSS](vrss-wps.md)。



## <a name="operating-system-compatibility"></a>作業系統相容性

您可以使用 RSS 上任何的多處理器或多核心電腦-vRSS 任何多處理器或多核心 VM 上-執行 Windows Server 2016。

多處理器或多核心的 Vm 執行下列的 Microsoft 作業系統也支援 vRSS。

- Windows Server 2016
- Windows 10 Pro 或 Enterprise
- Windows Server 2012 R2
- Windows 8.1 Pro 或 Enterprise
- Windows Server 2012 安裝的 Windows Server 2012 R2 整合元件。
- Windows 8 與 Windows Server 2012 R2 的整合元件安裝。

如需 vRSS 支援在 HYPER-V 上做為客體作業系統執行 FreeBSD 或 Linux Vm 的資訊，請參閱[支援的 Linux 和 FreeBSD 虛擬機器，在 Windows 上的 hyper-v](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows)。
  
## <a name="hardware-requirements"></a>硬體需求

以下是 vRSS 的硬體需求。
 
- 實體網路介面卡必須支援虛擬機器佇列\(VMQ\)。 如果 VMQ 已停用或不受支援，則已停用 Hyper-v 的 vRSS\-主機和主機上設定任何 Vm。
- 網路介面卡必須連結速度的 10 Gbps 以上。
- 超\-Hyperv 主機都必須設有多個處理器或至少一個多\-核心處理器使用 vRSS。
- 虛擬機器\(Vm\)必須設定為使用兩個或多個邏輯處理器。


## <a name="use-case-scenarios"></a>使用案例

下列的兩個使用案例說明常見的用法的處理器負載平衡的軟體負載平衡的 vRSS。

### <a name="processor-load-balancing"></a>處理器負載平衡
  
Anthony，網路系統管理員，設定新的 HYPER-V 主機，以支援單一根目錄輸入輸出虛擬化的兩個網路介面卡\(SR\-SR-IOV\)。 他會部署 Windows Server 2016，以便裝載檔案伺服器的 VM。

安裝硬體和軟體之後，Anthony 設定 VM 來使用八個虛擬處理器及 4096 MB 的記憶體。 不幸的是，Anthony 沒有選擇開啟 SR-IOV\-SR-IOV 他的 Vm 依賴透過他使用 Hyper-v 建立虛擬交換器強制執行原則，因此\-V 虛擬交換器管理員。 因為這個緣故，Anthony 決定使用 vRSS 而不是 SR\-SR-IOV。

一開始，Anthony 會將四個虛擬處理器指派至可供使用，因使用 vrss 而使用 Windows PowerShell。 使用檔案伺服器時，在一個星期後的似乎是實現日漸受到歡迎，所以 Anthony 檢查 VM 的效能。  他發現完整的四個虛擬處理器的使用率。

因為這個緣故，Anthony 決定程式 vRSS 加入 VM 以供使用的處理器。  他會將 2 個虛擬處理器給 VM，這會自動提供給 vRSS，以協助處理大量網路負載。 他的工作會導致有六個的處理器能夠有效率地處理網路流量負載的 VM 檔案伺服器時，較佳的效能。


### <a name="software-load-balancing"></a>軟體負載平衡

Sandra，網路系統管理員，設定在單一的高效能 VM 上其系統的其中一個做為軟體負載平衡器。 她已指派所有可用的邏輯處理器，這個單一 vm。

安裝 Windows Server 之後, 她使用 vRSS 取得平行處理的 VM 中的多個邏輯處理器上的網路流量。 因為 Windows Server 啟用 vRSS，Sandra 不必進行任何組態變更。


## <a name="related-topics"></a>相關主題

- [規劃使用 vRSS](vrss-plan.md)
- [啟用 vRSS 虛擬網路介面卡](vrss-enable.md)
- [管理 vRSS](vrss-manage.md)
- [vRSS 常見問題集](vrss-faq.md)
- [RSS 和 vRSS 的 Windows PowerShell 命令](vrss-wps.md)

---
