---
title: 虛擬接收端調整 (vRSS)
description: 深入了解虛擬接收端調整 (vRSS) 在 Windows Server，以及如何設定虛擬網路介面卡來跨多個邏輯處理器核心在 VM 中載入餘額連入網路流量。 您也可以設定多個實體核心的主機虛擬網路介面卡 (vNIC)。
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
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133684"
---
# 虛擬接收端調整 \(vRSS\)

>適用於：Windows Server (半年通道)、Windows Server 2016

本主題中，您會了解虛擬接收端調整 (vRSS)，以及如何設定虛擬網路介面卡來跨多個邏輯處理器核心在 VM 中載入餘額連入網路流量。 您也可以使用 vRSS 設定主機虛擬網路介面卡 \(vNIC\) 的多個實體核心。

此設定可在載入分散在虛擬機器中的多個虛擬處理器的虛擬網路介面卡從 \(VM\)，讓它可以使用單一的邏輯處理器比快速處理更多的網路流量的 VM。

>[!TIP]
>您可以使用 vRSS Vm 中具有多顆處理器，也就是一個多核心處理器，HYPER-V 主機上或多個多核心處理器安裝並設定 VM 使用的。

vRSS 可與所有其他 HYPER-V 網路功能技術相容。 vRSS 是取決於在 HYPER-V 主機中的虛擬機器佇列 \(VMQ\) 和 RSS VM 中，或在主機 vNIC。

根據預設，Windows Server 可讓 vRSS，但您可以停用在 VM 中使用 Windows PowerShell。 如需詳細資訊，請參閱[管理 vRSS](vrss-manage.md)和[RSS 」 和 「 vRSS 的 Windows PowerShell 命令](vrss-wps.md)。



## 作業系統相容性

您可以在任何多處理器或多核心的電腦-或 vRSS 任何多處理器或多核心 VM 上-執行 Windows Server 2016 上使用 RSS。

多處理器或多核心執行下列 Microsoft 作業系統的 Vm 也支援 vRSS。

- Windows Server 2016
- Windows 10 專業版或企業版
- Windows Server 2012 R2
- Windows 8.1 專業版或企業版
- Windows Server 2012 與 Windows Server 2012 R2 整合元件安裝。
- Windows 8 與 Windows Server 2012 R2 整合元件安裝。

執行 FreeBSD 或 Linux 做為客體作業系統上 HYPER-V 的 Vm 的 vRSS 支援相關的資訊，請參閱[Windows 上 hyper-v 支援的 Linux 和 FreeBSD 虛擬機器](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows)。
  
## 硬體需求

以下是 vRSS 的硬體需求。
 
- 實體網路介面卡必須支援虛擬機器佇列 \(VMQ\)。 如果停用或不支援 VMQ，HYPER-V 主機和主機上設定任何虛擬機器停用 vRSS。
- 網路介面卡必須有 10 gbps 連結速度或更多。
- HYPER-V 主機必須設定多個處理器或使用 vRSS 至少一個 multi\ 核心處理器。
- 必須設定虛擬機器 \(VMs\)，才能使用兩個或多個邏輯處理器。


## 使用案例情況

下列兩個用途的案例說明 vRSS 處理器負載平衡和軟體負載平衡常見的使用的方式。

### 處理器負載平衡
  
陳，網路系統管理員，設定新的 HYPER-V 主機，以支援單一根輸入輸出虛擬化 \(SR\-IOV\) 兩個網路介面卡。 他部署 Windows Server 2016，架設 VM 檔案伺服器。

安裝之後的硬體和軟體，陳會設定 VM 以使用 8 個虛擬處理器和 4096 MB 的記憶體。 陳不幸的是，沒有選擇開啟 SR\ IOV，因為其 Vm 依賴透過他使用 HYPER\-V 虛擬交換器管理員建立虛擬交換器的原則強制執行。 基於這個原因，決定使用而不是 SR\ IOV vRSS 陳。

一開始，陳指派四個虛擬處理器可供使用，以 vRSS 搭配使用 Windows PowerShell。 使用檔案伺服器在一星期後之後出現是很常見，因此陳會檢查 VM 的效能。  他發現完整使用率的四個虛擬處理器。

基於這個原因，陳決定要用於 VM 新增 vRSS 處理器。  他會更多的兩個虛擬處理器指派到 VM 中，會自動提供給 vRSS 為了處理大型網路負載。 他的努力產生較佳的效能，VM 檔案伺服器，而有效率地處理網路流量負載六個處理器。


### 軟體負載平衡

吳，網路系統管理員，設定單一 VM，高效能在其中她的系統上做為軟體負載平衡器。 她已指派所有可用的邏輯處理器，此單一 vm。

安裝 Windows Server 之後, 她使用 vRSS 取得平行處理多個邏輯處理器，在 VM 上的網路流量。 因為 Windows Server 可以讓 vRSS，吳不需要變更任何設定。


## 相關主題

- [計劃 vRSS 使用](vrss-plan.md)
- [啟用虛擬網路介面卡上的 vRSS](vrss-enable.md)
- [管理 vRSS](vrss-manage.md)
- [vRSS 常見問題集](vrss-faq.md)
- [RSS 」 和 「 vRSS 的 Windows PowerShell 命令](vrss-wps.md)

---
