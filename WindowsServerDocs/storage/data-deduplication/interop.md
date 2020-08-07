---
ms.assetid: 60fca6b2-f1c0-451f-858f-2f6ab350d220
title: 重複資料刪除互通性
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/16/2016
ms.openlocfilehash: 7f99b4d12821e505a229ac02d0198a9ac2ed31fa
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87936303"
---
# <a name="data-deduplication-interoperability"></a>重複資料刪除互通性

> 適用于： Windows Server (半年通道) 、Windows Server 2016、Windows Server 2019

## <a name="supported"></a>支援

### <a name="refs"></a>ReFS
從 Windows Server 2019，支援重復資料刪除。

### <a name="failover-clustering"></a>容錯移轉叢集

完全支援[容錯移轉叢集](../..//failover-clustering/failover-clustering-overview.md)，並假設叢集中的每個節點都已[安裝重複資料刪除功能](install-enable.md#install-dedup)。 其他重要注意事項：

* [手動啟動重複資料刪除工作](run.md#running-dedup-jobs-manually)必須在「叢集共用磁碟區」的「擁有者」節點上執行。
* 排定的重複資料刪除工作會儲存於排定的叢集工作中，因此，如果重複資料刪除磁碟區已由另一個節點接手，則排定的工作將會在下次排定的間隔執行。
* 重複資料刪除功能可完全與[叢集作業系統輪流升級](../..//failover-clustering/cluster-operating-system-rolling-upgrade.md)功能互相操作。
* [儲存空間直接存取](../storage-spaces/storage-spaces-direct-overview.md) NTFS 格式化的磁碟區 (鏡像或同位) 上完全支援重複資料刪除功能。 具有多層的磁碟區不支援刪除重複資料。 如需詳細資訊，請參閱 [ReFS 上的重複資料刪除](#unsupported)。

### <a name="storage-replica"></a>儲存體複本
完整支援[儲存體複本](../storage-replica/storage-replica-overview.md)。 「重複資料刪除」應該設定為不會在次要複本上執行。

### <a name="branchcache"></a>BranchCache
您可以藉由在伺服器與用戶端上啟用 [BranchCache](../../networking/branchcache/branchcache.md)，將透過網路存取資料的程序最佳化。 當已啟用 BranchCache 的系統透過 WAN 與執行重複資料刪除的遠端檔案伺服器通訊時，就已經編製所有刪除重複資料之檔案的索引並進行雜湊。 因此，會快速地計算從分公司存取資料的要求。 這類似於替已啟用 BranchCache 的伺服器預先編製索引或預先雜湊處理。

### <a name="dfs-replication"></a>DFS 複寫
重複資料刪除可與分散式檔案系統 (DFS) 複寫搭配使用。 將檔案最佳化或未最佳化不會觸發複寫，因為檔案不會變更。 DFS 複寫使用遠端差異壓縮 (RDC) (而非使用區塊存放區中的區塊) 來進行有線網路節約。 如果複本使用重複資料刪除，也可以使用重複資料刪除將複本上的檔案最佳化。

### <a name="quotas"></a>配額
重複資料刪除不支援在也會啟用重複資料刪除的磁碟區根資料夾上建立固定配額。 磁碟區根目錄上有固定配額時，磁碟區的實際可用空間與磁碟區的配額限制空間將會不同。 這可能會導致重複資料刪除最佳化工作失敗。 但是，很可能會在已啟用重複資料刪除的磁碟區根目錄上建立彈性配額。

在重複資料刪除磁碟區上啟用配額時，配額會使用檔案的邏輯大小，而不是檔案的實際大小。 重複資料刪除工作處理檔案時，不會變更配額使用量 (包含任何配額閾值)。 使用重複資料刪除時，所有其他配額功能 (包含磁碟區根目錄的彈性配額和子資料夾上的配額) 都能正常運作。

### <a name="windows-server-backup"></a>Windows Server Backup
Windows Server Backup 可以依原樣備份最佳化的磁碟區 (亦即，不需移除已經過重複資料刪除的資料)。 下列步驟示範如何備份磁碟區，以及如何還原磁碟區或磁碟區中選取的檔案。
1. 安裝 Windows Server Backup。
    ```PowerShell
    Install-WindowsFeature -Name Windows-Server-Backup
    ```

2. 執行下列命令將 E: 磁碟區備份至另一個磁碟區，並針對您的情況替代正確的磁碟區名稱。
    ```PowerShell
    wbadmin start backup –include:E: -backuptarget:F: -quiet
    ```
3. 取得您剛建立之備份的版本識別碼。

    ```PowerShell
    wbadmin get versions
    ```

    此輸出版本識別碼會是一個日期和時間字串，例如︰08/18/2016-06:22。

4. 還原整個磁碟區。
    ```PowerShell
    wbadmin start recovery –version:02/16/2012-06:22 -itemtype:Volume  -items:E: -recoveryTarget:E:
    ```

    **--或--**

    還原特定資料夾 (在此案例中為 E:\Docs 資料夾)：
    ```PowerShell
    wbadmin start recovery –version:02/16/2012-06:22 -itemtype:File  -items:E:\Docs  -recursive
    ```

## <a name="unsupported"></a>不支援

### <a name="windows-10-client-os"></a>Windows 10 (用戶端作業系統)
Windows 10 不支援重複資料刪除。 Windows 社群中有數篇熱門部落格文章說明如何將二進位檔從 Windows Server 2016 移除並安裝在 Windows 10，但是這個案例在「重複資料刪除」的開發過程中還未經驗證。 [在 Windows Server Storage UserVoice 上投票給 Windows 10 vNext 的這個項目](https://windowsserver.uservoice.com/forums/295056-storage/suggestions/9011008-add-deduplication-support-to-client-os)。

### <a name="windows-search"></a>Windows 搜尋
Windows 搜尋不支援重複資料刪除。 重複資料刪除會使用重新分析點，而使得 Windows 搜尋無法編製索引，因此 Windows 搜尋會略過所有重複資料刪除的檔案，並將之排除在索引內容之外。 因此，對重複資料刪除磁碟區的搜尋結果可能不完整。 [在 Windows Server Storage UserVoice 上投票給 Windows Server vNext 的這個項目](https://windowsserver.uservoice.com/forums/295056-storage/suggestions/17888647-make-windows-search-service-work-with-data-dedupli)。

### <a name="robocopy"></a>Robocopy
不建議搭配重複資料刪除功能執行 Robocopy，因為某些 Robocopy 命令可能會造成「區塊存放區」損毀。 區塊存放區會儲存於磁碟區的 [System Volume Information] 資料夾中。 如果刪除該資料夾，則從來源磁碟區複製的最佳化檔案 (重新分析點) 都會變成損毀，因為資料區塊不會複製到目的地磁碟區。
