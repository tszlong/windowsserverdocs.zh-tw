---
title: 儲存體複本的常見問題集
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 04/26/2019
ms.assetid: 12bc8e11-d63c-4aef-8129-f92324b2bf1b
ms.openlocfilehash: 89676ba821b99d44865bc6f45c34c05edb771d9d
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865262"
---
# <a name="frequently-asked-questions-about-storage-replica"></a>儲存體複本的常見問題集

>適用於：Windows Server 2019、Windows Server 2016、Windows Server (半年通道)

本主題包含關於「儲存體複本」的常見問題集 (FAQ) 解答。

## <a name="FAQ1"></a>Azure 上是否支援儲存體複本？
是的。 您可以搭配 Azure 使用下列案例：

1. Azure 內的伺服器對伺服器複寫（一或兩個資料中心容錯網域中的 IaaS Vm 同步或非同步），或兩個不同區域之間的非同步複寫
2. Azure 和內部部署之間的伺服器對伺服器非同步複寫（使用 VPN 或 Azure ExpressRoute）
3. Azure 內的叢集對叢集複寫（一或兩個資料中心容錯網域中的 IaaS Vm 同步或非同步），或兩個不同區域之間的非同步複寫
4. Azure 和內部部署之間的叢集對叢集非同步複寫（使用 VPN 或 Azure ExpressRoute）

如需 Azure 中來賓叢集的進一步注意事項，請參閱：[在 Microsoft Azure 中部署 IAAS VM 來賓](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126)叢集。

重要注意事項：

1. Azure 不支援共用 VHDX 來賓叢集，因此 Windows 容錯移轉叢集虛擬機器必須針對傳統共用儲存體的持續性磁片保留叢集或儲存空間直接存取使用 iSCSI 目標。
2. 在[建立儲存空間直接存取 SOFS 叢集與儲存體複本，以在跨 Azure 區域進行](https://aka.ms/azure-storage-replica-cluster)嚴重損壞修復時，儲存空間直接存取型儲存體複本叢集有 Azure Resource Manager 範本。  
3. Azure 中的叢集對叢集 RPC 通訊（叢集 Api 所需，以授與叢集之間的存取權）需要設定 CNO 的網路存取。 您必須允許 TCP 埠135和 TCP 埠49152以上的動態範圍。 [在 AZURE IAAS VM 上建立 Windows Server 容錯移轉叢集的參考–第2部分網路和建立](https://blogs.technet.microsoft.com/askcore/2015/06/24/building-windows-server-failover-cluster-on-azure-iaas-vm-part-2-network-and-creation/)。  
4. 您可以使用雙節點的來賓叢集，其中每個節點都會針對儲存體複本所複寫的非對稱叢集使用回送 iSCSI。 但這可能會有非常不佳的效能，而且應該僅用於非常有限的工作負載或測試。  

## <a name="FAQ2"></a>如何? 在初始同步期間查看複寫的進度嗎？  
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

## <a name="FAQ3"></a>我可以指定要用於複寫的特定網路介面嗎？  

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

    Set-SRNetworkConstraint -SourceComputerName sr-cluster01 -SourceRGName group1 -SourceNWInterface "Cluster Network 1","Cluster Network 2" -DestinationComputerName sr-cluster02 -DestinationRGName group2 -DestinationNWInterface "Cluster Network 1","Cluster Network 2"

## <a name="FAQ4"></a>是否可以設定一對多複寫或可轉移（A 到 B 到 C）複寫？  
否，儲存體複本僅支援伺服器、叢集或延展叢集節點的一對一複寫。 這可能會在未來版本中變更。 當然，您可以設定特定磁碟區組的各種伺服器間任一方向的複寫。 例如，伺服器 1 可以將其 D 磁碟區複寫到伺服器 2，並從伺服器 3 複寫其 E 磁碟區。

## <a name="FAQ5"></a>我可以增加或縮減儲存體複本所複寫的複寫磁片區嗎？  
您可以增加 (延伸) 磁碟區，而不是進行壓縮。 根據預設，儲存體複本防止系統管理員延伸複寫磁碟區。在調整大小之前，在來源群組上使用 `Set-SRGroup -AllowVolumeResize $TRUE` 選項。 例如:

1. 針對來源電腦使用：`Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. 使用任何您想要的技術增加磁碟區
3. 針對來源電腦使用：`Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE` 

## <a name="FAQ6"></a>是否可以讓目的地磁片區上線以進行唯讀存取？  
不在 Windows Server 2016 中。 當複寫開始，儲存體複本會卸載目的地磁碟區。 

不過，在 Windows Server 2019 和 Windows Server 半年通道中，從版本1709開始，現在可以選擇掛接目的地儲存體，這項功能稱為「測試容錯移轉」。 若要這樣做，您必須有目前未於目的地複寫的未使用磁碟區或者 NTFS 或 ReFS 格式化磁碟區。 然後便可暫時掛接已複寫存放裝置的快照集以作測試或備份之用。 

例如，若要建立測試容錯移轉，藉以在目的地伺服器「SRV2」的複寫群組「RG2」中複寫磁碟區「D:」，以及在 SRV2 上安裝未經複寫的「T:」磁碟機：

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`
 
現已可在 SRV2 上存取複寫的磁碟區 D:。 您可以在 D：路徑底下，以一般方式讀取和寫入檔案、複製檔案，或執行您儲存在其他地方的線上備份以妥善保存。 T：磁片區只會包含記錄資料。

若要移除測試容錯移轉快照並捨棄其變更：

 `Dismount-SRDestination -Name RG2 -Computername SRV2`
 
您只能將測試容錯移轉功能用於短期暫時作業。 此功能並不適合長期使用。 使用時，複寫會持續對實際目的地磁碟區進行。 

## <a name="FAQ7"></a>我可以在延展式叢集中設定向外延展檔案伺服器（SOFS）嗎？  
在技術上可行的情況下，這不是建議的設定，因為在計算節點中缺少網站感知，以聯絡 SOFS。 如果使用校園網路，其中延遲通常是毫秒，則此設定通常會運作而不會發生問題。   

如果設定叢集對叢集複寫，在兩個叢集之間進行複寫時，「儲存體複本」可完全支援「向外延展檔案伺服器」，包括使用「儲存空間直接存取」。  

## <a name="FAQ7.5"></a>CSV 是否需要在延展叢集或叢集之間進行複寫？  
資料分割 您可以使用叢集資源所擁有的 CSV 或持續性磁片保留（PDR）（例如檔案伺服器角色）進行複寫。 

如果設定叢集對叢集複寫，在兩個叢集之間進行複寫時，「儲存體複本」可完全支援「向外延展檔案伺服器」，包括使用「儲存空間直接存取」。  

## <a name="FAQ8"></a>是否可以使用儲存體複本來設定 stretch cluster 中的儲存空間直接存取？  
這不是 Windows Server 中支援的設定。 這可能會在未來版本中變更。 如果設定叢集對叢集複寫，「儲存體複本」可完全支援「向外延展檔案伺服器」和 Hyper-V 伺服器，包括使用「儲存空間直接存取」。  

## <a name="FAQ9"></a>如何? 設定非同步複寫嗎？  

指定 `New-SRPartnership -ReplicationMode` 並提供引數 **Asynchronous**。 根據預設值，「儲存體複本」中的所有複寫都是同步的。 您也可以使用 `Set-SRPartnership -ReplicationMode` 來變更模式。  

## <a name="FAQ10"></a>如何? 防止延展叢集自動容錯移轉？  
若要防止自動容錯移轉，您可以使用 PowerShell 來設定 `Get-ClusterNode -Name "NodeName").NodeWeight=0`。 這將會在災害復原網站中移除每個節點上的投票。 接著，您可以在主要網站的節點上使用 `Start-ClusterNode -PreventQuorum`，以及在災害網站的節點上使用 `Start-ClusterNode -ForceQuorum`，以強制執行容錯移轉。 沒有任何圖形化選項可防止自動容錯移轉，且不建議防止自動容錯移轉。  

## <a name="FAQ11"></a>如何? 停用虛擬機器復原嗎？
若要防止新的 Hyper-v 虛擬機器復原功能執行，因而暫停虛擬機器，而不是將它們容錯移轉至損毀修復網站，請執行`(Get-Cluster).ResiliencyDefaultPeriod=0`  

## <a name="FAQ12"></a>如何縮短初始同步處理的時間？

您可以使用精簡佈建的儲存體做為一種方式來加速初始同步時間。 「儲存體複本」會查詢並自動使用精簡佈建的儲存體，包括非叢集儲存空間、Hyper-V 動態磁碟與 SAN LUN。  

您也可以使用植入的資料磁片區，藉由確保目的地磁片區具有主要的部分資料子集，然後使用容錯移轉叢集管理員或`New-SRPartnership`中的植入選項，來減少頻寬使用量，有時還會發生時間。 如果磁碟區大部分都是空的，則使用植入的同步可能會降減少時間和頻寬使用量。 有多種方式可以植入資料，效力的程度各不相同：

1. 先前的複寫-藉由在包含磁片和磁片區的節點之間以一般初始同步進行複寫，移除複寫，在別處傳送目的地磁片，然後使用植入的選項新增複寫。 這是最有效的方法，因為儲存體複本保證區塊複製鏡像，而唯一要複寫的是差異區塊。
2. 還原快照集或還原的快照式備份-藉由將磁片區快照集還原到目的地磁片區上，區塊配置中應該會有最小的差異。 這是下一個最有效的方法，因為因為磁片區快照集是鏡像映射，所以區塊可能會相符。
3. 複製的檔案-藉由在目的地上建立從未用過的新磁片區，並執行資料的完整 robocopy/MIR 樹狀複本，可能會有區塊相符的專案。 使用 Windows 檔案瀏覽器或複製樹狀結構的某些部分，並不會建立許多區塊相符專案。 手動複製檔案是植入的最有效方法。

## <a name="FAQ13"></a>我可以將使用者委派給管理複寫嗎？  

您可以使用`Grant-SRDelegation` Cmdlet。 這可讓您在伺服器對伺服器、叢集對叢集及延展式複寫案例中設定特定使用者，就像擁有建立、修改或移除複寫的權限，而不需是本機系統管理員群組的成員。 例如:  

    Grant-SRDelegation -UserName contso\tonywang  

這個 Cmdlet 將提醒您，使用者必須登出，然後登入其正準備進行管理的伺服器，以便讓變更生效。 您可以使用 `Get-SRDelegation` 和 `Revoke-SRDelegation` 進一步控制此動作。  

## <a name="FAQ13"></a>複寫磁片區的備份與還原選項有哪些？
「儲存體複本」支援備份及還原來源磁碟區。 它也支援建立及還原來源磁碟區的快照。 您無法在目的地磁碟區受到「儲存體複本」保護時備份或還原該磁碟區，因為它並未掛接，也無法存取。 如果您遇到來源磁碟區遺失的災害時，使用 `Set-SRPartnership` 將前一個目的地磁碟區立即升級為讀取/可寫入來源，將可讓您備份或還原該磁碟區。 您也可以使用 `Remove-SRPartnership` 和 `Remove-SRGroup` 來移除複寫，以重新掛接該磁碟區做為讀取/可寫入來源。
若要定期建立應用程式一致快照，您可以在來源伺服器上使用 VSSADMIN.EXE 以建立複寫資料磁碟區的快照。 例如，您正在其中使用「儲存體複本」來複寫 F: 磁碟區：

    vssadmin create shadow /for=F:
接著，在您切換複寫方向、移除複寫，或者就只是仍位於相同來源磁碟機之後，您可以將任何快照還原到它的時間點。 例如，仍然使用 F：

    vssadmin list shadows
     vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
您也可以使用排程的工作，定期排程此工具來執行。 如需使用 VSS 的詳細資訊，請檢閱 [Vssadmin](../../administration/windows-commands/vssadmin.md)。 備份記錄檔磁碟區時沒有任何需要或值。 嘗試這麼做時，VSS 將會加以忽略。
使用 Windows Server Backup、Microsoft Azure 備份、Microsoft DPM 或其他快照，只要 VSS、虛擬機器或以檔案為基礎的技術是在磁碟區層內運作，就受到「儲存體複本」所支援。 「儲存體複本」不支援以區塊為基礎的備份及還原。

## <a name="FAQ14"></a>我可以設定複寫來限制頻寬使用量嗎？
是，可透過 SMB 頻寬限制器來設定。 這是適用於所有「儲存體複本」流量的全域設定，因此會影響所有來自此伺服器的複寫。 一般而言，只有使用「儲存體複本」初始同步設定時才需要此項，而其中的所有磁碟區資料都必須傳輸。 如果在初始同步之後需要執行此動作，您的網路頻寬對 IO 工作負載而言就會過低；請減少 IO 或增加頻寬。

這應該只能用於非同步複寫 (注意︰初始同步一律是非同步，即使您已指定同步也一樣)。
您也可以使用網路 QoS 原則，來為 「儲存體複本」流量塑型。 使用高度相符的植入「儲存體複本」複寫，也會大幅降低整體初始同步的頻寬使用量。


若要設定頻寬限制，請使用：

    Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond x

若要查看頻寬限制，請使用：

    Get-SmbBandwidthLimit -Category StorageReplication

若要移除頻寬限制，請使用：

    Remove-SmbBandwidthLimit -Category StorageReplication
    
## <a name="FAQ15"></a>儲存體複本需要哪些網路埠？
儲存體複本依賴 SMB 和 WSMAN 的複寫和管理。 這表示需要下列連接埠：

 445（SMB 複寫傳輸通訊協定，叢集 RPC 管理通訊協定）5445（iWARP SMB-只有在使用 iWARP RDMA 網路時才需要）5985（WMI/CIM/PowerShell 的 WSManHTTP 管理通訊協定）

注意:Test-srtopology Cmdlet 需要 ICMPv4/ICMPv6，但無法進行複寫或管理。

## <a name="FAQ15.5"></a>記錄檔磁片區的最佳作法為何？
記錄檔的最佳大小在每個環境和工作負載上都有很大的差異，並取決於您的工作負載執行多少寫入 IO。 

1.  較大或較小的記錄檔不會讓您更快或更慢
2.  較大或較小的記錄檔與10TB 的資料量（例如

較大的記錄檔只是在換出寫滿的資料之前收集和保留較多寫入 IO 而已。這可能會讓來源與目的地電腦之間發生的服務中斷 (例如網路斷線或目的地離線) 持續得更久。 如果記錄檔可以保留 10 小時的寫入，而網路中斷了 2 小時，當網路回復時，來源可以簡單地重新執行未同步的變更，非常迅速地將目的地的落差補足，您也很快地重新受到保護。 如果記錄檔保留 10 小時寫入而斷線了 2 天，來源現在則必須從不同的記錄檔 (稱為點陣圖) 開始重新執行，很可能較慢恢復到同步狀態。一旦進入同步狀態，就會回復使用原本的記錄檔。

儲存體複本完全仰賴記錄檔發揮寫入效能。 記錄檔效能對複寫效能非常重要。 因為記錄檔會序列化並循序化所有的寫入 IO，您必須確保記錄磁碟區的效能比資料磁碟區的效能還要好。 記錄磁碟區一定要使用像 SSD 這樣的快閃媒體。 絕不允許任何其他工作負載在記錄檔磁碟區上執行，同樣也不允許其他工作負載在 SQL 資料庫記錄檔磁碟區上執行。 

遍Microsoft 強烈建議記錄儲存體的速度比資料儲存更快，而且該記錄檔磁片區永遠不能用於其他工作負載。

您可以執行 Test-srtopology 工具來取得記錄大小的建議。 或者，您可以使用現有伺服器上的效能計數器，讓記錄檔大小 judgement。 此公式很簡單：監視工作負載下的資料磁片輸送量（Avg Write Bytes/Sec），並用它來計算填滿不同大小之記錄檔所需的時間量。 例如，50 MB/s 的資料磁片輸送量會導致120GB 的記錄以 120GB/50MB 秒或2400秒或40分鐘來包裝。 因此，在記錄檔包裝在40分鐘之前，目的地伺服器可能無法連線的時間量。 如果記錄檔已包裝，但目的地再次可連線，則來源會透過位對應記錄來重新執行區塊，而不是主記錄檔。 記錄檔的大小不會影響效能。

只應備份來源叢集的資料磁片。 儲存體複本記錄檔磁片不應進行備份，因為備份可能會與儲存體複本作業發生衝突。

## <a name="FAQ16"></a>為什麼要選擇延展叢集，而不是伺服器對伺服器拓撲？  
儲存體複本有三個主要設定：延展叢集、叢集對叢集，以及伺服器對伺服器。 各有不同的優點。

延展式叢集拓撲非常適合需要可透過協調流程自動容錯移轉的工作負載，例如 Hyper-V 私人雲端叢集和 SQL Server FCI。 其中也有使用 [容錯移轉叢集管理員] 的內建圖形介面。 透過持續保留使用 [儲存空間]、SAN、iSCSI 及 RAID 的傳統非對稱式叢集共用存放架構。 您只使用 2 個節點就可以執行這種拓撲。

叢集對叢集拓撲使用兩個不同的叢集，非常適合需要手動容錯移轉的系統管理員，尤其是在佈建第二網站作災害復原之用，而非日常使用時。 協調流程為手動。 不同于延展叢集，儲存空間直接存取可以在此設定中使用（有警告-請參閱儲存體複本常見問題和叢集對叢集檔）。 您只使用 4 個節點就可以執行這種拓撲。 

伺服器對伺服器拓撲非常適合執行無法納入叢集之硬體的客戶。 這需要手動容錯移轉及協調流程。 這適用于分公司與中央資料中心之間的便宜部署，特別是在使用非同步複寫時。 此設定通常可以取代用於單一主機災害復原案例之受 DFSR 保護的檔案伺服器執行個體。

在所有情況下，這些拓撲都支援在實體硬體及虛擬機器上執行。 在虛擬機器時，基礎 Hypervisor 不需要 Hyper-V；可以是 VMWare、KVM、Xen 等。

儲存複本也有伺服器對自我模式，您在此模式下將複寫指向同一台電腦上的兩個不同磁碟區。

## <a name="FAQ18"></a>儲存體複本是否支援重復資料刪除？

是，儲存體複本支援資料 Deduplcation。 在來源伺服器上的磁片區上啟用重復資料刪除，而在複寫期間，目的地伺服器會收到磁片區的重復資料刪除複本。

雖然您應該同時在來源和目的地伺服器上*安裝*重復資料刪除（請參閱[安裝和啟用重復資料刪除](../data-deduplication/install-enable.md)），但請務必不要在目的地伺服器上*啟用*重復資料刪除。 儲存體複本只允許來源伺服器上的寫入。 因為重復資料刪除會對磁片區進行寫入，所以它只能在來源伺服器上執行。 

## <a name="FAQ19"></a>我可以在 Windows Server 2019 和 Windows Server 2016 之間進行複寫嗎？

可惜的是，我們不支援在 Windows Server 2019 和 Windows Server 2016 之間建立*新*的合作關係。 您可以將執行 Windows Server 2016 的伺服器或叢集安全地升級至 Windows Server 2019，任何*現有*的合作關係將會繼續運作。

不過，若要取得 Windows Server 2019 的改良複寫效能，合作關係的所有成員都必須執行 Windows Server 2019，而且您必須刪除現有的合作關係和相關聯的複寫群組，然後再以植入的資料重新建立它們（可能是在 Windows 系統管理中心或使用 Get-srpartnership Cmdlet 建立合作關係時）。

## <a name="FAQ17"></a>如何? 報告儲存體複本或本指南的問題嗎？  
如需「儲存複本」的技術協助，您可以張貼於 [Microsoft TechNet 論壇](https://social.technet.microsoft.com/Forums/windowsserver/en-US/home?forum=WinServerPreview)。 您也可以透過電子郵件，將「儲存體複本」相關問題或與此文件的相關問題寄送到 srfeed@microsoft.com。 <https://windowsserver.uservoice.com> 站台是慣用的設計變更要求，因為它可讓您的客戶提供支援和意見反應，對您的想法。



## <a name="related-topics"></a>相關主題  
- [儲存體複本總覽](storage-replica-overview.md) 
- [使用共用存放裝置的延展叢集複寫](stretch-cluster-replication-using-shared-storage.md)  
- [伺服器對伺服器儲存體複寫](server-to-server-storage-replication.md)  
- [叢集對叢集儲存體複寫](cluster-to-cluster-storage-replication.md)  
- [儲存體複本：已知問題](storage-replica-known-issues.md)  

## <a name="see-also"></a>另請參閱  
- [儲存體總覽](../storage.md)  
- [儲存空間直接存取](../storage-spaces/storage-spaces-direct-overview.md)  
