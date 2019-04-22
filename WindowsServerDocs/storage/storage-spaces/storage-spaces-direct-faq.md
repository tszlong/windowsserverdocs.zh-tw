---
title: 儲存空間直接存取-常見問題集
description: 了解如何儲存空間直接存取
keywords: 儲存空間
ms.prod: windows-server-threshold
ms.author: kaushik
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: b17aa7ddc783e95fbcc19fe3913192d245133c7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818909"
---
# <a name="storage-spaces-direct---frequently-asked-questions-faq"></a>儲存空間直接存取-常見問題集 (FAQ)

本文列出一些常見和相關常見問題集[儲存空間直接存取](storage-spaces-direct-overview.md)。

## <a name="when-you-use-storage-spaces-direct-with-3-nodes-can-you-get-both-performance-and-capacity-tiers"></a>當您使用儲存空間直接存取搭配 3 個節點時，您可以獲得效能和容量層？

是，您可以取得效能和容量層中的 2 個節點或 3 個節點的儲存空間直接存取組態。 不過，您必須確定您有 2 個容量裝置。 這表示，您必須使用裝置的所有三種類型：NVME、 SSD 和 HDD。
 
## <a name="refs-file-system-provides-real-time-tiaring-with-storage-spaces-direct-does-refs-provides-the-same-functionality-with-shared-storage-spaces-in-2016"></a>Refs 檔案系統提供即時 tiaring 與儲存空間直接存取。 不 REFS 提供相同的功能與共用的儲存空間 2016 嗎？

否，您無法取得即時 2016年使用的共用存放裝置空間的階層處理。 這是僅適用於儲存空間直接存取。 
 
## <a name="can-i-use-an-ntfs-file-system-with-storage-spaces-direct"></a>可以使用 NTFS 檔案系統與儲存空間直接存取嗎？
  
是，您可以使用 NTFS 檔案系統與儲存空間直接存取。 不過，建議 REFS。 NTFS 不提供即時的階層處理。 
 
## <a name="i-have-configured-2-node-storage-spaces-direct-clusters-where-the-virtual-disk-is-configured-as-2-way-mirror-resiliency-if-i-add-a-new-fault-domain-will-the-resiliency-of-the-existing-virtual-disk-change"></a>我已設定 2 個節點儲存空間直接存取叢集，虛擬磁碟設定為 2 向鏡像復原的位置。 如果我新增一個新的容錯網域，請將現有的虛擬磁碟的復原變更嗎？

加入新的容錯網域之後，您建立新的虛擬磁碟會跳到 3 向鏡像。 不過，現有的虛擬磁碟會保留 2 向鏡像的磁碟。 您可以從現有的磁碟區，以取得新的復原功能的優點，以將資料複製到新的虛擬磁碟。
 
## <a name="the-storage-spaces-direct-was-created-using-the-autoconfig0-switch-and-the-pool-created-manually-when-i-try-to-query-the-storage-spaces-direct-pool-to-create-a-new-volume-i-get-a-message-that-says-enable-clusters2d-again-what-should-i-do"></a>儲存空間直接存取是使用建立 autoconfig:0參數，並以手動方式建立的集區。 當我嘗試將查詢儲存空間直接存取的集區，以建立新的磁碟區時，我收到訊息指出，「 Enable-ClusterS2D 一次。 」我該怎麼辦？

根據預設，當您使用啟用 S2D cmdlet，設定儲存空間直接存取 cmdlet 的所有項目為您做。 它會建立集區和階層。 當使用 autoconfig:0，所有項目必須手動完成。 如果您建立的集區時，不一定會建立層。 如果您有未完全建立層，或未對應至連接的裝置的方式建立階層，您會收到 「 一次 Enable-ClusterS2D 」 錯誤訊息。 我們建議您不要在生產環境中使用您的自動設定參數。 
 
## <a name="is-it-possible-to-add-a-spinning-disk-hdd-to-the-storage-spaces-direct-pool-after-you-have-created-storage-spaces-direct-with-ssd-devices"></a>是否可以在儲存空間直接存取的集區新增轉動式磁碟 (HDD) 之後您已建立儲存空間直接存取使用 SSD 裝置？

資料分割 根據預設，如果您使用單一裝置類型來建立集區時，它不會設定快取磁碟和所有磁碟要都用於容量。 您可以新增 NVME 磁碟組態，和 NVME 磁碟會設定為快取。
 
## <a name="i-have-configured-a-2-rack-fault-domain-rack-1-has-2-fault-domains-rack-2-has-1-fault-domain-each-server-has-4-capacity-100-gb-devices-can-i-use-all-1200-gb-of-space-from-the-pool"></a>我已設定 2 機架容錯網域：機架 1 有 2 個容錯網域，機架 2 有 1 個容錯網域。 每一部伺服器有 4 個容量 100GB 的裝置。 可以使用所有的 1200 GB 的空間從集區嗎？

否，您可以使用只有 800 GB。 在機架容錯網域中，您必須確定您有 2 向鏡像組態，因此，每個丟棄和其重複 land 不同機架中。
 
## <a name="what-should-the-cache-size-be-when-i-am-configuring-storage-spaces-direct"></a>快取大小應該為何當我正在設定儲存空間直接存取？

快取應該調整大小以容納工作集 (正在主動的資料讀取或寫入一次) 的應用程式和工作負載。

## <a name="how-can-i-determine-the-size-of-cache-that-is-being-used-by-storage-spaces-direct"></a>如何判斷正在使用儲存空間直接存取的快取大小？

您可以使用內建的公用程式 PerfMon，檢查快取遺漏。 檢閱快取遺漏每秒讀取從叢集儲存體的混合式磁碟計數器。 請記住，是否遺漏太多的讀取快取，您的快取是過小，而且要展開它。 
 
## <a name="is-there-a-calculator-that-shows-the-exact-size-of-the-disks-that-are-being-set-aside-for-cache-capacity-and-resiliency-that-would-enable-me-to-plan-better"></a>是否有顯示在一旁，快取、 容量，以及可讓我更佳的規劃的復原磁碟的確切大小的計算機？

您可以使用儲存空間計算器，以協助您進行規劃。 已在 http://aka.ms/s2dcalc。
 
## <a name="what-is-the-best-configuration-that-you-would-recommend-when-configuring-6-servers-and-3-racks"></a>什麼是最佳的組態設定 6 部和 3 個機架時，您會建議？

使用每個機架上的 2 部伺服器，可取得 3 向鏡像虛擬磁碟復原功能。 請記住您要提供組態的方式，則會放在機架中的作業系統時，才機架組態就會正確運作。 
 
## <a name="can-i-enable-maintenance-mode-for-a-specific-disk-on-a-specific-server-in-storage-spaces-direct-cluster"></a>啟用儲存空間直接存取叢集中的特定伺服器上的特定磁碟的維護模式？

是，您可以啟用特定磁碟和一個特定的容錯網域上的存放裝置維護模式。 當您暫停節點時，會自動叫用啟用 StorageMaintenanceMode 命令。 您可以針對特定的磁碟啟用它藉由執行下列命令：

```powershell
Get-PhysicalDisk -SerialNumber <SerialNumber> | Enable-StorageMaintenanceMode
```

## <a name="is-storage-spaces-direct-supported-on-my-hardware"></a>儲存空間直接存取支援我的硬體？

我們建議您連絡硬體廠商，以確認支援。 硬體廠商測試其硬體和註解是否支援或不在此解決方案。 例如，在此撰寫時，伺服器，例如 R730 / R730xd / 有 8 個以上的磁碟機插槽的 R630 可支援 SES 且與儲存空間直接存取相容。 Dell 支援只使用儲存空間直接存取 HBA330。 R620 nepodporuje SES，並與儲存空間直接存取不相容。

更多的硬體支援資訊，請移至下列網站：Windows Server Catalog
 
## <a name="how-does-storage-spaces-direct-make-use-of-ses"></a>如何儲存空間直接存取對使用 SES？

儲存空間直接存取會使用 SCSI 機箱服務 (SES) 對應以確定資料和中繼資料的 slab 橫跨容錯網域，以彈性的方式。 如果硬體不支援 SES，沒有對應的機箱，並不適合的資料位置。
 
## <a name="what-command-can-you-use-to-check-the-physical-extent-for-a-virtual-disk"></a>您可以使用哪些命令來檢查虛擬磁碟的實體範圍？
  
這一個：

```powershell
get-virtualdisk -friendlyname “xyz” | get-physicalextent
```
