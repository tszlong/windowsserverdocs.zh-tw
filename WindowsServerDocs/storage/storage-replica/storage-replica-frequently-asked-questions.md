---
description: 深入瞭解：儲存體複本的常見問題
title: 儲存體複本的常見問題集
manager: siroy
ms.author: nedpyle
ms.topic: how-to
author: nedpyle
ms.date: 04/15/2020
ms.assetid: 12bc8e11-d63c-4aef-8129-f92324b2bf1b
ms.openlocfilehash: 6cb59e94549aed049a4555ad2d9d7fd0448da4f8
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948614"
---
# <a name="frequently-asked-questions-about-storage-replica"></a>儲存體複本的常見問題集

>適用於：Windows Server 2019、Windows Server 2016、Windows Server (半年通道)

本主題包含關於「儲存體複本」的常見問題集 (FAQ) 解答。

## <a name="is-storage-replica-supported-on-azure"></a><a name="FAQ1"></a>Azure 支援儲存體複本嗎？

是。 您可以在 Azure 中使用下列案例：

1. Azure 內的伺服器對伺服器複寫 (在一或兩個資料中心容錯網域中的 IaaS Vm 之間同步或非同步，或在兩個不同區域之間進行非同步複寫) 
2. Azure 與內部部署之間的伺服器對伺服器非同步複寫 (使用 VPN 或 Azure ExpressRoute) 
3. Azure 內的叢集對叢集複寫 (在一或兩個資料中心容錯網域中的 IaaS Vm 之間同步或非同步地進行，或在兩個不同區域之間進行非同步複寫) 
4. 使用 VPN 或 Azure ExpressRoute 在 Azure 與內部部署之間進行叢集對叢集的非同步複寫 () 

如需 Azure 中的來賓叢集的進一步注意事項，請參閱： [在 Microsoft Azure 中部署 IAAS VM 來賓](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126)叢集。

重要事項：

1. Azure 不支援共用 VHDX 來賓叢集，因此 Windows 容錯移轉叢集虛擬機器必須針對傳統的共用儲存體持續性磁片保留叢集或儲存空間直接存取使用 iSCSI 目標。
2. 有 Azure Resource Manager 範本適用于以儲存空間直接存取為基礎的儲存體複本叢集， [建立具有儲存體複本的儲存空間直接存取 SOFS 叢集，以便跨 Azure 區域進行](https://aka.ms/azure-storage-replica-cluster)嚴重損壞修復。
3. 若要將叢集的存取權授與叢集的叢集 (叢集 Api 所需的叢集 Api，以將存取權授與叢集) 需要設定 CNO 的網路存取。 您必須允許 TCP 埠135和 TCP 埠49152上的動態範圍。 [在 AZURE IAAS VM 上建立 Windows Server 容錯移轉叢集的參考-第2部分網路和建立](/archive/blogs/askcore/building-windows-server-failover-cluster-on-azure-iaas-vm-part-2-network-and-creation)。
4. 您可以使用兩個節點的來賓叢集，其中每個節點都會針對儲存體複本所複寫的非對稱叢集使用回送 iSCSI。 但是，這可能會造成效能不佳，而且只能用於非常有限的工作負載或測試。

## <a name="how-do-i-see-the-progress-of-replication-during-initial-sync"></a><a name="FAQ2"></a> 如何? 在初始同步處理期間查看複寫的進度嗎？
目的地伺服器上「儲存體複本系統管理」事件記錄檔中所示的事件 1237 訊息，每隔 10 秒會顯示已複製的位元組數目和剩餘的位元組數目。 您也可以使用目的地上的「儲存體複本」效能計數器，該計數器針對一或多個複寫的磁碟區顯示 **\儲存體複本統計資料\已接收的位元組總數**。 您也可以使用 Windows PowerShell 來查詢複寫群組。 例如，此範例命令會取得目的地上的群組名稱，然後每隔 10 秒查詢一個名為 **Replication 2** 的群組來顯示進度：

```
Get-SRGroup

do{
    $r=(Get-SRGroup -Name "Replication 2").replicas
    [System.Console]::Write("Number of remaining bytes {0}`n", $r.NumOfBytesRemaining)
    Start-Sleep 10
}until($r.ReplicationStatus -eq 'ContinuouslyReplicating')
Write-Output "Replica Status: "$r.replicationstatus

```

## <a name="can-i-specify-specific-network-interfaces-to-be-used-for-replication"></a><a name="FAQ3"></a>我是否可以指定要用於複寫的特定網路介面？

是，請使用 `Set-SRNetworkConstraint`。 這個 Cmdlet 會在介面層上運作，並在叢集與非叢集案例上使用。
例如，利用獨立伺服器 (在每個節點上)：

```
Get-SRPartnership

Get-NetIPConfiguration
```
記下閘道和介面資訊 (在這兩部伺服器上) 及合作關係指引。 然後執行：

```
Set-SRNetworkConstraint -SourceComputerName sr-srv06 -SourceRGName rg02 -
SourceNWInterface 2 -DestinationComputerName sr-srv05 -DestinationNWInterface 3 -DestinationRGName rg01

Get-SRNetworkConstraint

Update-SmbMultichannelConnection

```

在延展式叢集上設定網路限制式：

```
Set-SRNetworkConstraint -SourceComputerName sr-cluster01 -SourceRGName group1 -SourceNWInterface "Cluster Network 1","Cluster Network 2" -DestinationComputerName sr-cluster02 -DestinationRGName group2 -DestinationNWInterface "Cluster Network 1","Cluster Network 2"
```

## <a name="can-i-configure-one-to-many-replication-or-transitive-a-to-b-to-c-replication"></a><a name="FAQ4"></a> 我可以設定一對多複寫或可轉移 (A 到 B 至 C) 複寫嗎？
否，儲存體複本只支援伺服器、叢集或延展叢集節點的一對一複寫。 這可能會在未來版本中變更。 當然，您可以設定特定磁碟區組的各種伺服器間任一方向的複寫。 例如，伺服器 1 可以將其 D 磁碟區複寫到伺服器 2，並從伺服器 3 複寫其 E 磁碟區。

## <a name="can-i-grow-or-shrink-replicated-volumes-replicated-by-storage-replica"></a><a name="FAQ5"></a>我是否可以增加或縮減「儲存體複本」所複寫的複寫磁碟區？
您可以增加 (延伸) 磁碟區，而不是進行壓縮。 根據預設，儲存體複本防止系統管理員延伸複寫磁碟區。在調整大小之前，在來源群組上使用 `Set-SRGroup -AllowVolumeResize $TRUE` 選項。 例如：

1. 針對來源電腦使用： `Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. 使用任何您想要的技術增加磁碟區
3. 針對來源電腦使用： `Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE`

## <a name="can-i-bring-a-destination-volume-online-for-read-only-access"></a><a name="FAQ6"></a>我是否可以使目的地磁碟區上線以進行唯讀存取？
不在 Windows Server 2016 中。 當複寫開始，儲存體複本會卸載目的地磁碟區。

不過，在 Windows Server 2019 和 Windows Server Semi-Annual 通道（從1709版開始），現在可以使用掛接目的地儲存體的選項，這項功能稱為「測試容錯移轉」。 若要這樣做，您必須有目前未於目的地複寫的未使用磁碟區或者 NTFS 或 ReFS 格式化磁碟區。 然後便可暫時掛接已複寫存放裝置的快照集以作測試或備份之用。

例如，若要建立測試容錯移轉，藉以在目的地伺服器「SRV2」的複寫群組「RG2」中複寫磁碟區「D:」，以及在 SRV2 上安裝未經複寫的「T:」磁碟機：

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`

現已可在 SRV2 上存取複寫的磁碟區 D:。 您可以正常讀取和寫入它、將檔案複製到其他位置，或執行您儲存在其他位置的線上備份，以在 D：路徑下儲存。 T：磁片區只會包含記錄資料。

若要移除測試容錯移轉快照並捨棄其變更：

 `Dismount-SRDestination -Name RG2 -Computername SRV2`

您只能將測試容錯移轉功能用於短期暫時作業。 此功能並不適合長期使用。 使用時，複寫會持續對實際目的地磁碟區進行。

## <a name="can-i-configure-scale-out-file-server-sofs-in-a-stretch-cluster"></a><a name="FAQ7"></a> 是否可以在延展叢集中設定擴充檔案伺服器 (SOFS) ？
雖然技術上可行，但這並不是建議的設定，因為 SOFS 的計算節點中缺乏網站感知。 如果使用校園間網路（延遲通常是毫秒），此設定通常不會有任何問題。

如果設定叢集對叢集複寫，在兩個叢集之間進行複寫時，「儲存體複本」可完全支援「向外延展檔案伺服器」，包括使用「儲存空間直接存取」。

## <a name="is-csv-required-to-replicate-in-a-stretch-cluster-or-between-clusters"></a><a name="FAQ7.5"></a> 需要 CSV 才能在延展叢集中複寫還是在叢集之間進行複寫？
不會。 您可以使用 CSV 或持續性磁片保留來複寫 (寮國) 由叢集資源所擁有，例如檔案伺服器角色。

如果設定叢集對叢集複寫，在兩個叢集之間進行複寫時，「儲存體複本」可完全支援「向外延展檔案伺服器」，包括使用「儲存空間直接存取」。

## <a name="can-i-configure-storage-spaces-direct-in-a-stretch-cluster-with-storage-replica"></a><a name="FAQ8"></a>我是否可以在延展式叢集上使用「儲存體複本」來設定「儲存空間直接存取」？
這不是 Windows Server 中支援的設定。 這可能會在未來版本中變更。 如果設定叢集對叢集複寫，「儲存體複本」可完全支援「向外延展檔案伺服器」和 Hyper-V 伺服器，包括使用「儲存空間直接存取」。

## <a name="how-do-i-configure-asynchronous-replication"></a><a name="FAQ9"></a>如何設定非同步複寫？

指定 `New-SRPartnership -ReplicationMode` 並提供引數 **Asynchronous**。 根據預設值，「儲存體複本」中的所有複寫都是同步的。 您也可以使用 `Set-SRPartnership -ReplicationMode` 來變更模式。

## <a name="how-do-i-prevent-automatic-failover-of-a-stretch-cluster"></a><a name="FAQ10"></a>如何防止延展式叢集進行自動容錯移轉？
若要防止自動容錯移轉，您可以使用 PowerShell 來設定 `Get-ClusterNode -Name "NodeName").NodeWeight=0`。 這將會在災害復原網站中移除每個節點上的投票。 接著，您可以在主要網站的節點上使用 `Start-ClusterNode -PreventQuorum`，以及在災害網站的節點上使用 `Start-ClusterNode -ForceQuorum`，以強制執行容錯移轉。 沒有任何圖形化選項可防止自動容錯移轉，且不建議防止自動容錯移轉。

## <a name="how-do-i-disable-virtual-machine-resiliency"></a><a name="FAQ11"></a>如何停用虛擬機器復原功能？

若要防止新的 Hyper-v 虛擬機器復原功能執行，並因而暫停虛擬機器，而不是將它們容錯移轉至損毀修復網站，請執行 `(Get-Cluster).ResiliencyDefaultPeriod=0`

## <a name="how-can-i-reduce-time-for-initial-synchronization"></a><a name="FAQ12"></a> 如何縮短初始同步處理的時間？

您可以使用精簡佈建的儲存體做為一種方式來加速初始同步時間。 「儲存體複本」會查詢並自動使用精簡佈建的儲存體，包括非叢集儲存空間、Hyper-V 動態磁碟與 SAN LUN。

您也可以使用植入的資料磁片區，藉由確保目的地磁片區具有主要部分的資料子集，然後使用容錯移轉叢集管理員或中的植入選項，以減少頻寬使用量和有時的時間。 `New-SRPartnership` 如果磁碟區大部分都是空的，則使用植入的同步可能會降減少時間和頻寬使用量。 有多種方式可植入資料，並具有不同程度的效力：

1. 先前的複寫-藉由在包含磁片和磁片區的節點之間的一般初始同步處理進行複寫，移除複寫，在其他位置傳送目的地磁片，然後使用植入的選項新增複寫。 這是最有效的方法，因為儲存體複本保證有區塊複製鏡像，而複寫的唯一內容是 delta 區塊。
2. 還原快照集或還原快照集為基礎的備份-藉由將磁片區快照集還原到目的地磁片區，區塊配置中的差異應該最低。 這是下一個最有效的方法，因為區塊可能會比對磁片區快照集的鏡像影像。
3. 複製的檔案-藉由在目的地上建立從未使用過的新磁片區，並執行資料的完整 robocopy/MIR 樹狀結構複製，可能會有封鎖的相符專案。 使用 Windows 檔案總管或複製樹狀結構的某些部分將不會建立許多區塊相符專案。 手動複製檔案是最不有效的植入方法。

## <a name="can-i-delegate-users-to-administer-replication"></a><a name="FAQ13"></a> 我可以委派使用者來管理複寫嗎？

您可以使用 `Grant-SRDelegation` Cmdlet。 這可讓您在伺服器對伺服器、叢集對叢集及延展式複寫案例中設定特定使用者，就像擁有建立、修改或移除複寫的權限，而不需是本機系統管理員群組的成員。 例如：

```
Grant-SRDelegation -UserName contso\tonywang
```

這個 Cmdlet 將提醒您，使用者必須登出，然後登入其正準備進行管理的伺服器，以便讓變更生效。 您可以使用 `Get-SRDelegation` 和 `Revoke-SRDelegation` 進一步控制此動作。

## <a name="what-are-my-backup-and-restore-options-for-replicated-volumes"></a><a name="FAQ13"></a> 複寫磁片區的備份和還原選項有哪些？

「儲存體複本」支援備份及還原來源磁碟區。 它也支援建立及還原來源磁碟區的快照。 您無法在目的地磁碟區受到「儲存體複本」保護時備份或還原該磁碟區，因為它並未掛接，也無法存取。 如果您遇到來源磁碟區遺失的災害時，使用 `Set-SRPartnership` 將前一個目的地磁碟區立即升級為讀取/可寫入來源，將可讓您備份或還原該磁碟區。 您也可以使用 `Remove-SRPartnership` 和 `Remove-SRGroup` 來移除複寫，以重新掛接該磁碟區做為讀取/可寫入來源。

若要定期建立應用程式一致快照，您可以在來源伺服器上使用 VSSADMIN.EXE 以建立複寫資料磁碟區的快照。 例如，您正在其中使用「儲存體複本」來複寫 F: 磁碟區：

```
vssadmin create shadow /for=F:
```

接著，在您切換複寫方向、移除複寫，或者就只是仍位於相同來源磁碟機之後，您可以將任何快照還原到它的時間點。 例如，仍然使用 F：

```
vssadmin list shadows
vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
```

您也可以使用排程的工作，定期排程此工具來執行。 如需使用 VSS 的詳細資訊，請檢閱 [Vssadmin](../../administration/windows-commands/vssadmin.md)。 備份記錄檔磁碟區時沒有任何需要或值。 嘗試這麼做時，VSS 將會加以忽略。

使用 Windows Server Backup、Microsoft Azure 備份、Microsoft DPM 或其他快照，只要 VSS、虛擬機器或以檔案為基礎的技術是在磁碟區層內運作，就受到「儲存體複本」所支援。 「儲存體複本」不支援以區塊為基礎的備份及還原。

## <a name="can-i-configure-replication-to-restrict-bandwidth-usage"></a><a name="FAQ14"></a> 我可以設定複寫來限制頻寬使用量嗎？

是，可透過 SMB 頻寬限制器來設定。 這是適用於所有「儲存體複本」流量的全域設定，因此會影響所有來自此伺服器的複寫。 一般而言，只有使用「儲存體複本」初始同步設定時才需要此項，而其中的所有磁碟區資料都必須傳輸。 如果在初始同步之後需要執行此動作，您的網路頻寬對 IO 工作負載而言就會過低；請減少 IO 或增加頻寬。

這應該只能用於非同步複寫 (注意︰初始同步一律是非同步，即使您已指定同步也一樣)。
您也可以使用網路 QoS 原則，來為 「儲存體複本」流量塑型。 使用高度相符的植入「儲存體複本」複寫，也會大幅降低整體初始同步的頻寬使用量。

若要設定頻寬限制，請使用：

```
Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond x
```

若要查看頻寬限制，請使用：

```
Get-SmbBandwidthLimit -Category StorageReplication
```

若要移除頻寬限制，請使用：

```
Remove-SmbBandwidthLimit -Category StorageReplication
```

## <a name="what-network-ports-does-storage-replica-require"></a><a name="FAQ15"></a>儲存體複本需要哪些網路連接埠？

儲存體複本依賴 SMB 和 WSMAN 的複寫和管理。 這表示需要下列連接埠：

- 445 (SMB 複寫傳輸通訊協定，叢集 RPC 管理通訊協定) 
- 5445 (只有在使用 iWARP RDMA 網路) 時才需要 iWARP SMB
- 5985 (WMI/CIM/PowerShell) 的 WSManHTTP 管理通訊協定

> !記Test-SRTopology Cmdlet 需要 ICMPv4/ICMPv6，但無法進行複寫或管理。

## <a name="what-are-the-log-volume-best-practices"></a><a name="FAQ15.5"></a>有哪些記錄檔磁碟區最佳做法？

每個環境和工作負載的記錄檔大小上限會有很大的差異，並取決於您的工作負載所執行的寫入 IO 量。

1. 較大或較小的記錄檔不會讓您更快或更慢
2. 較大或較小的記錄在10GB 資料磁片區與10TB 資料磁片區之間沒有任何關聯，例如

較大的記錄檔只是在換出寫滿的資料之前收集和保留較多寫入 IO 而已。這可能會讓來源與目的地電腦之間發生的服務中斷 (例如網路斷線或目的地離線) 持續得更久。 如果記錄檔可以保留 10 小時的寫入，而網路中斷了 2 小時，當網路回復時，來源可以簡單地重新執行未同步的變更，非常迅速地將目的地的落差補足，您也很快地重新受到保護。 如果記錄檔保留 10 小時寫入而斷線了 2 天，來源現在則必須從不同的記錄檔 (稱為點陣圖) 開始重新執行，很可能較慢恢復到同步狀態。一旦進入同步狀態，就會回復使用原本的記錄檔。

儲存體複本完全仰賴記錄檔發揮寫入效能。 記錄檔效能對複寫效能非常重要。 因為記錄檔會序列化並循序化所有的寫入 IO，您必須確保記錄磁碟區的效能比資料磁碟區的效能還要好。 記錄磁碟區一定要使用像 SSD 這樣的快閃媒體。 絕不允許任何其他工作負載在記錄檔磁碟區上執行，同樣也不允許其他工作負載在 SQL 資料庫記錄檔磁碟區上執行。

再說一次：Microsoft 強烈建議記錄檔儲存體必須比資料儲存體更快速，而且記錄磁碟區絕不能用於其他工作負載。

您可以執行 Test-SRTopology 工具來取得記錄大小建議。 或者，您可以使用現有伺服器上的效能計數器，讓記錄檔大小判斷來。 公式很簡單：監視工作負載下的資料磁片輸送量 (平均寫入位元組數/秒) ，並用它來計算填滿不同大小之記錄所需的時間量。 例如，50 MB/s 的資料磁片輸送量會導致120GB 的記錄在 120GB/50MB 秒或2400秒或40分鐘內換行。 因此，在記錄檔包裝之前，目的地伺服器無法連線的時間長度是40分鐘。 如果記錄檔已換行，但目的地再次變成可連接，則來源會透過位對應記錄來重新執行區塊，而不是主要記錄檔。 記錄檔的大小不會影響效能。

只應備份來源叢集的資料磁片。 儲存體複本記錄檔磁片不應備份，因為備份可能會與儲存體複本作業發生衝突。

## <a name="why-would-you-choose-a-stretch-cluster-versus-cluster-to-cluster-versus-server-to-server-topology"></a><a name="FAQ16"></a>您選擇延展式叢集、叢集對叢集或伺服器對伺服器拓撲會有不同原因，差異何在？

儲存體複本有三個主要設定： stretch cluster、叢集對叢集和伺服器對伺服器。 各有不同的優點。

延展式叢集拓撲非常適合需要可透過協調流程自動容錯移轉的工作負載，例如 Hyper-V 私人雲端叢集和 SQL Server FCI。 其中也有使用 [容錯移轉叢集管理員] 的內建圖形介面。 透過持續保留使用 [儲存空間]、SAN、iSCSI 及 RAID 的傳統非對稱式叢集共用存放架構。 您只使用 2 個節點就可以執行這種拓撲。

叢集對叢集拓撲使用兩個不同的叢集，非常適合需要手動容錯移轉的系統管理員，尤其是在佈建第二網站作災害復原之用，而非日常使用時。 協調流程為手動。 不同于延展叢集，儲存空間直接存取可用於此設定 (有一些注意事項-請參閱儲存體複本常見問題和叢集對叢集檔) 。 您只使用 4 個節點就可以執行這種拓撲。

伺服器對伺服器拓撲非常適合執行無法納入叢集之硬體的客戶。 這需要手動容錯移轉及協調流程。 它很適合分公司和中央資料中心之間的便宜部署，特別是在使用非同步複寫時。 此設定通常可以取代用於單一主機災害復原案例之受 DFSR 保護的檔案伺服器執行個體。

在所有情況下，這些拓撲都支援在實體硬體及虛擬機器上執行。 在虛擬機器時，基礎 Hypervisor 不需要 Hyper-V；可以是 VMWare、KVM、Xen 等。

儲存複本也有伺服器對自我模式，您在此模式下將複寫指向同一台電腦上的兩個不同磁碟區。

## <a name="is-data-deduplication-supported-with-storage-replica"></a><a name="FAQ18"></a> 儲存體複本是否支援重復資料刪除？

是，儲存體複本支援資料 Deduplcation。 在來源伺服器上的磁片區上啟用重復資料刪除，而且在複寫期間，目的地伺服器會收到磁片區的重復資料刪除複本。

雖然您應該在來源和目的地伺服器上同時 *安裝* 重復資料刪除 (請參閱 [安裝和啟用重復資料刪除](../data-deduplication/install-enable.md)) ，請務必不要在目的地伺服器上 *啟用* 重復資料刪除。 儲存體複本只允許在來源伺服器上寫入。 因為重復資料刪除會對磁片區進行寫入，所以它應該只在來源伺服器上執行。

## <a name="can-i-replicate-between-windows-server-2019-and-windows-server-2016"></a><a name="FAQ19"></a> 我可以在 Windows Server 2019 和 Windows Server 2016 之間進行複寫嗎？

可惜的是，我們不支援在 Windows Server 2019 和 Windows Server 2016 之間建立 *新* 的合作關係。 您可以安全地將執行 Windows Server 2016 的伺服器或叢集升級至 Windows Server 2019，任何 *現有* 的合作關係都將繼續運作。

不過，若要取得 Windows Server 2019 的改進複寫效能，合作關係的所有成員都必須執行 Windows Server 2019，而您必須刪除現有的合作關係和相關聯的複寫群組，然後使用植入的資料重新建立這些成員， (在 Windows Admin Center 或使用 New-SRPartnership Cmdlet) 建立合作關係時。

## <a name="how-do-i-report-an-issue-with-storage-replica-or-this-guide"></a><a name="FAQ17"></a> 如何? 報告儲存體複本或本指南的問題嗎？

如需儲存體複本的技術協助，您可以在 [Microsoft 論壇](/answers/index.html)張貼文章。 您也可以透過電子郵件，將「儲存體複本」相關問題或與此文件的相關問題寄送到 srfeed@microsoft.com。 [Windows Server 一般意見反應網站](https://windowsserver.uservoice.com/forums/295047-general-feedback)是設計變更要求的建議，因為它可讓您的客戶為您的想法提供支援和意見反應。

## <a name="can-storage-replica-be-configured-to-replicate-in-both-directions"></a><a name="FAQ18"></a> 是否可以將儲存體複本設定為雙向複寫？

儲存體複本是單向複寫技術。  它只會依據每個磁片區，從來源複寫到目的地。  您可以隨時反轉此方向，但仍只有一個方向。  不過，這並不表示您不能有一組磁片區 (來源和目的地) 以一種方向進行複寫，以及 (來源和目的地) 以相反方向複寫的不同磁片磁碟機組。  例如，您想要設定伺服器對伺服器複寫。  Server1 和 Server2 都有磁碟機號 L：、M：、N：和 O：而且您想要將磁片磁碟機 M：從 Server1 複寫至 Server2，但磁片磁碟機 O：從 Server2 複寫至 Server1。  只要每個群組都有個別的記錄磁片磁碟機，就可以完成這項工作。 即。

- Server1 來源磁片磁碟機 M：使用來源記錄磁片磁碟機 L：複寫至 Server2 目的地磁片磁碟機 M：目的地記錄磁片磁碟機 L：
- Server2 來源磁片磁碟機 O：使用來源記錄磁片磁碟機 N：複寫至 Server1 目的地磁片磁碟機 O：目的地記錄磁片磁碟機 N：

## <a name="related-topics"></a>[相關主題]
- [儲存體複本總覽](storage-replica-overview.md)
- [使用共用儲存體延展叢集複寫](stretch-cluster-replication-using-shared-storage.md)
- [伺服器對伺服器儲存體複寫](server-to-server-storage-replication.md)
- [叢集至叢集儲存體複寫](cluster-to-cluster-storage-replication.md)
- [儲存體複本：已知問題](storage-replica-known-issues.md)

## <a name="see-also"></a>另請參閱
- [儲存體概觀](../storage.yml)
- [儲存空間直接存取](../storage-spaces/storage-spaces-direct-overview.md)
