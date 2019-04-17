---
title: "儲存體複本的已知問題"
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 10/13/2016
ms.assetid: ceddb0fa-e800-42b6-b4c6-c06eb1d4bc55
ms.openlocfilehash: 6f02ece1f327cf53667df09e6d13ace001259885
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="known-issues-with-storage-replica"></a>儲存體複本的已知問題

>適用於：Windows Server (半年度管道)、Windows Server 2016

本章節將討論 WindowsServer 2016 中儲存體複本的已知問題。  

## <a name="after-removing-replication-disks-are-offline-and-you-cannot-configure-replication-again"></a>移除複寫之後，磁碟為離線狀態，且您無法再次設定複寫  
在 WindowsServer 2016 中，您可能無法在先前已複寫的磁碟區上佈建複寫，或者可能會發現無法掛接的磁碟區。 當某些錯誤狀況會防止移除複寫，或者當您在先前被複寫資料的電腦上重新安裝作業系統時，就可能會發生這個狀況。  

若要修正，您必須將隱藏的儲存體複本磁碟分割從磁碟上清除，並使用 `Clear-SRMetadata` Cmdlet 將它們恢復到可寫入的狀態。  

-   若要移除所有孤立的儲存體複本磁碟分割資料庫位置並重新掛接所有磁碟分割，請使用 `-AllPartitions` 參數，如下所示︰  

    ```  
    Clear-SRMetadata -AllPartitions  
    ```  

-   若要移除所有孤立的儲存體複本記錄檔資料，請使用 `-AllLogs` 參數，如下所示︰  

    ```  
    Clear-SRMetadata -AllLogs  
    ```  

-   若要移除所有孤立的容錯移轉叢集設定資料，請使用 `-AllConfiguration` 參數，如下所示︰  

    ```  
    Clear-SRMetadata -AllConfiguration  
    ```  

-   若要移除個別的複寫群組中繼資料，請使用 `-Name` 參數，並指定一個複寫群組，如下所示︰  

    ```  
    Clear-SRMetadata -Name RG01 -Logs -Partition  
    ```  

在清除磁碟分割資料庫之後，伺服器可能需要重新啟動；您可以使用 `-NoRestart` 暫時隱藏此項，但是如果 Cmdlet 要求重新啟動伺服器，您就不應該略過這個動作。 此 Cmdlet 不會移除資料磁碟區，也不會移除這些磁碟區所包含的資料。  

## <a name="during-initial-sync-see-event-log-4004-warnings"></a>初始同步處理期間，請參閱事件記錄檔 4004 警告  
在 WindowsServer 2016 中設定複寫時，來源和目的地伺服器可能會在每個初始同步處理期間顯示多個 **StorageReplica\Admin** 事件記錄檔 4004 警告，且狀態為「系統資源不足，無法完成 API」。 您可能也會看到 5014 錯誤。 這些資訊表示伺服器沒有足夠的可用記憶體 (RAM) 來同時執行初始同步處理以及執行工作負載。 請增加 RAM 或減少儲存體複本以外的功能與應用程式使用的 RAM。  

## <a name="when-using-guest-clusters-with-shared-vhdx-and-a-host-without-a-csv-virtual-machines-stop-responding-after-configuring-replication"></a>當使用包含共用 VHDX 的客體叢集與沒有 CSV 的主機時，虛擬機器會在設定複寫後停止回應  
在 WindowsServer 2016 中，針對儲存體複本測試或示範目的使用 Hyper-V 客體叢集，以及使用共用 VHDX 作為客體叢集儲存體時，虛擬機器會在您設定複寫後停止回應。 如果您重新啟動 Hyper-V 主機，虛擬機器就會開始回應，但複寫組態將不會完成，且不會發生複寫。  

當您使用 **fltmc.exe attach svhdxflt** 略過執行 CSV 之 Hyper-V 主機的要求時，就會發生這個行為。 不支援使用此命令，並僅供測試與示範使用。  

速度變慢的原因是 WindowsServer 2016 中的儲存體 QoS 功能以及手動附加的共用 VHDX 篩選器之間一個設計上的互通性問題。 若要解決此問題，請停用儲存體 QoS 篩選器驅動程式並重新啟動 Hyper-V 主機︰  

```  
SC config storqosflt start= disabled  
```  

## <a name="cannot-configure-replication-when-using-new-volume-and-differing-storage"></a>使用新磁碟區與不同的儲存體時，無法設定複寫  
當使用 `New-Volume` Cmdlet 搭配來源和目的地伺服器上不同組的儲存體 (例如兩個不同的 SAN 或使用不同磁碟的兩個 JBOD) 時，您後續可能無法使用 `New-SRPartnership` 設定複寫。 顯示的錯誤可能包括︰  

    Data partition sizes are different in those two groups  

使用 `New-Partition**` Cmdlet 而非 `New-Volume` 來建立磁碟區並將磁碟區格式化，因為第二個 Cmdlet 可能會將不同存放裝置陣列上的磁碟區大小四捨五入。 如果您已經建立 NTFS 磁碟區，您可以使用 `Resize-Partition` 來擴大或縮小其中一個磁碟區，以和另一個相符 (這無法透過 ReFS 磁碟區來完成)。 如果使用 **Diskmgmt** 或**伺服器管理員**，就不會發生進位。  

## <a name="running-test-srtopology-fails-with-name-related-errors"></a>執行發生名稱相關錯誤的 Test-SRTopology 失敗

嘗試使用 `Test-SRTopology` 時，您會收到下列其中一個錯誤：  

**錯誤範例 1：**

    WARNING: Invalid value entered for target computer name: sr-srv03. Test-SrTopology cmdlet does not accept IP address as  
    input for target computer name parameter. NetBIOS names and fully qualified domain names are acceptable inputs  
    WARNING: System.Exception  
    WARNING:    at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.BeginProcessing()  
    Test-SRTopology : Invalid value entered for target computer name: sr-srv03. Test-SrTopology cmdlet does not accept IP  
    address as input for target computer name parameter. NetBIOS names and fully qualified domain names are acceptable  
    inputs  
    At line:1 char:1  
    + Test-SRTopology -SourceComputerName sr-srv01 -SourceVolumeName d: -So ...  
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~  
        + CategoryInfo          : InvalidArgument: (:) [Test-SRTopology], Exception  
        + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand  

**錯誤範例 2：**

    WARNING: Invalid value entered for source computer name

**錯誤範例 3：**

    The specified volume cannot be found G: cannot be found on computer SRCLUSTERNODE1

此 Cmdlet 在 WindowsServer 2016 中報告錯誤的能力有限，並會針對許多常見的問題傳回相同的輸出。 可能會因為下列原因發生錯誤：  

* 您以本機使用者的身分登入來源電腦，而不是網域使用者身分。  
* 目的地電腦未執行或在網路上無法存取。  
* 您為目的地電腦指定的名稱不正確。  
* 您為目的地伺服器指定了 IP 位址。  
* 目的地電腦的防火牆封鎖了對 PowerShell 及/或 CIM 呼叫的存取。  
* 目的地電腦未執行 WMI 服務。   
* 當您從管理電腦遠端執行 `Test-SRTopology` Cmdlet 時未使用 CREDSSP。
* 指定的來源或目的地磁碟區是叢集節點上的本機磁碟，而不是叢集磁碟。  

## <a name="configuring-new-storage-replica-partnership-returns-an-error---failed-to-provision-partition"></a>設定新的儲存體複本合作關係會傳回錯誤 -「無法佈建磁碟分割」
當嘗試與 `New-SRPartnership` 建立新的複寫合作關係時，您會收到下列錯誤︰

    New-SRPartnership : Unable to create replication group test01, detailed reason: Failed to provision partition ed0dc93f-107c-4ab4-a785-afd687d3e734.
    At line: 1 char: 1
    + New-SRPartnership -SourceComputerName srv1 -SourceRGName test01
    + Categorylnfo : ObjectNotFound: (MSFT_WvrAdminTasks : root/ Microsoft/. . s) CNew-SRPartnership], CimException
    + FullyQua1ifiedErrorId : Windows System Error 1168 ,New-SRPartnership

這是因為選取和系統磁碟機位於相同磁碟分割上的資料磁碟區所造成 (例如，**C:** 磁碟機與其 Windows 資料夾)。 比方說，在同時包含從相同磁碟分割建立的 **C:** 和 **D:** 磁碟區的磁碟機上。 這在儲存體複本中不支援；您必須挑選不同的磁碟區來複寫。

## <a name="attempting-to-grow-a-replicated-volume-fails-due-to-missing-update"></a>由於遺失更新，嘗試增加複寫的磁碟區失敗
嘗試增加或延伸複寫的磁碟區時，您會收到下列錯誤︰

    PS C:\> Resize-Partition -DriveLetter d -Size 44GB
    Resize-Partition : The operation failed with return code 8
    At line:1 char:1
    + Resize-Partition -DriveLetter d -Size 44GB
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (StorageWMI:ROOT/Microsoft/.../MSFT_Partition
    [Resize-Partition], CimException
    + FullyQualifiedErrorId : StorageWMI 8,Resize-Partition

如果使用磁碟管理 MMC 嵌入式管理單元，您會收到此錯誤︰ 

    Element not found

即使您使用 `Set-SRGroup -Name rg01 -AllowVolumeResize $TRUE` 正確地在來源伺服器上啟用磁碟區調整大小，則會發生這個錯誤。 

在 Windows 10 版本 1607 的累積更新和 Windows Server 2016：2016 年 12 月 9 日 (KB3201845) 中已修正此問題。 

## <a name="attempting-to-grow-a-replicated-volume-fails-due-to-missing-step"></a>由於遺失步驟，嘗試增加複寫的磁碟區失敗
如果您沒有先設定 `-AllowResizeVolume $TRUE`，就嘗試在來源伺服器上調整複寫磁碟區大小，您將會收到下列錯誤︰

    PS C:\> Resize-Partition -DriveLetter I -Size 8GB
    Resize-Partition : Failed
    Activity ID: {87aebbd6-4f47-4621-8aa4-5328dfa6c3be}
    At line:1 char:1
    + Resize-Partition -DriveLetter I -Size 8GB
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (StorageWMI:ROOT/Microsoft/.../MSFT_Partition) [Resize-Partition], CimException
        + FullyQualifiedErrorId : StorageWMI 4,Resize-Partition

儲存體複本事件記錄檔錯誤 10307：

    Attempted to resize a partition that is protected by Storage Replica .

    DeviceName: \Device\Harddisk1\DR1
    PartitionNumber: 7
    PartitionId: {b71a79ca-0efe-4f9a-a2b9-3ed4084a1822}

    Guidance: To grow a source data partition, set the policy on the replication group containing the data partition.

    Set-SRGroup -ComputerName [ComputerName] -Name [ReplicationGroupName] -AllowVolumeResize $true

    Before you grow the source data partition, ensure that the destination data partition has enough space to grow to an equal size. Shrinking of data partition protected by Storage Replica is blocked.

磁碟管理嵌入式管理單元錯誤： 

    An unexpected error has occurred 

調整磁碟區大小之後，請記得使用 `Set-SRGroup -Name rg01 -AllowVolumeResize $FALSE` 停用調整大小。 這個參數防止系統管理員未先確定目的地磁碟區是否有充足的空間就嘗試調整磁碟區大小，通常是因為他們不知道儲存體複本的存在。 

## <a name="attempting-to-move-a-pdr-resource-between-sites-on-an-asynchronous-stretch-cluster-fails"></a>嘗試移動非同步延伸叢集上站台間的 PDR 資源失敗
嘗試移動實體磁碟資源附加角色 (例如一般用途的檔案伺服器) 以便移動非同步延伸叢集中的相關聯儲存體時，您會收到錯誤。

若為使用容錯移轉叢集管理員嵌入式管理單元：

    Error
    The operation has failed.
    The action 'Move' did not complete.
    Error Code: 0x80071398
    The operation failed because either the specified cluster node is not the owner of the group, or the node is not a possible owner of the group
    
若為使用叢集 PowerShell Cmdlet︰

    PS C:\> Move-ClusterGroup -Name sr-fs-006 -Node sr-srv07
    Move-ClusterGroup : An error occurred while moving the clustered role 'sr-fs-006'.
    The operation failed because either the specified cluster node is not the owner of the group, or the node is not a
    possible owner of the group
    At line:1 char:1
    + Move-ClusterGroup -Name sr-fs-006 -Node sr-srv07
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [Move-ClusterGroup], ClusterCmdletException
    + FullyQualifiedErrorId : Move-ClusterGroup,Microsoft.FailoverClusters.PowerShell.MoveClusterGroupCommand

這起因於 WindowsServer 2016 中內建的行為。 使用 `Set-SRPartnership` 移動非同步延伸叢集中的這些 PDR 磁碟。  

## <a name="attempting-to-add-disks-to-a-two-node-asymmetric-cluster-returns-no-disks-suitable-for-cluster-disks-found"></a>嘗試將磁碟新增至兩個節點的非對稱式叢集會傳回「找不到適用於叢集磁碟的磁碟」 
嘗試佈建僅具兩個節點的叢集時，在新增儲存體複本延展複寫前，您嘗試將第二個站台中的磁碟新增至可用的磁碟。 您會收到下列錯誤：

    "No disks suitable for cluster disks found. For diagnostic information about disks available to the cluster, use the Validate a Configuration Wizard to run Storage tests." 

如果叢集中至少有三個節點，就不會發生此錯誤。 這個問題的起因是 WindowsServer 2016 非對稱式儲存體叢集行為的內建程式碼變更。 

若要新增儲存體，您可以在第二個站台的節點上執行下列命令︰

`Get-ClusterAvailableDisk -All | Add-ClusterDisk`

這不適用於節點本機儲存體。 您可以使用 \[儲存體複本\] 在兩個總節點之間覆寫延展式叢集，**每個都使用自己的共用儲存體組**。 

## <a name="the-smb-bandwidth-limiter-fails-to-throttle-storage-replica-bandwidth"></a>SMB 頻寬限制器無法節流處理儲存體複本頻寬
指定儲存體複本的頻寬限制時，該限制會被忽略，並且使用完整頻寬。 例如：

`Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond 32MB`

因為儲存體複本與 SMB 之間的互通性問題而發生此問題。 此問題最初已在 Windows Server 2016 的 July 2017 年 7 月份累積更新以及 Windows Server 版本 1709 中修正。

## <a name="event-1241-warning-repeated-during-initial-sync"></a>在初始同步期間重複出現的事件 1241 警告
指定複寫合作關係為非同步時，來源電腦在「儲存體複本系統管理」通道中重複記錄警告事件 1241。 例如：

    Log Name:      Microsoft-Windows-StorageReplica/Admin
    Source:        Microsoft-Windows-StorageReplica
    Date:          3/21/2017 3:10:41 PM
    Event ID:      1241
    Task Category: (1)
    Level:         Warning
    Keywords:      (1)
    User:          SYSTEM
    Computer:      sr-srv05.corp.contoso.com
    Description:
    The Recovery Point Objective (RPO) of the asynchronous destination is unavailable.

    LocalReplicationGroupName: rg01
    LocalReplicationGroupId: {e20b6c68-1758-4ce4-bd3b-84a5b5ef2a87}
    LocalReplicaName: f:\
    LocalPartitionId: {27484a49-0f62-4515-8588-3755a292657f}
    ReplicaSetId: {1f6446b5-d5fd-4776-b29b-f235d97d8c63}
    RemoteReplicationGroupName: rg02
    RemoteReplicationGroupId: {7f18e5ea-53ca-4b50-989c-9ac6afb3cc81}
    TargetRPO: 30

    Guidance: This is typically due to one of the following reasons: 

非同步的目的地目前中斷連線。 RPO 可能在恢復連線後可以使用。

    The asynchronous destination is unable to keep pace with the source such that the most recent destination log record is no longer present in the source log. The destination will start block copying. The RPO should become available after block copying completes.

這是初始同步期間預期會出現的行為，可以安全地忽略。 此行為可能會在未來版本中變更。 如果您在進行中的非同步複寫期間看到此行為，請調查合作關係以判斷為何複寫延遲超過您設定的 RPO (預設 30 秒) 的原因。

## <a name="event-4004-warning-repeated-after-rebooting-a-replicated-node"></a>重新啟動複寫的節點之後重複出現事件 4004 警告
在罕見且通常無法重現的情況下，重新啟動具有合作關係的伺服器會導致複寫失敗，而重新啟動的節點記錄警告事件 4004 且出現存取被拒錯誤。

    Log Name:      Microsoft-Windows-StorageReplica/Admin
    Source:        Microsoft-Windows-StorageReplica
    Date:          3/21/2017 11:43:25 AM
    Event ID:      4004
    Task Category: (7)
    Level:         Warning
    Keywords:      (256)
    User:          SYSTEM
    Computer:      server.contoso.com
    Description:
    Failed to establish a connection to a remote computer.

    RemoteComputerName: server
    LocalReplicationGroupName: rg01
    LocalReplicationGroupId: {a386f747-bcae-40ac-9f4b-1942eb4498a0}
    RemoteReplicationGroupName: rg02
    RemoteReplicationGroupId: {a386f747-bcae-40ac-9f4b-1942eb4498a0}
    ReplicaSetId: {00000000-0000-0000-0000-000000000000}
    RemoteShareName:{a386f747-bcae-40ac-9f4b-1942eb4498a0}.{00000000-0000-0000-0000-000000000000}
    Status: {Access Denied}
    A process has requested access to an object, but has not been granted those access rights.

    Guidance: Possible causes include network failures, share creation failures for the remote replication group, or firewall settings. Make sure SMB traffic is allowed and there are no connectivity issues between the local computer and the remote computer. You should expect this event when suspending replication or removing a replication partnership.

請注意 `Status: "{Access Denied}"` 和訊息 `A process has requested access to an object, but has not been granted those access rights.`。這是儲存體複本中的已知問題，已於 2017 年 9 月 12 日品質更新中修正：KB4038782 (作業系統組建 14393.1715) https://support.microsoft.com/en-us/help/4038782/windows-10-update-kb4038782 

## <a name="error-failed-to-bring-the-resource-cluster-disk-x-online-with-a-stretch-cluster"></a>錯誤「無法使資源 'Cluster Disk x' 上線。」 出現在延展式叢集
當成功容錯移轉後嘗試使叢集磁碟上線時，您正嘗試再次讓原始來源站台成為主要，您會在 \[容錯移轉叢集管理員\] 中收到錯誤。 例如：

    Error
    The operation has failed.
    Failed to bring the resource 'Cluster Disk 2' online.
    
    Error Code: 0x80071397
    The operation failed because either the specified cluster node is not the owner of the resource, or the node is not a possible owner of the resource.
    
如果您嘗試手動移動磁碟或 CSV，您會收到額外的錯誤。 例如：

    Error
    The operation has failed.
    The action 'Move' did not complete.
    
    Error Code: 0x8007138d
    A cluster node is not available for this operation

此問題是將一個或多個未初始化的磁碟連接到一個或多個叢集節點所造成。 若要解決問題，請使用 DiskMgmt.msc、DISKPART.EXE 或 Initialize-Disk PowerShell Cmdlet 初始化所有連接的儲存體。

我們正努力提供可永久解決此問題的更新。 如果您願意協助我們並且您擁有 Microsoft 頂級支援合約，請透過 SRFEED@microsoft.com 寄送電子郵件給我們，我們才能為您提出向後修補的請求。

## <a name="gpt-error-when-attempting-to-create-a-new-sr-partnership"></a>當嘗試建立新的 SR 合作關係時出現 GPT 錯誤

執行 New-SRPartnership 時，它會失敗且出現錯誤︰ 

    Disk layout type for volume \\?\Volume{GUID}\ is not a valid GPT style layout.
    New-SRPartnership : Unable to create replication group SRG01, detailed reason: Disk layout type for volume
    \\?\Volume{GUID}\ is not a valid GPT style layout.
    At line:1 char:1
    + New-SRPartnership -SourceComputerName nodesrc01 -SourceRGName SRG01 ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (MSFT_WvrAdminTasks:root/Microsoft/...T_WvrAdminTasks) [New-SRPartnership]
    , CimException
    + FullyQualifiedErrorId : Windows System Error 5078,New-SRPartnership

在容錯移轉叢集管理員 GUI 中，沒有選項可設定磁碟的複寫。

執行 Test-SRTopology 時，它會失敗︰ 

    WARNING: Object reference not set to an instance of an object.
    WARNING: System.NullReferenceException
    WARNING:    at Microsoft.FileServices.SR.Powershell.MSFTPartition.GetPartitionInStorageNodeByAccessPath(String AccessPath, String ComputerName, MIObject StorageNode)
       at Microsoft.FileServices.SR.Powershell.Volume.GetVolume(String Path, String ComputerName)
       at Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand.BeginProcessing()
    Test-SRTopology : Object reference not set to an instance of an object.
    At line:1 char:1
    + Test-SRTopology -SourceComputerName nodesrc01 -SourceVolumeName U: - ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidArgument: (:) [Test-SRTopology], NullReferenceException
    + FullyQualifiedErrorId : TestSRTopologyFailure,Microsoft.FileServices.SR.Powershell.TestSRTopologyCommand 

這是因為叢集功能等級仍然設為 Windows Server 2012 R2 (i.e. FL 8) 所造成。 儲存體複本應該要在此傳回特定錯誤，但反而傳回不正確的錯誤對應。

在每個節點上執行 Get-Cluster | fl *。

如果 ClusterFunctionalLevel = 9，表示要在此節點實作儲存體複本需要 Windows 2016 ClusterFunctionalLevel 版本。
如果 ClusterFunctionalLevel 不是 9，ClusterFunctionalLevel 將需要進行更新，才能在此節點實作儲存體複本。

若要解決問題，請執行 PowerShell Cmdlet：Update-ClusterFunctionalLevel (https://technet.microsoft.com/itpro/powershell/windows/failoverclusters/update-clusterfunctionallevel) 來提升叢集功能等級。

## <a name="small-unknown-partition-listed-in-diskmgmt-for-each-replicated-volume"></a>每個複寫的磁碟區都有小型不明磁碟分割在 DISKMGMT 中列出

執行 [磁碟管理] 嵌入式管理單元 (DISKMGMT.MSC) 時，您會注意到有一個或多個磁碟區列出，但是未顯示任何標籤或磁碟機代號，而且大小為 1 MB。 您也許可以刪除不明磁碟區，或者可能收到訊息：

    "An Unexpected Error has Occurred"  

此行為是設計使然。 這不是磁碟區，而是磁碟分割。 儲存體複本會建立 512KB (舊版 DiskMgmt.msc 工具會四捨五入至最接近的 MB 數) 磁碟分割做為複寫作業的資料庫位置。 每個複寫的磁碟區會有像這樣的磁碟分割，是正常現象而且有其必要。 這個 512KB 磁碟分割只要不在使用中，即可隨意加以刪除，但是您不能刪除使用中的磁碟分割。 磁碟分割永遠不會擴大或縮小。 如果要重建複寫，建議您任由磁碟分割存留，因為儲存體複本自會回收未使用的磁碟分割。

若要檢視詳細資料，請使用 DISKPART 工具或 Get-Partition Cmdlet。 這些磁碟分割的 GPT 類型將會是 `558d43c5-a1ac-43c0-aac8-d1472b2923d1`。 

## <a name="see-also"></a>請參閱  
- [儲存體複本](storage-replica-overview.md)  
- [使用共用存放裝置的延展式叢集複寫](stretch-cluster-replication-using-shared-storage.md)  
- [伺服器對伺服器儲存體複寫](server-to-server-storage-replication.md)  
- [叢集對叢集儲存體複寫](cluster-to-cluster-storage-replication.md)  
- [儲存體複本：常見問題集](storage-replica-frequently-asked-questions.md)  
- [儲存空間 Direct](../storage-spaces/storage-spaces-direct-overview.md)  
