---
title: 儲存空間直接存取的常見問題
description: 深入瞭解儲存空間直接存取
ms.prod: windows-server
ms.author: kaushik
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: 5c033a5a810d1cdedeb4c733ba4bf0ac99e669f0
ms.sourcegitcommit: 3483f886f331b9d954a0e5dba8e910dbe5ee5765
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2020
ms.locfileid: "82977246"
---
# <a name="storage-spaces-direct---frequently-asked-questions-faq"></a>儲存空間直接存取-常見問題（FAQ）

本文列出一些與[儲存空間直接存取](storage-spaces-direct-overview.md)相關的常見常見問題和常見問題。

## <a name="when-you-use-storage-spaces-direct-with-3-nodes-can-you-get-both-performance-and-capacity-tiers"></a>當您使用具有3個節點的儲存空間直接存取時，您可以同時取得效能和容量層嗎？

是，您可以在2個節點或3個節點的儲存空間直接存取設定中取得效能和容量層級。 不過，您必須確定您有2個容量裝置。 這表示您必須使用三種類型的裝置： NVME、SSD 和 HDD。

## <a name="refs-file-system-provides-real-time-tiering-with-storage-spaces-direct-does-refs-provide-the-same-functionality-with-shared-storage-spaces-in-2016"></a>Refs 檔案系統提供儲存空間直接存取的即時分層。 REFS 在2016中是否提供與共享儲存空間相同的功能？

不可以，您不會透過2016，取得共用儲存空間的即時分層。 這僅適用于儲存空間直接存取。

## <a name="can-i-use-an-ntfs-file-system-with-storage-spaces-direct"></a>我可以搭配使用 NTFS 檔案系統與儲存空間直接存取嗎？

是的，您可以將 NTFS 檔案系統與儲存空間直接存取搭配使用。 不過，建議使用 REFS。 NTFS 不提供即時分層。

## <a name="i-have-configured-2-node-storage-spaces-direct-clusters-where-the-virtual-disk-is-configured-as-2-way-mirror-resiliency-if-i-add-a-new-fault-domain-will-the-resiliency-of-the-existing-virtual-disk-change"></a>我設定了2個節點儲存空間直接存取叢集，其中虛擬磁片設定為雙向鏡像復原。 如果我新增新的容錯網域，現有虛擬磁片的復原會變更嗎？

新增新的容錯網域之後，您建立的新虛擬磁片會跳到3向鏡像。 不過，現有的虛擬磁片將會維持為雙向鏡像磁片。 您可以從現有的磁片區將資料複製到新的虛擬磁片，以獲得新復原的優點。

## <a name="the-storage-spaces-direct-was-created-using-the-autoconfig0-switch-and-the-pool-was-created-manually-when-i-try-to-query-the-storage-spaces-direct-pool-to-create-a-new-volume-i-get-a-message-saying-enable-clusters2d-again-what-should-i-do"></a>儲存空間直接存取是使用自動設定：0所建立的switch 和集區是以手動方式建立。 當我嘗試查詢儲存空間直接存取集區以建立新的磁片區時，會收到一則訊息，指出：「再次啟用-Enable-clusters2d」。 我該怎麼辦？

根據預設，當您使用 enable-S2D Cmdlet 設定儲存空間直接存取時，此 Cmdlet 會為您執行所有工作。 它會建立集區和層級。 使用自動設定時：0時，必須以手動方式完成所有作業。 如果您只建立集區，就不一定會建立該層。 如果您未以對應于所連接裝置的方式建立所有層或尚未建立的階層，您將會收到「Enable-clusters2d 再次出現」錯誤訊息。 我們建議您不要在生產環境中使用自動設定參數。

## <a name="is-it-possible-to-add-a-spinning-disk-hdd-to-the-storage-spaces-direct-pool-after-you-have-created-storage-spaces-direct-with-ssd-devices"></a>在建立 SSD 裝置的儲存空間直接存取之後，是否可以將旋轉磁片（HDD）新增至儲存空間直接存取集區？

否。 根據預設，如果您使用單一裝置類型來建立集區，它不會設定快取磁片，而且所有磁片都會用於容量。 您可以在設定中新增 NVME 磁片，而 NVME 磁片則會針對快取進行設定。

## <a name="i-have-configured-a-2-rack-fault-domain-rack-1-has-2-fault-domains-rack-2-has-1-fault-domain-each-server-has-4-capacity-100-gb-devices-can-i-use-all-1200-gb-of-space-from-the-pool"></a>我設定了2個機架的容錯網域：機架1有2個容錯網域，第2機架有1個容錯網域。 每部伺服器都有4個容量 100 GB 的裝置。 我可以從集區使用所有 1200 GB 的空間嗎？

不可以，您只能使用 800 GB。 在機架容錯網域中，您必須確定您有雙向鏡像設定，讓每個 chuck 和其重複進入不同的機架。

## <a name="what-should-the-cache-size-be-when-i-am-configuring-storage-spaces-direct"></a>當我設定儲存空間直接存取時，快取大小應該為何？

應該調整快取大小，以容納應用程式和工作負載的工作集（在任何指定時間主動讀取或寫入的資料）。

## <a name="how-can-i-determine-the-size-of-cache-that-is-being-used-by-storage-spaces-direct"></a>如何判斷儲存空間直接存取所使用的快取大小？

使用內建的公用程式 PerfMon 來檢查快取遺漏。 從 [叢集存放裝置混合式磁片] 計數器中，檢查 [快取遺漏讀取/秒]。 請記住，如果有太多讀取遺失快取，您的快取會變小，而且您可能會想要將它展開。

## <a name="is-there-a-calculator-that-shows-the-exact-size-of-the-disks-that-are-being-set-aside-for-cache-capacity-and-resiliency-that-would-enable-me-to-plan-better"></a>是否有計算機會顯示針對快取、容量和復原所設定的確切磁片大小，讓我能夠更好地進行規劃？

您可以使用「儲存空間」計算機協助您進行規劃。 可以在https://aka.ms/s2dcalc取得。

## <a name="what-is-the-best-configuration-that-you-would-recommend-when-configuring-6-servers-and-3-racks"></a>設定6部伺服器和3個機架時，建議您最好的設定為何？

在每個機架上使用2部伺服器，以取得三向鏡像的虛擬磁片恢復功能。 請記住，只有當您將設定以機架上的方式提供給 OS 時，才會正常運作。

## <a name="can-i-enable-maintenance-mode-for-a-specific-disk-on-a-specific-server-in-storage-spaces-direct-cluster"></a>我可以在儲存空間直接存取叢集中的特定伺服器上，啟用特定磁片的維護模式嗎？

是，您可以在特定磁片和特定容錯網域上啟用存放裝置維護模式。 當您暫停節點時，會自動叫用 StorageMaintenanceMode 命令。 您可以執行下列命令，為特定的磁片啟用它：

```powershell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Enable-StorageMaintenanceMode
```

## <a name="is-storage-spaces-direct-supported-on-my-hardware"></a>我的硬體上是否支援儲存空間直接存取？

我們建議您洽詢硬體廠商以確認支援。 硬體廠商會在其硬體上測試解決方案，並針對是否支援它進行批註。 例如，在撰寫本文時，具有8個以上磁片磁碟機位置的伺服器（例如 R730/R730xd/R630）可以支援 SES 並與儲存空間直接存取相容。 Dell 僅支援具有儲存空間直接存取的 HBA330。 R620 不支援 SES，而且與儲存空間直接存取不相容。

如需更多硬體支援資訊，請移至下列網站： Windows Server Catalog

## <a name="how-does-storage-spaces-direct-make-use-of-ses"></a>儲存空間直接存取如何利用 SES？

儲存空間直接存取使用 SCSI 主機殼服務（SES）對應，以確保 slab 的資料和中繼資料會以彈性的方式分散到容錯網域。 如果硬體不支援 SES，就不會有機箱的對應，而且資料放置也不具復原能力。

## <a name="which-command-can-you-use-to-check-the-physical-extent-for-a-virtual-disk"></a>您可以使用哪一個命令來檢查虛擬磁片的實體範圍？

這個：

```powershell
get-virtualdisk -friendlyname "xyz" | get-physicalextent
```
