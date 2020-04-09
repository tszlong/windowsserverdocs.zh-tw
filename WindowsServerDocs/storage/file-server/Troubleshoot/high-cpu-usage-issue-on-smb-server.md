---
title: SMB 伺服器上的 CPU 使用量過高的問題
description: 介紹如何針對 SMB 伺服器上的高 CPU 使用量問題進行疑難排解。
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 9634ca5b0eb07beff8d8213f88e771d76734e6fc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80815432"
---
# <a name="high-cpu-usage-issue-on-the-smb-server"></a>SMB 伺服器上的 CPU 使用量過高的問題

本文討論如何針對 SMB 伺服器上的高 CPU 使用量問題進行疑難排解。

## <a name="high-cpu-usage-because-of-storage-performance-issues"></a>因儲存體效能問題而導致高 CPU 使用量

儲存體效能問題可能會導致 SMB 伺服器上的 CPU 使用量過高。 在進行疑難排解之前，請確定 SMB 伺服器上已安裝最新的更新彙總套件，以消除 srv2 中的任何已知問題。

在大部分的情況下，您會注意到系統進程中的 CPU 使用率過高的問題。 在繼續之前，請使用 Process Explorer 來確定 srv2 或 ntfs 會耗用過多的 CPU 資源。

### <a name="storage-area-network-san-scenario"></a>存放區域網路（SAN）案例

在匯總層級中，整體 SAN 效能可能看似正常。 不過，當您處理 SMB 問題時，個別的要求回應時間就是最重要的。

一般而言，此問題可能是由 SAN 中的某種形式的命令佇列所造成。 您可以使用**Perfmon**來捕獲**Microsoft Windows-StorPort**追蹤，並加以分析以精確判斷存放裝置回應能力。

### <a name="disk-io-latency"></a>磁片 IO 延遲

磁片 IO 延遲是建立和完成磁片 IO 要求的時間之間的延遲測量。

以 Perfmon 測量的 IO 延遲包括在硬體層中花費的時間，加上在 Microsoft 埠驅動程式佇列中花費的時間（適用于 SCSI 的 Storport）。 如果執行中的進程產生大型的 StorPort 佇列，測量的延遲就會增加。 這是因為 IO 必須等到再分派至硬體層。

在 Perfmon 中，下列計數器會顯示實體磁片延遲：

- 「實體磁片效能物件」-\> 「Avg. Disk sec/Read 計數器」–這會顯示平均讀取延遲。

- 「實體磁片效能物件」-\> 「Avg. Disk sec/Write 計數器」–這會顯示平均寫入延遲。

- 「實體磁片效能物件」-\> 「Avg. Disk sec/Transfer counter」–這會顯示讀取和寫入的平均總和。

「\_總計」實例是電腦中所有實體磁片的延遲平均值。 每個其他實例都代表個別的實體磁片。

> [!NOTE]
> 請勿將這些計數器與平均磁片傳輸/秒混淆。這些是完全不同的計數器。

### <a name="windows-storage-stack-follows"></a>Windows 儲存堆疊遵循

本節提供有關 Windows 儲存堆疊的簡短說明，如下所示。

當應用程式建立 IO 要求時，它會將要求傳送至堆疊頂端的 Windows IO 子系統。 然後，IO 會將堆疊向下移動到硬體「磁片」子系統。 然後，回應會一直往上移動。 在此過程中，每一層都會執行其函式，然後將 IO 交給下一層。

![堆疊流程](media/high-cpu-usage-issue-on-smb-server-1.png)

Perfmon 不會每秒建立任何效能資料。 相反地，它會耗用 Windows 中其他子系統所提供的資料。

對於「實體磁片效能物件」，資料會在儲存體堆疊的「分割管理員」層級上被捕獲。

當我們測量上一節所述的計數器時，我們會測量要求在「資料分割管理員」層級底下花費的時間。 當磁碟分割管理員將 IO 要求向下傳送堆疊時，我們會將它標示為時間戳記。 當它傳回時，我們會重新標記它，並計算時間差異。 時間差是延遲。

如此一來，我們就會計入下列元件所花費的時間：

- 類別驅動程式-這會管理裝置類型，例如磁片、磁帶等等。

- 埠驅動程式-這會管理傳輸通訊協定，例如 SCSI、FC、SATA 等等。

- 裝置迷你埠驅動程式-這是儲存介面卡的設備磁碟機。 這是由裝置製造商所提供，例如 Raid 控制器和 FC HBA。

- 磁片子系統-這包括裝置迷你埠驅動程式底下的所有專案。 這可能像是連接到單一實體硬碟的纜線一樣簡單，或與存放區域網路複雜。 如果問題是由此元件所造成，您可以洽詢硬體廠商，以取得疑難排解的詳細資訊。

### <a name="disk-queuing"></a>磁片佇列

磁片子系統在指定時間可以接受的 IO 數量有限。 多餘的 IO 會排入佇列，直到磁片再次接受 IO 為止。 在 [資料分割管理員] 層級下，IO 花費在佇列中的時間，會納入 [Perfmon 實體磁片延遲] 測量中。 隨著佇列變大，IO 必須等候較長的時間，測量的延遲也會增加。

「資料分割管理員」層級底下有多個佇列，如下所示：

- Microsoft 埠驅動程式佇列-SCSIport 或 Storport 佇列

- 製造商提供的設備磁碟機佇列-OEM 設備磁碟機

- 硬體佇列–例如磁碟控制卡佇列、SAN 交換器佇列、陣列控制器佇列和硬碟佇列

我們也會將硬碟投入時間，以主動處理 IO，以及讓要求回到「分割管理員」層級以標示為已完成的行進時間。

最後，我們必須特別注意埠驅動程式佇列（適用于 SCSI Storport）。 埠驅動程式是最後一個要觸及 IO 的 Microsoft 元件，再將它交給製造商提供的裝置迷你埠驅動程式。

如果裝置的迷你埠驅動程式無法接受任何 IO，因為其佇列或其底下的硬體佇列已飽和，我們會開始在埠驅動程式佇列上累積 IO。 Microsoft 埠驅動程式佇列的大小只受限於可用的系統記憶體（RAM），而且可能會變得非常大。 這會導致大量測量的延遲。

## <a name="high-cpu-caused-by-enumerating-folders"></a>列舉資料夾導致高 CPU 

若要對此問題進行疑難排解，請停用以存取為基礎的列舉（ABE）功能。

若要判斷哪些 SMB 共用已啟用 ABE，請執行下列 PowerShell 命令。

```PowerShell
Get-SmbShare | Select Name, FolderEnumerationMode
```

不受限制 = ABE 已停用。 <br />
AccessBase = 已啟用 ABE。


您可以在**伺服器管理員**中啟用 ABE。 Navigatie 至檔案**和存放服務** > **共用**，在共用上按一下滑鼠右鍵，**選取 [** 內容]，移至 [**設定**]，然後選取 [**啟用存取型列舉**]。

![UI 選項](media/high-cpu-usage-issue-on-smb-server-2.png)

此外，您可以將**ABELevel**降低為較低層級（1或2）以改善效能。

當列舉變慢時，您可以透過主控台或 RDP 會話在本機開啟資料夾，以檢查磁片效能。
