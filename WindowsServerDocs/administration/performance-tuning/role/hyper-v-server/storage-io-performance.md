---
title: Hyper-v 存放裝置 i/o 效能
description: Hyper-v 效能調整中的儲存體 i/o 效能考慮
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 83b22c47cb23b02bb9984e03d78fcae93be1ca0a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851811"
---
# <a name="hyper-v-storage-io-performance"></a>Hyper-v 存放裝置 i/o 效能

本節說明在虛擬機器中微調儲存體 i/o 效能的不同選項和考慮。 存放裝置 i/o 路徑會從來賓儲存體堆疊，透過主機虛擬化層擴充到主機存放裝置堆疊，然後再延伸至實體磁片。 以下說明如何在每個階段進行優化。

## <a name="virtual-controllers"></a>虛擬控制器

Hyper-v 提供三種類型的虛擬控制器： IDE、SCSI 和虛擬主機匯流排介面卡（Hba）。

## <a name="ide"></a>IDE

IDE 控制器會將 IDE 磁片公開給虛擬機器。 IDE 控制器已模擬，而且只有在沒有虛擬機器 Integration Services 的情況下，才可供執行舊版 Windows 的來賓 Vm 使用的控制器。 使用虛擬機器 Integration Services 所提供的 IDE 篩選驅動程式執行的磁片 i/o，明顯優於模擬 IDE 控制器所提供的磁片 i/o 效能。 我們建議您只將 IDE 磁片用於作業系統磁片，因為它們有效能限制，因為可以發行到這些裝置的 i/o 大小上限。

## <a name="scsi-sas-controller"></a>SCSI （SAS 控制器）

SCSI 控制器會向虛擬機器公開 SCSI 磁片，而每個虛擬 SCSI 控制器最多可支援64部裝置。 為了達到最佳效能，我們建議您將多個磁片連接至單一虛擬 SCSI 控制器，並建立額外的控制器，而只需要調整連線到虛擬機器的磁片數目。 SCSI 路徑不會模擬，使其成為作業系統磁片以外任何磁片的慣用控制器。 事實上，在第2代 Vm 中，這是唯一可行的控制器類型。 在 Windows Server 2012 R2 中引進，此控制器會回報為 SAS 以支援共用 VHDX。

## <a name="virtual-fibre-channel-hbas"></a>虛擬光纖通道 Hba

虛擬光纖通道 Hba 可設定為允許虛擬機器直接存取，以透過乙太網路（FCoE） Lun 光纖通道和光纖通道。 虛擬光纖通道磁片會略過根磁碟分割中的 NTFS 檔案系統，以減少儲存體 i/o 的 CPU 使用量。

在多部虛擬機器之間共用的大型資料磁片磁碟機和磁片磁碟機（適用于來賓叢集案例）是虛擬光纖通道磁片的主要候選項目。

虛擬光纖通道磁片需要在主機上安裝一或多個光纖通道主機匯流排介面卡（Hba）。 每個主機 HBA 都必須使用支援 Windows Server 2016 虛擬光纖通道/NPIV 功能的 HBA 驅動程式。 SAN 網狀架構應支援 NPIV，而且應該在支援 NPIV 的光纖通道拓撲中，設定用於虛擬光纖通道的 HBA 埠。

若要在隨一個以上的 HBA 安裝的主機上最大化輸送量，建議您在 Hyper-v 虛擬機器內設定多個虛擬 Hba （每個虛擬機器最多可以設定四個 Hba）。 Hyper-v 會自動進行平衡虛擬 Hba 的工作，以裝載存取相同虛擬 SAN 的 Hba。

## <a name="virtual-disks"></a>虛擬磁片

磁片可以透過虛擬控制器向虛擬機器公開。 這些磁片可以是虛擬硬碟，它們是磁片上的檔案抽象或主機上的傳遞磁片。

## <a name="virtual-hard-disks"></a>虛擬硬碟

有兩種虛擬硬碟格式： VHD 和 VHDX。 這兩種格式都支援三種類型的虛擬硬碟檔案。

## <a name="vhd-format"></a>VHD 格式

VHD 格式是 Hyper-v 在過去版本中唯一支援的虛擬硬碟格式。 在 Windows Server 2012 中引進的 VHD 格式已經過修改，以提供更好的對齊方式，讓新的大型磁區磁片上的效能大幅提升。

在 Windows Server 2012 或更新版本上建立的任何新 VHD 都具有最佳的 4 KB 對齊。 這個對齊的格式與舊版的 Windows Server 作業系統完全相容。 不過，如果剖析器的新配置不是 4 KB 對齊感知（例如來自舊版 Windows Server 的 VHD 剖析器或非 Microsoft 剖析器），則對齊屬性將會中斷。

從舊版本移動的任何 VHD 不會自動轉換成這個新的改良 VHD 格式。

若要轉換成新的 VHD 格式，請執行下列 Windows PowerShell 命令：

``` syntax
Convert-VHD –Path E:\vms\testvhd\test.vhd –DestinationPath E:\vms\testvhd\test-converted.vhd
```

您可以檢查系統上所有 Vhd 的 [對齊] 屬性，並將它轉換為最佳的 4 KB 對齊方式。 您可以使用原始 VHD 中的資料，透過 [**建立來源**] 選項來建立新的 VHD。

若要使用 Windows Powershell 檢查對齊方式，請檢查對齊線，如下所示：

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

若要使用 Windows PowerShell 確認對齊，請檢查對齊線，如下所示：

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

VHDX 是 Windows Server 2012 引進的新虛擬硬碟格式，可讓您建立彈性高效能的虛擬磁片，最多可達 64 tb。 此格式的優點包括：

-   支援高達 64 tb 的虛擬硬碟儲存體容量。

-   將更新記錄至 VHDX 中繼資料結構，預防電力中斷時發生資料損毀。

-   能夠儲存有關檔案的自訂中繼資料，使用者可能會想要記錄，例如套用的作業系統版本或修補程式。

VHDX 格式也提供下列效能優點：

-   改善虛擬硬碟格式的對齊方式，以便使用於大型磁區磁碟。

-   動態和差異磁片的較大區塊大小，可讓這些磁片順應至工作負載的需求。

-   4 KB 的邏輯磁區虛擬磁片，可在專為 4 KB 磁區設計的應用程式和工作負載使用時，提供更高的效能。

-   效率表示資料，這會產生較小的檔案大小，並允許基礎實體存放裝置回收未使用的空間。 （Trim 需要傳遞或 SCSI 磁片，以及與 Trim 相容的硬體）。

當您升級至 Windows Server 2016 時，建議您將所有 VHD 檔案轉換成 VHDX 格式，因為這些優點。 只有當虛擬機器可能會移至不支援 VHDX 格式的舊版 Hyper-v 時，才會有意義，讓檔案保持 VHD 格式是合理的情況。

## <a name="types-of-virtual-hard-disk-files"></a>虛擬硬碟檔案的類型

VHD 檔案有三種類型。 下列各節是類型之間的效能特性和取捨。

關於選取 VHD 檔案類型，請考慮下列建議：

-   使用 VHD 格式時，建議您使用固定類型，因為相較于其他 VHD 檔案類型，其復原能力和效能特性更佳。

-   使用 VHDX 格式時，建議您使用動態類型，因為除了節省空間，只有在需要執行這項操作時，才會提供復原保證。

-   當裝載磁片區上的儲存體未受到主動監視以確保在執行時間擴充 VHD 檔案時有足夠的磁碟空間時，也建議使用固定類型，而不論格式。

-   虛擬機器的快照集會建立差異 VHD，以儲存磁片的寫入。 只有幾個快照集可以提升儲存體 i/o 的 CPU 使用量，但可能不會明顯影響效能，除非是高度 i/o 密集的伺服器工作負載。 不過，擁有大量的快照集會明顯影響效能，因為從 VHD 讀取可能需要在許多差異 Vhd 中檢查要求的區塊。 將快照集鏈保持簡短，對於維護良好的磁片 i/o 效能相當重要。

## <a name="fixed-virtual-hard-disk-type"></a>已修正虛擬硬碟類型

建立 VHD 檔案時，會先配置 VHD 的空間。 這種類型的 VHD 檔案較不可能片段，這可減少單一 i/o 分割成多個 i/o 時的 i/o 輸送量。 這三個 VHD 檔案類型的 CPU 額外負荷最低，因為讀取和寫入不需要查閱區塊的對應。

## <a name="dynamic-virtual-hard-disk-type"></a>動態虛擬硬碟類型

VHD 的空間會視需要配置。 磁片中的區塊會啟動為未配置的區塊，並不受檔案中的任何實際空間支援。 第一次寫入區塊時，虛擬化堆疊必須在區塊的 VHD 檔案內配置空間，然後更新中繼資料。 這會增加寫入所需的磁片 i/o 數目，並增加 CPU 使用量。 在中繼資料中查詢區塊對應時，讀取和寫入現有區塊會產生磁片存取和 CPU 負荷。

## <a name="differencing-virtual-hard-disk-type"></a>差異虛擬硬碟類型

VHD 會指向父 VHD 檔案。 對區塊的任何寫入都不會寫入，因而導致 VHD 檔案中配置空間，如同動態擴充的 VHD。 如果區塊已寫入，則會從 VHD 檔案提供讀取。 否則，它們會從父 VHD 檔案進行服務。 在這兩種情況下，都會讀取中繼資料來決定區塊的對應。 對此 VHD 的讀取和寫入可能會耗用更多的 CPU，並導致比固定 VHD 檔案更多的 i/o。

## <a name="block-size-considerations"></a>區塊大小考慮

區塊大小可能會大幅影響效能。 將區塊大小與使用磁片的工作負載配置模式進行比對，是最佳做法。 例如，如果應用程式是以 16 MB 的區塊進行配置，則虛擬硬碟區塊大小最好是 16 MB。 只有在具有 VHDX 格式的虛擬硬碟上，才可以使用 &gt;2 MB 的區塊大小。 如果區塊大小大於隨機 i/o 工作負載的配置模式，將會大幅增加主機上的空間使用量。

## <a name="sector-size-implications"></a>磁區大小的影響

大部分的軟體產業都依存于512個位元組的磁片磁區，但標準會移到 4 KB 的磁片磁區。 為了減少磁區大小變更時可能發生的相容性問題，硬碟廠商引進了一種轉換大小，稱為512模擬磁片磁碟機（512e）。

這些模擬磁片磁碟機提供 4 KB 磁片磁區原生磁片磁碟機所提供的一些優點，例如改良的格式效率，以及改善錯誤修正碼（ECC）的配置。 在磁片介面上公開 4 KB 磁區大小，就會產生較少的相容性問題。

## <a name="support-for-512e-disks"></a>支援512e 磁片

512e 磁片只能以實體磁區的角度執行寫入，也就是說，它無法直接寫入發行給它的512byte 磁區。 在磁片中進行這些寫入的內部程式，可能會遵循下列步驟：

-   磁片會將 4 KB 實體磁區讀取至其內部快取，其中包含寫入中所參考的512位元組邏輯磁區。

-   4 KB 緩衝區中的資料會修改成包含更新的 512 位元組磁區。

-   磁片會將更新的 4 KB 緩衝區寫回磁片上的實體磁區。

此程式稱為讀取-修改-寫入（RMW）。 RMW 程式的整體效能影響取決於工作負載。 RMW 程式會導致虛擬硬碟的效能降低，原因如下：

-   動態和差異虛擬硬碟在其資料裝載前方具有512個位元組的磁區點陣圖。 此外，頁尾、標頭和父系定位器會對齊512位元組的磁區。 虛擬硬碟驅動程式通常會發出512位元組的寫入命令來更新這些結構，因而導致先前所述的 RMW 程式。

-   應用程式通常會以 4 KB 大小的倍數（NTFS 的預設叢集大小）來發出讀取和寫入。 由於動態和差異虛擬硬碟的資料承載區塊前面有512個位元組的磁區點陣圖，因此 4 KB 區塊不會對齊實體 4 KB 界限。 下圖顯示未與實體 4 KB 界限對齊的 VHD 4 KB 區塊（反白顯示）。

![vhd 4 kb 區塊](../../media/perftune-guide-vhd-4kb-block.png)

目前剖析器所發出的每個 4 KB 寫入命令都會更新承載資料，而導致磁片上兩個區塊的兩次讀取，然後更新並隨後寫回兩個磁片區塊。 Windows Server 2016 中的 hyper-v 會藉由準備先前提到的結構來對齊 VHD 格式的 4 KB 界限，以減輕 VHD 堆疊上512e 磁片的一些效能影響。 這可避免在存取虛擬硬碟檔案中的資料時，以及在更新虛擬硬碟元資料結構時的 RMW 效果。

如先前所述，從舊版 Windows Server 複製的 Vhd 不會自動對齊 4 KB。 您可以使用 VHD 介面中提供的 [**從來源磁碟複製**] 選項，手動將它們轉換成最佳對齊方式。

根據預設，Vhd 會以512個位元組的實體磁區大小來公開。 這麼做是為了確保當應用程式和 Vhd 從舊版 Windows Server 移動時，不會影響實體磁區大小的相依應用程式。

根據預設，會使用 4 KB 的實體磁區大小來建立具有 VHDX 格式的磁片，以優化其效能設定檔的一般磁片和大型磁區磁片。 若要充分利用 4 KB 的磁區，建議使用 VHDX 格式。

## <a name="support-for-native-4kb-disks"></a>原生 4 KB 磁片的支援

Windows Server 2012 R2 和以上的 hyper-v 支援 4 KB 的原生磁片。 但是，您仍然可以在 4 KB 的原生磁片上儲存 VHD 磁片。 這是藉由在虛擬儲存堆疊層中執行軟體 RMW 演算法來完成，將512位元組存取和更新要求轉換為對應的 4 KB 存取和更新。

因為 VHD 檔案只能將自己公開為512個位元組的邏輯磁區大小磁片，所以很有可能會有應用程式發出512位元組的 i/o 要求。 在這些情況下，RMW 層會滿足這些要求，並導致效能降低。 這也適用于使用 VHDX 格式化的磁片，其邏輯磁區大小為512個位元組。

您可以將 VHDX 檔案設定為以 4 KB 的邏輯磁區大小磁片的形式公開，而且當磁片裝載于 4 KB 的原生實體裝置上時，這會是最佳的效能設定。 請小心，以確保使用虛擬磁片的來賓和應用程式都是由 4 KB 的邏輯磁區大小所支援。 VHDX 格式可以在 4 KB 的邏輯磁區大小裝置上正確運作。

## <a name="pass-through-disks"></a>傳遞磁片

虛擬機器中的 VHD 可以直接對應到實體磁片或邏輯單元編號（LUN），而不是 VHD 檔案。 其優點是，此設定會略過根磁碟分割中的 NTFS 檔案系統，以減少儲存體 i/o 的 CPU 使用量。 其風險是，實體磁片或 Lun 在機器之間移動時，可能會比 VHD 檔案更容易。

由於虛擬機器遷移案例所引進的限制，應避免傳遞磁片。

## <a name="advanced-storage-features"></a>先進的儲存體功能

### <a name="storage-quality-of-service-qos"></a>存放裝置服務品質 (QoS)

從 Windows Server 2012 R2 開始，Hyper-v 包含在虛擬機器上為存放裝置設定特定服務品質（QoS）參數的能力。 存放裝置 QoS 在多組織用戶環境提供存放效能隔離，還提供在存放裝置的 I/O 效能沒有達到設定的閾值而無法有效執行虛擬機器工作負載時通知您的機制。

存放裝置 QoS 能夠指定虛擬硬碟每秒輸入/輸出作業 (IOPS) 的最大值。 系統管理員可調節存放裝置 I/O，阻止一個用戶耗用過多存放裝置資源而影響另一位用戶。

您也可以設定最小 IOPS 值。 當指定的虛擬硬碟的 IOPS 低於達到最佳效能所需的閾值時，會通知系統管理員。

虛擬機器計量基礎結構也會使用存放裝置相關的參數加以更新，以便允許系統管理員監視效能及計價相關的參數。

最大和最小值是根據正規化 IOPS 來指定，其中每 8 KB 的資料都會計算為 i/o。

其中一些限制如下：

-   僅適用于虛擬磁片

-   差異磁片不能在不同的磁片區上有父系虛擬磁片

-   複本-複本網站的 QoS 已與主要網站分開設定

-   不支援共用 VHDX

如需存放裝置服務品質的詳細資訊，請參閱[hyper-v 的存放裝置服務品質](https://technet.microsoft.com/library/dn282281.aspx)。

### <a name="numa-io"></a>NUMA I/O

Windows Server 2012 （含）以上的支援大型虛擬機器，而且任何大型虛擬機器設定（例如，Microsoft SQL Server 以64虛擬處理器執行的設定）也需要以 i/o 輸送量為依據的擴充性。

Windows Server 2012 儲存堆疊和 Hyper-v 中首次引進的下列主要改良功能，提供大型虛擬機器的 i/o 擴充性需求：

-   在來賓裝置與主機存放裝置堆疊之間建立的通道數目增加。

-   更有效率的 i/o 完成機制，涉及虛擬處理器之間的中斷分佈，以避免耗費資源的 interprocessor 中斷。

在 Windows Server 2012 中引進了一些登錄專案，位於 HKLM\\系統\\CurrentControlSet\\Enum\\VMBUS\\{device id}\\{instance id}\\StorChannel，可讓您調整頻道數目。 它們也會將處理 i/o 完成的虛擬處理器，與應用程式指派給 i/o 處理器的虛擬 Cpu 對齊。 登錄設定會根據裝置的硬體機碼，以每個介面卡為基礎進行設定。

-   **ChannelCount （DWORD）** 要使用的通道總數，最大值為16。 其預設值為上限，這是虛擬處理器/16 的數目。

-   **ChannelMask （QWORD）** 通道的處理器親和性。 如果未設定或設定為0，則會預設為您用於一般儲存體或網路通道的現有通道散發演算法。 這可確保您的儲存通道不會與您的網路通道衝突。

### <a name="offloaded-data-transfer-integration"></a>卸載的資料傳輸整合

Vhd 的重要維護工作，例如 merge、move 和 compact，會相依于複製大量資料。 目前複製資料的方法要求在不同的位置讀入和寫入資料，但這很耗費時間。 它也會使用主機上的 CPU 和記憶體資源，這可能已用來服務虛擬機器。

存放區域網路 (SAN) 廠商正努力提供接近即時的大量資料複製作業。 此儲存體的設計可讓磁片上的系統指定將特定資料集從一個位置移到另一個位置。 此硬體功能稱為卸載的資料傳輸。

Windows Server 2012 和以上的 hyper-v 支援卸載資料傳輸（ODX）作業，可讓您從客體作業系統將這些作業傳遞到主機硬體。 這可確保工作負載可以使用已啟用 ODX 的儲存體，就像在非虛擬化環境中執行一樣。 Hyper-v 存放裝置堆疊也會在 Vhd 的維護作業期間發出 ODX 作業，例如合併磁片，以及移動大量資料的儲存體遷移中繼作業。

### <a name="unmap-integration"></a>取消對應整合

虛擬硬碟檔案會以檔案的形式存在於存放磁片區上，並與其他檔案共用可用空間。 因為這些檔案的大小通常會很大，所以取用的空間可能會快速成長。 對更多實體儲存體的需求會影響 IT 硬體預算。 請務必盡可能將實體儲存體的使用優化。

在 Windows Server 2012 之前，當應用程式刪除虛擬硬碟中的內容（這實際上會放棄內容的儲存空間）時，客體作業系統和 Hyper-v 主機中的 Windows 儲存堆疊具有防止此資訊傳送到虛擬硬碟和實體存放裝置的限制。 這使得 Hyper-v 存放裝置堆疊無法將 VHD 虛擬磁片檔案的空間使用量優化。 它也會防止基礎存放裝置回收已刪除資料先前所佔用的空間。

從 Windows Server 2012 開始，Hyper-v 支援取消對應通知，讓 VHDX 檔案在代表其中的資料時更有效率。 這會產生較小的檔案大小，並允許基礎實體存放裝置回收未使用的空間。

只有 Hyper-v 特定的 SCSI、啟用 rms IDE 和虛擬光纖通道控制器允許來自來賓的取消對應命令到達主機虛擬儲存體堆疊。 在虛擬硬碟上，只有格式化為 VHDX 的虛擬磁片支援來自來賓的取消對應命令。

基於這些理由，建議您在不使用虛擬光纖通道磁片的情況下，使用連接到 SCSI 控制器的 VHDX 檔案。

## <a name="see-also"></a>另請參閱

-   [Hyper-V 術語](terminology.md)

-   [Hyper-V 架構](architecture.md)

-   [Hyper-V 伺服器設定](configuration.md)

-   [Hyper-V 處理器效能](processor-performance.md)

-   [Hyper-V 記憶體效能](memory-performance.md)

-   [Hyper-V 網路 I/O 效能](network-io-performance.md)

-   [偵測虛擬化環境中的瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
