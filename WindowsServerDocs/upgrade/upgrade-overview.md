---
title: Windows Server 升級總覽 |Microsoft Docs
description: 瞭解一些一般的 Windows Server 升級資訊，以及執行實際升級之前應考慮的事項。
ms.prod: windows server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/10/2019
ms.openlocfilehash: 6f57e52657ca3c80c92d56c54ea87e43aabd1e99
ms.sourcegitcommit: 27f0caf74e88781054250455c3c1adf06deb6234
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2019
ms.locfileid: "71124788"
---
# <a name="overview-about-windows-server-upgrades"></a>Windows Server 升級總覽

升級至較新版本的 Windows Server 的程式可能會有很大的差異，取決於您開始使用的作業系統和您採取的路徑。 我們使用下列詞彙來區別不同的動作，其中任何一項都可能涉及新的 Windows Server 部署。

- **更新.** 也稱為「就地升級」。 您可以從舊版作業系統移至較新的版本，同時保持在相同的實體硬體上。 **這是我們將在本節中涵蓋的方法。**

    >[!Important]
    >公用或私人雲端公司也可能支援就地升級;不過，您必須向雲端提供者確認詳細資料。 此外，您將無法在設定為**從 VHD 開機**的任何 Windows 伺服器上執行就地升級。

- **過程.** 也稱為「全新安裝」。 您可以從舊版作業系統移至較新的版本，並刪除較舊的作業系統。

- **移動.** 您可以將舊版作業系統移至較新版本的作業系統，方法是傳輸到不同的硬體或虛擬機器集合。

- **叢集 OS 輪流升級。** 您可以升級叢集節點的作業系統，而不需要停止 Hyper-v 或擴充檔案伺服器工作負載。 這項功能可讓您避免可能影響服務等級協定的停機時間。 如需詳細資訊，請參閱叢集[作業系統輪流升級](../failover-clustering/cluster-operating-system-rolling-upgrade.md)

- **授權轉換。** 使用簡單的命令和適當的授權金鑰，在單一步驟中將版本的特定版本轉換成另一版的相同版本。 我們稱之為「授權轉換」。 例如，如果您的伺服器執行 Windows Server 2016 Standard，可以將其轉換為 Windows Server 2016 Datacenter。

## <a name="which-version-of-windows-server-should-i-upgrade-to"></a>我應該升級哪個版本的 Windows Server？

我們建議您升級至最新版本的 Windows Server：Windows Server 2019。 執行最新版本的 Windows Server 可讓您使用最新的功能（包括最新的安全性功能），並提供最佳效能。

不過，我們發現這不一定可行。 您可以使用下列圖表，根據您目前所在的版本來找出您可以升級到的 Windows Server 版本：

![可用的就地升級路徑](media/upgrade-paths.png)

Windows Server 通常可以透過至少一個（有時甚至兩個版本）升級。 例如，Windows Server 2012 R2 和 Windows Server 2016 可以就地升級至 Windows Server 2019。

您也可以從作業系統的評估版升級至零售版，從較舊的零售版升級到較新的版本，或在某些情況下，從大量授權版本的作業系統升級到一般的零售版。 如需就地升級以外之升級選項的詳細資訊，請參閱[Windows Server 的升級和轉換選項](../get-started/supported-upgrade-paths.md)。
