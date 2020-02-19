---
title: 瞭解並查看儲存體重新同步
description: 有關儲存體重新同步發生時間，以及如何在 Windows Server 2019 中查看的詳細資訊。
keywords: 儲存空間直接存取、儲存體重新同步、重新同步、儲存體、S2D
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/14/2019
ms.localizationpriority: medium
ms.openlocfilehash: d271d92a14278e52a6020c60f96f48b1c8b35871
ms.sourcegitcommit: 2a15de216edde8b8e240a4aa679dc6d470e4159e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465302"
---
# <a name="understand-and-monitor-storage-resync"></a>了解和監視存放裝置重新同步

>適用于： Windows Server 2019

儲存體重新同步警示是 Windows Server 2019 中[儲存空間直接存取](storage-spaces-direct-overview.md)的新功能，可讓健全狀況服務在 resyncing 儲存體時擲回錯誤。 警示適用于在發生重新同步時通知您，因此您不會意外地將更多的伺服器關機（這可能會導致多個容錯網域受到影響，因而導致叢集停止運作）。 

本主題提供在具有儲存空間直接存取的 Windows Server 容錯移轉叢集中，瞭解和查看存放裝置重新同步的背景和步驟。

## <a name="understanding-resync"></a>瞭解重新同步

讓我們從一個簡單的範例開始，以瞭解儲存體如何同步。請記住，任何共用的「無」（僅限本機磁片磁碟機）分散式儲存體解決方案都會出現這種行為。 如下所示，如果某個伺服器節點停止運作，其磁片磁碟機將不會更新，直到它恢復上線為止-這適用于任何超融合式架構。 

假設我們想要儲存字串 "HELLO"。 

![字串 "hello" 的 ASCII](media/understand-storage-resync/hello.png)

Asssuming，我們有三向鏡像復原，我們有三個此字串的複本。 現在，如果我們暫時讓伺服器 #1 關閉（維護），就無法存取 copy #1。

![無法存取複製 #1](media/understand-storage-resync/copy1.png)

假設我們將字串從 "HELLO" 更新為 "HELP！" 目前的時間。

![字串 "help！" 的 ASCII](media/understand-storage-resync/help.png)

更新字串之後，複製 #2 和 #3 將會成功更新。 不過，仍然無法存取複製 #1，因為伺服器 #1 暫時關閉（適用于維護）。 

![寫入以複製 #2 和 #2 的 Gif」](media/understand-storage-resync/write.gif)

現在，我們有複製 #1，其中包含不同步的資料。作業系統會使用細微的變更區域追蹤來追蹤不同步的位。如此一來，當伺服器 #1 重新上線時，我們就可以從複製 #2 讀取資料，或 #3 並覆寫複製 #1 中的資料，以同步處理變更。 這種方法的優點是，我們只需要複製過時的資料，而不是從伺服器 #2 或伺服器 #3 中 resyncing 所有資料。

![覆寫以複製 #1 "的 Gif](media/understand-storage-resync/overwrite.gif)

因此，這會說明資料不同步的方式。但這在高階方面的外觀為何？ 假設在此範例中，我們有三部伺服器超融合式叢集。 當伺服器 #1 處於 [維護中] 時，您會看到它處於關閉狀態。 當您讓伺服器 #1 備份時，它會使用細微的中途區域追蹤（如上所述）開始 resyncing 其所有的儲存體。 一旦資料全部恢復同步，所有伺服器都會顯示為 [啟動]。

![重新同步處理之系統管理視圖的 Gif](media/understand-storage-resync/admin.gif)

## <a name="how-to-monitor-storage-resync-in-windows-server-2019"></a>如何在 Windows Server 2019 中監視存放裝置重新同步

既然您已瞭解儲存體重新同步處理的運作方式，讓我們來瞭解這在 Windows Server 2019 中的顯示方式。 我們已將新的錯誤新增至將會在 resyncing 儲存體時顯示的[健全狀況服務](../../failover-clustering/health-service-overview.md)。

若要在 PowerShell 中查看此錯誤，請執行：

``` PowerShell
Get-HealthFault
```

這是 Windows Server 2019 中的新錯誤，並會出現在 PowerShell 中、叢集驗證報告中，以及任何在健康情況錯誤上建立的其他位置。 

若要取得更深入的觀點，您可以在 PowerShell 中查詢時間序列資料庫，如下所示：

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

值得注意的是，Windows 管理中心會使用健全狀況錯誤來設定叢集節點的狀態和色彩。 因此，這個新的錯誤會導致叢集節點從紅色（向下）轉換為黃色（resyncing）到綠色（向上），而不是從 [HCI] 儀表板上的 [紅色] 到 [綠色]。

![重新同步處理的2016與2019觀點的影像](media/understand-storage-resync/compare.png)

藉由顯示整體儲存體重新同步處理進度，您可以精確地得知有多少資料不同步，以及您的系統是否正在進行正向進度。 當您開啟 Windows 管理中心並移至*儀表板*時，您會看到新的警示，如下所示：

![Windows 系統管理中心內的警示影像](media/understand-storage-resync/alert.png)

警示適用于在發生重新同步時通知您，因此您不會意外地將更多的伺服器關機（這可能會導致多個容錯網域受到影響，因而導致叢集停止運作）。 

如果您流覽至 [Windows 系統管理中心] 中的 [*伺服器*] 頁面，按一下 [*清查*]，然後選擇特定的伺服器，您就可以更詳細地瞭解此儲存體重新同步的外觀，以查看每一伺服器。 如果您流覽至您的伺服器，並查看 [*儲存體*] 圖表，您會看到需要修復的資料量，並在前面加上正確數位的*紫色*行。 當伺服器關閉（需要 resynced 更多資料）時，此數量會增加，並在伺服器恢復上線（資料正在進行同步處理）時逐漸降低。 當需要修復的資料量是0時，您的儲存體會 resyncing 完成，您現在可以視需要將伺服器關閉。 Windows 管理中心的此體驗的螢幕擷取畫面如下所示：

![Windows 系統管理中心內伺服器視圖的影像](media/understand-storage-resync/server.png)

## <a name="how-to-see-storage-resync-in-windows-server-2016"></a>如何在 Windows Server 2016 中查看存放裝置重新同步

如您所見，此警示特別有助於取得儲存層中所發生之情況的整體觀點。 它會有效地摘要說明您可以從 Get-storagejob 指令程式取得的資訊，此 Cmdlet 會傳回長時間執行之儲存模組工作的相關資訊，例如儲存空間上的修復作業。 範例如下所示：

```PowerShell
Get-StorageJob
```

以下是範例輸出：

```
Name                  ElapsedTime           JobState              PercentComplete       IsBackgroundTask
----                  -----------           --------              ---------------       ----------------
Regeneration          00:01:19              Running               50                    True

```

此視圖較細微，因為列出的儲存作業是每個磁片區，您可以看到正在執行的作業清單，而且您可以追蹤其個別的進度。 此 Cmdlet 適用于 Windows Server 2016 和2019。

## <a name="see-also"></a>另請參閱

- [使伺服器離線以進行維護](maintain-servers.md)
- [儲存空間直接存取總覽](storage-spaces-direct-overview.md)