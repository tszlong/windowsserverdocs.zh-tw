---
title: 儲存空間直接存取的硬體需求
description: 測試儲存空間直接存取的最低硬體需求。
ms.author: eldenc
manager: eldenc
ms.topic: article
author: eldenchristensen
ms.date: 09/24/2020
ms.localizationpriority: medium
ms.openlocfilehash: fcbd7a955efda11543010d3e4705bf52b7d9bdea
ms.sourcegitcommit: f89639d3861c61620275c69f31f4b02fd48327ab
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/29/2020
ms.locfileid: "91517494"
---
# <a name="storage-spaces-direct-hardware-requirements"></a>儲存空間直接存取的硬體需求

> 適用於：Windows Server 2019、Windows Server 2016

本主題說明 Windows Server 上儲存空間直接存取的最低硬體需求。 針對 Azure Stack HCI 的硬體需求，我們的作業系統是針對與雲端連線所設計的超融合式，請參閱 [部署之前的 Azure Stack HCI：判斷硬體需求](/azure-stack/hci/deploy/before-you-start#determine-hardware-requirements)。

在生產環境中，Microsoft 建議您從我們的合作夥伴購買經過驗證的硬體/軟體解決方案，其中包括部署工具和程式。 這些解決方案是針對我們的參考架構來設計、組合和驗證，以確保相容性和可靠性，讓您可以快速啟動並執行。 若為 Windows Server 2019 解決方案，請造訪 [Azure Stack HCI 解決方案網站](https://azure.microsoft.com/overview/azure-stack/hci)。 若為 Windows Server 2016 解決方案，請在 [Windows Server 軟體定義](https://microsoft.com/wssd)中深入瞭解。

   > [!TIP]
   > 想要評估儲存空間直接存取但沒有硬體嗎？ 使用 Hyper-v 或 Azure 虛擬機器，如 [使用來賓虛擬機器叢集中的儲存空間直接存取](storage-spaces-direct-in-vm.md)所述。

## <a name="base-requirements"></a>基底需求

系統、元件、裝置和驅動程式必須通過您在 [Windows Server Catalog](https://www.windowsservercatalog.com)中使用之作業系統的認證。 此外，我們建議伺服器、磁片磁碟機、主機匯流排介面卡和網路介面卡具有 **軟體定義的資料中心 (sddc) 標準** 及/或 **軟體定義的資料中心 (sddc) ** 高階額外資格 (AQs) ，如下圖所示。 有超過1000個具有 SDDC AQs 的元件。

![Windows Server 類別目錄的螢幕擷取畫面，其中顯示包含軟體定義資料中心 (SDDC) Premium 認證的系統](media/hardware-requirements/sddc-aqs.png)

完整設定的叢集 (伺服器、網路和存放裝置) 必須在容錯移轉叢集管理員中，或使用 PowerShell 中的 Cmdlet，傳遞所有叢集[驗證測試](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732035(v=ws.10)) `Test-Cluster` [cmdlet](/powershell/module/failoverclusters/test-cluster) 。

此外，也適用下列需求：

## <a name="servers"></a>伺服器

- 至少 2 部伺服器，最多不超過 16 部
- 建議所有伺服器都是相同的製造商和型號

## <a name="cpu"></a>CPU

- Intel Nehalem 或更新版本的相容處理器;或
- AMD EPYC 或更新版本的相容處理器

## <a name="memory"></a>記憶體

- 適用于 Windows Server、Vm 和其他應用程式或工作負載的記憶體;加
- 每部伺服器上有 4 GB 的 RAM，每 tb (TB 的快取磁片磁碟機容量) 的儲存空間直接存取中繼資料

## <a name="boot"></a>Boot

- Windows Server 支援的任何開機裝置， [現在包含 SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)
- RAID 1 鏡像 **並非** 必要，但支援開機
- 建議： 200 GB 大小下限

## <a name="networking"></a>網路功能

儲存空間直接存取需要每個節點之間有可靠的高頻寬、低延遲的網路連接。

小型調整2-3 節點的最小互連
- 10 Gbps 網路介面卡 (NIC) 或更快
- 每個節點上的兩個或多個網路連線，建議用於冗余和效能

適用于高效能、大規模或部署 4 + 的建議互連
- 遠端直接記憶體存取 (RDMA) 支援的 Nic、iWARP (建議的) 或 RoCE
- 每個節點上的兩個或多個網路連線，建議用於冗余和效能
- 25 Gbps NIC 或更快

切換或 switchless 節點互連
- 切換：網路交換器必須正確設定，才能處理頻寬和網路類型。  如果使用 RDMA 來實行 RoCE 通訊協定，網路裝置和交換器設定更重要。
- Switchless：節點可以使用直接連線來互相連接，以避免使用參數。  每個節點都必須與叢集的每個其他節點直接連接。


## <a name="drives"></a>磁碟機

儲存空間直接存取適用于直接連接的 SATA、SAS、NVMe 或持續性記憶體 (PMem) 磁片磁碟機，而這些磁片磁碟機實際上只連接到一部伺服器。 如需選擇磁片磁碟機的詳細說明，請參閱 [選擇磁片磁碟機](choosing-drives.md) 及 [瞭解及部署持續性記憶體](deploy-pmem.md) 主題。

- 所有支援 SATA、SAS、持續性記憶體和 NVMe (M. 2、U. 2 和增益集) 磁片磁碟機
- 支援512n、512e 和4K 原生磁片磁碟機
- 固態硬碟必須提供 [電源中斷保護](https://techcommunity.microsoft.com/t5/storage-at-microsoft/don-t-do-it-consumer-grade-solid-state-drives-ssd-in-storage/ba-p/425914)
- 每部伺服器中的磁片磁碟機數目和類型-請參閱 [磁片磁碟機的對稱考慮](drive-symmetry-considerations.md)
- 快取裝置必須是 32 GB 或更大
- 持續性記憶體裝置會在區塊儲存模式中使用
- 使用持續性記憶體裝置作為快取裝置時，您必須使用 NVMe 或 SSD 容量裝置 (您無法使用 Hdd) 
- NVMe 驅動程式是 Microsoft 提供的驅動程式，內含于 Windows ( # A0) 
- 建議：容量磁片磁碟機數目是快取磁片磁碟機數目的完整倍數
- 建議：快取磁片磁碟機應該具有高寫入耐用性：每日至少3個磁片磁碟機-每日 (DWPD) 或至少 4 tb 寫入 (TBW) ，請參閱 [瞭解磁片磁碟機每日寫入 (DWPD) 、tb 寫入 (TBW) ，以及建議的最低儲存空間直接存取](https://techcommunity.microsoft.com/t5/storage-at-microsoft/understanding-ssd-endurance-drive-writes-per-day-dwpd-terabytes/ba-p/426024)

以下是磁片磁碟機可以連線以進行儲存空間直接存取的方式：

- 直接連接的 SATA 磁片磁碟機
- 直接連接 NVMe 磁片磁碟機
- SAS 主機匯流排介面卡 (HBA) 與 SAS 磁片磁碟機
- SAS 主機匯流排介面卡 (HBA) 與 SATA 磁片磁碟機
- **不支援：** RAID 控制器卡或 SAN (光纖通道、iSCSI、FCoE) 儲存體。 主機匯流排介面卡 (HBA) 卡必須執行簡單的通過模式。

![顯示支援的磁片磁碟機互連的圖表，但不支援 RAID 卡片](media/hardware-requirements/drive-interconnect-support-1.png)

磁片磁碟機可以是伺服器內部或連接到單一伺服器的外部主機殼。 需要 (SE) 的 SCSI 主機殼服務，才能進行位置對應和識別。 每個外部主機殼都必須提供唯一識別碼)  (唯一識別碼。

- 伺服器內部磁片磁碟機
- 外部主機殼中的磁片磁碟機 ( 「JBOD」 ) 連線到一部伺服器
- **不支援：** 連接到多部伺服器或任何形式的多重路徑 IO 的共用 SAS 主機殼 (MPIO) ，可透過多個路徑來存取磁片磁碟機。

![此圖顯示如何支援直接連線至伺服器的內部和外部磁片磁碟機，但共用的 SAS 並非](media/hardware-requirements/drive-interconnect-support-2.png)

### <a name="minimum-number-of-drives-excludes-boot-drive"></a> (排除開機磁片磁碟機的磁片磁碟機數目下限) 

- 如有磁碟機用為快取，每部伺服器至少必須 2 部
- 每部伺服器至少必須有 4 個容量 (非快取) 磁碟機

| 顯示的磁碟機類型   | 所需數目下限 |
|-----------------------|-------------------------|
| 相同模型的所有持續性記憶體 ()  | 4持續性記憶體 |
| 全 NVMe (同模型) | 4 NVMe                  |
| 全 SSD (同模型)  | 4 SSD                   |
| 持續性記憶體 + NVMe 或 SSD | 2個持續性記憶體 + 4 NVMe 或 SSD |
| NVMe + SSD            | 2 NVMe + 4 SSD          |
| NVMe + HDD            | 2 NVMe + 4 HDD          |
| SSD + HDD             | 2 SSD + 4 HDD           |
| NVMe + SSD + HDD      | 2 NVMe + 4 部其他       |

   >[!NOTE]
   > 下表提供硬體部署的最小值。 如果您要使用虛擬機器和虛擬化存放裝置（例如 Microsoft Azure）進行部署，請參閱 [使用來賓虛擬機器叢集中的儲存空間直接存取](storage-spaces-direct-in-vm.md)。

### <a name="maximum-capacity"></a>最大容量

| 大                | Windows Server 2019  | Windows Server 2016  |
| ---                     | ---------            | ---------            |
| 每一伺服器的原始容量 | 400 TB               | 100 TB               |
| 集區容量           | 4 PB (4000 TB)       | 1 PB                 |
