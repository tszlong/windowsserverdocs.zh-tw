---
title: 刪除節點時發生問題
description: 本文說明從作用中的容錯移轉叢集成員資格移除節點時所遇到的問題。
ms.prod: windows-server
ms.technology: server-general
ms.date: 05/28/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 30714e8a9c5ca4ad13ed6757c6ce1da211b279ac
ms.sourcegitcommit: ef089864980a1d4793a35cbf4cbdd02ce1962054
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/28/2020
ms.locfileid: "84150226"
---
# <a name="having-a-problem-with-nodes-being-removed-from-active-failover-cluster-membership"></a>從作用中的容錯移轉叢集成員資格移除節點時發生問題

這篇文章介紹如何解決從「主動容錯移轉叢集」成員資格中隨機移除節點的問題。

## <a name="symptoms"></a>徵兆

當問題發生時，您會看到像這樣的事件會記錄在系統事件記錄檔中：

![事件1135](media/problem-nodes-failover-cluster/1135-1.png)

此事件將會記錄在叢集中的所有節點上，但已移除的節點除外。 這個事件的原因是因為叢集中的其中一個節點標示為 [關閉] 節點。 接著，它會通知事件的所有其他節點。 當節點收到通知時，它們會中斷並終止其與已停止節點的心跳連接。

## <a name="what-caused-the-node-to-be-marked-down"></a>導致節點被標示為關閉的原因

Windows 2008 或 2008 R2 容錯移轉叢集中的所有節點，會透過設定為允許在此網路上進行叢集網路通訊的網路彼此交談。 節點會將跨這些網路的心跳封包傳送至其他所有節點。 這些封包應該會由其他節點接收，然後回應就會送回。 叢集中的每個節點都有自己的要監視的心跳，以確保網路已啟動，而其他節點已啟動。 下面的範例應該有助於闡明這一點：

![節點](media/problem-nodes-failover-cluster/Node2.png)

如果未傳回任何一個封包，則會將特定的信號視為失敗。 例如，W2K8.WIM-R2-節點2會傳送要求，並接收來自 W2K8.WIM-R2-1-4 的回應至信號封包，讓它判斷網路和節點已啟動。  如果 W2K8.WIM--1-節點將要求傳送至 W2K8.WIM-R2-節點2，而 W2K8.WIM-R2-節點未取得回應，則會將它視為遺失的信號，而 W2K8.WIM--1-a 會追蹤它。  這個錯過的回應可以讓 W2K8.WIM 在收到另一個信號要求之前，向下顯示網路。

根據預設，叢集節點在5秒內有五個失敗的限制，然後才會將連接標示為關閉。 因此，如果 W2K8.WIM-R2-節點在時間週期內未收到回應五次，則會將該特定路由視為 W2K8.WIM-R2-節點2。 如果其他路由仍被視為已啟動，則 W2K8.WIM-R2-節點2會保留為作用中成員。

如果所有的路由都標示為 W2K8.WIM-R2-節點2，則會從作用中的容錯移轉叢集成員資格中移除，而且會記錄您在第一個區段中看到的事件1135。 在 W2K8.WIM-R2-節點2上，叢集服務會終止，然後重新開機，讓它可以嘗試重新加入叢集。

如需如何處理具有三個或更多節點之特定路由的詳細資訊，請參閱 Jeff Hughes 所寫入的「分割」叢集[網路](/archive/blogs/askcore/partitioned-cluster-networks)的 blog。

## <a name="now-that-we-know-how-the-heartbeat-process-works-what-are-some-of-the-known-causes-for-the-process-to-fail"></a>既然我們已經知道如何處理該程式的運作方式，進程失敗的已知原因有哪些

1. 實際的網路硬體故障。 如果在節點之間的網路上遺失封包，則會導致心跳失敗。 與這兩個節點相關的網路追蹤都會顯示這種情況。

2. 您的網路連線的設定檔可能會從網域跳動到公用，並再次回到網域。 在這些變更的轉換期間，可能會封鎖網路 i/o。 您可以查看網路設定檔作業記錄，查看是否為這種情況。 您可以藉由開啟 [事件檢視器並流覽至 [應用程式和服務 Logs\Microsoft\Windows\NetworkProfile\Operational.] 來尋找此記錄檔。 查看事件識別碼：1135中所述節點上此記錄中的事件，並查看目前是否已變更該設定檔。 若是如此，請參閱知識庫文章：[網路位置設定檔在 windows 7 或 Windows Server 2008 R2 中的「網域」變更為「公用](https://support.microsoft.com/help/2524478/the-network-location-profile-changes-from-domain-to-public-in-windows)」。

3. 您已在伺服器上啟用 IPv6，但已針對 Windows 防火牆中的輸入和輸出停用下列兩個規則：

    - 核心網路-鄰居探索公告
    - 核心網路-鄰居探索請求

4. 防毒軟體也可能幹擾此程式。 如果您有疑問，請停用或卸載軟體來進行測試。 請自行承擔風險，因為您會在此時不受病毒感染。

5. 您網路上的延遲也可能會導致此狀況發生。 節點之間的封包可能不會遺失，但它們在超時時間到期之前，可能無法快速取得節點。

6. IPv6 是容錯移轉叢集將用於其心跳的預設通訊協定。 「心跳本身」是透過埠3343進行通訊的 UDP 單播網路封包。 如果沒有適當設定的交換器、防火牆或路由器可允許此流量通過，您可能會遇到像這樣的問題。

7. IPsec 安全性原則重新整理也會造成此問題。 具體的問題是在 IPSec 群組原則更新期間，所有 IPsec 安全性關聯（SAs）都是由 Windows 防火牆 with Advanced Security （WFAS）進行破壞。 發生這種情況時，所有網路連線都會遭到封鎖。 當重新交涉安全性關聯時，如果使用 Active Directory 執行驗證時發生延遲，這些延遲（所有網路通訊都會遭到封鎖）也會封鎖叢集的心跳，使叢集健康狀態監視無法通過，如果節點未在5秒的閾值內回應，則會導致叢集健全狀況監控。

8. 舊的或過時的網路卡驅動程式和/或固件。  有時，網路卡或交換器的簡單錯誤設定可能也會導致遺失信號。

9. 新式網路卡和虛擬網路卡可能會遇到封包遺失。  這可以藉由開啟效能監視器並新增「網路 Interface\Packets 已捨棄」計數器來加以追蹤。  這個計數器是累計的，只有在伺服器重新開機後才會增加。  看到此處捨棄的大量封包，可能是因為網路卡上的接收緩衝區設定過低，或是伺服器執行速度太慢，而且無法處理輸入流量。  每張網路介面卡製造商選擇是否要在網路卡的內容中公開這些設定，因此您必須參考製造商的網站，以瞭解如何增加這些值，以及應該使用的建議值。  如果您是在 VMware 上執行，下列的 blog 會詳細討論這個問題，包括如何判斷這是否為問題，以及將變更的設定指向 VMWare 文章。

    [從 VMWare ESX 上的容錯移轉叢集成員資格移除節點](/archive/blogs/askcore/nodes-being-removed-from-failover-cluster-membership-on-vmware-esx)

這些是記錄這些事件最常見的原因，但也可能有其他原因。 此 blog 的重點是讓您深入瞭解此程式，並也提供要尋找之事項的想法。 有些會將下列值提升到其最大值，以嘗試讓此問題停止。

|參數|預設|範圍|
|---|---|---|
|**SameSubnetDelay**|1000毫秒|250-2000 毫秒|
|**CrossSubnetDelay**|1000毫秒|250-4000 毫秒|
|**SameSubnetThreshold**|5|3-10|
|**CrossSubnetThreshold**|5|3-10|
||||

將這些值增加到其最大值，可能會讓事件和節點移除消失，而只會遮罩問題。 它不會修正任何問題。 要做的最佳做法，是找出失敗的根本原因，並加以修正。 增加這些值的唯一實際需求是在多網站案例中，節點位於不同的位置，而無法克服網路延遲。
