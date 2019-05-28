---
title: 在 HYPER-V 上的 Linux 和 FreeBSD 虛擬機器的功能描述
description: 說明使用 Linux 和 FreeBSD 虛擬機器上時，會影響核心元件，例如網路、 儲存體、 記憶體的功能
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a9ee931d-91fc-40cf-9a15-ed6fa6965cb6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: a574275f6d3495a9cc9bff36fa785f28a7cd8d6f
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222879"
---
# <a name="feature-descriptions-for-linux-and-freebsd-virtual-machines-on-hyper-v"></a>在 HYPER-V 上的 Linux 和 FreeBSD 虛擬機器的功能描述

>適用於：Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 Hyper-V Server 2012 R2 中，Windows Server 2012 Hyper-V Server 2012，Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

使用 Linux 和 FreeBSD 虛擬機器上時，本文會說明中的元件，例如核心、 網路、 儲存體和記憶體可用的功能。

## <a name="core"></a>核心

|**功能**|**描述**|
|-|-|
|整合式的關機|使用此功能，以系統管理員可以關閉虛擬機器從 HYPER-V 管理員。 如需詳細資訊，請參閱 <<c0> [ 作業系統關機](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_Shutdown)。|
|時間同步處理|這項功能可確保虛擬機器內的維護時間保持與維護的時間同步在主機上。 如需詳細資訊，請參閱 <<c0> [ 時間同步化](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_time)。|
|Windows Server 2016 正確時間|這項功能可讓使用 Windows Server 2016 正確時間功能，進而改善與主應用程式 1 毫秒的時間同步，客體會精確度。 如需詳細資訊，請參閱[Windows Server 2016 正確時間](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-2016-accurate-time)|
|多處器支援|這項功能，虛擬機器可以使用多個處理器的主機上設定多個虛擬 Cpu。|
|活動訊號|主應用程式可以使用這項功能，來追蹤虛擬機器的狀態。 如需詳細資訊，請參閱 <<c0> [ 活動訊號](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_heartbeat)。|
|整合式的滑鼠支援|利用此功能，使用滑鼠在虛擬機器的桌面即可也使用虛擬機器的 Windows Server 桌面與 HYPER-V 主控台之間的順暢地滑鼠。|
|HYPER-V 特定的存放裝置|這項功能會授與高效能儲存體裝置的存取權連接至虛擬機器。|
|HYPER-V 特定網路裝置|這項功能會高效能的存取權授與連接至虛擬機器的網路介面卡。|

## <a name="networking"></a>網路功能

|**功能**|**描述**|
|-|-|
|大型訊框|利用此功能，系統管理員可以增加大小網路框架超過 1500 個位元組，這可能導致大幅提升網路效能。|
|VLAN 標記和主幹連線|這項功能可讓您設定虛擬機器的虛擬 LAN (VLAN) 流量。|
|即時移轉|使用此功能，您可以移轉虛擬機器從一部主機到另一部主機。 如需詳細資訊，請參閱 <<c0> [ 虛擬機器即時移轉概觀](https://technet.microsoft.com/library/hh831435.aspx)。|
|靜態 IP 資料隱碼攻擊|利用此功能，您可以在它有其不同的主機上的複本容錯移轉之後複寫虛擬機器的靜態 IP 的位址。 這類 IP 複寫可確保網路工作負載繼續順暢地運作，容錯移轉事件之後。|
|vRSS （虛擬接收端調整）|從虛擬網路介面卡的負載分配的虛擬機器中的多個虛擬處理器。如需詳細資訊，請參閱 < [Windows Server 2012 R2 中的虛擬接收端調整](https://technet.microsoft.com/library/dn383582.aspx)。|
|TCP 分割和總和檢查碼卸載|傳輸區隔與總和檢查碼網路資料傳輸期間使用從來賓 CPU 的主機虛擬交換器或網路介面卡。|
|大型接收卸載 (LRO)|藉由彙總到較大的緩衝區，並減少 CPU 額外負荷的多個封包的高頻寬連線的輸入的輸送量的增加。|
|SR-IOV|單一根目錄 I/O 裝置使用 DDA 允許來賓存取權，以降低的延遲，並增加的輸送量的特定 nic 介面卡的部分。 SR-IOV 需要在主機上的最新狀態的實體函式 (PF) 驅動程式和客體上的虛擬函式 (VF) 驅動程式。|

## <a name="storage"></a>儲存體

|**功能**|**描述**|
|-|-|
|VHDX 調整大小|利用此功能，系統管理員可以調整連結至虛擬機器的大小固定.vhdx 檔案。 如需詳細資訊，請參閱 <<c0> [ 線上的虛擬硬碟調整大小概觀](https://technet.microsoft.com/library/dn282286.aspx)。|
|虛擬光纖通道|使用此功能，虛擬機器可以辨識的光纖通道裝置，並將它掛接到原生。 如需詳細資訊，請參閱 [Hyper-V 虛擬光纖通道概觀](https://technet.microsoft.com/library/hh831413.aspx)。|
|即時虛擬機器備份|這項功能有助於減少時間即時的虛擬機器備份的零。<br /><br />請注意，備份作業不成功，如果虛擬機器具有虛擬硬碟 (Vhd) 所裝載的遠端儲存體，例如伺服器訊息區塊 (SMB) 共用或 iSCSI 磁碟區。 此外，請確定備份的目標不會位於相同的磁碟區，您將備份的磁碟區上。|
|修剪支援|修剪提示通知的磁碟機，某些先前所配置的磁區不再需要應用程式，且可加以清除。 通常在應用程式透過檔案，讓大型的空間配置，然後自我管理的配置檔案，例如虛擬硬碟檔案時，會運用這個程序。|
|SCSI WWN|Storvsc 驅動程式擷取的裝置連接至虛擬機器的節點與連接埠的全球名稱 (WWN) 資訊，並建立適當的 sysfs 檔案。 |

## <a name="memory"></a>記憶體

|**功能**|**描述**|
|-|-|
|PAE 核心支援|實體位置延伸 (PAE) 技術可讓 32 位元核心，以存取超過 4 GB 的實體的位址空間。 較舊的 Linux 散發套件，例如 RHEL 5.x 用來寄送另一項核心，已啟用 PAE。 更新的發行例如 RHEL 6.x 有預先建置的 PAE 支援。|
|MMIO 間距的組態|設備製造商可以利用這項功能，來設定記憶體對應 I/O (MMIO) 間距的位置。 MMIO 差距通常會用來將設備只夠 Operating Systems (JeOS) 和實際的軟體基礎結構提供設備之間可用的實體記憶體中。|
|動態記憶體的熱新增|主應用程式可以動態增加或減少虛擬機器可以使用的記憶體數量，在作業時。 佈建之前，系統管理員會啟用動態記憶體的虛擬機器設定 面板中，並指定虛擬機器的啟動記憶體、 最小記憶體和最大記憶體。 您可以變更虛擬機器時無法停用動態記憶體的作業和只有最小值和最大值設定。 （它是最佳的作法，來指定這些記憶體大小為 128 MB 的倍數）。<br /><br />當虛擬機器第一次啟動時可用的記憶體是否等於**啟動記憶體**。 由於應用程式工作負載的記憶體需求增加時 HYPER-V 可能會以動態方式配置更多記憶體的熱新增的機制，透過虛擬機器如果支援該版本的核心。 最大配置的記憶體數量的值會受限於**最大記憶體**參數。<br /><br />HYPER-V 管理員中的 [記憶體] 索引標籤會顯示指派給虛擬機器的記憶體數量，但虛擬機器內的記憶體統計資料會顯示最高的已配置的記憶體數量。<br /><br />如需詳細資訊，請參閱 < [HYPER-V 動態記憶體概觀](https://technet.microsoft.com/library/hh831766.aspx)。<br /><br />|
|動態記憶體-佔用|主應用程式可以動態增加或減少虛擬機器可以使用的記憶體數量，在作業時。 佈建之前，系統管理員會啟用動態記憶體的虛擬機器設定 面板中，並指定虛擬機器的啟動記憶體、 最小記憶體和最大記憶體。 您可以變更虛擬機器時無法停用動態記憶體的作業和只有最小值和最大值設定。 （它是最佳的作法，來指定這些記憶體大小為 128 MB 的倍數）。<br /><br />當虛擬機器第一次啟動時可用的記憶體是否等於**啟動記憶體**。 由於應用程式工作負載的記憶體需求增加時 HYPER-V 可能會動態配置到虛擬機器 （請見上方） 的熱新增機制透過更多的記憶體。 隨著記憶體需求的降低 HYPER-V 可能會自動取消佈建的球形文字說明機制透過虛擬機器的記憶體。 HYPER-V 會取消佈建下列記憶體**最小記憶體**參數。<br /><br />HYPER-V 管理員中的 [記憶體] 索引標籤會顯示指派給虛擬機器的記憶體數量，但虛擬機器內的記憶體統計資料會顯示最高的已配置的記憶體數量。<br /><br />如需詳細資訊，請參閱 < [HYPER-V 動態記憶體概觀](https://technet.microsoft.com/library/hh831766.aspx)。<br /><br />|
|執行階段記憶體大小調整|系統管理員可以設定可用的記憶體數量的虛擬機器時在作業中，增加記憶體 （「 熱新增 」），或減少 （「 熱移除 」）。 記憶體時，會傳回至 HYPER-V 上，透過 balloon 驅動程式 （請參閱 「 動態記憶體-佔用 」）。 Balloon 驅動程式會維護最小可用的記憶體之後佔用，稱為 「 下限 」，因此指派的記憶體數量無法會縮小到低於目前的需求，再加上此 floor 數量。 HYPER-V 管理員中的 [記憶體] 索引標籤會顯示指派給虛擬機器的記憶體數量，但虛擬機器內的記憶體統計資料會顯示最高的已配置的記憶體數量。 （它是最佳的作法，若要指定記憶體值為 128 MB 的倍數）。|

## <a name="video"></a>視訊

|**功能**|**描述**|
|-|-|
|Hyper-v 特定視訊裝置|這項功能會針對虛擬機器提供高效能圖形和優異的解析度。 此裝置不提供增強的工作階段模式或 RemoteFX 功能。|

## <a name="miscellaneous"></a>其他

|**功能**|**描述**|
|-|-|
|KVP （索引鍵 / 值組） exchange|這項功能提供的索引鍵/值組 (KVP) exchange 服務虛擬機器。 通常系統管理員會使用 KVP 機制執行讀取和寫入虛擬機器上的自訂資料作業。 如需詳細資訊，請參閱[資料交換：使用索引鍵 / 值組共用有關 HYPER-V 主機與客體之間](https://technet.microsoft.com/library/dn798287.aspx)。|
|非遮罩式插斷|這項功能，以系統管理員可以透過發出非遮罩式插斷 （nmi） 傳送至虛擬機器。 NMIs 可用於取得損毀傾印的變得沒有回應，因為應用程式錯誤的作業系統。 在您重新啟動之後，您可以診斷這些損毀傾印。|
|從主機到客體檔案複製|這項功能可讓檔案從主機實體電腦複製到客體虛擬機器而不使用網路介面卡。 如需詳細資訊，請參閱 <<c0> [ 客體服務](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_guest)。|
|lsvmbus 命令|此命令會取得類似於類 lspci 的資訊命令上的 HYPER-V 虛擬機器匯流排 (VMBus) 裝置的相關資訊。|
|HYPER-V 通訊端|這是主機和客體作業系統之間的其他通訊通道。 若要載入，並使用 HYPER-V 通訊端核心模組，請參閱[讓您自己的整合服務](https://msdn.microsoft.com/virtualization/hyperv_on_windows/develop/make_mgmt_service)。|
|PCI 通過/DDA|Windows Server 2016 系統管理員可以通過 PCI Express 的裝置，透過不連續的裝置指派機制。 常見的裝置是網路卡、 圖形卡和特殊的存放裝置。 虛擬機器將會需要適當的驅動程式使用公開的硬體。 硬體必須指派給虛擬機器，才能使用。<br /><br />如需詳細資訊，請參閱 <<c0> [ 離散的裝置指派-描述和背景](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/)。<br /><br />DDA 是 SR-IOV 網路功能的必要條件。 虛擬通訊埠必須要指派給虛擬機器和虛擬機器必須使用正確的虛擬函式 (VF) 驅動程式進行多工裝置。|

## <a name="generation-2-virtual-machines"></a>第 2 代虛擬機器

|**功能**|**描述**|
|-|-|
|使用 UEFI 開機|這項功能可讓開機使用統一可延伸韌體介面 (UEFI) 的虛擬機器。<br /><br />如需詳細資訊，請參閱[第 2 代虛擬機器概觀](https://technet.microsoft.com/library/dn282285.aspx)。|
|安全開機|這項功能可讓虛擬機器，以使用基礎的 UEFI 安全開機模式。 當虛擬機器開機在安全模式下時，使用 UEFI 資料存放區中的簽章驗證各種作業系統元件。<br /><br />如需詳細資訊，請參閱[安全開機](https://technet.microsoft.com/library/dn486875.aspx)。|

## <a name="see-also"></a>另請參閱

* [支援 CentOS 和 Red Hat Enterprise Linux 在 HYPER-V 上的虛擬機器](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-V 上支援的 Debian 虛擬機器](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 Oracle Linux 虛擬機器](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 SUSE 虛擬機器](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 Ubuntu 虛擬機器](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支援的 FreeBSD 虛擬機器](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上執行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [在 HYPER-V 上執行 FreeBSD 的最佳做法](Best-practices-for-running-FreeBSD-on-Hyper-V.md)