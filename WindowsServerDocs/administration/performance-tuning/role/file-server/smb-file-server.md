---
title: SMB 檔案伺服器的效能微調
description: SMB 檔案伺服器的效能微調
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: NedPyle; Danlo; DKruse
ms.date: 4/14/2017
ms.openlocfilehash: 5383d16ac4c98651aa6afe996dbad88a6d60ee7a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370232"
---
# <a name="performance-tuning-for-smb-file-servers"></a>SMB 檔案伺服器的效能微調

## <a name="smb-configuration-considerations"></a>SMB 設定考慮
請勿啟用檔案伺服器和用戶端不需要的任何服務或功能。 這些可能包括 SMB 簽署、用戶端快取、檔案系統迷你篩選、搜尋服務、排定的工作、NTFS 加密、NTFS 壓縮、IPSEC、防火牆篩選器、Teredo 和 SMB 加密。

請確定已視需要設定 BIOS 和作業系統電源管理模式，其中可能包括高效能模式或改變的 C 狀態。 請確定已安裝最新、最具復原能力和最快速的儲存體和網路設備磁碟機。

複製檔案是在檔案伺服器上執行的常見操作。 Windows Server 有數個內建的檔案複製公用程式，您可以使用命令提示字元來執行。 建議使用 Robocopy。 在 Windows Server 2008 R2 中引進，Robocopy 的 **/mt**選項在複製多個小型檔案時，可以使用多個執行緒大幅提升遠端檔案傳輸的速度。 我們也建議使用 **/log**選項，將記錄檔重新導向至 NUL 裝置或檔案，以減少主控台輸出。 當您使用 Xcopy 時，建議您將 **/q**和 **/k**選項新增至現有的參數。 先前的選項會藉由減少主控台輸出來降低 CPU 負擔，後者會減少網路流量。

## <a name="smb-performance-tuning"></a>SMB 效能微調


檔案伺服器效能和可用的 tunings 取決於每個用戶端和伺服器之間，以及已部署的檔案伺服器功能上所協商的 SMB 通訊協定。 目前可用的最高通訊協定版本是 Windows Server 2016 和 Windows 10 中的 SMB 3.1.1。 您可以在用戶端上使用 Windows PowerShell **SMBConnection**來檢查您的網路上使用的 SMB 版本，並**取得 SMBSession |** 伺服器上的 FL。

### <a name="smb-30-protocol-family"></a>SMB 3.0 通訊協定系列

SMB 3.0 是在 Windows Server 2012 中引進，並在 Windows Server 2012 R2 （SMB 3.02）和 Windows Server 2016 （SMB 3.1.1）中進一步增強。 此版本引進的技術可大幅提升檔案伺服器的效能和可用性。 如需詳細資訊，請參閱[Windows Server 2012 中的 SMB 和 2012 R2 2012](https://aka.ms/smb3plus)和[smb 3.1.1 的新功能](https://aka.ms/smb311)。



### <a name="smb-direct"></a>SMB 直接傳輸

SMB 直接傳輸引進了使用 RDMA 網路介面的功能，可提供低延遲和低 CPU 使用率的高輸送量。

當 SMB 偵測到支援 RDMA 的網路時，它會自動嘗試使用 RDMA 功能。 不過，如果 SMB 用戶端因任何原因而無法使用 RDMA 路徑進行連線，則只會繼續使用 TCP/IP 連接。 所有與 SMB 直接傳輸相容的 RDMA 介面都必須同時執行 TCP/IP 堆疊，而 SMB 多重通道則知道這一點。

Smb 直接傳輸在任何 SMB 設定中都不是必要的，但對於需要較低延遲和較低 CPU 使用率的使用者，則一律建議使用此方式。

如需 SMB 直接傳輸的詳細資訊，請參閱[使用 Smb 直接傳輸改善檔案伺服器的效能](https://aka.ms/smbdirect)。

### <a name="smb-multichannel"></a>SMB 多重通道

SMB 多重通道可讓檔案伺服器同時使用多個網路連線，並提供更高的輸送量。

如需 SMB 多重通道的詳細資訊，請參閱[部署 Smb 多重](https://aka.ms/smbmulti)通道。

### <a name="smb-scale-out"></a>SMB 相應放大

SMB 相應放大允許在叢集設定中使用 SMB 3.0，在叢集的所有節點中顯示共用。 此主動/主動設定可讓您進一步擴充檔案伺服器叢集，而不需要具有多個磁片區、共用和叢集資源的複雜設定。 最大共用頻寬是所有檔案伺服器叢集節點的總頻寬。 總頻寬不再受單一叢集節點的頻寬限制，而是取決於支援儲存系統的功能。 您可以新增節點以增加總頻寬。

如需 SMB 向外延展的詳細資訊，請參閱[用於應用程式資料的向外延展檔案伺服器總覽](https://technet.microsoft.com/library/hh831349.aspx)，以及要相應放大或不向外延展的 blog 文章[，這就是問題](http://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx)所在。

### <a name="performance-counters-for-smb-30"></a>SMB 3.0 的效能計數器

下列 SMB 效能計數器是在 Windows Server 2012 中引進的，當您監視 SMB 2 和更新版本的資源使用量時，會將它們視為一組基本的計數器。 將效能計數器記錄到本機的原始（.blg）效能計數器記錄檔。 使用萬用字元（\*）收集所有實例會比較便宜，然後使用重新執行，在後置處理期間解壓縮特定的實例。

-   **SMB 用戶端共用**

    這些計數器會顯示使用 SMB 2.0 或更高版本的用戶端所存取之伺服器上檔案共用的相關資訊。

    如果您已熟悉 Windows 中的一般磁片計數器，您可能會注意到某些非常相似。 這不是意外的。 SMB 用戶端共用效能計數器的設計目的是要完全符合磁片計數器。 如此一來，您就可以輕鬆地重複使用目前所擁有之應用程式磁片效能調整的任何指引。 如需有關計數器對應的詳細資訊，請參閱[每個共用用戶端效能計數器的 blog](http://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx)。

-   **SMB 伺服器共用**

    這些計數器會顯示伺服器上 SMB 2.0 或更高版本檔案共用的相關資訊。

-   **SMB 伺服器會話**

    這些計數器會顯示使用 SMB 2.0 或更高版本之 SMB 伺服器會話的相關資訊。

    開啟伺服器端上的計數器（伺服器共用或伺服器會話），對於高 IO 工作負載可能會有顯著的效能影響。

-   **繼續金鑰篩選**

    這些計數器會顯示有關 [繼續金鑰] 篩選器的資訊。

-   **SMB 直接連線**

    這些計數器會測量連接活動的不同層面。 一部電腦可以有多個 SMB 直接連線。 SMB 直接連線計數器會將每個連線表示為一組 IP 位址和埠，其中第一個 IP 位址和埠代表連線的本機端點，而第二個 IP 位址和埠代表連線的遠端端點。

-   **實體磁片、SMB、CSV FS 效能計數器關聯性**

    如需實體磁片、SMB 和 CSV FS （檔案系統）計數器相關方式的詳細資訊，請參閱下列 blog 文章：[叢集共用磁碟區效能計數器](http://blogs.msdn.com/b/clustering/archive/2014/06/05/10531462.aspx)。

## <a name="tuning-parameters-for-smb-file-servers"></a>調整 SMB 檔案伺服器的參數


下列 REG @ no__t-0DWORD 登錄設定可能會影響 SMB 檔案伺服器的效能：

- **Smb2CreditsMin**和**Smb2CreditsMax**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMin
  ```

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMax
  ```

  預設值分別是512和8192。 這些參數可讓伺服器在指定的界限內動態地對用戶端操作並行進行節流。 某些用戶端可能會以較高的平行存取限制來達到增加的輸送量，例如，透過高頻寬、高延遲的連結複製檔案。
    
  > [!TIP]
  > 在 Windows 10 和 Windows Server 2016 之前，授與用戶端的信用額度數目會根據演算法，在 Smb2CreditsMin 與 Smb2CreditsMax 之間動態變化，這是依據嘗試判斷根據網路延遲而授與的最佳信用額度數目和點數使用方式。 在 Windows 10 和 Windows Server 2016 中，SMB 伺服器已變更為在要求時，無條件將信用額度授與已設定的最大點數。 做為這項變更的一部分，信用節流機制會在伺服器發生記憶體不足的壓力時，減少每個連線的點數視窗大小。 觸發節流的核心記憶體不足事件只會在伺服器記憶體不足（< 幾 MB）時才會收到信號，表示毫無用處。 因為伺服器不會再縮減信用額度視窗，所以不再需要 Smb2CreditsMin 設定，而且現在會忽略。
  > 
  > 您可以監視 SMB 用戶端共用 @ no__t-0Credit 會停止的秒數，以查看是否有任何信用額度問題。

- **AdditionalCriticalWorkerThreads**

    ```
    HKLM\System\CurrentControlSet\Control\Session Manager\Executive\AdditionalCriticalWorkerThreads
    ```

    預設值為0，表示不會新增任何額外的重要核心背景工作執行緒。 此值會影響檔案系統快取用於預先讀取和寫入後置要求的執行緒數目。 引發此值可以讓儲存子系統中有更多佇列的 i/o，而且它可以改善 i/o 效能，特別是在具有許多邏輯處理器和強大儲存硬體的系統上。

    >[!TIP]
    > 如果快取管理員的已變更資料量（效能計數器快取 @ no__t-0Dirty 頁面）已成長而耗用大量（超過 ~ 25%），則此值可能需要增加在記憶體中，或如果系統正在執行許多同步讀取 i/o。

- **MaxThreadsPerQueue**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\MaxThreadsPerQueue
  ```

  預設值為20。 增加此值會引發檔案伺服器可用於服務並行要求的執行緒數目。 當有大量作用中的連線需要服務，且硬體資源（例如存放裝置頻寬）足夠時，增加價值可以改善伺服器的擴充性、效能和回應時間。

  >[!TIP]
  > 可能需要增加值的指示是，如果 SMB2 工作佇列的成長非常大（效能計數器「伺服器工作佇列 @ no__t-0Queue Length @ no__t-1SMB2 非封鎖 \* ' 一致地高於 ~ 100）。

  >[!Note]
  >在 Windows 10 和 Windows Server 2016 中，MaxThreadsPerQueue 無法使用。 執行緒集區的執行緒數目會是「20 * NUMA 節點中的處理器數目」。
     

- **AsynchronousCredits**

  ``` 
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\AsynchronousCredits
  ```

  預設值為512。 此參數會限制單一連接上允許的並行非同步 SMB 命令數目。 在某些情況下（例如，當前端伺服器具有後端 IIS 伺服器時）需要大量的平行存取（尤其是針對檔案變更通知要求）。 此專案的值可以增加以支援這些案例。

### <a name="smb-server-tuning-example"></a>SMB 伺服器微調範例

下列設定可在許多情況下，優化電腦的檔案伺服器效能。 在所有電腦上，這些設定並不是最佳或適當的。 套用個別設定之前，您應該評估其影響。

| 參數                       | 值 | 預設 |
|---------------------------------|-------|---------|
| AdditionalCriticalWorkerThreads | 64    | 0       |
| MaxThreadsPerQueue              | 64    | 20      |


### <a name="smb-client-performance-monitor-counters"></a>SMB 用戶端效能監視器計數器

如需 SMB 用戶端計數器的詳細資訊，請參閱 @no__t 0Windows Server 2012 檔案伺服器提示：每個共用的新 SMB 用戶端效能計數器提供了 @ no__t-0 的絕佳深入解析。
