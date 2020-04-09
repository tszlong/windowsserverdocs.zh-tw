---
title: Hyper-v 上 Linux 和 FreeBSD 虛擬機器的功能描述
description: 說明在虛擬機器上使用 Linux 和 FreeBSD 時，會影響核心元件（例如網路、儲存體、記憶體）的功能
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: a9ee931d-91fc-40cf-9a15-ed6fa6965cb6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: d3dfcd3bdd4c30e5bd1a51d8e842aeca8831babf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853271"
---
# <a name="feature-descriptions-for-linux-and-freebsd-virtual-machines-on-hyper-v"></a>Hyper-v 上 Linux 和 FreeBSD 虛擬機器的功能描述

>適用于： Windows Server 2016、Hyper-v Server 2016、Windows Server 2012 R2、Hyper-v Server 2012 R2、Windows Server 2012、Hyper-v Server 2012、Windows Server 2008 R2、Windows 10、Windows 8.1、Windows 8、Windows 7.1、Windows 7

本文說明在虛擬機器上使用 Linux 和 FreeBSD 時，核心、網路、儲存體和記憶體等元件中可用的功能。

## <a name="core"></a>核心版

|**特徵**|**描述**|
|-|-|
|整合式關機|有了這項功能，系統管理員就可以從 Hyper-v 管理員關閉虛擬機器。 如需詳細資訊，請參閱[作業系統關機](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_Shutdown)。|
|時間同步處理|這項功能可確保虛擬機器內的維護時間與主機上的維護時間保持同步。 如需詳細資訊，請參閱[時間同步](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_time)處理。|
|Windows Server 2016 精確時間|這項功能可讓來賓使用 Windows Server 2016 的精確時間功能，以提高與主機的同步時間以1毫秒準確度。 如需詳細資訊，請參閱[Windows Server 2016 精確時間](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-2016-accurate-time)|
|多重處理支援|有了這項功能，虛擬機器就可以藉由設定多個虛擬 Cpu，在主機上使用多個處理器。|
|活動訊號|透過這項功能，主機可以追蹤虛擬機器的狀態。 如需詳細資訊，請參閱「[心跳](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_heartbeat)」。|
|整合式滑鼠支援|有了這項功能，您可以在虛擬機器的桌面上使用滑鼠，同時在 Windows Server desktop 與虛擬機器的 Hyper-v 主控台之間順暢地使用滑鼠。|
|Hyper-v 特定存放裝置|這項功能會授與連接至虛擬機器之存放裝置的高效能存取權。|
|Hyper-v 特定網路裝置|這項功能會授與連接至虛擬機器之網路介面卡的高效能存取。|

## <a name="networking"></a>網路功能

|**特徵**|**描述**|
|-|-|
|Jumbo 框架|有了這項功能，系統管理員可以增加超過1500個位元組的網路框架大小，這會導致網路效能大幅增加。|
|VLAN 標記和中繼|這項功能可讓您設定虛擬機器的虛擬 LAN （VLAN）流量。|
|即時移轉|透過這項功能，您可以將虛擬機器從一部主機遷移至另一部主機。 如需詳細資訊，請參閱[虛擬機器即時移轉總覽](https://technet.microsoft.com/library/hh831435.aspx)。|
|靜態 IP 插入|有了這項功能，您可以在虛擬機器故障後複寫至不同主機上的複本之後，複寫其靜態 IP 位址。 這類 IP 複寫可確保網路工作負載在容錯移轉事件之後繼續順暢地工作。|
|vRSS （虛擬接收端調整）|將虛擬網路介面卡的負載分散到虛擬機器中的多個虛擬處理器。如需詳細資訊，請參閱[Windows Server 2012 R2 中的虛擬接收端調整](https://technet.microsoft.com/library/dn383582.aspx)。|
|TCP 分割和總和檢查碼卸載|在網路資料傳輸期間，將分割和總和檢查碼工作從來賓 CPU 轉移到主機虛擬交換器或網路介面卡。|
|大型接收卸載（LRO）|藉由將多個封包匯總成較大的緩衝區來增加高頻寬連線的輸入輸送量，因而降低 CPU 負荷。|
|SR-IOV|單一根目錄 i/o 裝置會使用 DDA，讓來賓能夠存取特定 NIC 卡的某些部分，以降低延遲並增加輸送量。 SR-IOV 需要來賓上主機和虛擬函式（VF）驅動程式的最新實體功能（PF）驅動程式。|

## <a name="storage"></a>儲存體

|**特徵**|**描述**|
|-|-|
|VHDX 調整大小|有了這項功能，系統管理員可以調整連接至虛擬機器的固定大小 .vhdx 檔案大小。 如需詳細資訊，請參閱[線上虛擬硬碟調整大小總覽](https://technet.microsoft.com/library/dn282286.aspx)。|
|虛擬光纖通道|有了這項功能，虛擬機器就可以辨識光纖通道裝置，並以原生方式掛接它。 如需詳細資訊，請參閱 [Hyper-V 虛擬光纖通道概觀](https://technet.microsoft.com/library/hh831413.aspx)。|
|即時虛擬機器備份|這項功能可促進即時虛擬機器的零停機時間備份。<p>請注意，如果虛擬機器的虛擬硬碟（Vhd）裝載于遠端存放，例如伺服器訊息區（SMB）共用或 iSCSI 磁片區，則備份操作不會成功。 此外，請確定備份目標不在您備份的磁片區所在的磁片區上。|
|修剪支援|TRIM 提示會通知磁片磁碟機，應用程式不再需要先前配置的特定磁區，而且可以清除。 當應用程式透過檔案進行大量的空間配置，然後自我管理檔案的配置（例如虛擬硬碟檔案）時，通常會使用此進程。|
|SCSI WWN|Storvsc 驅動程式會從連接至虛擬機器之裝置的埠和節點，解壓縮全球名稱（WWN）資訊，並建立適當的 sysfs 檔案。 |

## <a name="memory"></a>記憶體

|**特徵**|**描述**|
|-|-|
|PAE 核心支援|實體位址延伸模組（PAE）技術可讓32位核心存取大於4GB 的實體位址空間。 舊版的 Linux 散發套件（例如 RHEL 5.x）用來運送已啟用 PAE 的個別核心。 較新的發行版本（例如 RHEL 6.x）提供預先建立的 PAE 支援。|
|設定 MMIO 間隙|透過這項功能，設備製造商可以設定記憶體對應 i/o （MMIO）間隙的位置。 MMIO 間隙通常用來分割設備的足夠作業系統（JeOS）與應用裝置的實際軟體基礎結構之間的可用實體記憶體。|
|動態記憶體-熱新增|主機可以動態地增加或減少虛擬機器在運作時可用的記憶體數量。 在布建之前，系統管理員會在 [虛擬機器設定] 面板中啟用動態記憶體，並指定虛擬機器的啟動記憶體、最小記憶體和最大記憶體。 當虛擬機器在作業中時動態記憶體無法停用，而且只能變更最小值和最大值設定。 （最佳作法是將這些記憶體大小指定為128MB 的倍數）。<p>當虛擬機器第一次啟動時，可用的記憶體會等於**啟動記憶體**。 由於應用程式工作負載導致記憶體需求增加時，Hyper-v 可能會透過熱新增機制，動態配置更多記憶體給虛擬機器（如果該核心版本支援的話）。 配置的記憶體數量上限是以**最大記憶體**參數的值為上限。<p>Hyper-v 管理員的 [記憶體] 索引標籤會顯示指派給虛擬機器的記憶體數量，但虛擬機器中的記憶體統計資料將會顯示配置的最高記憶體數量。<p>如需詳細資訊，請參閱[hyper-v 動態記憶體總覽](https://technet.microsoft.com/library/hh831766.aspx)。<p>|
|動態記憶體-佔用|主機可以動態地增加或減少虛擬機器在運作時可用的記憶體數量。 在布建之前，系統管理員會在 [虛擬機器設定] 面板中啟用動態記憶體，並指定虛擬機器的啟動記憶體、最小記憶體和最大記憶體。 當虛擬機器在作業中時動態記憶體無法停用，而且只能變更最小值和最大值設定。 （最佳作法是將這些記憶體大小指定為128MB 的倍數）。<p>當虛擬機器第一次啟動時，可用的記憶體會等於**啟動記憶體**。 由於應用程式工作負載導致記憶體需求增加，Hyper-v 可能會透過熱新增機制（如上）動態配置更多記憶體給虛擬機器。 當記憶體需求降低時，Hyper-v 可能會透過氣球機制自動從虛擬機器取消布建記憶體。 Hyper-v 不會將記憶體解除布建在**最小記憶體**參數之下。<p>Hyper-v 管理員的 [記憶體] 索引標籤會顯示指派給虛擬機器的記憶體數量，但虛擬機器中的記憶體統計資料將會顯示配置的最高記憶體數量。<p>如需詳細資訊，請參閱[hyper-v 動態記憶體總覽](https://technet.microsoft.com/library/hh831766.aspx)。<p>|
|執行時間記憶體大小調整|系統管理員可以設定虛擬機器在運作時可用的記憶體數量、增加記憶體（「熱新增」）或減少它（「熱移除」）。 記憶體會透過氣球驅動程式傳回到 Hyper-v （請參閱 "動態記憶體-佔用"）。 氣球驅動程式會在佔用（稱為「樓層」）後維持最小的可用記憶體數量，因此指派的記憶體無法縮減低於目前的需求加上此樓層數量。 Hyper-v 管理員的 [記憶體] 索引標籤會顯示指派給虛擬機器的記憶體數量，但虛擬機器中的記憶體統計資料將會顯示配置的最高記憶體數量。 （最佳作法是將記憶體值指定為128MB 的倍數）。|

## <a name="video"></a>視訊

|**特徵**|**描述**|
|-|-|
|Hyper-v 特定的影片裝置|這項功能可為虛擬機器提供高效能圖形和絕佳解析度。 此裝置不提供增強的會話模式或 RemoteFX 功能。|

## <a name="miscellaneous"></a>其他

|**特徵**|**描述**|
|-|-|
|KVP （機碼值組）交換|這項功能提供虛擬機器的索引鍵/值組（KVP）交換服務。 系統管理員通常會使用 KVP 機制來執行虛擬機器上的讀取和寫入自訂資料作業。 如需詳細資訊，請參閱[資料交換：使用機碼值組在 hyper-v 上的主機和來賓之間共用資訊](https://technet.microsoft.com/library/dn798287.aspx)。|
|非遮罩式插斷|透過這項功能，系統管理員可以將非遮罩式插斷（NMI）發行至虛擬機器。 Nmi 有助於取得因應用程式錯誤而變得沒有回應的作業系統損毀傾印。 重新開機之後，即可診斷這些損毀傾印。|
|從主機到來賓的檔案複製|這項功能可讓您從主機實體電腦將檔案複製到來賓虛擬機器，而不需要使用網路介面卡。 如需詳細資訊，請參閱[來賓服務](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_guest)。|
|lsvmbus 命令|此命令會取得 Hyper-v 虛擬機器匯流排（VMBus）上裝置的相關資訊，類似于 lspci 之類的資訊命令。|
|Hyper-v 通訊端|這是主機與客體作業系統之間的額外通道。 若要載入並使用 Hyper-v 通訊端核心模組，請參閱[建立您自己的整合服務](https://msdn.microsoft.com/virtualization/hyperv_on_windows/develop/make_mgmt_service)。|
|PCI 通過/DDA|有了 Windows Server 2016，系統管理員可以透過不同的裝置指派機制，傳遞 PCI Express 裝置。 常見的裝置包括網路卡、圖形配接器和特殊的儲存裝置。 虛擬機器將需要適當的驅動程式，才能使用公開的硬體。 硬體必須指派給虛擬機器，才能使用。<p>如需詳細資訊，請參閱[離散裝置指派-描述和背景](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/)。<p>DDA 是 SR-IOV 網路的必要條件。 虛擬通訊埠必須指派給虛擬機器，且虛擬機器必須使用正確的虛擬函式（VF）驅動程式來進行裝置多工的工作。|

## <a name="generation-2-virtual-machines"></a>第 2 代虛擬機器

|**特徵**|**描述**|
|-|-|
|使用 UEFI 開機|這項功能可讓虛擬機器使用整合可延伸韌體介面（UEFI）開機。<p>如需詳細資訊，請參閱[第 2 代虛擬機器概觀](https://technet.microsoft.com/library/dn282285.aspx)。|
|安全開機|這項功能可讓虛擬機器使用以 UEFI 為基礎的安全開機模式。 當虛擬機器以安全模式開機時，會使用 UEFI 資料存放區中存在的簽章來驗證各種作業系統元件。<p>如需詳細資訊，請參閱[安全開機](https://technet.microsoft.com/library/dn486875.aspx)。|

## <a name="see-also"></a>另請參閱

* [Hyper-v 上支援的 CentOS 和 Red Hat Enterprise Linux 虛擬機器](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-V 上支援的 Debian 虛擬機器](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 Oracle Linux 虛擬機器](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 SUSE 虛擬機器](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 Ubuntu 虛擬機器](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支援的 FreeBSD 虛擬機器](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 Hyper-v 上執行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [在 Hyper-v 上執行 FreeBSD 的最佳做法](Best-practices-for-running-FreeBSD-on-Hyper-V.md)