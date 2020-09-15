---
title: Hyper-v 儲存體 i/o 效能
description: Hyper-v 效能調整中的儲存體 i/o 效能考慮
ms.topic: article
ms.author: asmahi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 762ff719eb60a2fbcec61c0b9b6cb2e14f9ba677
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078216"
---
# <a name="hyper-v-storage-io-performance"></a>Hyper-v 儲存體 i/o 效能

本節說明在虛擬機器中調整儲存體 i/o 效能的不同選項和考慮。 儲存體 i/o 路徑會從來賓儲存體堆疊延伸至主機虛擬化層、主機存放裝置堆疊，然後擴充至實體磁片。 以下是每個階段如何進行優化的說明。

## <a name="virtual-controllers"></a>虛擬控制器

Hyper-v 提供三種類型的虛擬控制器： IDE、SCSI 和虛擬主機匯流排介面卡， (Hba) 。

## <a name="ide"></a>IDE

IDE 控制器會將 IDE 磁片公開給虛擬機器。 IDE 控制器是模擬的，而且它是在沒有虛擬機器 Integration Services 的情況下執行舊版 Windows 的來賓 Vm 唯一可用的控制器。 使用虛擬機器 Integration Services 所提供的 IDE 篩選器驅動程式所執行的磁片 i/o，明顯優於模擬 IDE 控制器提供的磁片 i/o 效能。 建議您只將 IDE 磁片用於作業系統磁片，因為它們有效能限制，因為可以對這些裝置發出的 i/o 大小上限。

## <a name="scsi-sas-controller"></a>SCSI (SAS 控制器) 

SCSI 控制器會將 SCSI 磁片公開給虛擬機器，而每個虛擬 SCSI 控制器最多可支援64部裝置。 為了達到最佳效能，建議您將多個磁片連結至單一虛擬 SCSI 控制器，並建立額外的控制器，因為它們需要調整連線到虛擬機器的磁片數目。 SCSI 路徑不會模擬，因此它會成為作業系統磁片以外的任何磁片的慣用控制器。 事實上，第2代 Vm 是唯一可能的控制器類型。 在 Windows Server 2012 R2 中引進，此控制器會回報為支援共用 VHDX 的 SAS。

## <a name="virtual-fibre-channel-hbas"></a>虛擬光纖通道 Hba

虛擬光纖通道 Hba 可設定為允許虛擬機器直接存取，以透過 Ethernet (FCoE) Lun 來光纖通道和光纖通道。 虛擬光纖通道磁片會在根磁碟分割中略過 NTFS 檔案系統，以減少儲存體 i/o 的 CPU 使用量。

在多部虛擬機器之間共用的大型資料磁片磁碟機和磁片磁碟機 (用於來賓叢集案例) 是虛擬光纖通道磁片的主要候選。

虛擬光纖通道磁片需要在主機上安裝一或多個光纖通道的主機匯流排介面卡 (Hba) 。 每個主機 HBA 都必須使用支援 Windows Server 2016 Virtual 光纖通道/NPIV 功能的 HBA 驅動程式。 SAN 網狀架構應該支援 NPIV，且必須在支援 NPIV 的光纖通道拓撲中設定用於虛擬光纖通道的 HBA 埠 (s) 。

若要將具有多個 HBA 的主機上的輸送量最大化，建議您在 Hyper-v 虛擬機器中設定多個虛擬 Hba， (可為每個虛擬機器) 設定四個 Hba。 Hyper-v 會自動盡最大努力平衡虛擬 Hba，以裝載可存取相同虛擬 SAN 的 Hba。

## <a name="virtual-disks"></a>虛擬磁片

您可以透過虛擬控制器將磁片公開給虛擬機器。 這些磁片可以是虛擬硬碟，也就是磁片的檔案抽象概念或主機上的傳遞磁片。

## <a name="virtual-hard-disks"></a>虛擬硬碟

有兩個虛擬硬碟格式： VHD 和 VHDX。 這兩種格式都支援三種類型的虛擬硬碟檔案。

## <a name="vhd-format"></a>VHD 格式

VHD 格式是 Hyper-v 在舊版本中唯一支援的虛擬硬碟格式。 在 Windows Server 2012 中引進的 VHD 格式已經過修改，以提供更好的對齊方式，讓新的大型磁區磁片上的效能大幅提升。

在 Windows Server 2012 或更新版本上建立的任何新 VHD 都有最佳的 4 KB 對齊。 這種對齊的格式與舊版 Windows Server 作業系統完全相容。 不過，對齊屬性將會因為剖析器的新配置中斷，且不是 4 KB 對齊感知的 (例如舊版 Windows Server 的 VHD 剖析器或非 Microsoft 剖析器) 。

任何從舊版移動的 VHD 都不會自動轉換成新的改良 VHD 格式。

若要轉換為新的 VHD 格式，請執行下列 Windows PowerShell 命令：

``` syntax
Convert-VHD –Path E:\vms\testvhd\test.vhd –DestinationPath E:\vms\testvhd\test-converted.vhd
```

您可以檢查系統上所有 Vhd 的對齊屬性，並將它轉換成最佳的 4 KB 對齊。 您可以使用 [ **從來源建立** ] 選項，從原始 vhd 中的資料建立新的 vhd。

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

若要使用 Windows PowerShell 來確認對齊，請檢查對齊線，如下所示：

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

VHDX 是 Windows Server 2012 中引進的新虛擬硬碟格式，可讓您建立高達 64 tb 的復原高效能虛擬磁片。 此格式的優點包括：

-   支援高達 64 tb 的虛擬硬碟儲存體容量。

-   將更新記錄至 VHDX 中繼資料結構，預防電力中斷時發生資料損毀。

-   能夠儲存有關檔案的自訂中繼資料（使用者可能想要記錄），例如套用的作業系統版本或修補程式。

VHDX 格式也提供下列效能優點：

-   改善虛擬硬碟格式的對齊方式，以便使用於大型磁區磁碟。

-   動態和差異磁片的較大區塊大小，可讓這些磁片 attune 至工作負載的需求。

-   4 KB 的邏輯磁區虛擬磁片，可在為 4 KB 磁區設計的應用程式和工作負載使用時提高效能。

-   表示資料的效率，這會產生較小的檔案大小，並允許基礎實體儲存體裝置回收未使用的空間。  (Trim 需要傳遞或 SCSI 磁片以及與 Trim 相容的硬體。 ) 

當您升級至 Windows Server 2016 時，建議您將所有 VHD 檔案轉換成 VHDX 格式，因為這些優點。 使用 VHD 格式來保存檔案的唯一情況，就是當虛擬機器有可能移至不支援 VHDX 格式的舊版 Hyper-v 時，是可行的。

## <a name="types-of-virtual-hard-disk-files"></a>虛擬硬碟檔案的類型

VHD 檔案有三種類型。 下列各節是類型之間的效能特性和取捨。

選取 VHD 檔案類型時，應考慮下列建議：

-   使用 VHD 格式時，建議您使用固定類型，因為相較于其他 VHD 檔案類型，它具有更佳的復原能力和效能特性。

-   使用 VHDX 格式時，建議您使用動態類型，因為它除了節省空間之外，還會提供復原保證，而只有在需要時才配置空間。

-   當裝載磁片區上的儲存體未受到主動監視時，也建議使用固定類型，以確保在執行時間擴充 VHD 檔案時，有足夠的磁碟空間。

-   虛擬機器的快照集會建立差異 VHD 來儲存磁片的寫入。 只有幾個快照集可以提升儲存體 i/o 的 CPU 使用量，但可能不會對效能造成顯著的影響，但高度 i/o 密集型伺服器工作負載除外。 不過，擁有大量的快照集會明顯影響效能，因為從 VHD 讀取可能需要檢查許多差異 Vhd 中要求的區塊。 將快照集鏈保持簡短，對於維護良好的磁片 i/o 效能相當重要。

## <a name="fixed-virtual-hard-disk-type"></a>固定虛擬硬碟類型

建立 VHD 檔案時，會先配置 VHD 的空間。 這種類型的 VHD 檔案較不可能是片段，可在單一 i/o 分割成多個 i/o 時減少 i/o 輸送量。 因為讀取和寫入不需要查閱區塊的對應，所以它具有三個 VHD 檔案類型的最低 CPU 額外負荷。

## <a name="dynamic-virtual-hard-disk-type"></a>動態虛擬硬碟類型

VHD 的空間是視需要配置。 磁片中的區塊會啟動為未配置的區塊，且不會受檔案中的任何實際空間支援。 當區塊首次寫入時，虛擬化堆疊必須在該區塊的 VHD 檔案內配置空間，然後更新中繼資料。 這會增加寫入所需的磁片 i/o 數量，並增加 CPU 使用量。 在中繼資料中查閱區塊的對應時，對現有區塊的讀取和寫入會產生磁片存取和 CPU 額外負荷。

## <a name="differencing-virtual-hard-disk-type"></a>差異虛擬硬碟類型

VHD 指向父 VHD 檔案。 未寫入區塊的任何寫入，都會導致 VHD 檔案中配置空間，如同動態擴充的 VHD。 如果已將區塊寫入，則會從 VHD 檔案提供讀取。 否則，它們會從父 VHD 檔案進行服務。 在這兩種情況下，都會讀取中繼資料來判斷區塊的對應。 對此 VHD 的讀取和寫入可能會耗用更多的 CPU，而且會產生比固定 VHD 檔案更多的 i/o。

## <a name="block-size-considerations"></a>區塊大小考慮

區塊大小會大幅影響效能。 最好將區塊大小與使用磁片之工作負載的配置模式進行比對。 例如，如果應用程式是以 16 MB 的區塊配置，最佳的做法是將虛擬硬碟區塊大小設為 16 MB。 &gt;只有具有 VHDX 格式的虛擬硬碟才能有 2 MB 的區塊大小。 擁有較大的區塊大小而非隨機 i/o 工作負載的配置模式，會大幅增加主機上的空間使用量。

## <a name="sector-size-implications"></a>磁區大小的含意

大部分的軟體產業都依存于512個位元組的磁片磁區，但標準將移至 4 KB 的磁片區。 為了減少磁區大小變更時可能發生的相容性問題，硬碟廠商引進了轉換大小，稱為512模擬磁片磁碟機 (512e) 。

這些模擬磁片磁碟機提供 4 KB 磁片區原生磁片磁碟機所提供的一些優點，例如改良的格式效率，以及改善的錯誤修正程式碼配置 (ECC) 。 它們的相容性問題較少，會在磁片介面上公開 4 KB 磁區大小。

## <a name="support-for-512e-disks"></a>512e 磁片的支援

512e 磁片只能以實體磁區來執行寫入，也就是說，它無法直接寫入發行給它的512byte 磁區。 磁片中可進行這些寫入的內部程式可能會遵循下列步驟：

-   磁片會將 4 KB 的實體磁區讀入其內部快取，其中包含寫入中所參考的512個位元組邏輯磁區。

-   4 KB 緩衝區中的資料會修改成包含更新的 512 位元組磁區。

-   磁碟會執行將已更新的 4 KB 緩衝區寫回到其磁碟上的實體磁區。

此程式稱為「讀取-修改-寫入」 (RMW) 。 RMW 程式的整體效能影響取決於工作負載。 RMW 程式會導致虛擬硬碟的效能降低，原因如下：

-   動態和差異虛擬硬碟的資料裝載前面都有512個位元組的磁區點陣圖。 此外，頁尾、頁首和父定位器會對齊512個位元組的磁區。 虛擬硬碟驅動程式通常會發出512位元組的寫入命令來更新這些結構，因此會產生稍早所述的 RMW 流程。

-   應用程式通常會以 4 KB 大小的倍數來發出讀取和寫入 (NTFS) 的預設叢集大小。 因為動態和差異虛擬硬碟的資料承載區塊前面有512個位元組的磁區點陣圖，所以 4 KB 區塊不會對齊實體 4 KB 界限。 下圖顯示 (反白顯示的 VHD 4 KB 區塊，) 未與實體 4 KB 界限對齊。

![vhd 4 kb 區塊](../../media/perftune-guide-vhd-4kb-block.png)

由目前剖析器所發出的每 4 KB 寫入命令來更新承載資料，會導致磁片上兩個區塊的兩次讀取，然後更新並隨後再寫回兩個磁片區塊。 Windows Server 2016 中的 hyper-v 藉由準備先前提及的結構，以 VHD 格式對齊 4 KB 界限，來降低 VHD 堆疊上512e 磁片的一些效能影響。 這可避免在存取虛擬硬碟檔案內的資料，以及更新虛擬硬碟元資料結構時，RMW 的效果。

如先前所述，從舊版 Windows Server 複製的 Vhd 不會自動對齊 4 KB。 您可以使用 VHD 介面中可用的 [ **從來源磁碟複製** ] 選項，以手動方式將它們轉換成最佳的對齊方式。

依預設，會以512個位元組的實體磁區大小來公開 Vhd。 這是為了確保應用程式和 Vhd 從舊版 Windows Server 移出時，不會影響實體磁區大小相依的應用程式。

根據預設，會使用 4 KB 的實體磁區大小來建立具有 VHDX 格式的磁片，以優化其效能設定檔的一般磁片和大型磁區磁片。 若要充分利用 4 KB 的磁區，建議使用 VHDX 格式。

## <a name="support-for-native-4kb-disks"></a>原生 4 KB 磁片的支援

Windows Server 2012 R2 和以上的 hyper-v 支援 4 KB 原生磁片。 但是，您仍然可以在 4 KB 的原生磁片上儲存 VHD 磁片。 這是藉由在虛擬儲存體堆疊層中執行軟體 RMW 演算法來完成，此演算法會將512位元組存取和更新要求轉換為對應的 4 KB 存取和更新。

因為 VHD 檔案只能將本身公開為512個位元組的邏輯磁區大小磁片，所以很有可能會有應用程式發出512位元組的 i/o 要求。 在這些情況下，RMW 層會滿足這些要求並導致效能降低。 這也適用于使用 VHDX 格式化的磁片，其邏輯磁區大小為512個位元組。

您可以將 VHDX 檔案設定成以 4 KB 的邏輯磁區大小磁片來公開，當磁片裝載于 4 KB 的原生實體裝置時，這會是最佳的效能設定。 請小心確保使用虛擬磁片的來賓和應用程式受到 4 KB 邏輯磁區大小的支援。 VHDX 格式化可在 4 KB 的邏輯磁區大小裝置上正確運作。

## <a name="pass-through-disks"></a>傳遞磁片

虛擬機器中的 VHD 可以直接對應到實體磁片或邏輯單元編號 (LUN) ，而不是 VHD 檔案。 好處是，此設定會略過根磁碟分割中的 NTFS 檔案系統，以減少儲存體 i/o 的 CPU 使用量。 風險是，實體磁片或 Lun 在機器之間移動時可能會比 VHD 檔案更困難。

由於虛擬機器遷移案例所引進的限制，應避免傳遞磁片。

## <a name="advanced-storage-features"></a>先進的儲存體功能

### <a name="storage-quality-of-service-qos"></a>存放裝置服務品質 (QoS)

從 Windows Server 2012 R2 開始，Hyper-v 能夠為虛擬機器上的存放裝置設定特定服務品質 (QoS) 參數。 存放裝置 QoS 在多組織用戶環境提供存放效能隔離，還提供在存放裝置的 I/O 效能沒有達到設定的閾值而無法有效執行虛擬機器工作負載時通知您的機制。

存放裝置 QoS 能夠指定虛擬硬碟每秒輸入/輸出作業 (IOPS) 的最大值。 系統管理員可調節存放裝置 I/O，阻止一個用戶耗用過多存放裝置資源而影響另一位用戶。

您也可以設定最小 IOPS 值。 當指定的虛擬硬碟的 IOPS 低於達到最佳效能所需的閾值時，會通知系統管理員。

虛擬機器計量基礎結構也會使用存放裝置相關的參數加以更新，以便允許系統管理員監視效能及計價相關的參數。

最大值和最小值是根據正規化 IOPS 來指定的，其中每 8 KB 的資料都會計算為 i/o。

部分限制如下所示：

-   僅適用于虛擬磁片

-   差異磁片在不同磁片區上不能有父虛擬磁片

-   複本-複本-QoS for replica site 與主要網站分開設定

-   不支援共用 VHDX

如需儲存體服務品質的詳細資訊，請參閱 [hyper-v 的存放裝置服務品質](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn282281(v=ws.11))。

### <a name="numa-io"></a>NUMA I/O

除了支援大型虛擬機器和任何大型虛擬機器設定以外，Windows Server 2012 和更大的虛擬機器設定 (例如，使用具有64虛擬處理器之 Microsoft SQL Server 的設定，) 也需要在 i/o 輸送量方面提供的擴充性。

以下是 Windows Server 2012 儲存體堆疊和 Hyper-v 中首次引進的重要改良功能，提供大型虛擬機器的 i/o 擴充性需求：

-   在來賓裝置與主機存放裝置堆疊之間建立的通道數目增加。

-   更有效率的 i/o 完成機制，包括虛擬處理器之間的中斷分配，以避免昂貴的 interprocessor 中斷。

Windows Server 2012 中引進了幾個登錄專案，位於 HKLM \\ 系統 \\ CurrentControlSet \\ Enum \\ VMBUS \\ {device id} \\ {instance id} \\ StorChannel，可讓您調整通道數目。 它們也會將處理 i/o 完成的虛擬處理器，與應用程式所指派的虛擬 Cpu 對齊，以作為 i/o 處理器。 登錄設定是針對裝置的硬體金鑰，以每個介面卡為基礎進行設定。

-   **ChannelCount (DWORD) ** 要使用的通道總數，最大值為16。 它預設為上限，也就是虛擬處理器數目/16。

-   **ChannelMask (QWORD) ** 通道的處理器親和性。 如果未設定或設定為0，它會預設為您用於一般儲存體或網路通道的現有通道分配演算法。 這可確保您的儲存體通道不會與您的網路通道衝突。

### <a name="offloaded-data-transfer-integration"></a>卸載資料傳輸整合

Vhd 的重要維護工作（例如合併、移動和壓縮）相依于複製大量資料。 目前複製資料的方法要求在不同的位置讀入和寫入資料，但這很耗費時間。 它也會使用主機上的 CPU 和記憶體資源，這些資源可用來服務虛擬機器。

存放區域網路 (SAN) 廠商正努力提供接近即時的大量資料複製作業。 此存放裝置的設計目的是要讓磁片上的系統能夠指定將特定資料集從某個位置移到另一個位置。 此硬體功能稱為卸載資料傳輸。

Windows Server 2012 和以上的 hyper-v 支援卸載資料傳輸 (ODX) 作業，讓這些作業可以從客體作業系統傳遞到主機硬體。 這可確保工作負載可以使用啟用 ODX 的儲存體，就像在非虛擬化環境中執行一樣。 Hyper-v 儲存體堆疊也會在 Vhd 的維護作業期間發出 ODX 作業，例如合併磁片和儲存體遷移中繼作業（這是大量資料的移動位置）。

### <a name="unmap-integration"></a>取消對應整合

虛擬硬碟檔案會以檔案的形式存在於存放磁片區上，並與其他檔案共用可用的空間。 因為這些檔案的大小可能很大，所以它們耗用的空間可能會快速成長。 更多實體儲存體的需求會影響 IT 硬體預算。 請務必盡可能將實體儲存體的使用優化。

在 Windows Server 2012 之前，當應用程式刪除虛擬硬碟中的內容，而該虛擬硬碟實際上放棄了內容的儲存空間時，客體作業系統和 Hyper-v 主機中的 Windows 儲存體堆疊會有限制，防止此資訊被傳達給虛擬硬碟和實體儲存裝置。 這會防止 Hyper-v 儲存體堆疊將 VHD 虛擬磁片檔案的空間使用量優化。 它也會防止基礎存放裝置回收先前被刪除資料所佔用的空間。

從 Windows Server 2012 開始，Hyper-v 支援取消對應通知，讓 VHDX 檔案在代表其中的資料時更有效率。 這會產生較小的檔案大小，並允許基礎實體儲存體裝置回收未使用的空間。

只有 Hyper-v 特定的 SCSI、啟用 rms IDE 和 Virtual 光纖通道控制器可讓來自來賓的取消對應命令連線到主機虛擬儲存堆疊。 在虛擬硬碟上，只有格式化為 VHDX 的虛擬磁片支援來自來賓的取消對應命令。

基於這些理由，建議您在未使用虛擬光纖通道磁片時，使用附加至 SCSI 控制器的 VHDX 檔案。

## <a name="additional-references"></a>其他參考資料

-   [Hyper-V 術語](terminology.md)

-   [Hyper-V 架構](architecture.md)

-   [Hyper-V 伺服器設定](configuration.md)

-   [Hyper-V 處理器效能](processor-performance.md)

-   [Hyper-V 記憶體效能](memory-performance.md)

-   [Hyper-V 網路 I/O 效能](network-io-performance.md)

-   [偵測虛擬化環境中的瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)