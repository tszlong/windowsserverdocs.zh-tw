---
title: 了解並查看儲存體重新同步處理
description: 儲存體重新同步處理狀況的發生時機，以及如何在 Windows Server 2019 中看到它的詳細的資訊。
keywords: 儲存空間直接存取、 儲存體重新同步處理，重新同步處理儲存體，S2D
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: 81b1136a4b6a5cf8423a99e898b482a9b2849b5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813459"
---
# <a name="understand-and-monitor-storage-resync"></a>了解和監視存放裝置重新同步

>適用於：Windows Server 2019

儲存體重新同步處理警示是的新功能[儲存空間直接存取](storage-spaces-direct-overview.md)中可讓您的儲存體重新同步時所擲回錯誤健全狀況服務的 Windows Server 2019。 警示可用於時通知您發生重新同步處理，使您不小心在更多的伺服器清單 （這可能導致多個容錯網域會受到影響，導致您的叢集停止運作）。 

本主題提供背景和步驟，以了解並查看儲存體中儲存空間直接存取的 Windows Server 容錯移轉叢集的重新同步。

## <a name="understanding-resync"></a>了解重新同步處理

讓我們開始了解如何取得同步儲存體的簡單範例。請記住任何無共用 （本機磁碟機只） 分散式儲存體解決方案，將示範此行為。 如您所見，如果一個伺服器節點故障，則不會更新其磁碟機，直到它再次上線-這是針對任何超交集架構，則為 true。 

假設我們想要儲存字串"HELLO"。 

![字串"hello"的 ASCII](media/understand-storage-resync/hello.png)

我們有三向鏡像復原的 Asssuming，我們有三個複本，這個字串。 現在，我們才關閉的伺服器 #1 暫時 （進行維護），如果我們無法存取複製 #1。

![無法存取複製 #1](media/understand-storage-resync/copy1.png)

假設我們更新我們字串"HELLO"」 說明 ！ 」 在此階段中。

![字串"help ！"的 ASCII](media/understand-storage-resync/help.png)

一旦我們更新字串時，複製 #2 與快照 #3 會更新已成功。 不過，複本 #1 仍然無法存取，因為伺服器 #1 已關閉暫時 （適用於維護）。 

![寫入複製 #2 與快照 #2 的 Gif"](media/understand-storage-resync/write.gif)

現在，我們可以複製 #1 具有不同步的資料。作業系統會使用更細微的廢棄區域追蹤來追蹤未同步的位元。如此一來當伺服器 #1 重新上線，我們可以讀取資料，從 #2 或 3 的複本，並覆寫複製 #1 中的資料同步所做的變更。 此方法的優點是，我們只需要複製 已過時，而不是重新同步的所有資料從伺服器 #2 或伺服器 #3 的資料。

![若要複製 #1 覆寫 Gif"](media/understand-storage-resync/overwrite.gif)

因此，本節將說明如何取得同步資料。但是，它看起來像什麼高層級的呢？ 假設此範例中，我們有三個伺服器的超交集叢集。 在維護伺服器 #1 時，您會看到它，為已關閉。 當您將伺服器 #1 備份時，就會開始重新同步所有使用 （如上所述） 的細微廢棄區域追蹤其儲存體。 所有伺服器資料全部重新同步之後，將會都顯示為已啟動。

![系統管理員檢視的重新同步處理的 Gif"](media/understand-storage-resync/admin.gif)

## <a name="how-to-monitor-storage-resync-in-windows-server-2019"></a>如何監視在 Windows Server 2019 的重新同步處理儲存體

既然您了解儲存體重新同步處理的運作方式，讓我們看看如何這顯示在 Windows Server 2019。 我們已新增新的錯誤，以[健全狀況服務](../../failover-clustering/health-service-overview.md)重新同步您的儲存體時，它會顯示在。

若要在 PowerShell 中檢視此錯誤，請執行：

``` PowerShell
Get-HealthFault
```

這是在 Windows Server 2019，新的錯誤，而且會出現在 PowerShell 中，在叢集驗證報告中，與其他任何地方，是根據健康情況錯誤。 

若要取得更深入的檢視，您可以查詢時間序列資料庫，在 PowerShell 中的執行，如下所示：

```PowerShell
Get-ClusterNode | Get-ClusterPerf -ClusterNodeSeriesName ClusterNode.Storage.Degraded
```
以下是一些輸出範例︰

```
Object Description: ClusterNode Server1

Series                       Time                Value Unit
------                       ----                ----- ----
ClusterNode.Storage.Degraded 01/11/2019 16:26:48     214 GB
```

值得注意的是，Windows Admin Center 會使用健康情況錯誤，來設定 狀態 和 叢集節點的色彩。 因此，此新的錯誤會造成叢集節點，以從 red （下） 轉換為 （重新同步） 為綠色 （上），而不直接從紅色轉為綠色、 黃色 HCI 儀表板上。

![2016 vs 2019 檢視內的重新同步處理的映像 」](media/understand-storage-resync/compare.png)

藉由顯示整體儲存體重新同步處理進度，您可以精確地知道多少資料同步，且您的系統是否進行順向進度。 當您開啟 Windows Admin Center，並移至*儀表板*，您會看到新的警示，如下所示：

![映像的 Windows Admin Center 中的警示 」](media/understand-storage-resync/alert.png)

警示可用於時通知您發生重新同步處理，使您不小心在更多的伺服器清單 （這可能導致多個容錯網域會受到影響，導致您的叢集停止運作）。 

如果您瀏覽至*伺服器*在 Windows Admin Center 頁面上，按一下*清查*，然後選擇特定的伺服器，您可以取得更詳細的檢視此儲存體重新同步處理每個伺服器上的外觀。 如果您瀏覽至您的伺服器，並看看*儲存體*圖表，您會看到需要修復中的資料量*紫色*確切的數字列正上方。 此數量會增加時伺服器已關機 （詳細資料必須 resynced），並逐漸降低伺服器恢復連線時 （資料會同步處理）。 必須是修復為 0 的資料量，您的儲存體時進行重新同步-現在您已記下的伺服器，如果您需要免費。 這項體驗 Windows Admin Center 中的螢幕擷取畫面如下所示：

![映像的 Windows Admin Center 中的伺服器檢視 」](media/understand-storage-resync/server.png)

## <a name="how-to-see-storage-resync-in-windows-server-2016"></a>如何查看 Windows Server 2016 中的重新同步處理儲存體

如您所見，此警示會特別有幫助，取得儲存層的事情的整體檢視。 有效地摘要說明您可以從傳回長時間執行的 Storage 模組工作，例如儲存空間上的修復作業的相關資訊的 Get-storagejob 指令程式取得的資訊。 範例如下所示：

```PowerShell
Get-StorageJob
```

以下是範例輸出：

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

此檢視是更加細微，由於所列的儲存體作業是每個磁碟區，您可以看到正在執行的作業清單，並且您可以追蹤其個別的進度。 此 cmdlet 適用於 Windows Server 2016 和 2019年。

## <a name="see-also"></a>另請參閱

- [將伺服器離線進行維護](maintain-servers.md)
- [儲存空間直接存取概觀](storage-spaces-direct-overview.md)