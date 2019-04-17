---
title: 了解並查看存放裝置重新同步處理
description: 存放裝置重新同步發生時，以及如何在 Windows Server 2019 中看到上的詳細的資訊。
keywords: 儲存空間直接存取、 存放裝置重新同步處理，重新同步處理儲存體，S2D
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: 81b1136a4b6a5cf8423a99e898b482a9b2849b5f
ms.sourcegitcommit: 748eccd10fc0c3c962d6e64ff6ead08017ac1947
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/15/2019
ms.locfileid: "9009622"
---
# 了解並監視存放裝置重新同步處理

>適用於： Windows Server 2019

存放裝置重新同步警示是存取的[儲存空間直接存取](storage-spaces-direct-overview.md)中，當您儲存空間 resyncing 擲回錯誤健全狀況服務可讓 Windows Server 2019 的新功能。 警示是用於通知您時是否發生重新同步處理，以便您不小心不要花更多伺服器向下 （這會造成會受到影響，多個容錯網域產生向您叢集中）。 

本主題提供背景和步驟，以了解並查看在 Windows Server 容錯移轉叢集上使用儲存空間直接存取的存放裝置重新同步。

## 了解重新同步處理

讓我們開始以了解如何取得同步儲存簡單的範例。請記住任何無共用 （本機磁碟機只） 分散式存放裝置解決方案展示此行為。 如您所見，如果一個伺服器節點故障，則不會更新其磁碟機，直到它恢復連線-這是任何超交集架構，則為 true。 

假設我們想要儲存的字串 「 HELLO 」。 

![字串 「 hello 」 的 ASCII](media/understand-storage-resync/hello.png)

我們有三向鏡像復原 Asssuming，我們有三份這個字串。 現在，我們會關閉伺服器 #1 暫時 （適用於維護），如果我們無法存取複製 #1。

![無法存取複製 #1](media/understand-storage-resync/copy1.png)

假設我們更新我們從 「 HELLO 」 字串到 「 說明 ！ 」 在這個階段。

![ASCII 的字串 「 說明 ！ 」](media/understand-storage-resync/help.png)

一旦我們更新字串，複製 #2 和 #3 將會成功更新。 不過，複製 #1 仍然無法存取，因為伺服器 #1 是向下暫時 （適用於維護）。 

![撰寫 #2 和 #2 複製的 Gif 」](media/understand-storage-resync/write.gif)

現在，我們已經複製 #1 是同步的資料。作業系統會使用追蹤來追蹤同步的位元的細微廢棄區域。伺服器 #1 恢復連線時如此一來，我們可以透過從複本 #2 或 #3 讀取資料，並覆寫中複製 #1 的資料同步的變更。 此方法的優點是，我們只需要複製過去已過時，而不是 resyncing 的所有資料從伺服器 #2 或伺服器 #3 的資料。

![若要複製 #1 的覆寫 Gif 」](media/understand-storage-resync/overwrite.gif)

因此，這會說明如何取得同步的資料。但什麼沒有看起來會像是在高階？ 假設這個範例中，我們有三個伺服器超融合式叢集。 在維護伺服器 #1 時，您會看到它為向下。 當您將伺服器 #1 備份好時，它將會開始 resyncing 所有使用 （上方所述） 的細微的廢棄區域追蹤其儲存體。 一旦所有重新同步資料，所有伺服器將會都顯示為最多。

![獲得系統管理員檢視的重新同步處理的 Gif 」](media/understand-storage-resync/admin.gif)

## 如何監視存放裝置重新同步處理，在 Windows Server 2019

現在，您了解儲存空間重新同步處理的運作方式，讓我們看看如何之所以會顯示在 Windows Server 2019 中。 我們新增了[健全狀況服務](../../failover-clustering/health-service-overview.md)，當您儲存空間 resyncing 將顯示新的容錯。

若要在 PowerShell 中檢視此錯誤，請執行：

``` PowerShell
Get-HealthFault
```

這是在 Windows Server 2019，將新的錯誤，而且會出現在 PowerShell 中，在叢集驗證報告，以及其他地方建置上健全狀況的錯誤。 

若要取得更深入的檢視，您可以查詢時間系列資料庫在 PowerShell 中的，如下所示：

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

值得注意的是，Windows Admin Center 會使用健全狀況的錯誤，設定的狀態與叢集節點的色彩。 因此，這個新的錯誤會造成從紅色 （向下） 轉換為黃色 (resyncing) 以綠色 （向上），而不直接從紅色前往綠色，叢集節點 HCI 儀表板上。

![2016 vs 2019 檢視的重新同步處理的映像 」](media/understand-storage-resync/compare.png)

藉由顯示整體存放裝置重新同步處理進度，您可以準確知道多少資料同步的且您的系統是否正在向前進度。 當您開啟 Windows Admin Center，並移至*儀表板*時，您會看到新的警示，如下所示：

![Windows Admin Center 中的警示的映像 」](media/understand-storage-resync/alert.png)

警示是用於通知您時是否發生重新同步處理，以便您不小心不要花更多伺服器向下 （這會造成會受到影響，多個容錯網域產生向您叢集中）。 

如果您瀏覽至 [Windows Admin Center 中的*伺服器*] 頁面，按一下 [*詳細目錄*]，然後選擇 [特定伺服器，您可以取得此存放裝置重新同步處理以個別伺服器為基礎的外觀的詳細的檢視。 如果您瀏覽至您的伺服器，並看看*儲存*圖表，您會看到需要修復*紫色*一行，使用確切數目中的資料量上面。 這段會增加伺服器時向下 （詳細資料的需求重新同步），並逐漸減少伺服器恢復連線時 （正在同步處理資料）。 必須是修復為 0 的資料量，存放裝置時完成 resyncing-您現在是免費的伺服器會關閉，如果您需要。 Windows Admin Center 中的這個體驗的螢幕擷取畫面如下所示：

![Windows Admin Center 中的伺服器檢視的映像 」](media/understand-storage-resync/server.png)

## 如何查看 Windows Server 2016 中的存放裝置重新同步

如您所見，此警示是特別有幫助中取得儲存層發生了什麼的完整檢視。 有效地摘要說明您可以從 Get-StorageJob cmdlet，此功能會傳回長時間執行儲存模組工作，例如修復作業上儲存空間的相關資訊以取得的資訊。 範例如下所示：

```PowerShell
Get-StorageJob
```

以下是範例輸出︰

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

這個檢視是很多更細微的因為所列的存放裝置工作是每個磁碟區，您可以看到正在執行的工作清單，您可以追蹤其個別的進度。 這個 cmdlet 於 Windows Server 2016 和 2019年的運作方式。

## 請參閱

- [使伺服器離線以進行維護](maintain-servers.md)
- [儲存空間直接存取概觀](storage-spaces-direct-overview.md)