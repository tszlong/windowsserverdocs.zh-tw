---
title: 容錯移轉叢集硬體需求和存放裝置選項
description: 建立容錯移轉叢集的硬體需求和存放裝置選項。
ms.topic: article
author: JasonGerend
ms.author: jgerend
manager: lizross
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 65d2d21138efbae3c5ada56bfa8628f06c4dcbe7
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990795"
---
# <a name="failover-clustering-hardware-requirements-and-storage-options"></a>容錯移轉叢集硬體需求和存放裝置選項

適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您需要下列硬體才能建立容錯移轉叢集。 若要得到 Microsoft 的支援，所有硬體必須通過您執行的 Windows Server 版本的認證，而且完整的容錯移轉叢集解決方案必須通過 [驗證設定精靈] 中的所有測試。 如需驗證容錯移轉叢集的詳細資訊，請參閱[驗證容錯移轉叢集的硬體](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>)。

- **伺服器**：建議您使用包含相同或類似元件的一組相符電腦。
- **網路介面卡及纜線 (用於網路通訊)**：如果您使用 iSCSI，則每個網路介面卡只能專用於網路通訊或 iSCSI，而非同時用於兩者。

    在連接叢集節點的網路結構中，避免發生單一失敗點。 例如，您可以透過多種不同的網路連接叢集節點。 或者，您也可以將叢集節點與一個使用網路介面卡組成的網路、多餘的交換器、冗余路由器，或移除單一失敗點的類似硬體，連接在一起。

    >[!NOTE]
    >如果您將叢集節點與單一網路連接，網路將通過驗證設定精靈中的備援需求。 然而，精靈的報告將包含警告，指出網路不應該有單一失敗點。

- **用於存放的裝置控制器或適當的介面卡**：

  - **序列連結 SCSI 或光纖通道**：如果您使用序列連結 SCSI 或光纖通道，則所有叢集伺服器中的存放裝置堆疊的所有元素都應該相同。 多重路徑 i/o (MPIO) 軟體必須相同，且裝置特定模組 (DSM) 軟體相同。 建議將大型存放裝置控制器（主機匯流排介面卡）（ (HBA) 、HBA 驅動程式及 HBA 固件）連接到叢集存放區相同。 如果您使用不相同的 HBA，則應向存放裝置廠商確認，以符合其支援或建議的設定。
  - **iSCSI**：如果您使用 iSCSI，則每一部叢集伺服器都應該有一或多個專用於叢集存放裝置的網路介面卡或 HBA。 用於 iSCSI 的網路不應該用於網路通訊。 在所有叢集伺服器中，用來連接 iSCSI 存放目標的網路介面卡都應該相同，建議您使用 Gigabit Ethernet 或速度更快的介面卡。
- **存放裝置**：您必須使用與 windows Server 2012 R2 或 windows server 2012 相容的[儲存空間直接存取](../storage/storage-spaces/storage-spaces-direct-overview.md)或共用存放裝置。 您可以使用已連接的共用存放裝置，也可以使用 SMB 3.0 檔案共用，作為在容錯移轉叢集中設定之執行 Hyper-v 的伺服器的共用存放裝置。 如需詳細資訊，請參閱[部署透過 SMB 的 Hyper-V](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)。

    在大部分情況下，連接的存放裝置應包含在硬體層級設定的多個不同磁碟 (邏輯單元編號或 LUN)。 某些叢集會使用一個磁碟做為磁碟見證 (於本段落最後說明)。 其他磁碟則包含叢集角色 (原名為叢集服務或應用程式) 所需的檔案。 存放裝置的需求包含下列各項：

  - 若要使用容錯移轉叢集中內含的原生磁碟支援，請使用基本磁碟而非動態磁碟。
  - 建議您將磁碟分割格式化為 NTFS。 如果您使用叢集共用磁碟區 (CSV)，它們的磁碟分割必須是 NTFS。

    >[!NOTE]
    >如果您的仲裁設定有設定磁碟見證，您可以將磁碟格式化為 NTFS 或復原檔案系統 (ReFS)。

  - 至於磁碟的磁碟分割樣式，您可以使用主開機記錄 (MBR) 或 GUID 磁碟分割表格 (GPT)。

    磁片見證是叢集存放裝置中的磁片，指定要保存叢集設定資料庫的複本。 容錯移轉叢集只有在將磁碟見證指定為仲裁設定的一部分時，才會有磁碟見證。 如需詳細資訊，請參閱[瞭解儲存空間直接存取中的仲裁](../storage/storage-spaces/understand-quorum.md)。

## <a name="hardware-requirements-for-hyper-v"></a>Hyper-V 的硬體需求

如果您要建立包含叢集虛擬機器的容錯移轉叢集，叢集伺服器必須支援 Hyper-V 角色的硬體需求。 Hyper-V 需要包含下列項目的 64 位元處理器：

- 硬體協助虛擬化。 這可在包含虛擬選項的處理器使用，特別是使用 Intel 虛擬化技術 (Intel VT) 或 AMD 虛擬化 (AMD-V) 技術的處理器。
- 必須提供並啟用硬體強制的資料執行防止 (DEP)。 具體而言，您必須啟用 Intel XD 位元 (執行停用位元) 或 AMD NX 位元 (無執行位元)。

如需 Hyper-v 角色的詳細資訊，請參閱[Hyper-v 總覽](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831531(v%3dws.11)>)。

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>部署容錯移轉叢集的存放區域網路

部署容錯移轉叢集的存放區域網路 (SAN) 時，請依照下列指導方針：

- **確認存放裝置的相容性**：向製造商和廠商確認存放裝置（包括用於存放裝置的驅動程式、固件和軟體）與您正在執行的 Windows Server 版本中的容錯移轉叢集相容。
- **隔離存放裝置 (每部裝置一個叢集)**：來自不同叢集的伺服器不得存取同一存放裝置。 在大多數情況下，用於某一組叢集伺服器的 LUN 應透過 LUN 遮罩或分區，與所有其他伺服器隔離。
- **考慮使用多重路徑 I/O 軟體或網路介面卡小組**：在高度可用的存放裝置網狀架構中，您可以使用多重路徑 I/O 軟體或網路介面卡小組 (也稱為負載平衡及容錯移轉，又稱 LBFO) 來部署具備多個主機匯流板介面卡的容錯移轉叢集。 這樣可提供最高層級的備援和可用性。 對於 Windows Server 2012 R2 或 Windows Server 2012，您的多重路徑解決方案必須以 Microsoft 多重路徑 i/o (MPIO) 為基礎。 雖然 Windows Server 的作業系統已包含一或多個 DSM，但是您的硬體廠商通常會為硬體提供 MPIO 裝置專屬模組 (DSM)。

    如需 LBFO 的詳細資訊，請參閱 Windows Server 技術文件庫中的[NIC 小組總覽](../networking/technologies/nic-teaming/nic-teaming.md)。

    >[!IMPORTANT]
    >主機匯流排介面卡和多重路徑 I/O 軟體可能對版本相當敏感。 如果您的叢集實作多重路徑解決方案，請與硬體廠商密切配合，為您正在執行的 Windows Server 版本選擇正確的介面卡、韌體及軟體。

- **考慮使用儲存空間**：如果您打算部署序列連接的 SCSI (SAS) 使用儲存空間設定的叢集存放裝置，請參閱針對需求[部署叢集儲存空間](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)。

## <a name="more-information"></a>更多資訊

- [容錯移轉叢集](./failover-clustering-overview.md)
- [儲存空間](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831739(v%3dws.11)>)
- [使用客體叢集以提供高可用性](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)