---
title: Minroot
description: 設定主機 CPU 資源控制
author: allenma
ms.date: 12/15/2017
ms.topic: article
ms.openlocfilehash: fc65159474f9b1cd8bf282acf00ff06f4727673b
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994058"
---
# <a name="hyper-v-host-cpu-resource-management"></a>Hyper-v 主機 CPU 資源管理

Windows Server 2016 或更新版本中引進的 hyper-v 主機 CPU 資源控制，可讓 Hyper-v 系統管理員更有效地管理和配置「根」或管理磁碟分割和來賓 Vm 之間的主機伺服器 CPU 資源。
系統管理員可以使用這些控制項，將主機系統的處理器子集專用於根磁碟分割。
這可以在 Hyper-v 主機中，從在系統處理器的個別子集上執行的工作負載中，將完成的工作與來賓虛擬機器中執行的工作負載隔離。

如需 Hyper-v 主機硬體的詳細資訊，請參閱[Windows 10 Hyper-v 系統需求](/virtualization/hyper-v-on-windows/reference/hyper-v-requirements)。

## <a name="background"></a>背景

在設定 Hyper-v 主機 CPU 資源的控制權之前，最好先參閱 Hyper-v 架構的基本概念。
您可以在[Hyper-v 架構](../../../administration/performance-tuning/role/hyper-v-server/architecture.md)一節中找到一般摘要。
以下是這篇文章的重要概念：

* Hyper-v 會建立及管理虛擬機器磁碟分割，在這些資料分割中，會在管理者控制下配置和共用計算資源。  分割區可在所有來賓虛擬機器之間，以及在來賓 Vm 與根磁碟分割之間提供強式隔離界限。

* 根磁碟分割本身是虛擬機器磁碟分割，雖然它具有與來賓虛擬機器不同的唯一屬性和更高的許可權。  根磁碟分割提供管理服務，可控制所有來賓虛擬機器、提供來賓的虛擬裝置支援，以及管理來賓虛擬機器的所有裝置 i/o。  Microsoft 強烈建議您不要在主機分割區中執行任何應用程式工作負載。

* 根磁碟分割的每個虛擬處理器 (VP) 會將1:1 對應到基礎邏輯處理器 (LP) 。  主機副總一律會在相同的基礎 LP 上執行–不會遷移根磁碟分割的 VPs。

* 根據預設，主機 VPs 執行所在的 LPs 也可以執行來賓 VPs。

* 在任何可用的邏輯處理器上執行的虛擬程式都可能會排程來賓副總裁。  當「執行程式管理者」在排程來賓副總時，會負責考慮時態性快取位置、NUMA 拓朴和許多其他因素，最後就是在任何主機 LP 上排定副總裁。

## <a name="the-minimum-root-or-minroot-configuration"></a>最小根或 "Minroot" 設定

舊版的 Hyper-v 具有每個分割區 64 VPs 的架構上限。  這會同時套用至根和來賓磁碟分割。  當高階伺服器上出現超過64個邏輯處理器的系統時，Hyper-v 也會演變其主機分級限制，以支援這些較大的系統，其中一點支援最多 320 LPs 的主機。  不過，每個分割區的限制為64副總裁，當時呈現了幾項挑戰，並引進了支援每個資料分割超過 64 VPs 的複雜度。  為了解決此情況，Hyper-v 將指定給根磁碟分割的 VPs 數目限制為64，即使基礎電腦有更多可用的邏輯處理器也一樣。  虛擬程式管理人員會繼續使用所有可用的 LPs 來執行來賓 VPs，但在64的根磁碟分割上按人工限制。  這種設定稱為「最小根」或「minroot」設定。  效能測試已確認，即使在具有超過 64 LPs 的大型系統上，根目錄也不需要超過64根 VPs，就能對大量的來賓 Vm 和來賓 VPs 提供足夠的支援–事實上，根據來賓 Vm 的數目和大小，幾乎小於64的根 VPs 通常是足夠的、正在執行的特定工作負載等。

這個「minroot」概念今天會繼續使用。  事實上，即使 Windows Server 2016 Hyper-v 增加了主機 LPs 到 512 LPs 的最大架構支援限制，根磁碟分割仍會限制為最多 320 LPs。

## <a name="using-minroot-to-constrain-and-isolate-host-compute-resources"></a>使用 Minroot 來限制和隔離主機計算資源
在 Windows Server 2016 Hyper-v 中，具有高預設閾值 320 LPs，minroot 設定只會在非常大的伺服器系統上使用。  不過，Hyper-v 主機管理員可以將這項功能設定為較低的臨界值，因此會利用來大幅限制根磁碟分割可用的主機 CPU 資源數量。  當然，您必須謹慎選擇要使用的特定根 LPs 數目，以支援配置給主機的 Vm 和工作負載的最大需求。  不過，您可以透過仔細評估和監視生產工作負載，並在非生產環境中進行廣泛部署之前進行驗證，來判斷主機 LPs 數目合理的值。

## <a name="enabling-and-configuring-minroot"></a>啟用和設定 Minroot

Minroot 設定是透過「虛擬程式」 BCD 專案來控制。 若要啟用 minroot，請從具有系統管理員許可權的 cmd 提示字元：

```
     bcdedit /set hypervisorrootproc n
```
其中 n 是根 VPs 的數目。

系統必須重新開機，而且在 OS 開機的存留期間，新的根處理器數目將會保存。  Minroot 設定無法在執行時間動態變更。

如果有多個 NUMA 節點，每個節點都會取得 `n/NumaNodeCount` 處理器。

請注意，使用多個 NUMA 節點時，您必須確定 VM 的拓撲是有足夠的免費 LPs (也就是 LPs 在每個 NUMA 節點上都沒有根 VPs) ，以執行對應的 VM NUMA 節點 VPs。

## <a name="verifying-the-minroot-configuration"></a>正在驗證 Minroot 設定

您可以使用 [工作管理員] 來驗證主機的 minroot 設定，如下所示。

![[工作管理員] 中顯示的主機 minroot 設定](./media/minroot-taskman.png)

當 Minroot 為作用中時，[工作管理員] 會顯示目前配置給主機的邏輯處理器數目，以及系統中的邏輯處理器總數。