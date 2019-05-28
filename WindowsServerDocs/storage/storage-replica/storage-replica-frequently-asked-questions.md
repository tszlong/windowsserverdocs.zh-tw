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
ms.openlocfilehash: e832dce3eed7d0e5103254fb48683726b82af2e6
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475941"
---
# <a name="frequently-asked-questions-about-storage-replica"></a>儲存體複本的常見問題集

>適用於：Windows Server 2019，Windows Server 2016 中，Windows Server （半年通道）

本主題包含關於「儲存體複本」的常見問題集 (FAQ) 解答。

## <a name="FAQ1"></a> Azure 上支援的儲存體複本？
是的。 您可以搭配 Azure 使用下列案例：

1. 在 Azure 內的伺服器對伺服器複寫 （同步或非同步之間在一或兩個資料中心容錯網域，IaaS Vm 或以非同步方式兩個不同區域之間）
2. Azure 與內部部署之間的伺服器對伺服器非同步複寫 （使用 VPN 或 Azure ExpressRoute）
3. 在 Azure 內的叢集對叢集複寫 （同步或非同步之間在一或兩個資料中心容錯網域，IaaS Vm 或以非同步方式兩個不同區域之間）
4. 叢集對叢集 Azure 與內部部署之間的非同步複寫 （使用 VPN 或 Azure ExpressRoute）

在 Azure 中的客體叢集上的進一步資訊，參閱：[部署 Microsoft Azure 中的 IaaS VM 客體叢集](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126)。

重要注意事項：

1. Azure 不支援共用的 VHDX 客體叢集，因此 Windows 容錯移轉叢集虛擬機器必須使用傳統的共用儲存體永續性磁碟保留叢集或儲存空間直接存取的 iSCSI 目標。
2. Azure Resource Manager 範本在叢集的儲存空間直接存取基礎儲存體複本[跨 Azure 區域的災害復原建立儲存空間直接存取 SOFS 叢集與儲存體複本](https://aka.ms/azure-storage-replica-cluster)。  
3. 叢集 （叢集 Api 的叢集之間的存取權授與必要） 的 Azure 中的 RPC 通訊的叢集需要設定網路存取 cno。 TCP 連接埠 49152 上方，您必須允許 TCP 連接埠 135 和動態的範圍。 參考[建置 Windows Server 容錯移轉叢集在 Azure IAAS VM – 第 2 個網路與建立](https://blogs.technet.microsoft.com/askcore/2015/06/24/building-windows-server-failover-cluster-on-azure-iaas-vm-part-2-network-and-creation/)。  
4. 可以使用雙節點客體叢集，其中每個節點時，適用於非對稱式叢集複寫的儲存體複本使用回送 iSCSI。 但這可能會有很差的效能，且應僅適用於非常有限的工作負載或測試。  

## <a name="FAQ2"></a> 如何查看複寫初始同步處理期間的進度？  
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

## <a name="FAQ3"></a>指定要用於複寫的特定網路介面？  

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

    Set-SRNetworkConstraint -SourceComputerName sr-srv01 -SourceRGName group1 -SourceNWInterface "Cluster Network 1","Cluster Network 2" -DestinationComputerName sr-srv03 -DestinationRGName group2 -DestinationNWInterface "Cluster Network 1","Cluster Network 2"  

## <a name="FAQ4"></a> 可以設定一對多的複寫或可轉移 (A 到 B 到 C) 複寫嗎？  
否，儲存體複本支援伺服器、 叢集或延展式叢集節點的只有一對一的複寫。 這可能會在未來版本中變更。 當然，您可以設定特定磁碟區組的各種伺服器間任一方向的複寫。 例如，伺服器 1 可以將其 D 磁碟區複寫到伺服器 2，並從伺服器 3 複寫其 E 磁碟區。

## <a name="FAQ5"></a> 我是否可以擴大或縮小複寫所複寫的儲存體複本的磁碟區？  
您可以增加 (延伸) 磁碟區，而不是進行壓縮。 根據預設，儲存體複本防止系統管理員延伸複寫磁碟區。在調整大小之前，在來源群組上使用 `Set-SRGroup -AllowVolumeResize $TRUE` 選項。 例如: 

1. 使用對來源電腦： `Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. 使用任何您想要的技術增加磁碟區
3. 使用對來源電腦： `Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE` 

## <a name="FAQ6"></a>可以將目的地磁碟區上線的唯讀存取權嗎？  
不在 Windows Server 2016 中。 當複寫開始，儲存體複本會卸載目的地磁碟區。 

不過，在 Windows Server 2019 和 Windows Server 半年通道版起，1709，掛接的目的地儲存體選項，就可以使用-這項功能稱為 「 測試容錯移轉 」。 若要這樣做，您必須有目前未於目的地複寫的未使用磁碟區或者 NTFS 或 ReFS 格式化磁碟區。 然後便可暫時掛接已複寫存放裝置的快照集以作測試或備份之用。 

例如，若要建立測試容錯移轉，藉以在目的地伺服器「SRV2」的複寫群組「RG2」中複寫磁碟區「D:」，以及在 SRV2 上安裝未經複寫的「T:」磁碟機：

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`
 
現已可在 SRV2 上存取複寫的磁碟區 D:。 您可以讀取和寫入它正常、 檔案複製到外，或執行您儲存在其他地方以利妥善保存，d： 路徑下的線上備份。 T： 磁碟區只會包含記錄資料。

若要移除測試容錯移轉快照並捨棄其變更：

 `Dismount-SRDestination -Name RG2 -Computername SRV2`
 
您只能將測試容錯移轉功能用於短期暫時作業。 此功能並不適合長期使用。 使用時，複寫會持續對實際目的地磁碟區進行。 

## <a name="FAQ7"></a> 我可以延展式叢集中設定向外延展檔案伺服器 (SOFS) 嗎？  
儘管技術上可行，但這不是建議的設定，因為連絡 SOFS 的計算節點中的站台感知缺少。 如果使用校園距離的網路，其中的延遲通常以子毫秒，這項設定通常適用於沒有任何問題。   

如果設定叢集對叢集複寫，在兩個叢集之間進行複寫時，「儲存體複本」可完全支援「向外延展檔案伺服器」，包括使用「儲存空間直接存取」。  

## <a name="FAQ7.5"></a> CSV，才可將延展式叢集中，或叢集之間複寫？  
資料分割 您可以使用 CSV 或叢集資源，例如檔案伺服器角色所擁有的永續性磁碟保留 (PDR) 進行複寫。 

如果設定叢集對叢集複寫，在兩個叢集之間進行複寫時，「儲存體複本」可完全支援「向外延展檔案伺服器」，包括使用「儲存空間直接存取」。  

## <a name="FAQ8"></a>設定儲存空間直接存取與儲存體複本延展式叢集中？  
這不是 Windows Server 中支援的設定。 這可能會在未來版本中變更。 如果設定叢集對叢集複寫，「儲存體複本」可完全支援「向外延展檔案伺服器」和 Hyper-V 伺服器，包括使用「儲存空間直接存取」。  

## <a name="FAQ9"></a>如何設定非同步複寫？  

指定 `New-SRPartnership -ReplicationMode` 並提供引數 **Asynchronous**。 根據預設值，「儲存體複本」中的所有複寫都是同步的。 您也可以使用 `Set-SRPartnership -ReplicationMode` 來變更模式。  

## <a name="FAQ10"></a>如何防止延展式叢集的自動容錯移轉？  
若要防止自動容錯移轉，您可以使用 PowerShell 來設定 `Get-ClusterNode -Name "NodeName").NodeWeight=0`。 這將會在災害復原網站中移除每個節點上的投票。 接著，您可以在主要網站的節點上使用 `Start-ClusterNode -PreventQuorum`，以及在災害網站的節點上使用 `Start-ClusterNode -ForceQuorum`，以強制執行容錯移轉。 沒有任何圖形化選項可防止自動容錯移轉，且不建議防止自動容錯移轉。  

## <a name="FAQ11"></a>如何停用虛擬機器復原功能？
若要避免執行，因而暫停虛擬機器，而不是災害復原站台到將進行容錯移轉從新的 HYPER-V 虛擬機器復原功能，請執行 `(Get-Cluster).ResiliencyDefaultPeriod=0`  

## <a name="FAQ12"></a> 如何減少初始同步處理時間？

您可以使用精簡佈建的儲存體做為一種方式來加速初始同步時間。 「儲存體複本」會查詢並自動使用精簡佈建的儲存體，包括非叢集儲存空間、Hyper-V 動態磁碟與 SAN LUN。  

您可以也使用植入的資料磁碟區來減少頻寬使用量和有時時間，確定目的地磁碟區會有一些資料，然後使用 [已植入] 選項在 「 容錯移轉叢集管理員 」 中的主要或`New-SRPartnership`。 如果磁碟區大部分都是空的，則使用植入的同步可能會降減少時間和頻寬使用量。 有多種方式來識別值種子資料，有不同程度的效率：

1. 先前的複寫-藉由使用一般的初始複寫進行同步處理在本機的磁碟和磁碟區、 移除複寫，在含有寄送目的地磁碟的節點之間其他位置，然後新增植入的選項與複寫。 這會是最有效方法，是因為儲存體複本保證區塊複製鏡像和複寫的作法就是差異阻止。
2. 還原快照集或快照集為基礎的還原備份-還原到目的地磁碟區，磁碟區為基礎的快照集應該有區塊配置的最小差異。 這會是下一個最有效方法，因為區塊都可能由於正在鏡像映像的磁碟區快照集比對。
3. 複製的檔案-從來未曾使用過在目的地上建立新的磁碟區，然後執行完整的 robocopy /MIR 樹狀目錄中複製的資料，有可能是區塊相符項目。 使用 Windows 檔案總管或複製樹狀結構的某些部分將不會建立多區塊相符項目。 手動複製這些檔案是最有效的方法，來植入。

## <a name="FAQ13"></a> 我是否可以委派使用者管理複寫？  

您可以使用`Grant-SRDelegation`cmdlet。 這可讓您在伺服器對伺服器、叢集對叢集及延展式複寫案例中設定特定使用者，就像擁有建立、修改或移除複寫的權限，而不需是本機系統管理員群組的成員。 例如:   

    Grant-SRDelegation -UserName contso\tonywang  

這個 Cmdlet 將提醒您，使用者必須登出，然後登入其正準備進行管理的伺服器，以便讓變更生效。 您可以使用 `Get-SRDelegation` 和 `Revoke-SRDelegation` 進一步控制此動作。  

## <a name="FAQ13"></a> 什麼是我的備份及還原複寫的磁碟區的選項？
「儲存體複本」支援備份及還原來源磁碟區。 它也支援建立及還原來源磁碟區的快照。 您無法在目的地磁碟區受到「儲存體複本」保護時備份或還原該磁碟區，因為它並未掛接，也無法存取。 如果您遇到來源磁碟區遺失的災害時，使用 `Set-SRPartnership` 將前一個目的地磁碟區立即升級為讀取/可寫入來源，將可讓您備份或還原該磁碟區。 您也可以使用 `Remove-SRPartnership` 和 `Remove-SRGroup` 來移除複寫，以重新掛接該磁碟區做為讀取/可寫入來源。
若要定期建立應用程式一致快照，您可以在來源伺服器上使用 VSSADMIN.EXE 以建立複寫資料磁碟區的快照。 例如，您正在其中使用「儲存體複本」來複寫 F: 磁碟區：

    vssadmin create shadow /for=F:
接著，在您切換複寫方向、移除複寫，或者就只是仍位於相同來源磁碟機之後，您可以將任何快照還原到它的時間點。 例如，仍然使用 F：

    vssadmin list shadows
     vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
您也可以使用排程的工作，定期排程此工具來執行。 如需使用 VSS 的詳細資訊，請檢閱 [Vssadmin](../../administration/windows-commands/vssadmin.md)。 備份記錄檔磁碟區時沒有任何需要或值。 嘗試這麼做時，VSS 將會加以忽略。
使用 Windows Server Backup、Microsoft Azure 備份、Microsoft DPM 或其他快照，只要 VSS、虛擬機器或以檔案為基礎的技術是在磁碟區層內運作，就受到「儲存體複本」所支援。 「儲存體複本」不支援以區塊為基礎的備份及還原。

## <a name="FAQ14"></a> 設定複寫來限制頻寬使用量？
是，可透過 SMB 頻寬限制器來設定。 這是適用於所有「儲存體複本」流量的全域設定，因此會影響所有來自此伺服器的複寫。 一般而言，只有使用「儲存體複本」初始同步設定時才需要此項，而其中的所有磁碟區資料都必須傳輸。 如果在初始同步之後需要執行此動作，您的網路頻寬對 IO 工作負載而言就會過低；請減少 IO 或增加頻寬。

這應該只能用於非同步複寫 (注意︰初始同步一律是非同步，即使您已指定同步也一樣)。
您也可以使用網路 QoS 原則，來為 「儲存體複本」流量塑型。 使用高度相符的植入「儲存體複本」複寫，也會大幅降低整體初始同步的頻寬使用量。


若要設定頻寬限制，請使用：

    Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond x

若要查看頻寬限制，請使用：

    Get-SmbBandwidthLimit -Category StorageReplication

若要移除頻寬限制，請使用：

    Remove-SmbBandwidthLimit -Category StorageReplication
    
## <a name="FAQ15"></a>儲存體複本會需要哪些網路連接埠？
儲存體複本依賴 SMB 和 WSMAN 的複寫和管理。 這表示需要下列連接埠：

 445 (SMB-複寫的傳輸通訊協定、 叢集 RPC 管理通訊協定) 5445 (iWARP SMB-使用 iWARP RDMA 網路功能時，才需要) 5985 (WSManHTTP-適用於 WMI/CIM/PowerShell 管理通訊協定)

注意:Test-srtopology cmdlet 需要 ICMPv4/ICMPv6，但不是能用於複寫或管理。

## <a name="FAQ15.5"></a>記錄磁碟區的最佳作法有哪些？
記錄檔的最佳大小差異極大，每個環境和工作負載，而決定多少撰寫您的工作負載執行的 IO。 

1.  記錄檔放大或縮小並不會讓您變得再快或再慢一點
2.  例如，更大或更小的記錄檔對小至 10GB 的資料磁碟區和大至 10TB 的資料磁碟區都不會有任何影響。

較大的記錄檔只是在換出寫滿的資料之前收集和保留較多寫入 IO 而已。這可能會讓來源與目的地電腦之間發生的服務中斷 (例如網路斷線或目的地離線) 持續得更久。 如果記錄檔可以保留 10 小時的寫入，而網路中斷了 2 小時，當網路回復時，來源可以簡單地重新執行未同步的變更，非常迅速地將目的地的落差補足，您也很快地重新受到保護。 如果記錄檔保留 10 小時寫入而斷線了 2 天，來源現在則必須從不同的記錄檔 (稱為點陣圖) 開始重新執行，很可能較慢恢復到同步狀態。一旦進入同步狀態，就會回復使用原本的記錄檔。

儲存體複本完全仰賴記錄檔發揮寫入效能。 記錄檔效能對複寫效能非常重要。 因為記錄檔會序列化並循序化所有的寫入 IO，您必須確保記錄磁碟區的效能比資料磁碟區的效能還要好。 記錄磁碟區一定要使用像 SSD 這樣的快閃媒體。 絕不允許任何其他工作負載在記錄檔磁碟區上執行，同樣也不允許其他工作負載在 SQL 資料庫記錄檔磁碟區上執行。 

一次：Microsoft 強烈建議記錄儲存體是較快的資料存放區和記錄磁碟區必須永遠不會使用針對其他工作負載。

您可以藉由執行 Test-srtopology 工具取得記錄檔的大小調整建議。 或者，您可以使用現有的伺服器上的效能計數器，讓記錄檔大小判斷。 公式很簡單： 監視工作負載下的資料磁碟輸送量 （平均寫入位元組/秒），並使用它來計算的填滿記錄檔的大小不同的時間量。 比方說，資料磁碟輸送量為 50 MB/s 會導致將包裝在 120 GB/50 MB 秒或 2400 秒或 40 分鐘 120 GB 的記錄檔。 因此在目的地伺服器可能無法連線到包裝的記錄檔之前量是時間的 40 分鐘。 如果記錄檔換行，但再次變成可連線的目的地，來源就會重新執行透過元對應記錄，而不是主要的記錄檔區塊。 記錄檔的大小沒有影響效能。

只從來源叢集的資料磁碟應該是備份。 儲存體複本記錄檔磁碟應該不會備份因為儲存體複本作業的備份可能會發生衝突。

## <a name="FAQ16"></a> 為什麼要選擇與叢集對叢集伺服器對伺服器拓撲與延展式叢集？  
儲存體複本共有三種主要的設定： 自動縮放叢集、 叢集對叢集和伺服器對伺服器。 各有不同的優點。

延展式叢集拓撲非常適合需要可透過協調流程自動容錯移轉的工作負載，例如 Hyper-V 私人雲端叢集和 SQL Server FCI。 其中也有使用 [容錯移轉叢集管理員] 的內建圖形介面。 透過持續保留使用 [儲存空間]、SAN、iSCSI 及 RAID 的傳統非對稱式叢集共用存放架構。 您只使用 2 個節點就可以執行這種拓撲。

叢集對叢集拓撲使用兩個不同的叢集，非常適合需要手動容錯移轉的系統管理員，尤其是在佈建第二網站作災害復原之用，而非日常使用時。 協調流程為手動。 延展式叢集，與儲存空間直接存取可以用於此組態中 （含警告-請參閱儲存體複本常見問題集和叢集對叢集文件）。 您只使用 4 個節點就可以執行這種拓撲。 

伺服器對伺服器拓撲非常適合執行無法納入叢集之硬體的客戶。 這需要手動容錯移轉及協調流程。 它相當適合分公司與中央資料中心之間的低成本部署的尤其是使用非同步複寫。 此設定通常可以取代用於單一主機災害復原案例之受 DFSR 保護的檔案伺服器執行個體。

在所有情況下，這些拓撲都支援在實體硬體及虛擬機器上執行。 在虛擬機器時，基礎 Hypervisor 不需要 Hyper-V；可以是 VMWare、KVM、Xen 等。

儲存複本也有伺服器對自我模式，您在此模式下將複寫指向同一台電腦上的兩個不同磁碟區。

## <a name="FAQ18"></a> 使用儲存體複本支援的重複資料刪除？

是，資料 Deduplcation 支援與儲存體複本。 在來源伺服器上的磁碟區上啟用重複資料刪除，並在複寫期間，目的地伺服器接收重複資料刪除磁碟區的複本。

雖然您應該*安裝*重複資料刪除，來源和目的地伺服器上 (請參閱[安裝和啟用重複資料刪除](../data-deduplication/install-enable.md))，不重要到*啟用*重複資料刪除，在目的地伺服器上。 儲存體複本可讓您只能在來源伺服器上的寫入。 因為重複資料刪除會寫入至磁碟區，應該只能在來源伺服器上執行。 

## <a name="FAQ19"></a> 我可以複寫 Windows Server 2019 與 Windows Server 2016？

不幸的是，我們不支援建立*新*Windows Server 2019 和 Windows Server 2016 資料庫之間的關係。 您可以放心地升級的伺服器或叢集到 Windows Server 2019 以及任何執行 Windows Server 2016*現有*合作關係會繼續運作。

不過，若要取得 Windows Server 2019 的提升的複寫效能，此合作關係中的所有成員必須都執行 Windows Server 2019 時，必須刪除現有的合作關係和相關聯的複寫群組，然後重新建立它們，以植入的資料 （無論是當建立合作關係中 Windows Admin Center 或新增 SRPartnership cmdlet）。

## <a name="FAQ17"></a> 如何報告與儲存體複本 」 或 「 本指南的問題？  
如需「儲存複本」的技術協助，您可以張貼於 [Microsoft TechNet 論壇](https://social.technet.microsoft.com/Forums/windowsserver/en-US/home?forum=WinServerPreview)。 您也可以透過電子郵件，將「儲存體複本」相關問題或與此文件的相關問題寄送到 srfeed@microsoft.com。  https://windowsserver.uservoice.com 站台是慣用的設計變更要求，因為它可讓您的客戶提供支援和意見反應，對您的想法。



## <a name="related-topics"></a>相關主題  
- [儲存體複本概觀](storage-replica-overview.md) 
- [使用共用存放裝置的延展式叢集複寫](stretch-cluster-replication-using-shared-storage.md)  
- [伺服器對伺服器儲存體複寫](server-to-server-storage-replication.md)  
- [叢集對叢集儲存體複寫](cluster-to-cluster-storage-replication.md)  
- [儲存體複本：已知的問題](storage-replica-known-issues.md)  

## <a name="see-also"></a>另請參閱  
- [儲存體概觀](../storage.md)  
- [儲存空間直接存取](../storage-spaces/storage-spaces-direct-overview.md)  
