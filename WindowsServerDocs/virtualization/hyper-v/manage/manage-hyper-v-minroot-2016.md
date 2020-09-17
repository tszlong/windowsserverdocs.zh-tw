---
title: Minroot
description: 設定主機 CPU 資源控制
ms.author: benarm
author: BenjaminArmstrong
ms.date: 12/15/2017
ms.topic: article
ms.openlocfilehash: 4a222151a9236fb19ef98eda2526524f2d113094
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746583"
---
# <a name="hyper-v-host-cpu-resource-management"></a>Hyper-v 主機 CPU 資源管理

Windows Server 2016 或更新版本中引進的 hyper-v 主機 CPU 資源控制可讓 Hyper-v 系統管理員更妥善地管理和配置「根」、管理分割區和來賓 Vm 之間的主機伺服器 CPU 資源。
系統管理員可以使用這些控制項，將主機系統的處理器子集專用於根磁碟分割。
這可能會將 Hyper-v 主機中執行的工作，與在虛擬機器中執行的工作負載隔離，方法是在系統處理器的個別子集上執行這些工作負載。

如需 Hyper-v 主機硬體的詳細資訊，請參閱 [Windows 10 Hyper-v 系統需求](/virtualization/hyper-v-on-windows/reference/hyper-v-requirements)。

## <a name="background"></a>背景

在設定 Hyper-v 主機 CPU 資源的控制權之前，請先複習 Hyper-v 架構的基本概念。
您可以在 [Hyper-v 架構](../../../administration/performance-tuning/role/hyper-v-server/architecture.md) 一節中找到一般摘要。
以下是此文章的重要概念：

* Hyper-v 會建立和管理虛擬機器磁碟分割，在這些資料分割中，系統會在管理程式控制下配置並共用計算資源。  磁碟分割可在所有來賓虛擬機器之間，以及在來賓 Vm 和根磁碟分割之間提供強式隔離界限。

* 根磁碟分割本身是虛擬機器磁碟分割，雖然它具有唯一的屬性，而且許可權比來賓虛擬機器更高。  根磁碟分割提供管理服務，可控制所有來賓虛擬機器、提供來賓的虛擬裝置支援，以及管理來賓虛擬機器的所有裝置 i/o。  Microsoft 強烈建議不要在主機磁碟分割中執行任何應用程式工作負載。

* 根磁碟分割的每個虛擬處理器 (VP) 都會將1:1 對應到基礎邏輯處理器 (LP) 。  主機 VP 一律會在相同的基礎 LP 上執行–不會遷移根磁碟分割的 VPs。

* 根據預設，主機 VPs 執行所在的 LPs 也可以執行來賓 VPs。

* 在任何可用的邏輯處理器上執行的程式管理者可能會排程來賓副總。  雖然「管理器」排程器會考慮暫時快取位置、NUMA 拓撲，以及排程來賓 VP 時的許多其他因素，但最終可能會在任何主機 LP 上排程 VP。

## <a name="the-minimum-root-or-minroot-configuration"></a>最小根或 "Minroot" 設定

舊版 Hyper-v 具有每個分割區 64 VPs 的架構上限。  這會套用至根節點和來賓磁碟分割。  由於具有超過64個邏輯處理器的系統出現在高階伺服器上，因此 Hyper-v 也演進了其主機規模限制，以支援這些大型系統，同時支援最多 320 LPs 的主機。  不過，將每個分割區的 64 VP 限制在該時間帶來幾項挑戰，並引進一些複雜性，讓每個分割區的支援超過 64 VPs。  為了解決這個情況，Hyper-v 將提供給根磁碟分割的 VPs 數目限制為64，即使基礎電腦有更多可用的邏輯處理器也一樣。  虛擬程式會繼續利用所有可用的 LPs 來執行來賓 VPs，但在64的根磁碟分割上按了。  這種設定稱為「最小根」或「minroot」設定。  效能測試已確認，即使是在具有 64 LPs 以上的大型系統上，根目錄不需要超過64個根 VPs，就能為大量的來賓 Vm 和來賓 VPs 提供足夠的支援–事實上，少於64的根 VPs 通常已足夠，視來賓 Vm 的數目和大小而定。、正在執行的特定工作負載等。

這種「minroot」的概念目前仍可繼續使用。  事實上，即使 Windows Server 2016 Hyper-v 將主機 LPs 的最大架構支援限制增加至 512 LPs，根磁碟分割仍會限制為最多 320 LPs。

## <a name="using-minroot-to-constrain-and-isolate-host-compute-resources"></a>使用 Minroot 來限制和隔離主機計算資源
在 Windows Server 2016 Hyper-v 中，具有高預設閾值 320 LPs，minroot 設定只會在最大的伺服器系統上使用。  不過，Hyper-v 主機系統管理員可以將這項功能設定為較低的臨界值，進而大幅限制根分割區可用的主機 CPU 資源數量。  當然，必須小心選擇要使用的特定根 LPs 數目，以支援 Vm 的最大需求和配置給主機的工作負載。  不過，您可以透過審慎評定和監視生產工作負載，並在廣泛部署之前于非生產環境中進行驗證，來決定主機 LPs 數目的合理值。

## <a name="enabling-and-configuring-minroot"></a>啟用和設定 Minroot

Minroot 設定是透過管理程式 BCD 專案來控制。 若要啟用 minroot，請從具有系統管理員許可權的命令提示字元：

```
     bcdedit /set hypervisorrootproc n
```
其中 n 是根 VPs 的數目。

系統必須重新開機，而新的根處理器數目將會在 OS 開機的存留期內保存。  您無法在執行時間動態變更 minroot 設定。

如果有多個 NUMA 節點，每個節點都會取得 `n/NumaNodeCount` 處理器。

請注意，有了多個 NUMA 節點，您必須確保 VM 的拓撲可以有足夠的可用 LPs (例如，在每個 NUMA 節點上) 沒有根 VPs 的 LPs，以執行對應的 VM NUMA 節點 VPs。

## <a name="verifying-the-minroot-configuration"></a>正在驗證 Minroot 設定

您可以使用工作管理員來驗證主機的 minroot 設定，如下所示。

![工作管理員中顯示的主機 minroot 設定](./media/minroot-taskman.png)

當 Minroot 為作用中時，除了系統中的邏輯處理器總數之外，工作管理員將會顯示目前分配給主機的邏輯處理器數目。