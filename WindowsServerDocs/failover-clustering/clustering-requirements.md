---
title: 容錯移轉叢集的硬體需求及儲存選項
description: 硬體需求及建立的容錯移轉叢集的儲存選項。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4706372b06d0554196b692c3ddcda145dee5bae5
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082003"
---
# <a name="failover-clustering-hardware-requirements-and-storage-options"></a>容錯移轉叢集的硬體需求及儲存選項

適用於： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2016

您需要建立的容錯移轉叢集的下列硬體。 要支援的 Microsoft，所有硬體必須都認證為執行 Windows Server 的版本及完成的容錯移轉叢集解決方案必須通過驗證設定精靈] 中的所有測試。 如需驗證的容錯移轉叢集的詳細資訊，請參閱 ＜[驗證容錯移轉叢集的硬體](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>)。

- **伺服器**： 我們建議您使用一組的比對包含相同或類似元件的電腦。
- **網路介面卡及纜線 （適用於網路通訊）**： 如果您使用 iSCSI，應網路通訊或 iSCSI 不可同時專用每個網路介面卡。

    在連接叢集節點的網路基礎結構，避免擁有單一失敗點。 例如，您可以連接叢集節點多個、 不同的網路。 或者，您可以與一個建構而來的網路連線叢集節點與小組的網路介面卡、 備援交換器、 備援的路由器或類似會移除單一失敗點的硬體。

    >[!NOTE]
    >如果您使用單一網路連線叢集節點、 網路將會傳遞備援需求的驗證設定精靈]。 不過，[精靈] 的報表將會包含一則警告訊息網路不應該有單一失敗點。

- **裝置控制器或適當的配接器來儲存**：

  - **Serial Attached SCSI 或光纖通道**： 如果您使用序列連接 SCSI 或光纖通道中所有叢集的伺服器、 儲存堆疊的所有元素應該都是相同。 有必要多重路徑 I/O (MPIO) 軟體能相同而裝置特定模組 (DSM) 軟體能相同。 它具有建議的大型存放裝置控制器 — 主匯流排介面卡 (HBA)、 HBA 驅動及 HBA 韌體 — 會附加至叢集中存放區必須相同。 如果您使用不相似 Hba，您應該驗證存放裝置廠商與追蹤其支援或建議的設定。
  - **iSCSI**： 如果您使用 iSCSI，每個叢集的伺服器應該有一個或多個網路介面卡專用於叢集中存放區的 Hba。 您使用 iSCSI 網路不應用於網路通訊。 在所有叢集的伺服器，您用來連線至 iSCSI 儲存目標之網路介面卡應位於相同，因此建議您使用 Gigabit Ethernet 或更高。
- **儲存體**： 您必須使用[直接儲存空格](../storage/storage-spaces/storage-spaces-direct-overview.md)或共用相容於 Windows Server 2012 R2 或 Windows Server 2012 的存放區。 您可以使用所連接的共用儲存區與您也可以使用共用儲存為 SMB 3.0 檔案共用，正在執行 HYPER-V 的容錯移轉叢集中所設定的伺服器。 如需詳細資訊，請參閱[部署 HYPER-V over SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)。

    在大多數情況下，附加的儲存應該包含多個、 不同層級的硬體設定的磁碟 （邏輯單位號碼或 Lun）。 某些叢集，一個磁碟做磁碟見證 （結尾處的本小節所述）。 其他磁碟包含叢集 （以前稱為叢集的服務或應用程式） 的角色所需的檔案。 儲存需求如下：

  - 若要使用包含在容錯移轉叢集的原生磁碟支援，使用基本磁碟、 非動態磁碟。
  - 我們建議您格式與 NTFS 分割區。 如果您使用叢集共用磁碟區 (CSV)、 以外的每個分割區必須是 NTFS。

    >[!NOTE]
    >如果您有磁碟見證仲裁設定時，您可以格式化與 NTFS 或彈性檔案系統 (ReFS) 的磁碟。

  - 磁碟分割區樣式，您可以使用主要開機記錄 (MBR) 或 GUID 磁碟分割表格 (GPT)。

    磁碟見證是在具有指定保留資料庫副本的叢集設定叢集中存放區中的磁碟。 容錯移轉叢集有磁碟見證只有當指定此參數為仲裁組態的一部分。 如需詳細資訊，請參閱[了解仲裁中直接儲存空格](../storage/storage-spaces/understand-quorum.md)。

## <a name="hardware-requirements-for-hyper-v"></a>HYPER-V 的硬體需求

如果您要建立包含叢集的虛擬機器的容錯移轉叢集、 叢集伺服器必須支援的硬體需求-HYPER-V 角色。 HYPER-V 需要 64 位元處理器，包含下列各項：

- 硬體協助虛擬化。 這是可用的包括虛擬化選項的處理器 — 特別是使用 Intel 虛擬化技術 (Intel VT) 或 AMD Virtualization (AMD-V) 技術的處理器。
- 硬體強制執行資料執行防止 (DEP) 必須提供並已啟用。 特別是，您必須啟用位元的 Intel XD （執行停用位元） 或 AMD NX 元 （沒有執行位元）。

如需 HYPER-V 角色的詳細資訊，請參閱 ＜ [HYPER-V 概觀 （英文）](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831531(v%3dws.11)>)。

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>部署儲存區域網路使用容錯移轉叢集

部署時的容錯移轉叢集與儲存區域網路 (SAN)，請遵循以下指導方針：

- **確認相容性的存放區**： 確認製造商及存放區，包括驅動程式、 韌體及存放區、 所使用的軟體是相容於您要執行的 Windows Server 版本的容錯移轉叢集的廠商。
- **隔離用以儲存裝置、 裝置每一個叢集**： 從不同的叢集伺服器必須無法存取的相同的儲存裝置。 在大多數情況下，用於叢集伺服器的一組 LUN 應之外的所有其他伺服器流向 LUN 遮罩或分區。
- **考慮使用多重路徑 I/O 軟體或一起合作可以使用網路介面卡**： 中高度可用的儲存空間 fabric，您可以使用多重路徑 I/O 軟體部署具有多個主匯流排介面卡的容錯移轉叢集或網路介面卡速度 （也稱為負載平衡及容錯移轉或 LBFO）。 這提供最高層級的備援與可用性。 Windows Server 2012 R2 或 Windows Server 2012、 多重路徑解決方案必須根據 Microsoft 多重路徑 I/O (MPIO)。 雖然 Windows Server 作業系統的一部分包含一或多個 Dsm，硬體廠商通常會提供您的硬體 MPIO 裝置特定模組 (DSM)。

    如需 LBFO 的詳細資訊，請參閱 ＜ Windows Server 技術文件庫中的[NIC 速度概觀](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)。

    >[!IMPORTANT]
    >主匯流排介面卡和多重路徑 I/O 軟體可極版本機密。 如果您要為您的叢集實作多重路徑的解決方案，密切合作與硬體廠商選擇正確的介面卡、 韌體和軟體正在執行的 Windows Server 的版本。

- **考慮使用存放空間**： 如果您打算部署序列連接的 SCSI (SAS) 叢集存放裝置設定使用存放空間，請參閱 ＜[部署叢集存放空間](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)需求。

## <a name="more-information"></a>更多資訊

- [容錯移轉叢集](failover-clustering.md)
- [儲存空間](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831739(v%3dws.11)>)
- [使用客體叢集以提供高可用性](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)