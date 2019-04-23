---
title: Minroot
description: 設定主機 CPU 資源控制
keywords: windows 10, hyper-v
author: allenma
ms.date: 12/15/2017
ms.topic: article
ms.prod: windows-10-hyperv
ms.service: windows-10-hyperv
ms.assetid: ''
ms.openlocfilehash: e1269c11df32c8ce95cc7455d47d7170e9d0b1c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844329"
---
# <a name="hyper-v-host-cpu-resource-management"></a>Hyper V 主機 CPU 資源管理

HYPER-V 主機 CPU 資源控制 Windows Server 2016 中引進，或更新版本允許 HYPER-V 系統管理員更妥善管理和配置主機伺服器之間的 「 根 」，或管理資料分割和客體 Vm 的 CPU 資源。 使用這些控制項，系統管理員，可以專心子集處理器的主機系統根磁碟分割。 這可以區隔來自客體虛擬機器中執行的系統處理器的個別子集上執行這些工作負載的 HYPER-V 主機中完成的工作。

如需 HYPER-V 主機的硬體的詳細資訊，請參閱[Windows 10 HYPER-V 系統需求](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements)。

## <a name="background"></a>背景

設定控制 hyper-v 主機的 CPU 資源之前，最好先檢閱 HYPER-V 架構的基本概念。  
您可以尋找在一般摘要[HYPER-V 架構](https://docs.microsoft.com/windows-server/administration/performance-tuning/role/hyper-v-server/architecture)一節。
以下是這個發行項的重要概念：

* HYPER-V 會建立和管理虛擬機器資料分割，在哪些計算資源配置及共用，hypervisor 的控制之下。  資料分割提供強式的隔離界限，所有客體虛擬機器，以及客體 Vm 與根磁碟分割。

* 在根資料分割本身是虛擬機器的磁碟分割，不過它唯一的屬性和更大的權限，比客體虛擬機器。  在根磁碟分割提供控制所有客體虛擬機器管理服務、 虛擬裝置支援的來賓，以及管理客體虛擬機器的所有裝置 I/O。  Microsoft 強烈建議未在主機磁碟分割中執行任何應用程式工作負載。

* 在根磁碟分割每個虛擬處理器 (VP) 是對應的 1:1 管理基礎的邏輯處理器 (LP)。  主應用程式副總裁一定會執行相同的基礎 LP 上 – 沒有移轉的根磁碟分割 VPs。  

* 根據預設，在其執行主機 VPs LPs 也可以執行客體 VPs。

* Hypervisor 可能排定客體副總裁，任何可用的邏輯處理器上執行。  Hypervisor 排程器會負責排程客體副總裁時考慮暫時快取位置、 NUMA 拓撲及許多其他因素，而最終無法排定副總裁 LP 任何主機上。

## <a name="the-minimum-root-or-minroot-configuration"></a>最小的根目錄中或者 「 Minroot 」 組態

舊版的 HYPER-V 有每個資料分割的 64 VPs 架構最大限制。  這套用至根和客體的分割區。  為具有超過 64 個邏輯處理器的系統會出現在高階的伺服器上，HYPER-V 也演變以支援這些較大系統中，在點支援多達 320 LPs 的主機其主應用程式規模限制。  不過，中斷 64 副總裁，每個資料分割限制在該時間看到幾項挑戰，導入進行支援 64 個以上的 VPs，每個分割區高昂的複雜性。  若要解決此問題，HYPER-V 會限制 VPs 給根磁碟分割設為 64 的數目，即使基礎的電腦有許多的多個邏輯處理器可用。  Hypervisor 會繼續使用所有可用的 LPs 執行客體 VPs，但以人為方式上限為 64 在根磁碟分割。  這項設定成為所謂的 「 最小的根 」 或 「 minroot 」 組態。  效能測試確認，甚至是大規模在系統上使用超過 64 LPs，根本不需要 64 個以上的根 VPs 提供足夠的支援，以大量的客體 Vm 和客體 VPs – 事實上，遠小於 64 根 VPs 通常已足夠當然視客體 Vm 的特定工作負載正在執行等等的大小與數量。

這個 「 minroot 」 概念，就會繼續立即利用。  事實上，即使在 Windows Server 2016 HYPER-V 主機 LPs 512 LPs 其最大的架構支援限制的增加，根磁碟分割會仍受限於 320 LPs 的最大值。

## <a name="using-minroot-to-constrain-and-isolate-host-compute-resources"></a>使用 Minroot 限制並隔離主機計算資源
320 LPs 在 Windows Server 2016 HYPER-V 的高的預設臨界值，與 minroot 設定將只會利用非常大的伺服器系統上。  不過，這項功能可以更低的閾值設定為 HYPER-V 主機系統管理員，並因此可用來大幅限制對根磁碟分割的 可用的主機 CPU 資源數量。  使用根 LPs 特定數目必須當然仔細選擇為支援最大的 Vm 和配置到主機的工作負載需求。  不過，主機 LPs 數目的合理值可以決定透過謹慎的評估和監視的生產工作負載，並在非生產環境中廣泛部署之前已驗證。

## <a name="enabling-and-configuring-minroot"></a>啟用及設定 Minroot

Minroot 組態是透過 hypervisor BCD 項目加以控制。 若要啟用 minroot，從命令提示字元，以系統管理員權限：

```
    bcdedit /set hypervisorrootproc n
```
其中 n 是根 VPs 數目。 

系統必須重新開機，以及新的根的處理器數目將會保存 OS 開機的存留期。  無法在執行階段動態變更 minroot 組態。

如果有多個 NUMA 節點，每個節點會`n/NumaNodeCount`處理器。

若要執行對應的虛擬機器的 NUMA 節點 VPs 每個 NUMA 節點上，請注意，具有多個 NUMA 節點，您必須確定 VM 的拓樸，因此會有足夠的可用 LPs (亦即，不含根 VPs LPs)。

## <a name="verifying-the-minroot-configuration"></a>正在驗證 Minroot 組態

您可以確認使用工作管理員 中，主機的 minroot 組態，如下所示。

![](./media/minroot-taskman.png)

Minroot 作用中時，工作管理員 會顯示目前配置給主機，除了系統中的邏輯處理器的總數的邏輯處理器的數目。
 
