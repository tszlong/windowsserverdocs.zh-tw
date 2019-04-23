---
title: 儲存空間直接存取的硬體需求
ms.prod: windows-server-threshold
description: 測試儲存空間直接存取的最低硬體需求。
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.openlocfilehash: 84d10ab3e25500720dd13e2ba057dc3c5bf05a6f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849319"
---
# <a name="storage-spaces-direct-hardware-requirements"></a>儲存空間直接存取的硬體需求

> 適用於：Windows Server 2016 中，Windows Server Insider Preview

本主題描述儲存空間直接存取的最低硬體的需求。

對於生產環境中，Microsoft 建議這些[Windows Server 軟體定義](https://microsoft.com/wssd)硬體/軟體提供從我們的合作夥伴，包括部署工具和程序。 它們是設計、 組合和針對我們參考架構，以確保相容性和可靠性，讓您快速並執行驗證。 深入了解[ https://microsoft.com/wssd ](https://microsoft.com/wssd)。

![商標圖樣的 Windows Server 軟體定義的合作夥伴](media/hardware-requirements/wssd-partners.png)

   > [!TIP]
   > 要評估儲存空間直接存取，但沒有硬體嗎？ 使用 HYPER-V 或 Azure 虛擬機器中所述[使用儲存空間直接存取客體虛擬機器叢集中](storage-spaces-direct-in-vm.md)。

## <a name="base-requirements"></a>基底的需求

必須是系統、 元件、 裝置和驅動程式**認證的 Windows Server 2016**每個[Windows Server Catalog](https://www.windowsservercatalog.com)。 此外，我們建議伺服器、 磁碟機、 主機匯流排介面卡，以及網路介面卡**軟體定義資料中心 (SDDC) 標準**及/或**軟體定義資料中心 (SDDC) Premium**其他的識別資格 (AQs)，如以下所示。 有超過 1,000 個 SDDC AQs 元件。

![Windows Server catalog 顯示 SDDC AQs 的螢幕擷取畫面](media/hardware-requirements/sddc-aqs.png)

完整設定的叢集 （伺服器、 網路及存放裝置） 必須通過所有[叢集驗證測試](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx)精靈在容錯移轉叢集管理員或使用每`Test-Cluster` [cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)在 PowerShell 中。

此外，適用下列需求：

## <a name="servers"></a>伺服器

- 至少 2 部伺服器，最多不超過 16 部
- 建議所有伺服器都都在相同的製造商和型號

## <a name="cpu"></a>CPU

- Intel Nehalem 或更新版本相容的處理器;或
- AMD EPYC 或更新版本相容的處理器

## <a name="memory"></a>記憶體

- Windows Server、 Vm 和其他應用程式或工作負載; 的記憶體plus
- 4 GB 的 RAM，每個快取磁碟機容量，每部伺服器上，儲存空間直接存取的中繼資料的兆位元組 (TB)

## <a name="boot"></a>開機

- Windows Server 支援的任何開機裝置的[現在包含 SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)
- RAID 1 鏡像**不**必要，但開機支援
- 建議：200 GB 的大小下限

## <a name="networking"></a>網路功能

最小值 （適用於小規模 2-3 節點）
- 10 Gbps 網路介面
- 2 節點直接連接 (switchless) 支援

建議 （適用於高效能、 小數位數或部署的 4 個以上的節點）
- Nic 的遠端直接記憶體存取 (RDMA 功能，) （建議選項） 的 iWARP 或 RoCE
- 備援性和效能的兩個或多個 Nic
- 25 Gbps 網路介面或更高版本

## <a name="drives"></a>磁碟機

儲存空間直接存取搭配直接連接 SATA、 SAS 或 NVMe 磁碟機已實體附加至一台伺服器。 如需選擇磁碟機的說明，請參閱[選擇磁碟機](choosing-drives.md)主題。

- 所有支援 SATA、 SAS 和 NVMe （M.2 U.2 和在卡片中新增），磁碟機
- 512n、 512e 和 4k 原生磁碟機都有支援
- 固態磁碟機必須提供[電源中斷的保護](https://blogs.technet.microsoft.com/filecab/2016/11/18/dont-do-it-consumer-ssd/)
- 相同的數目和類型的磁碟機，在每一部伺服器-請參閱[磁碟機對稱性考量](drive-symmetry-considerations.md)
- NVMe 驅動程式是 Microsoft 的內建或更新 NVMe 驅動程式。
- 建議：容量磁碟機數目是整個快取磁碟機數目的倍數
- 建議：快取磁碟機應該有很高的寫入耐力： 至少 3 個磁碟機寫入-每天 (DWPD) 或每日 – 寫入 (TBW) 至少 4 tb 看到[了解磁碟機的寫入是每日 (DWPD)，而撰寫的 tb (TBW) 和最小值建議用於儲存體空間直接存取](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/)

以下是如何針對儲存空間直接存取連接的磁碟機：

1. 直接連接的 SATA 磁碟機
2. 直接連結 NVMe 磁碟機
3. SAS 主機匯流排介面卡 (HBA) 的 SAS 磁碟機
4. SAS 主機匯流排介面卡 (HBA) 與 SATA 磁碟機
5. **不支援：** RAID 控制卡或 SAN （光纖通道、 iSCSI、 FCoE） 儲存體。 主機匯流排介面卡 (HBA) 卡必須實作簡單的傳遞模式。

![支援的磁碟機的圖表互連](media/hardware-requirements/drive-interconnect-support-1.png)

磁碟機可以在伺服器中，內部或外部的機箱中連線到只剩一個伺服器。 需要位置對應，並且識別 SCSI 機箱服務 (SES)。 每個外部的機箱都必須提供唯一的識別項 (唯一 ID)。

1. 在伺服器內部的磁碟機
2. 外接式機殼"」 (JBOD) 連線到一部伺服器中的磁碟機
3. **不支援：** 共用的 SAS 機箱連接到多部伺服器或任何形式的多重路徑 IO (MPIO) 磁碟機所在位置，供多個路徑。

![支援的磁碟機的圖表互連](media/hardware-requirements/drive-interconnect-support-2.png)

### <a name="minimum-number-of-drives-excludes-boot-drive"></a>磁碟機的最小數目 （不含開機磁碟機）

- 如有磁碟機用為快取，每部伺服器至少必須 2 部
- 每部伺服器至少必須有 4 個容量 (非快取) 磁碟機

| 顯示的磁碟機類型   | 所需數目下限 |
|-----------------------|-------------------------|
| 全 NVMe (同模型) | 4 NVMe                  |
| 全 SSD (同模型)  | 4 SSD                   |
| NVMe + SSD            | 2 NVMe + 4 SSD          |
| NVMe + HDD            | 2 NVMe + 4 HDD          |
| SSD + HDD             | 2 SSD + 4 HDD           |
| NVMe + SSD + HDD      | 2 NVMe + 4 部其他       |

   >[!NOTE]
   > 下表提供硬體部署的最小值。 如果您是部署虛擬機器和虛擬化儲存體，例如 Microsoft Azure，請參閱 <<c0> [ 使用儲存空間直接存取客體虛擬機器叢集中](storage-spaces-direct-in-vm.md)。

### <a name="maximum-capacity"></a>最大容量

- 建議：每一部伺服器的最大值 100 tb 原始儲存體容量
- 最大 1 pb (1,000 TB) 原始容量存放集區
