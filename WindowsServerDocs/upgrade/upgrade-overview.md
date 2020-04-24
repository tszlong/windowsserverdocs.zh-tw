---
title: 關於 Windows Server 升級的概觀 | Microsoft Docs
description: 了解某些一般 Windows Server 升級資訊，以及執行實際升級之前所應考量的事項。
ms.prod: windows-server
ms.technology: server-general
ms.topic: upgrade
author: RobHindman
ms.author: robhind
ms.date: 09/10/2019
ms.openlocfilehash: 1ac4cbe8b9bda4ac2de2c7ad7ec27b1534c0de72
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "80854231"
---
# <a name="overview-about-windows-server-upgrades"></a>關於 Windows Server 升級的概觀

根據您所使用的作業系統和採取的方式，升級至新版 Windows Server 的程序可能有極大的差異。 我們使用下列詞彙來區別不同的動作，任何一項都可能涉及新的 Windows Server 部署。

- **升級。** 也稱為「就地升級」。 您可以從舊版作業系統移至較新的版本，同時保留在相同的實體硬體上。 **這就是我們將在本節中說明的方法。**

    >[!Important]
    >公用或私人雲端公司也可能支援就地升級；不過，您必須向雲端提供者確認詳細資料。 此外，您無法在任何設定為**從 VHD 開機**的 Windows 伺服器上執行就地升級。

- **安裝。** 也稱為「全新安裝」。 您可以從舊版作業系統移至較新的版本，並刪除較舊的作業系統。

- **移轉。** 您可以將舊版作業系統轉換到一組不同的硬體或虛擬機器，以將其移至較新版本的作業系統。

- **叢集作業系統輪流升級。** 您可以升級叢集節點的作業系統，而無須停止 Hyper-V 或向外延展檔案伺服器工作負載。 這項功能可讓您避免可能影響服務等級協定的停機時間。 如需詳細資訊，請參閱[叢集作業系統輪流升級](../failover-clustering/cluster-operating-system-rolling-upgrade.md)

- **授權轉換。** 使用簡單命令與適當的授權金鑰執行一個步驟，將版本的特定版次轉換成相同版本的另一個版次。 我們將此方式稱為「授權轉換」。 例如，如果您的伺服器執行 Windows Server 2016 Standard，則可以將其轉換為 Windows Server 2016 Datacenter。

## <a name="which-version-of-windows-server-should-i-upgrade-to"></a>我應升級至哪個版本的 Windows Server？

建議您升級至最新版本的 Windows Server：Windows Server 2019。 執行最新版的 Windows Server，可讓您使用最新的功能 (包括最新的安全性功能)，並可提供最佳效能。

不過，我們發現這不一定可行。 您可以利用下列圖表，根據您目前使用的版本找出您可以升級到的 Windows Server 版本：

![可用的就地升級路徑](media/upgrade-paths.png)

Windows Server 通常可以經由至少一個 (有時甚至是兩個) 版本進行升級。 例如，Windows Server 2012 R2 和 Windows Server 2016 可以就地升級至 Windows Server 2019。

您也可以從評估版的作業系統升級至零售版，或從較舊的零售版升級至較新的版本，而在某些情況下，您甚至可以從大量授權版本的作業系統升級至一般的零售版。 若要進一步了解就地升級以外的升級選項，請參閱 [Windows Server 的升級和轉換選項](../get-started/supported-upgrade-paths.md)。
""'