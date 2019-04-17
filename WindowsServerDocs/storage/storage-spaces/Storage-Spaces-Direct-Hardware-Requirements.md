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
ms.sourcegitcommit: 515b4fd5c40fcbae0e12a2c30090384533972353
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/29/2018
ms.locfileid: "8232527"
---
# 儲存空間直接存取的硬體需求

> 適用於： Windows Server 2016、 Windows Server Insider Preview

本主題說明儲存空間直接存取的最低硬體的需求。

用於生產環境，Microsoft 建議這些[Windows Server 軟體定義](https://microsoft.com/wssd)的硬體/軟體供應項目，由我們的合作夥伴，其中包含部署工具和程序。 它們是設計、 組合，且核對我們參考架構，以確保相容性和可靠性，因此您讓啟動和執行快速。 深入了解，請參閱[https://microsoft.com/wssd](https://microsoft.com/wssd)。

![我們的 Windows Server 軟體定義合作夥伴的標誌](media/hardware-requirements/wssd-partners.png)

   > [!TIP]
   > 想要評估儲存空間直接存取，但不需要的硬體嗎？ 使用 HYPER-V 或 Azure 虛擬機器中[使用儲存空間直接存取客體虛擬機器叢集中](storage-spaces-direct-in-vm.md)所述。

## 基本需求

**Windows Server 2016 認證**每個[Windows Server 類別目錄](https://www.windowsservercatalog.com)必須是系統、 元件、 裝置和驅動程式。 此外，我們建議您伺服器、 磁碟機、 主機匯流排介面卡，以及網路介面卡有**Software-Defined 資料中心 (SDDC) 標準**和/或**Software-Defined 資料中心 (SDDC) 的進階**安全性的需求 (AQs)，如圖所示下方。 有具有 SDDC AQs 超過 1000 個元件。

![螢幕擷取畫面顯示 SDDC AQs 的 Windows Server 類別目錄](media/hardware-requirements/sddc-aqs.png)

完全設定的叢集 （伺服器、 網路及儲存空間） 必須通過所有[叢集驗證測試](https://technet.microsoft.com/library/cc732035(v=ws.10).aspx)每個精靈，在容錯移轉叢集管理員或使用`Test-Cluster` [cmdlet](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) PowerShell。

此外，亦適用下列需求：

## 伺服器

- 至少 2 部伺服器，最多不超過 16 部
- 建議您所有伺服器都是同一個製造商與型號

## CPU

- 是 Intel Nehalem 或更新版本的相容處理器;或
- AMD EPYC 或更新版本的相容處理器

## 記憶體

- Windows Server、 Vm，和其他應用程式或工作負載; 記憶體plus
- 4 GB 的 RAM，每個 tb 的快取磁碟機容量，每個伺服器上，儲存空間直接存取的中繼資料

## 開機

- Windows Server，[現在包含 SATADOM](https://cloudblogs.microsoft.com/windowsserver/2017/08/30/announcing-support-for-satadom-boot-drives-in-windows-server-2016/)哪些所支援的任何開機裝置
- RAID 1 鏡像會**不**必要動作，但支援開機
- 建議： 200 GB 的大小下限

## 網路

最小值 （適用於小型的縮放比例 2-3 節點）
- 10 Gbps 網路介面
- 直接連接 (switchless) 支援使用 2 節點

建議 （適用於高效能，在縮放比例或 4 個以上節點的部署）
- NICs that are remote-direct memory access (RDMA) capable, iWARP (recommended) or RoCE
- 兩個或多個 Nic 用於備援及效能
- 25 Gbps 網路介面或更高版本

## 磁碟機

儲存空間直接存取搭配直接連接的 SATA、 SAS 或 NVMe 磁碟機是實際連接至只需一個伺服器。 如需選擇磁碟機的說明，請參閱[選擇磁碟機](choosing-drives.md)主題。

- 所有支援 SATA、 SAS 和 NVMe （M.2、 U.2，以及新增中卡） 磁碟機
- 512n、 512e 和 4k 原生的磁碟機完全支援
- 固態硬碟必須提供[斷電保護](https://blogs.technet.microsoft.com/filecab/2016/11/18/dont-do-it-consumer-ssd/)
- 相同的數量和類型中每個伺服器 – 的磁碟機，請參閱 「[磁碟機倍數考量](drive-symmetry-considerations.md)
- NVMe 驅動程式是 Microsoft 的內建或更新的 NVMe 驅動程式。
- 建議： 容量磁碟機數目是整個快取磁碟機數目的倍數
- 建議： 快取磁碟機應該有高寫入耐力： 至少 3 個磁碟機-每日寫入 (DWPD) 或至少 4 tb 寫入 (TBW) 每日 – 請參閱[了解磁碟機每日寫入 (DWPD)、 tb 寫入 (TBW) 和最低建議做為儲存空間空間直接存取](https://blogs.technet.microsoft.com/filecab/2017/08/11/understanding-dwpd-tbw/)

以下是如何可連線的磁碟機的儲存空間直接存取：

1. 直接連接的 SATA 磁碟機
2. 直接連結的 NVMe 磁碟機
3. 使用 SAS 磁碟機 SAS 主機匯流排介面卡 (HBA)
4. SAS 主機匯流排介面卡 (HBA) 與 SATA 磁碟機
5. **不支援：** RAID 控制器卡或 SAN （光纖通道、 iSCSI、 FCoE） 儲存體。 主機匯流排介面卡 (HBA) 卡必須實作簡單傳遞的模式。

![支援的磁碟機的圖表連接](media/hardware-requirements/drive-interconnect-support-1.png)

磁碟機可以是內部伺服器，或在外部的機箱連線至只需一個伺服器。 需要插槽對應與識別性 SCSI Enclosure 服務 (SES)。 每個外部機箱都必須顯示唯一識別碼 （唯一識別碼）。

1. 內部伺服器的磁碟機
2. 磁碟機中連接一部伺服器外部機箱 (「 JBOD 」)
3. **不支援：** 共用的 SAS 磁碟機連接到多部伺服器或任何形式的多重路徑 IO (MPIO) 磁碟機的多個路徑可存取的位置。

![支援的磁碟機的圖表連接](media/hardware-requirements/drive-interconnect-support-2.png)

### 磁碟機數目下限 （排除開機磁碟機）

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
   > 此表格提供硬體部署最小值。 如果您要部署與虛擬機器，且虛擬存放裝置，例如，在 Microsoft Azure，請參閱[使用儲存空間直接存取中客體虛擬機器叢集](storage-spaces-direct-in-vm.md)。

### 最大容量

- 建議： 最大 100 tb 原始儲存容量，每個伺服器
- 最大 1 不超過 pb (1000 TB) 未經處理容量儲存集區
