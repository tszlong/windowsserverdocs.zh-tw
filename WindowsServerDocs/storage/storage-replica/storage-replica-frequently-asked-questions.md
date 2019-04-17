---
title: "儲存體複本的常見問題集"
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 07/20/2017
ms.assetid: 12bc8e11-d63c-4aef-8129-f92324b2bf1b
ms.openlocfilehash: f29ca24a39e00f5142fe700e6abb09d1673acf7b
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="frequently-asked-questions-about-storage-replica"></a>儲存體複本的常見問題集

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題包含關於「儲存體複本」的常見問題集 (FAQ) 解答。

## <a name="FAQ1"></a>Nano 伺服器上是否支援「儲存體複本」？  
是。  

> [!NOTE]
> 您必須在安裝期間使用**儲存體** Nano 伺服器套件。 如需部署 Nano 伺服器的詳細資訊，請參閱[開始使用 Nano 伺服器](https://technet.microsoft.com/library/mt126167.aspx)。  

使用 PowerShell 遠端執行功能，在 Nano 伺服器上安裝「儲存體複本」，如下所示：  

1. 將 Nano 伺服器新增至您的用戶端信任清單。  
   > [!NOTE]
   > 只有在電腦不是 Active Directory 網域服務樹系的成員，或位於不受信任的樹系中時，才需要此步驟。 它在 PSSession 遠端執行功能中新增 NTLM 支援，但基於安全性因素預設會停用此支援。 如需詳細資訊，請參閱 [PowerShell 遠端執行功能安全性考量](https://msdn.microsoft.com/powershell/scriptiwinrmsecurity)。  

   ```  
       Set-Item WSMan:\localhost\Client\TrustedHosts "<computer name of Nano Server>"  
   ```  
2.  若要安裝「儲存體複本」功能，請從管理電腦執行下列 Cmdlet：  

    ```  
    Install-windowsfeature -Name storage-replica,RSAT-Storage-Replica -ComputerName <nano server> -Restart -IncludeManagementTools  
    ```  

    在 Windows Server 2016 中使用 `Test-SRTopology` Cmdlet 搭配 Nano 伺服器，需要含有 CredSSP 的遠端指令碼引動過程。 不像其他「儲存體複本」Cmdlet，`Test-SRTopology` 需要在來源伺服器上本機執行。   
在 Nano 伺服器上 (透過遠端 PSSession)：  

    >[!NOTE]
    >`Test-SRTopology` Cmdlet 中對於 Kerberos 雙躍點支援需要有 CREDSSP，而其他「儲存體複本」Cmdlet 則不需要，這些 Cmdlet 會自動處理分散式系統認證。 不建議在一般情況中使用 CREDSSP。 如需 CREDSSP 的的替代方案，請檢閱下列 Microsoft 部落格文章："PowerShell Remoting Kerberos Double Hop Solved Securely" - https://blogs.technet.microsoft.com/ashleymcglone/2016/08/30/powershell-remoting-kerberos-double-hop-solved-securely/ (安全解決的 PowerShell Remoting Kerberos 雙躍點) 

        Enable-WSManCredSSP -role server       

    在管理電腦上：  

         Enable-WSManCredSSP Client -DelegateComputer <remote server name>  

         $CustomCred = Get-Credential  

         Invoke-Command -ComputerName sr-srv01 -ScriptBlock { Test-SRTopology <commands> } -Authentication Credssp -Credential $CustomCred  

       接著將結果複製到您的管理電腦或共用路徑。 因為 Nano 缺少必要的圖形庫，您可以使用 Test-SRTopology 來處理結果，並提供含有圖表的報告檔案。 例如：  

        Test-SRTopology -GenerateReport -DataPath \\sr-srv05\c$\temp  


## <a name="FAQ2"></a>如何在初始同步期間查看複寫的進度？  
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

## <a name="FAQ3"></a>我是否可以指定要用於複寫的特定網路介面？  

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

## <a name="FAQ4"></a>我是否可以設定一對多的複寫或可轉移 (A 到 B 到 C) 的複寫？  
不在 Windows Server 2016 中。 這個版本只支援伺服器、叢集或延展式叢集節點的一對一複寫。 這可能會在未來版本中變更。 當然，您可以設定特定磁碟區組的各種伺服器間任一方向的複寫。 例如，伺服器 1 可以將其 D 磁碟區複寫到伺服器 2，並從伺服器 3 複寫其 E 磁碟區。

## <a name="FAQ5"></a>我是否可以增加或縮減「儲存體複本」所複寫的複寫磁碟區？  
您可以增加 (延伸) 磁碟區，而不是進行壓縮。 根據預設，儲存體複本防止系統管理員延伸複寫磁碟區。在調整大小之前，在來源群組上使用 `Set-SRGroup -AllowVolumeResize $TRUE` 選項。 例如：

1. 針對來源電腦使用： `Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. 使用任何您想要的技術增加磁碟區
3. 針對來源電腦使用： `Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE` 

## <a name="FAQ6"></a>我是否可以使目的地磁碟區上線以進行唯讀存取？  
不在 Windows Server 2016 RTM (亦即所謂的「RS1」版本) 中。 當複寫開始，儲存體複本會卸載目的地磁碟區。 

不過，現已可以在 Windows Server 版本 1709 中掛接目的地存放裝置，這項功能稱為「測試容錯移轉」。 若要這樣做，您必須有目前未於目的地複寫的未使用磁碟區或者 NTFS 或 ReFS 格式化磁碟區。 然後便可暫時掛接已複寫存放裝置的快照集以作測試或備份之用。 

例如，若要建立測試容錯移轉，藉以在目的地伺服器「SRV2」的複寫群組「RG2」中複寫磁碟區「D:」，以及在 SRV2 上安裝未經複寫的「T:」磁碟機：

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`
 
現已可在 SRV2 上存取複寫的磁碟區 D:。 您可以正常對其讀取和寫入、將檔案複製出來，或執行要儲存在其他位置妥善保管的線上備份。

若要移除測試容錯移轉快照並捨棄其變更：

 `Dismount-SRDestination -Name RG2 -Computername SRV2`
 
您只能將測試容錯移轉功能用於短期暫時作業。 此功能並不適合長期使用。 使用時，複寫會持續對實際目的地磁碟區進行。 

## <a name="FAQ7"></a>是否可以在延展式叢集中設定向外延展檔案伺服器 (SOFS)？  
儘管技術上可行，但這不是 Windows Server 2016 中的建議設定，因為連絡 SOFS 的計算節點中缺少站台感知。 如果使用校園距離的網路 (其中的延遲通常是以子毫秒為單位)，此設定一般會正確運作而不會產生問題。   

如果設定叢集對叢集複寫，在兩個叢集之間進行複寫時，「儲存體複本」可完全支援「向外延展檔案伺服器」，包括使用「儲存空間直接存取」。  

## <a name="FAQ8"></a>我是否可以在延展式叢集上使用「儲存體複本」來設定「儲存空間直接存取」？  
Windows Server 2016 中不支援此設定。  這可能會在未來版本中變更。 如果設定叢集對叢集複寫，「儲存體複本」可完全支援「向外延展檔案伺服器」和 Hyper-V 伺服器，包括使用「儲存空間直接存取」。  

## <a name="FAQ9"></a>如何設定非同步複寫？  

指定 `New-SRPartnership -ReplicationMode` 並提供引數 **Asynchronous**。 根據預設值，「儲存體複本」中的所有複寫都是同步的。 您也可以使用 `Set-SRPartnership -ReplicationMode` 來變更模式。  

## <a name="FAQ10"></a>如何防止延展式叢集進行自動容錯移轉？  
若要防止自動容錯移轉，您可以使用 PowerShell 來設定 `Get-ClusterNode -Name "NodeName").NodeWeight=0`。 這將會在災害復原網站中移除每個節點上的投票。 接著，您可以在主要網站的節點上使用 `Start-ClusterNode -PreventQuorum`，以及在災害網站的節點上使用 `Start-ClusterNode -ForceQuorum`，以強制執行容錯移轉。 沒有任何圖形化選項可防止自動容錯移轉，且不建議防止自動容錯移轉。  

## <a name="FAQ11"></a>如何停用虛擬機器復原功能？  
若要防止新的 Hyper-V 虛擬機器復原功能執行，因而暫停虛擬機器，而不是讓它們容錯移轉到災害復原站台，請執行 `(Get-Cluster).ResiliencyDefaultPeriod=0`  

## <a name="FAQ12"></a>如何減少初始同步的時間？  

您可以使用精簡佈建的儲存體做為一種方式來加速初始同步時間。 「儲存體複本」會查詢並自動使用精簡佈建的儲存體，包括非叢集儲存空間、Hyper-V 動態磁碟與 SAN LUN。  

您也可以使用植入的資料磁碟區來減少頻寬使用量，有時還可以減少時間，方法是確定目的地磁碟區含有來自主要站台的一些資料子站台 (透過還原的備份、舊的快照、先前的複寫、複製的檔案等)，然後使用「容錯移轉叢集管理員」或 `New-SRPartnership` 中的 [已植入] 選項。 如果磁碟區大部分都是空的，則使用植入的同步可能會降減少時間和頻寬使用量。

## <a name="FAQ13"></a>我是否可以委派使用者管理複寫？  

您可以使用 Windows Server 2016 中的 `Grant-SRDelegation` Cmdlet。 這可讓您在伺服器對伺服器、叢集對叢集及延展式複寫案例中設定特定使用者，就像擁有建立、修改或移除複寫的權限，而不需是本機系統管理員群組的成員。 例如：  

    Grant-SRDelegation -UserName contso\tonywang  

這個 Cmdlet 將提醒您，使用者必須登出，然後登入其正準備進行管理的伺服器，以便讓變更生效。 您可以使用 `Get-SRDelegation` 和 `Revoke-SRDelegation` 進一步控制此動作。  

## <a name="FAQ13"></a>我有哪些適用於複寫磁碟區的備份與還原選項？
「儲存體複本」支援備份及還原來源磁碟區。 它也支援建立及還原來源磁碟區的快照。 您無法在目的地磁碟區受到「儲存體複本」保護時備份或還原該磁碟區，因為它並未掛接，也無法存取。 如果您遇到來源磁碟區遺失的災害時，使用 `Set-SRPartnership` 將前一個目的地磁碟區立即升級為讀取/可寫入來源，將可讓您備份或還原該磁碟區。 您也可以使用 `Remove-SRPartnership` 和 `Remove-SRGroup` 來移除複寫，以重新掛接該磁碟區做為讀取/可寫入來源。
若要定期建立應用程式一致快照，您可以在來源伺服器上使用 VSSADMIN.EXE 以建立複寫資料磁碟區的快照。 例如，您正在其中使用「儲存體複本」來複寫 F: 磁碟區：

    vssadmin create shadow /for=F:
接著，在您切換複寫方向、移除複寫，或者就只是仍位於相同來源磁碟機之後，您可以將任何快照還原到它的時間點。 例如，仍然使用 F：

    vssadmin list shadows
     vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
您也可以使用排程的工作，定期排程此工具來執行。 如需使用 VSS 的詳細資訊，請檢閱 [Vssadmin](https://technet.microsoft.com/library/cc754968.aspx)。 備份記錄檔磁碟區時沒有任何需要或值。 嘗試這麼做時，VSS 將會加以忽略。
使用 Windows Server Backup、Microsoft Azure 備份、Microsoft DPM 或其他快照，只要 VSS、虛擬機器或以檔案為基礎的技術是在磁碟區層內運作，就受到「儲存體複本」所支援。 「儲存體複本」不支援以區塊為基礎的備份及還原。

## <a name="FAQ14"></a>我是否可以設定複寫來限制頻寬使用量？
是，可透過 SMB 頻寬限制器來設定。 這是適用於所有「儲存體複本」流量的全域設定，因此會影響所有來自此伺服器的複寫。 一般而言，只有使用「儲存體複本」初始同步設定時才需要此項，而其中的所有磁碟區資料都必須傳輸。 如果在初始同步之後需要執行此動作，您的網路頻寬對 IO 工作負載而言就會過低；請減少 IO 或增加頻寬。

這應該只能用於非同步複寫 (注意︰初始同步一律是非同步，即使您已指定同步也一樣)。
您也可以使用網路 QoS 原則，來為「儲存體複本」流量塑型。 使用高度相符的植入「儲存體複本」複寫，也會大幅降低整體初始同步的頻寬使用量。


若要設定頻寬限制，請使用：

    Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond x

若要查看頻寬限制，請使用：

    Get-SmbBandwidthLimit -Category StorageReplication

若要移除頻寬限制，請使用：

    Remove-SmbBandwidthLimit -Category StorageReplication
    
## <a name="FAQ15"></a>儲存體複本需要哪些網路連接埠？
儲存體複本依賴 SMB 和 WSMAN 的複寫和管理。 這表示需要下列連接埠：

   445 (SMB - 複寫傳輸通訊協定) 5445 (iWARP SMB - 只在 iWARP RDMA 網路時需要) 5895 (WSManHTTP - WMI/CIM/PowerShell 管理通訊協定)

注意：Test-SRTopology cmdlet 需要 ICMPv4/ICMPv6，但不適用於複寫或管理。

## <a name="FAQ15"></a>有哪些記錄檔磁碟區最佳做法？
記錄檔的最佳大小會依照環境和工作負載而有很大差別，取決於您的工作負載執行多少寫入 IO。 

1.  記錄檔放大或縮小並不會讓您變得再快或再慢一點
2.  例如，更大或更小的記錄檔對小至 10GB 的資料磁碟區和大至 10TB 的資料磁碟區都不會有任何影響。

較大的記錄檔只是在換出寫滿的資料之前收集和保留較多寫入 IO 而已。這可能會讓來源與目的地電腦之間發生的服務中斷 (例如網路斷線或目的地離線) 持續得更久。 如果記錄檔可以保留 10 小時的寫入，而網路中斷了 2 小時，當網路回復時，來源可以簡單地重新執行未同步的變更，非常迅速地將目的地的落差補足，您也很快地重新受到保護。 如果記錄檔保留 10 小時寫入而斷線了 2 天，來源現在則必須從不同的記錄檔 (稱為點陣圖) 開始重新執行，很可能較慢恢復到同步狀態。一旦進入同步狀態，就會回復使用原本的記錄檔。

會有 SR 效能計數器告訴您記錄檔的變換速率，讓您可以做一些判斷。 此外，Test-SRTopology Cmdlet 也有同樣的作用。 您基本上只是查看現有工作負載的寫入 IO，來決定每分鐘、每小時或每天有多少 IO 可能在記錄檔流動。

儲存體複本完全仰賴記錄檔發揮寫入效能。 記錄檔效能對複寫效能非常重要。 因為記錄檔會序列化並循序化所有的寫入 IO，您必須確保記錄磁碟區的效能比資料磁碟區的效能還要好。 記錄磁碟區一定要使用像 SSD 這樣的快閃媒體。 絕不允許任何其他工作負載在記錄檔磁碟區上執行，同樣也不允許其他工作負載在 SQL 資料庫記錄檔磁碟區上執行。 

再說一次：Microsoft 強烈建議記錄檔儲存體必須比資料儲存體更快速，而且記錄磁碟區絕不能用於其他工作負載。

## <a name="FAQ16"></a>您選擇延展式叢集、叢集對叢集或伺服器對伺服器拓撲會有不同原因，差異何在？  
儲存體複本有三個主要設定：延展式叢集、叢集對叢集和伺服器對伺服器。 各有不同的優點。

延展式叢集拓撲非常適合需要可透過協調流程自動容錯移轉的工作負載，例如 Hyper-V 私人雲端叢集和 SQL Server FCI。 其中也有使用 [容錯移轉叢集管理員] 的內建圖形介面。 透過持續保留使用 [儲存空間]、SAN、iSCSI 及 RAID 的傳統非對稱式叢集共用存放架構。 您只使用 2 個節點就可以執行這種拓撲。

叢集對叢集拓撲使用兩個不同的叢集，非常適合需要手動容錯移轉的系統管理員，尤其是在佈建第二網站作災害復原之用，而非日常使用時。 協調流程為手動。 和延展式叢集不同的是，此設定可以使用儲存空間直接存取。 您只使用 4 個節點就可以執行這種拓撲。 

伺服器對伺服器拓撲非常適合執行無法納入叢集之硬體的客戶。 這需要手動容錯移轉及協調流程。 適用於分公司與中央資料中心之間的低成本部署，尤其是在使用非同步複寫時。 此設定通常可以取代用於單一主機災害復原案例之受 DFSR 保護的檔案伺服器執行個體。

在所有情況下，這些拓撲都支援在實體硬體及虛擬機器上執行。 在虛擬機器時，基礎 Hypervisor 不需要 Hyper-V；可以是 VMWare、KVM、Xen 等。

儲存複本也有伺服器對自我模式，您在此模式下將複寫指向同一台電腦上的兩個不同磁碟區。

## <a name="FAQ17"></a>我該如何回報有關「儲存體複本」或本指南的問題？  
如需「儲存體複本」的技術協助，您可以張貼於 [Microsoft TechNet 論壇](https://social.technet.microsoft.com/Forums/windowsserver/en-US/home?forum=WinServerPreview)。 您也可以透過電子郵件，將「儲存體複本」相關問題或與此文件的相關問題寄送到 srfeed@microsoft.com。 偏好使用 https://windowsserver.uservoice.com 網站來了解設計變更要求，因為它可讓您的客戶對您的想法提供支援和意見。



## <a name="related-topics"></a>[相關主題]  
- [儲存體複本概觀](storage-replica-overview.md) 
- [使用共用存放裝置的延展式叢集複寫](stretch-cluster-replication-using-shared-storage.md)  
- [伺服器對伺服器儲存體複寫](server-to-server-storage-replication.md)  
- [叢集對叢集儲存體複寫](cluster-to-cluster-storage-replication.md)  
- [儲存體複本：已知問題](storage-replica-known-issues.md)  

## <a name="see-also"></a>另請參閱  
- [儲存體總覽](../storage.md)  
- [Windows Server 2016 中的儲存空間直接存取](../storage-spaces/storage-spaces-direct-overview.md)  
