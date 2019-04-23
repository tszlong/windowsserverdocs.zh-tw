---
title: HYPER-V 儲存體 I/O 效能
description: 在 HYPER-V 效能調整的儲存體 i/o 效能考量
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fedc23083914bcf97a8cde12b78c0b174143de25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831309"
---
# <a name="hyper-v-storage-io-performance"></a>HYPER-V 儲存體 I/O 效能

本章節描述不同的選項和微調儲存體的虛擬機器中的 I/O 效能的考量。 從客體儲存體堆疊，透過主機虛擬化層級的儲存體 I/O 路徑擴充，主應用程式的儲存體堆疊，然後再到 實體磁碟。 以下是說明如何最佳化可能會有在每個階段。

## <a name="virtual-controllers"></a>虛擬控制器

HYPER-V 提供三種類型的虛擬控制器：IDE、 SCSI，以及虛擬主機匯流排介面卡 (Hba)。

## <a name="ide"></a>IDE

IDE 控制器公開 （expose) 至虛擬機器的 IDE 磁碟。 模擬的 IDE 控制器，且適用於執行舊版本的 Windows 不使用虛擬機器整合服務的客體 Vm 的唯一控制站。 磁碟 I/O 執行的虛擬機器整合服務搭配使用所提供的 IDE 篩選器驅動程式是遠優於磁碟 I/O 效能所提供的模擬的 IDE 控制器。 我們建議，IDE 磁碟僅適用於作業系統磁碟因為它們的效能限制，因為可以核發給這些裝置的最大 I/O 大小。

## <a name="scsi-sas-controller"></a>SCSI （SAS 控制站）

SCSI 控制器公開 （expose） 至虛擬機器的 SCSI 磁碟和每個虛擬 SCSI 控制器可以支援最多 64 個裝置。 為了達到最佳效能，建議您將多個磁碟連結至單一虛擬 SCSI 控制器，並建立額外的控制站，只要在需要調整連接到虛擬機器的磁碟數目。 SCSI 路徑，不會模擬，因此任何不同的作業系統磁碟的磁碟喜好的控制器。 事實上與第 2 代 Vm，它是控制器的唯一可能類型。 Windows Server 2012 R2 中引入，此控制器會回報為 SAS 支援共用的 VHDX。

## <a name="virtual-fibre-channel-hbas"></a>虛擬光纖通道 Hba

允許透過乙太網路 (FCoE) Lun 來光纖通道與光纖通道的虛擬機器的直接存取可以設定虛擬光纖通道 Hba。 虛擬光纖通道磁碟略過 NTFS 檔案系統中的根磁碟分割，進而減少儲存體 I/O 的 CPU 使用量。

大型的資料磁碟機 （適用於客體叢集案例） 的多個虛擬機器之間共用的磁碟機等虛擬光纖通道磁碟的最佳候選項。

虛擬光纖通道磁碟需要一或多個光纖通道主機匯流排介面卡 (Hba) 安裝在主機上。 每個主機 HBA，才能使用支援的功能，Windows Server 2016 虛擬光纖通道/NPIV 的 HBA 驅動程式。 SAN 架構應該支援 NPIV，並使用虛擬光纖通道 HBA 連接埠應該設定在支援 NPIV 的光纖通道拓撲中。

若要在多個 HBA 與一起安裝的主機上的輸送量最大化，我們建議您設定 HYPER-V 虛擬機器內的多個虛擬 Hba （可針對每個虛擬機器設定最多四個 Hba）。 HYPER-V 會自動將盡全力來平衡虛擬 Hba，以存取相同的虛擬 SAN 的主機 Hba。

## <a name="virtual-disks"></a>虛擬磁碟

可以透過虛擬的控制站的虛擬機器公開的磁碟。 這些磁碟可能是檔案的磁碟或主應用程式的傳遞磁碟的抽象概念的虛擬硬碟。

## <a name="virtual-hard-disks"></a>虛擬硬碟

有兩個虛擬硬碟格式，VHD 和 VHDX。 每種格式支援三種類型的虛擬硬碟檔案。

## <a name="vhd-format"></a>VHD 格式

VHD 格式是舊版中的 hyper-v 所支援的唯一的虛擬硬碟格式。 Windows Server 2012 中引入，VHD 格式已經過修改以允許更好的對齊方式，會導致新的大型磁區磁碟上的大幅提升效能。

在 Windows Server 2012 或更新版本建立任何新的 VHD 有最佳的 4 KB 對齊方式。 此對齊的格式會與舊版的 Windows Server 作業系統完全相容。 不過，您將會中斷來自剖析器的未 4 KB 的對齊方式感知 （從舊版的 Windows Server VHD 剖析器） 或非 Microsoft 的剖析器等的新配置的對齊屬性。

移動來自舊版本的任何 VHD 無法自動轉換為這個新的改良 VHD 格式。

若要將轉換為新的 VHD 格式，請執行下列 Windows PowerShell 命令：

``` syntax
Convert-VHD –Path E:\vms\testvhd\test.vhd –DestinationPath E:\vms\testvhd\test-converted.vhd
```

您可以檢查系統上的所有 Vhd 的對齊屬性，它應該轉換成最佳的 4 KB 對齊方式。 從原始的 VHD 的資料建立使用新的 VHD**建立從來源**選項。

若要使用 Windows Powershell 檢查對齊方式，請檢查 [對齊] 行中，如下所示：

``` syntax
Get-VHD –Path E:\vms\testvhd\test.vhd

Path                    : E:\vms\testvhd\test.vhd
VhdFormat               : VHD
VhdType                 : Dynamic
FileSize                : 69245440
Size                    : 10737418240
MinimumSize             : 10735321088
LogicalSectorSize       : 512
PhysicalSectorSize      : 512
BlockSize               : 2097152
ParentPath              :
FragmentationPercentage : 10
Alignment               : 0
Attached                : False
DiskNumber              :
IsDeleted               : False
Number                  :
```

若要使用 Windows PowerShell 確認對齊方式，請檢查 [對齊] 行中，如下所示：

``` syntax
Get-VHD –Path E:\vms\testvhd\test-converted.vhd

Path                    : E:\vms\testvhd\test-converted.vhd
VhdFormat               : VHD
VhdType                 : Dynamic
FileSize                : 69369856
Size                    : 10737418240
MinimumSize             : 10735321088
LogicalSectorSize       : 512
PhysicalSectorSize      : 512
BlockSize               : 2097152
ParentPath              :
FragmentationPercentage : 0
Alignment               : 1
Attached                : False
DiskNumber              :
IsDeleted               : False
Number                  :
```

## <a name="vhdx-format"></a>VHDX 格式

VHDX 是新的虛擬硬碟格式，Windows Server 2012，可讓您建立具有恢復功能高效能多達 64 tb 虛擬磁碟中導入。 這種格式的優點包括：

-   支援的最多 64 tb 的虛擬硬碟儲存體容量。

-   將更新記錄至 VHDX 中繼資料結構，預防電力中斷時發生資料損毀。

-   若要儲存的檔案，使用者可能會想要搜尋的記錄，例如作業系統版本或修補程式套用的自訂中繼資料的能力。

VHDX 格式也提供下列效能優勢：

-   改善虛擬硬碟格式的對齊方式，以便使用於大型磁區磁碟。

-   動態與差異磁碟，可讓這些磁碟順應工作負載的需求的較大區塊大小。

-   4 KB 邏輯磁區虛擬磁碟能夠提升效能，當應用程式和工作負載所設計供針對 4KB 磁區。

-   表示資料，這會導致較小的檔案大小，並允許基礎實體儲存裝置回收未使用的空間效率。 （修剪要求通過或 SCSI 磁碟和修剪相容的硬體）。

當您升級至 Windows Server 2016 時，我們建議您將所有的 VHD 檔案轉換為 VHDX 格式，因為這些優點。 它可以在其中進行合理地保護檔案的 VHD 格式的唯一案例時，虛擬機器有可能會移到不支援 VHDX 格式的 HYPER-V 的舊版。

## <a name="types-of-virtual-hard-disk-files"></a>類型的虛擬硬碟檔案

有三種類型的 VHD 檔案。 下列各節是效能特性和型別之間的取捨。

下列建議應該列入考量，與選取的 VHD 檔案類型：

-   使用 VHD 格式時，我們建議您使用的是固定的型別，因為它有更好的恢復功能和效能特性，相較於其他 VHD 檔案類型。

-   使用 VHDX 格式時，我們建議您使用動態型別，因為它提供除了節省空間與配置空間時，才需要執行這項操作相關聯的復原能力。

-   固定的型別時，也建議，不論格式，主機磁碟區上的儲存體不會主動監視以確保有足夠的磁碟空間在執行階段擴充 VHD 檔案時。

-   虛擬機器的快照集可讓您建立差異 VHD 來儲存寫入磁碟。 有只有少數的快照集可以提高 CPU 使用量的儲存體 I/o，但是可能不明顯影響以外的高 I/O 密集使用伺服器工作負載的效能。 不過，有大型的一連串的快照集可能明顯影響效能因為 VHD 讀取可能需要檢查要求的區塊，在許多差異 Vhd。 保持簡短快照鏈結是維護良好的磁碟 I/O 效能的重要的。

## <a name="fixed-virtual-hard-disk-type"></a>固定虛擬硬碟類型

VHD 檔案建立時，會先配置空間的 VHD。 這種類型的 VHD 檔案就比較不容易片段，以減少 I/O 輸送量，當單一 I/O 分裂成數個 I/o。 因為讀取和寫入不需要查閱區塊的對應，它就會有三種 VHD 檔案類型的最低的 CPU 額外負荷。

## <a name="dynamic-virtual-hard-disk-type"></a>動態虛擬硬碟類型

Vhd 的空間是隨選配置。 磁碟中的區塊會啟動為未配置的區塊，並不受任何實際的空間，檔案中。 第一個區塊時寫入，虛擬化堆疊必須將 VHD 檔案內的空間配置區塊的標籤，然後再更新中繼資料。 這會增加寫入的數目所需的磁碟 I/o，並增加 CPU 使用量。 讀取並寫入至現有的區塊產生磁碟存取和 CPU 負荷查閱中繼資料中的區塊的對應時。

## <a name="differencing-virtual-hard-disk-type"></a>差異虛擬硬碟類型

VHD 會指向父 VHD 檔案。 任何寫入區塊不會寫入至導致如同動態擴充 VHD 的 VHD 檔案，在配置的空間。 如果已寫入區塊，會從 VHD 檔案服務讀取。 否則，它們被由父代 VHD 檔案。 在這兩種情況下，會讀取中繼資料，來判斷區塊的對應。 讀取和寫入此 VHD 可以取用更多的 CPU，並導致更多的 I/o，比固定的 VHD 檔案。

## <a name="block-size-considerations"></a>區塊大小的考量

區塊大小會大幅影響效能。 最好是將符合工作負載使用磁碟的配置模式的區塊大小。 比方說，如果應用程式 16 mb 的區塊中配置，它會為 16 MB 的虛擬硬碟區塊大小的最佳。 區塊大小為&gt;2MB 可只在使用 VHDX 格式虛擬硬碟上。 具有較大的區塊大小比隨機 I/O 工作負載的配置模式，可大幅提升在主機上的空間使用量。

## <a name="sector-size-implications"></a>磁區大小的影響

大部分的軟體產業都相依磁碟磁區為 512 個位元組，但標準會移至 4 KB 磁碟磁區。 若要減少可能引起的磁區大小變更的相容性問題，硬碟廠商引進過渡期的大小為 512 的模擬磁碟機 (512e) 參考。

這些模擬磁碟機提供一些 4 KB 磁碟磁區原生磁碟機，例如改進的格式效率及改良的配置錯誤修正碼 (ECC) 所提供的優點。 它們來自較少藉由公開於在磁碟介面的 4 KB 磁區大小，就會發生的相容性問題。

## <a name="support-for-512e-disks"></a>512e 磁碟的支援

512e 磁碟只能從實體磁區寫入執行 — 也就是它無法直接寫入對它發出的 512 位元組磁區。 可讓這些寫入磁碟中的內部程序遵循下列步驟：

-   磁碟讀取 4 KB 實體磁區設成其內部快取，其中包含寫入中參照的 512 位元組邏輯磁區。

-   4 KB 緩衝區中的資料會修改成包含更新的 512 位元組磁區。

-   磁碟會在磁碟上執行已更新的 4 KB 緩衝區，回到其實體磁區的寫入。

此程序稱為讀取-修改-寫入 (RMW)。 此 RMW 程序的整體效能影響取決於工作負載。 RMW 程序虛擬硬碟會造成效能降低，原因如下：

-   動態與差異虛擬硬碟具有其資料裝載前面的 512 位元組磁區點陣圖。 此外，頁尾、 頁首和父代定位器對齊 512 位元組磁區。 通常會發出 512 位元組寫入命令來更新這些結構，導致先前所述的 RMW 程序的虛擬硬碟驅動程式。

-   應用程式通常問題讀取和寫入的 4 KB 大小 （預設叢集大小的 NTFS） 的倍數。 因為沒有動態資料裝載區塊前面的 512 位元組磁區點陣圖，而且差異虛擬硬碟，4 KB 區塊不會對齊實體 4 KB 界限。 下圖顯示 VHD 4 KB 區塊 （反白顯示） 也就是未對齊實體 4 KB 界限。

![vhd 4 kb 區塊](../../media/perftune-guide-vhd-4kb-block.png)

目前的剖析器為了更新裝載資料所發出的每個 4 KB 寫入命令會導致兩次讀取會進行更新，並接著寫回至兩個磁碟區塊的磁碟上的兩個區塊。 Windows Server 2016 中的 HYPER-V 可減輕部分 VHD 堆疊上 512e 磁碟上的效能影響由先前所述的結構準備 VHD 格式的 4 KB 界限的對齊方式。 存取虛擬硬碟檔案內的資料時，和更新的虛擬硬碟的中繼資料結構時，這可避免的 RMW 影響。

如先前所述，從舊版的 Windows Server 中複製的 Vhd 將不會自動對齊 4 KB。 您可以手動將它們轉換使用以最佳方式對齊**從來源複製**磁碟是 VHD 介面中的 可用的選項。

根據預設，Vhd 會公開的 512 位元組實體磁區大小。 這是為了確保應用程式和 Vhd 從舊版的 Windows Server 移動時不會影響實體磁區大小相依的應用程式。

根據預設，磁碟使用 VHDX 格式會以 4 KB 實體磁區大小，以最佳化其效能設定檔的一般磁碟和大型磁區磁碟。 若要充分利用 4 KB 磁區建議使用 VHDX 格式。

## <a name="support-for-native-4kb-disks"></a>原生 4 KB 磁碟支援

Hyper-v 在 Windows Server 2012 R2 和更新支援 4 KB 原生磁碟版本。 但仍可在原生 4 KB 的磁碟上儲存 VHD 磁碟。 這是藉由實作軟體 RMW 演算法在虛擬儲存體堆疊層將 512 位元組存取和更新要求轉換成對應的 4 KB 存取和更新。

VHD 檔案可以只會公開本身為 512 位元組邏輯磁區大小的磁碟，因為它是很有可能會發出 512 位元組 I/O 要求的應用程式。 在這些情況下，RMW 層會滿足這些要求，並會導致效能降低。 這也適用於格式化使用的邏輯磁區大小為 512 個位元組的 VHDX 磁碟。

可設定公開為 4 KB 邏輯磁區大小磁碟的 VHDX 檔案，並在 4 KB 原生實體裝置上裝載的磁碟時，這會是最佳的組態，以效能。 應該小心以確保來賓，使用虛擬磁碟的應用程式都會受到 4 KB 邏輯磁區大小。 VHDX 格式將會在 4 KB 邏輯磁區大小的裝置上正確運作的。

## <a name="pass-through-disks"></a>傳遞磁碟

在虛擬機器 VHD 可以是直接對應到實體磁碟或邏輯單元編號 (LUN)，而不是到 VHD 檔案。 優點是這項設定會略過 NTFS 檔案系統中的根磁碟分割，進而減少儲存體 I/O 的 CPU 使用量。 風險在於，實體磁碟或 Lun 可能更難以比 VHD 檔案在電腦之間移動。

傳遞磁碟都應該避免使用所導入的虛擬機器的移轉案例的限制。

## <a name="advanced-storage-features"></a>進階儲存體功能

### <a name="storage-quality-of-service-qos"></a>存放裝置服務品質 (QoS)

從 Windows Server 2012 R2 開始，HYPER-V 會包含在虛擬機器上設定儲存體的特定服務品質 (QoS) 參數的能力。 存放裝置 QoS 在多組織用戶環境提供存放效能隔離，還提供在存放裝置的 I/O 效能沒有達到設定的閾值而無法有效執行虛擬機器工作負載時通知您的機制。

存放裝置 QoS 能夠指定虛擬硬碟每秒輸入/輸出作業 (IOPS) 的最大值。 系統管理員可調節存放裝置 I/O，阻止一個用戶耗用過多存放裝置資源而影響另一位用戶。

您也可以設定最小 IOPS 值。 當指定的虛擬硬碟的 IOPS 低於達到最佳效能所需的閾值時，會通知系統管理員。

虛擬機器計量基礎結構也會使用存放裝置相關的參數加以更新，以便允許系統管理員監視效能及計價相關的參數。

根據其中每 8 KB 的資料服務各會計為 I/O 的正規化 IOPS 來指定最大和最小值。

某些限制，如下所示：

-   只針對虛擬磁碟

-   差異磁碟上不同的磁碟區不能有父代的虛擬磁碟

-   複本-複本站台分別設定從主要站台的 QoS

-   不支援共用的 VHDX

如需存放裝置服務品質的詳細資訊，請參閱[存放裝置服務品質 hyper-v](https://technet.microsoft.com/library/dn282281.aspx)。

### <a name="numa-io"></a>NUMA I/O

Windows Server 2012 和更新版本支援大型虛擬機器和任何大型虛擬機器組態 （例如，具有以虛擬的 64 個處理器執行 Microsoft SQL Server 的組態） 也需要延展性方面的 I/O 輸送量。

第一次引進的 Windows Server 2012 儲存堆疊和 HYPER-V 中的下列重要改良功能提供大型虛擬機器的 I/O 延展性需求：

-   建立客體裝置與主機儲存體堆疊之間的通訊通道的數目增加。

-   涉及插斷分佈，以及虛擬處理器，以避免昂貴的 interprocessor 中斷而更有效率 I/O 完成機制。

Windows Server 2012 中引入，有幾個登錄項目，位於 HKLM\\系統\\CurrentControlSet\\Enum\\VMBUS\\{裝置識別碼}\\{執行個體識別碼}\\StorChannel 可調整的通道數目。 它們也會對齊處理由應用程式可以 I/O 處理器指派虛擬 Cpu I/O 完成虛擬處理器。 登錄設定上為每個介面卡裝置的硬體金鑰。

-   **ChannelCount (DWORD)** 的使用，最多 16 個的通道總數。 預設為上限，也就是虛擬處理器/16 的數目。

-   **ChannelMask (QWORD)** 通道的處理器親和性。 如果未設定或設為 0，則會預設為您使用標準儲存體或網路頻道的現有通道分配演算法。 這可確保您的儲存體通道將不會衝突的網路通道。

### <a name="offloaded-data-transfer-integration"></a>卸載的資料傳輸的整合

重要的維護工作的 Vhd，如合併、 移動，並壓縮，相依複製大量資料。 目前複製資料的方法要求在不同的位置讀入和寫入資料，但這很耗費時間。 它也會使用在主機上，無法使用服務的虛擬機器的 CPU 和記憶體資源。

存放區域網路 (SAN) 廠商正努力提供接近即時的大量資料複製作業。 此儲存體被設計成允許指定的特定資料集，從一個位置之間移動的磁碟上的系統。 此硬體功能，稱為卸載資料傳輸。

HYPER-V 在 Windows Server 2012 和更新版本支援卸載資料傳輸 (ODX) 作業，因此這些作業可以從客體作業系統傳送到主機硬體。 這可確保工作負載可以使用已啟用 ODX 的儲存體，如同它已在非虛擬化環境中執行。 HYPER-V 儲存堆疊也會發出 Vhd 的維護作業期間 ODX 作業例如合併磁碟和存放裝置移轉中繼作業其中會移動大量資料。

### <a name="unmap-integration"></a>取消對應整合

虛擬硬碟檔案是存在於儲存體磁碟區上的檔案，並與其他檔案共用可用空間。 這些檔案的大小通常很大，因為它們會耗用的空間可能會快速地擴增。 更多的實體儲存體的需求會影響 IT 硬體預算。 請務必最佳化的最大的實體儲存體使用。

之前 Windows Server 2012，當應用程式刪除內容內的虛擬硬碟，其有效地放棄內容的儲存空間，客體作業系統和 HYPER-V 主機中的 Windows 儲存堆疊必須避免這樣的限制從要與虛擬硬碟的實體存放裝置進行通訊的資訊。 這可以防止 HYPER-V 儲存堆疊最佳化 VHD 為基礎的虛擬磁碟檔案的空間使用量。 它也可以防止基礎儲存裝置回收已刪除的資料之前所佔據的空間。

從 Windows Server 2012 開始，HYPER-V 支援取消對應通知，以便能夠更有效率，表示該資料的 VHDX 檔案。 這會導致較小的檔案大小，並可讓基礎實體儲存裝置回收未使用的空間。

只有 Hyper-v 特定 SCSI、 啟用的 IDE，以及虛擬光纖通道控制器允許 unmap 命令連線到客體的主機虛擬儲存體堆疊。 虛擬硬碟中，只有虛擬磁碟設定為支援 VHDX 格式的取消對應從來賓的命令。

基於這些理由，我們建議您使用時未使用虛擬光纖通道磁碟連結到 SCSI 控制器的 VHDX 檔案。

## <a name="see-also"></a>另請參閱

-   [HYPER-V 術語](terminology.md)

-   [HYPER-V 架構](architecture.md)

-   [HYPER-V 伺服器組態](configuration.md)

-   [HYPER-V 處理器效能](processor-performance.md)

-   [HYPER-V 記憶體效能](memory-performance.md)

-   [HYPER-V 網路 I/O 效能](network-io-performance.md)

-   [虛擬化環境中偵測瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
