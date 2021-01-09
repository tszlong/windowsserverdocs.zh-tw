---
title: Hyper-v 上的 Linux 和 FreeBSD 虛擬機器的功能描述
description: 描述在虛擬機器上使用 Linux 和 FreeBSD 時，會影響核心元件（例如網路、儲存體、記憶體）的功能
ms.topic: article
ms.assetid: a9ee931d-91fc-40cf-9a15-ed6fa6965cb6
ms.author: benarm
author: BenjaminArmstrong
ms.date: 01/08/2021
ms.openlocfilehash: 0a62c5a7aa706464b514dd61d1e41d0b23a6be89
ms.sourcegitcommit: 209b0995a11c89bb9ece3db0d48a35d7ba5bbd9d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/09/2021
ms.locfileid: "98053631"
---
# <a name="feature-descriptions-for-linux-and-freebsd-virtual-machines-on-hyper-v"></a>Hyper-v 上的 Linux 和 FreeBSD 虛擬機器的功能描述

>適用于： Azure Stack HCI、版本 20H2;Windows Server 2016、Hyper-v Server 2016、Windows Server 2012 R2、Hyper-v Server 2012 R2、Windows Server 2012、Hyper-v Server 2012、Windows Server 2008 R2、Windows 10、Windows 8.1、Windows 8、Windows 7.1、Windows 7

本文說明在虛擬機器上使用 Linux 和 FreeBSD 時的元件（例如核心、網路、儲存體和記憶體）所提供的功能。

## <a name="core"></a>核心

|**功能**|**說明**|
|-|-|
|整合式關機|有了這項功能，系統管理員就可以從 Hyper-v 管理員關閉虛擬機器。 如需詳細資訊，請參閱 [作業系統關機](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn798297(v=ws.11)#BKMK_Shutdown)。|
|時間同步|這項功能可確保虛擬機器內的維護時間與主機上的維護時間保持同步。 如需詳細資訊，請參閱 [時間同步](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn798297(v=ws.11)#BKMK_time)處理。|
|Windows Server 2016 精確時間|這項功能可讓來賓使用 Windows Server 2016 精確的時間功能，以改善與主機之間的時間同步處理，以1毫秒精確度。 如需詳細資訊，請參閱 [Windows Server 2016 精確時間](../../networking/windows-time-service/accurate-time.md)|
|多重處理支援|透過這項功能，虛擬機器可以設定多個虛擬 Cpu，在主機上使用多個處理器。|
|活動訊號|使用這項功能時，主機可以追蹤虛擬機器的狀態。 如需詳細資訊，請參閱 [心跳](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn798297(v=ws.11)#BKMK_heartbeat)。|
|整合式滑鼠支援|使用這項功能，您可以在虛擬機器的桌面上使用滑鼠，也可以在 Windows Server desktop 和 Hyper-v 主控台之間無縫地使用該虛擬機器的滑鼠。|
|Hyper-v 特定的儲存裝置|這項功能可將高效能存取權授與連接至虛擬機器的存放裝置。|
|Hyper-v 特定網路裝置|這項功能可將高效能存取權授與連接至虛擬機器的網路介面卡。|

## <a name="networking"></a>網路功能

|**功能**|**說明**|
|-|-|
|大型訊框|有了這項功能，系統管理員可以將網路框架的大小增加到超過1500個位元組，這會導致網路效能大幅增加。|
|VLAN 標記和中繼|這項功能可讓您設定虛擬 LAN (VLAN) 虛擬機器的流量。|
|即時移轉|您可以使用這項功能，將虛擬機器從一部主機遷移至另一部主機。 如需詳細資訊，請參閱 [虛擬機器即時移轉總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831435(v=ws.11))。|
|靜態 IP 插入|使用這項功能時，您可以在虛擬機器容錯移轉至不同主機上的複本之後，將該虛擬機器的靜態 IP 位址複寫。 這類 IP 複寫可確保網路工作負載在容錯移轉事件之後仍能順暢地運作。|
| (虛擬接收端調整) 的 vRSS|將虛擬網路介面卡的負載散佈到虛擬機器中的多個虛擬處理器。如需詳細資訊，請參閱 [Windows Server 2012 R2 中的虛擬接收端調整](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383582(v=ws.11))。|
|TCP 分割和總和檢查碼卸載|在網路資料傳輸期間，將分割和總和檢查碼工作從來賓 CPU 傳送至主機虛擬交換器或網路介面卡。|
|大型接收卸載 (LRO) |藉由將多個封包匯總成較大的緩衝區，減少 CPU 額外負荷，來增加高頻寬連線的輸入輸送量。|
|SR-IOV|單一根目錄 i/o 裝置使用 DDA 來允許來賓存取特定 NIC 卡的部分，以降低延遲並增加輸送量。 SR-IOV 需要最新的實體函式 (PF) 主機和虛擬函式上的驅動程式 (VF) 在來賓上的驅動程式。|

## <a name="storage"></a>儲存體

|**功能**|**說明**|
|-|-|
|VHDX 調整大小|有了這項功能，系統管理員就可以調整附加至虛擬機器的固定大小 .vhdx 檔案的大小。 如需詳細資訊，請參閱 [線上虛擬硬碟調整大小總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn282286(v=ws.11))。|
|虛擬光纖通道|使用這項功能，虛擬機器可以辨識光纖通道裝置，並以原生方式掛接。 如需詳細資訊，請參閱 [Hyper-V 虛擬光纖通道概觀](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831413(v=ws.11))。|
|即時虛擬機器備份|這項功能可加速即時虛擬機器的零停機時間備份。<p>請注意，如果虛擬機器的虛擬硬碟 (Vhd) 裝載于遠端存放裝置（例如伺服器訊息區 (SMB) 共用或 iSCSI 磁片區），則備份作業不會成功。 此外，請確定備份目標不是與您備份的磁片區位於相同的磁片區上。|
|TRIM 支援|TRIM 提示會通知磁片磁碟機，應用程式不再需要先前配置的某些磁區，且可以清除。 此程式通常會在應用程式透過檔案進行大量配置，然後自行管理檔案配置（例如虛擬硬碟檔案）時使用。|
|SCSI WWN|Storvsc 驅動程式會從連接到虛擬機器之裝置的埠和節點中，將全球名稱解壓縮 (WWN) 資訊，並建立適當的 sysfs 檔。 |

## <a name="memory"></a>記憶體

|**功能**|**說明**|
|-|-|
|PAE 核心支援|實體位址延伸 (PAE) 技術可讓32位核心存取大於4GB 的實體位址空間。 舊版的 Linux 發行版本（例如 RHEL 5.x）是用來送出已啟用 PAE 的個別核心。 較新的發行版本（例如 RHEL 6.x）已預先建立 PAE 支援。|
|設定 MMIO 間距|透過這項功能，設備製造商可以設定記憶體對應 i/o (MMIO) 間距的位置。 MMIO 間隙通常用來將設備的可用實體記憶體除以足夠的作業系統 (JeOS) 以及提供設備的實際軟體基礎結構。|
|動態記憶體-Hot-Add|當虛擬機器正在運作時，主機可以動態增加或減少可用的記憶體數量。 在布建之前，系統管理員會在 [虛擬機器設定] 面板中啟用動態記憶體，並指定虛擬機器的啟動記憶體、最小記憶體和最大記憶體。 當虛擬機器在作業中時動態記憶體無法停用，而且只能變更最小和最大設定。  (最佳作法是將這些記憶體大小指定為128MB 的倍數 ) <p>當虛擬機器第一次啟動時，可用記憶體等於 **啟動記憶體**。 由於應用程式工作負載的記憶體需求增加，Hyper-v 可能會透過 Hot-Add 機制，動態配置更多記憶體給虛擬機器（如果該版本的核心支援的話）。 配置的最大記憶體數量是以 **最大記憶體** 參數的值為上限。<p>[Hyper-v 管理員] 的 [記憶體] 索引標籤會顯示指派給虛擬機器的記憶體數量，但虛擬機器內的記憶體統計資料會顯示配置的最高記憶體量。<p>如需詳細資訊，請參閱 [hyper-v 動態記憶體總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831766(v=ws.11))。<p>|
|動態記憶體-佔用|當虛擬機器正在運作時，主機可以動態增加或減少可用的記憶體數量。 在布建之前，系統管理員會在 [虛擬機器設定] 面板中啟用動態記憶體，並指定虛擬機器的啟動記憶體、最小記憶體和最大記憶體。 當虛擬機器在作業中時動態記憶體無法停用，而且只能變更最小和最大設定。  (最佳作法是將這些記憶體大小指定為128MB 的倍數 ) <p>當虛擬機器第一次啟動時，可用記憶體等於 **啟動記憶體**。 由於應用程式工作負載的記憶體需求增加，Hyper-v 可能會透過) 上述的 Hot-Add (機制，動態配置更多記憶體給虛擬機器。 當記憶體需求減少時，Hyper-v 可能會透過氣球機制自動解除布建虛擬機器的記憶體。 Hyper-v 不會將記憶體解除布建在 **最小記憶體** 參數下。<p>[Hyper-v 管理員] 的 [記憶體] 索引標籤會顯示指派給虛擬機器的記憶體數量，但虛擬機器內的記憶體統計資料會顯示配置的最高記憶體量。<p>如需詳細資訊，請參閱 [hyper-v 動態記憶體總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831766(v=ws.11))。<p>|
|執行時間記憶體大小調整|系統管理員可以設定虛擬機器在作業時可用的記憶體數量，將記憶體增加 ( 「熱新增」 ) 或減少 ( 「熱移除」 ) 。 記憶體會透過 balloon 驅動程式傳回到 Hyper-v (請參閱 "動態記憶體-佔用" ) 。 氣球驅動程式會在佔用之後維持最小的可用記憶體數量（稱為「樓層」），因此指派的記憶體無法減少低於目前的需求加上此樓層數量。 [Hyper-v 管理員] 的 [記憶體] 索引標籤會顯示指派給虛擬機器的記憶體數量，但虛擬機器內的記憶體統計資料會顯示配置的最高記憶體量。  (最佳作法是將記憶體值指定為128MB 的倍數 ) |

## <a name="video"></a>視訊

|**功能**|**說明**|
|-|-|
|Hyper-v 特定的影片裝置|這項功能可為虛擬機器提供高效能的圖形和絕佳的解析度。 此裝置不提供增強的會話模式或 RemoteFX 功能。|

## <a name="miscellaneous"></a>其他

|**功能**|**說明**|
|-|-|
|KVP () exchange 的索引鍵/值組|這項功能可為虛擬機器提供 (KVP) exchange 服務的索引鍵/值組。 系統管理員通常會使用 KVP 機制，在虛擬機器上執行讀取和寫入自訂資料作業。 如需詳細資訊，請參閱 [資料交換：使用索引鍵/值組在 hyper-v 上的主機和來賓之間共用資訊](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn798287(v=ws.11))。|
|非遮罩式插斷|有了這項功能，系統管理員就可以將非遮罩式插斷 (NMI) 給虛擬機器。 Nmi 適用于取得因應用程式錯誤而變成沒有回應之作業系統的損毀傾印。 您可以在重新開機之後診斷這些損毀傾印。|
|從主機到來賓的檔案複製|這項功能可讓您將檔案從主機實體電腦複製到來賓虛擬機器，而不需要使用網路介面卡。 如需詳細資訊，請參閱 [來賓服務](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn798297(v=ws.11)#BKMK_guest)。|
|lsvmbus 命令|此命令會取得 Hyper-v 虛擬機器匯流排上的裝置相關資訊 (VMBus) 類似于 lspci 之類的資訊命令。|
|Hyper-v 通訊端|這是主機與客體作業系統之間的額外通道。 若要載入和使用 Hyper-v 通訊端核心模組，請參閱 [建立您自己的整合服務](/virtualization/hyper-v-on-windows/user-guide/make-integration-service)。|
|PCI 傳遞/DDA|具有 Windows Server 2016 的系統管理員可以透過離散裝置指派機制通過 PCI Express 裝置。 常見的裝置是網路卡、圖形卡和特殊的儲存裝置。 虛擬機器需要適當的驅動程式，才能使用公開的硬體。 必須將硬體指派給虛擬機器，才能使用它。<p>如需詳細資訊，請參閱 [離散裝置指派-描述和背景](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/)。<p>DDA 是 SR-IOV 網路的先決條件。 虛擬埠將需要指派給虛擬機器，而且虛擬機器必須使用正確的虛擬函式 (VF) 驅動程式來進行裝置多工。|

## <a name="generation-2-virtual-machines"></a>第 2 代虛擬機器

|**功能**|**說明**|
|-|-|
|使用 UEFI 開機|這項功能可讓虛擬機器使用整合可延伸韌體介面 (UEFI) 來開機。<p>如需詳細資訊，請參閱[第 2 代虛擬機器概觀](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn282285(v=ws.11))。|
|安全開機|這項功能可讓虛擬機器使用以 UEFI 為基礎的安全開機模式。 當虛擬機器以安全模式開機時，會使用 UEFI 資料存放區中存在的簽章來驗證各種作業系統元件。<p>如需詳細資訊，請參閱[安全開機](/previous-versions/windows/it-pro/windows-8.1-and-8/dn486875(v=ws.11))。|

## <a name="see-also"></a>另請參閱

* [Hyper-v 上支援的 CentOS 和 Red Hat Enterprise Linux 虛擬機器](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-V 上支援的 Debian 虛擬機器](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 Oracle Linux 虛擬機器](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 SUSE 虛擬機器](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 Ubuntu 虛擬機器](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 FreeBSD 虛擬機器](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 Hyper-v 上執行 Linux 的最佳作法](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [在 Hyper-v 上執行 FreeBSD 的最佳作法](Best-practices-for-running-FreeBSD-on-Hyper-V.md)