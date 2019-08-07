---
title: 儲存空間直接存取的硬體需求
ms.prod: windows-server-threshold
description: 測試儲存空間直接存取的最低硬體需求。
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 08/05/2019
ms.localizationpriority: medium
ms.openlocfilehash: d899ec41b9a87089f03a576fa11dfa7d210fe194
ms.sourcegitcommit: b68ff64ecd87959cd2acde4a47506a01035b542a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2019
ms.locfileid: "68830893"
---
# <a name="storage-spaces-direct-hardware-requirements"></a>儲存空間直接存取的硬體需求

> 適用於：Windows Server 2019、Windows Server 2016

本主題說明儲存空間直接存取的最低硬體需求。

針對生產環境, Microsoft 建議向我們的合作夥伴購買經過驗證的硬體/軟體解決方案, 包括部署工具和程式。 這些解決方案是針對我們的參考架構而設計、組裝和驗證, 以確保相容性和可靠性, 讓您快速啟動並執行。 如需 Windows Server 2019 解決方案, 請造訪[AZURE STACK HCI 解決方案網站](https://azure.microsoft.com/overview/azure-stack/hci)。 如需 Windows Server 2016 解決方案, 請在[Windows Server 軟體定義](https://microsoft.com/wssd)中深入瞭解。

   > [!TIP]
   > 想要評估儲存空間直接存取, 但沒有硬體嗎？ 如在[來賓虛擬機器叢集中使用儲存空間直接存取中](storage-spaces-direct-in-vm.md)所述, 使用 Hyper-v 或 Azure 虛擬機器。

## <a name="base-requirements"></a>基本需求

系統、元件、裝置和驅動程式都必須是 windows server **2016 認證**, 每個[windows server Catalog](https://www.windowsservercatalog.com)。 此外, 我們建議伺服器、磁片磁碟機、主機匯流排介面卡和網路介面卡具有**軟體定義的資料中心 (sddc) 標準**和 (或)**軟體定義的資料中心 (sddc)** 高階額外資格 (AQs), 如圖所示如下. 有超過1000個元件具有 SDDC AQs。

![顯示 SDDC AQs 之 Windows Server 類別目錄的螢幕擷取畫面](media/hardware-requirements/sddc-aqs.png)

完整設定的叢集 (伺服器、網路和存放裝置) 必須在容錯移轉叢集管理員中, 或使用 PowerShell 中的`Test-Cluster` [Cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) , 以在每個 wizard 中傳遞所有叢集[驗證測試](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx)。

此外, 也適用下列需求:

## <a name="servers"></a>伺服器

- 至少 2 部伺服器，最多不超過 16 部
- 建議所有伺服器都是相同的製造商和型號

## <a name="cpu"></a>CPU

- Intel Nehalem 或更新版本相容處理器;或
- AMD EPYC 或更新版本相容處理器

## <a name="memory"></a>記憶體

- 適用于 Windows Server、Vm 和其他應用程式或工作負載的記憶體;增加
- 每部伺服器上每 tb 的快取磁片磁碟機容量 4 GB RAM, 用於儲存空間直接存取中繼資料

## <a name="boot"></a>啟

- Windows Server 支援的任何開機裝置,[現在包含 SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)
- **不**需要 RAID 1 鏡像, 但支援開機
- 建議：200 GB 大小下限

## <a name="networking"></a>網路功能

儲存空間直接存取在每個節點之間需要可靠的高頻寬、低延遲的網路連接。  

小型縮放2-3 節點的最小互連
- 10 Gbps 網路介面卡 (NIC) 或更快
- 每個節點中的兩個或多個網路連線, 建議用於冗余和效能

適用于高效能、大規模或部署 4 + 的建議互連 
- 支援遠端直接記憶體存取 (RDMA)、iWARP (建議) 或 RoCE 的 Nic
- 每個節點中的兩個或多個網路連線, 建議用於冗余和效能
- 25 Gbps NIC 或更快

切換或 switchless 的節點互連
- 換必須正確設定網路交換器, 才能處理頻寬和網路類型。  如果使用執行 RoCE 通訊協定的 RDMA, 網路裝置和交換器設定更為重要。 
- Switchless:您可以使用直接連線來連接節點, 避免使用交換器。  每個節點都必須與叢集的每個其他節點具有直接連線。


## <a name="drives"></a>磁碟機

儲存空間直接存取適用于直接連接的 SATA、SAS 或 NVMe 磁片磁碟機, 它們實際上只會連接到一部伺服器。 如需選擇磁碟機的說明，請參閱[選擇磁碟機](choosing-drives.md)主題。

- 支援 SATA、SAS 和 NVMe (M. 2、U. 2 和增益集卡) 磁片磁碟機
- 支援512n、512e 和4K 原生磁片磁碟機
- 固態硬碟必須提供[電源中斷保護](https://blogs.technet.microsoft.com/filecab/2016/11/18/dont-do-it-consumer-ssd/)
- 每一部伺服器中的磁片磁碟機數目和類型–請參閱[磁片磁碟機對稱考慮](drive-symmetry-considerations.md)
- 快取裝置必須是 32 GB 或更大
- 使用持續性記憶體裝置做為快取裝置時, 您必須使用 NVMe 或 SSD 容量裝置 (您無法使用 Hdd)
- NVMe 驅動程式是 Microsoft 的現成或更新的 NVMe 驅動程式。
- 建議：容量磁片磁碟機數目是快取磁片磁碟機數目的整數倍數
- 建議：快取磁片磁碟機應具有高寫入耐用性: 每日至少3個磁片磁碟機寫入 (DWPD) 或每天至少 4 tb 的寫入 (TBW) –請參閱[瞭解每天的磁片磁碟機寫入 (DWPD)、tb 寫入的 (TBW), 以及建議的最低儲存空間直接存取](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/)

以下是連接磁片磁碟機以進行儲存空間直接存取的方式:

- 直接連接的 SATA 磁片磁碟機
- 直接連接的 NVMe 磁片磁碟機
- SAS 主機匯流排介面卡 (HBA) 與 SAS 磁片磁碟機
- 具有 SATA 磁片磁碟機的 SAS 主機匯流排介面卡 (HBA)
- **不支援:** RAID 控制器卡或 SAN (光纖通道、iSCSI、FCoE) 存放裝置。 主機匯流排介面卡 (HBA) 卡片必須執行簡單的傳遞模式。

![支援的磁片磁碟機互連的圖表](media/hardware-requirements/drive-interconnect-support-1.png)

磁片磁碟機可以是伺服器的內部, 或是連接到只有一部伺服器的外部主機殼。 需要 SCSI 主機殼服務 (SES) 才能進行位置對應和識別。 每個外部主機殼都必須顯示唯一識別碼 (唯一 ID)。

- 伺服器內部的磁片磁碟機
- 連接到一部伺服器的外部主機殼 (「JBOD」) 中的磁片磁碟機
- **不支援:** 連線到多部伺服器的共用 SAS 主機殼, 或任何形式的多重路徑 IO (MPIO), 其中的磁片磁碟機可由多個路徑存取。

![支援的磁片磁碟機互連的圖表](media/hardware-requirements/drive-interconnect-support-2.png)

### <a name="minimum-number-of-drives-excludes-boot-drive"></a>磁片磁碟機數目下限 (不包括開機磁碟機)

- 如有磁碟機用為快取，每部伺服器至少必須 2 部
- 每部伺服器至少必須有 4 個容量 (非快取) 磁碟機

| 顯示的磁碟機類型   | 所需數目下限 |
|-----------------------|-------------------------|
| 所有持續性記憶體 (相同模型) | 4持續性記憶體 |
| 全 NVMe (同模型) | 4 NVMe                  |
| 全 SSD (同模型)  | 4 SSD                   |
| 持續性記憶體 + NVMe 或 SSD | 2持續性記憶體 + 4 NVMe 或 SSD |
| NVMe + SSD            | 2 NVMe + 4 SSD          |
| NVMe + HDD            | 2 NVMe + 4 HDD          |
| SSD + HDD             | 2 SSD + 4 HDD           |
| NVMe + SSD + HDD      | 2 NVMe + 4 部其他       |

   >[!NOTE]
   > 下表提供硬體部署的最小值。 如果您要使用虛擬機器和虛擬化存放裝置 (如 Microsoft Azure 中的) 進行部署, 請參閱在[來賓虛擬機器叢集中使用儲存空間直接存取](storage-spaces-direct-in-vm.md)。

### <a name="maximum-capacity"></a>最大容量

| 最                | Windows Server 2019  | Windows Server 2016  |
| ---                     | ---------            | ---------            |
| 每一伺服器的原始容量 | 100 TB               | 100 TB               |
| 集區容量           | 4 PB (4000 TB)      | 1 PB                 |
