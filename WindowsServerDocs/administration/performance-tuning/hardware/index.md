---
title: 伺服器硬體效能考量
description: Windows Server 2016 的伺服器硬體效能考量
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: phstee
author: phstee
ms.date: 01/08/2018
ms.openlocfilehash: f9c7f0e589980f7d985f165e318667ebe2e5d5c5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866753"
---
# <a name="server-hardware-performance-considerations"></a>伺服器硬體效能考量

下節將列出您選擇伺服器硬體時應考量的重要項目。 遵循這些指導方針有助於清除可能降低伺服器效能的效能瓶頸。

## <a name="processor-recommendations"></a>處理器建議事項

為伺服器選擇 64 位元的處理器。 64 位元處理器有更多的位址空間，而且是 Windows Server 2016 的必要項目。 雖然不提供 32 位元版本的作業系統，但 32 位元的應用程式可在 64 位元的 Windows Server 2016 作業系統上執行。

若要增加伺服器中的計算資源，您可以使用核心頻率較高的處理器，或增加處理器核心的數目。 如果 CPU 在系統中是受限資源，2x 頻率核心通常會比兩個 1x 頻率核心提供更大的效能改善。

多核心並不會提供完美的線性縮放比例，而且若啟用超執行緒，縮放係數甚至會變得很低，因為超執行緒會依賴相同實體核心的共用資源。


>[!Important]
> 請搭配 CPU 效能比對並調整記憶體和 I/O 子系統，反之亦然。

請勿在製造商和處理器世代之間比較 CPU 頻率，因為此比較可能會導致錯誤的速度指標。

針對 Hyper-V，請確定處理器支援 SLAT (第二層位址轉譯)。 這會實作為 Intel 的延伸分頁表 (EPT) 及 AMD 的巢狀分頁表 (NPT)。 您可以使用伺服器上的 SystemInfo.exe 來確認是否有這項功能。

## <a name="cache-recommendations"></a>快取建議

選擇大型的 L2 或 L3 處理器快取。 較新的架構上 (例如 Haswell 或 Skylake) 會有統整的末級快取 (LLC) 或 L4。 較大型的快取通常可提供較佳的效能，而且通常比原始 CPU 頻率具有更大的影響力。

## <a name="memory-ram-and-paging-storage-recommendations"></a>記憶體 (RAM) 和分頁儲存體建議

>[!Note] 
> 相較於 Windows Server 2012 R2，執行 Windows Server 2016 的全新安裝時，部分系統可能會出現儲存空間效能降低的情形。 在 Windows Server 2016 的開發期間我們做了一些變更，以改善平台的安全性及可靠性。 其中有些變更 (例如預設啟用 Windows Defender) 會導致較長的 I/O 路徑，因而降低特定工作負載和模式中的 I/O 效能。 Microsoft 不建議停用 Windows Defender，因為它是您系統很重要的一層保護。 

請增加 RAM 以符合您的記憶體需求。
當您電腦的記憶體不足且立即需要更多記憶體時，Windows 會透過名為分頁的程序，使用硬碟空間來補充系統 RAM。 太多分頁會降低整體系統效能。
您可以使用下列的分頁檔位置指導方針來最佳化分頁：
- 將分頁檔獨立放在自己的存放裝置中，或至少確定其不會與其他經常存取的檔案共用相同存放裝置。 例如，將分頁檔與作業系統檔案放在不同的實體磁碟機上。

- 將分頁檔放在不可容錯的磁碟機上。 如果磁碟故障，可能會發生系統當機。 如果您將分頁檔放在可容錯的磁碟機上，請注意容錯系統寫入資料的速度通常比較慢，因為須將資料寫入多個位置。

- 如果您需要額外的磁碟頻寬來進行分頁，請使用多個磁碟或磁碟陣列。 請勿將多個分頁檔放在相同實體磁碟機的不同分割區上。

## <a name="peripheral-bus-recommendations"></a>週邊匯流排建議
在 Windows Server 2016 中，主要儲存體和網路介面應該是 PCI Express (PCIe)，因此建議使用搭配 PCIe 匯流排的伺服器。 若要避免匯流排速度遭到限制，請使用適用於 10+ GB 乙太網路卡的 PCIe x8 及更高階插槽。

## <a name="disk-recommendations"></a>磁碟建議
選擇旋轉速度較高的磁碟，以降低隨機要求服務的時間 (比較 7,200 和 15,000 RPM 的磁碟機時，平均時間為 ~2 毫秒)，並增加循序要求的頻寬。 不過，旋轉速度高的磁碟會有成本、電源和其他方面的相關考量。

相較於 3.5 英吋磁碟機，對等的企業級 2.5 英吋磁碟每秒可以處理更大量的隨機要求。

將經常存取的資料 (尤其是循序存取的資料) 儲存在磁碟開始位置的附近，因為這可大致對應到最外層的 (最快的) 追蹤。

將小型裝置合併到較少的高容量磁碟機可能會降低整體儲存體效能。 主軸較少表示並行的要求服務會減少；因此，可能會造成較低的輸送量和較長的回應時間 (取決於工作負載強度)。

SSD 和高速快閃磁碟適合用來讀取大部分具有高 I/O 比率或延遲敏感型 I/O 的磁碟。 開機磁碟就十分適合使用 SSD 或高速快閃磁碟，因為可以大幅改善開機時間。

NVMe SSD 可提供更進階的效能，包含更優異的命令佇列深度、更有效率的插斷要求處理，以及效率更高的 4KB 命令。 這對需要大量同步 I/O 的案例特別有幫助。


## <a name="network-and-storage-adapter-recommendations"></a>網路和儲存裝置介面卡建議

下節將列出建議用於高效能伺服器的網路及儲存裝置介面卡特性。 這些設定有助於防止網路或儲存體硬體在負載過重時遭遇瓶頸。

### <a name="certified-adapter-usage"></a>使用通過認證的介面卡
使用已通過 Windows 硬體認證測試套件的介面卡。

### <a name="64-bit-capability"></a>64 位元功能
64 位元的介面卡可對高實體記憶體位置 (大於 4 GB) 執行直接記憶體存取 (DMA) 作業。 如果驅動程式不支援大於 4 GB 的DMA，則系統會以雙緩衝方式將 I/O 傳送至小於 4 GB 的實體位址空間。

### <a name="copper-and-fiber-adapters"></a>銅線和光纖介面卡
銅線介面卡通常與其相對的光纖項目具有同等效能，而且有些光纖通道介面卡上既可使用銅線也可使用光纖。 某些環境適合銅線介面卡，而某些環境較適合光纖介面卡。

### <a name="dual--or-quad-port-adapters"></a>兩個或四個連接埠介面卡
多連接埠介面卡適用於 PCI 插槽數量有限的伺服器。

若要解決 SCSI 對可連結到 SCSI 匯流排的磁碟數目限制，某些介面卡會在單一介面卡上提供兩個或四個 SCSI 匯流排。 光纖通道介面卡通常不會限制連結到介面卡的磁碟數目，除非這些磁碟隱藏在 SCSI 介面背後。

序列連結 SCSI (SAS) 和序列 ATA (SATA) 介面卡也有連結數目限制，因為這些通訊協定具有序列性質，但您可以使用交換器來連結更多磁碟。

網路介面卡會將這項功能用於負載平衡或容錯移轉案例。 對於相同的工作負載，使用兩個單一連接埠網路介面卡產生的效能通常優於使用一個雙連接埠網路介面卡。

PCI 匯流排限制是限制多連接埠介面卡效能的主要因素。 因此，務必考慮將介面卡放在高效能的 PCIe 插槽，以提供足夠的頻寬。

### <a name="interrupt-moderation"></a>降低插斷要求
某些介面卡可降低其中斷主機處理器以指示活動或其完成狀態的頻率。 降低插斷要求通常可降低主機上的 CPU 負載，但前提是插斷降低功能能夠以智能方式執行，而節省 CPU 用量可能會增加延遲。

### <a name="receive-side-scaling-rss-support"></a>接收端調整 (RSS) 支援
RSS 能夠讓封包執行接收處理，以根據可用的電腦處理器數目進行調整。 這對 10 GB 乙太網路而言特別重要，而且可讓速度更快。

### <a name="offload-capability-and-other-advanced-features-such-as-message-signaled-interrupt-msi-x"></a>卸載功能及其他進階功能 (例如訊息訊號插斷 (MSI)-X)
具卸載功能的介面卡可節省 CPU 以產生改善的效能。

### <a name="dynamic-interrupt-and-deferred-procedure-call-dpc-redirection"></a>動態插斷和延遲的程序呼叫 (DPC) 重新導向
在 Windows Server 2016 中，Numa I/O 可讓 PCIe 儲存裝置介面卡以動態方式重新導向插斷和 DPC，並且可藉由改善工作負載分割來協助任何多處理器系統，還可快取點擊率，以及針對 I/O 密集的工作負載啟用硬體互連。

## <a name="see-also"></a>另請參閱
- [伺服器硬體電源的考量](power.md)
- [電源與效能調整](power/power-performance-tuning.md)
- [處理器電源管理](power/processor-power-management-tuning.md)
- [建議的平衡方案參數](power/recommended-balanced-plan-parameters.md)
