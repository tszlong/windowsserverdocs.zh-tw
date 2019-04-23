---
title: SMB 檔案伺服器的效能微調
description: SMB 檔案伺服器的效能微調
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
author: phstee
ms.author: NedPyle; Danlo; DKruse
ms.date: 4/14/2017
ms.openlocfilehash: 93718cf13f28cde8f25b35b42ce20ca75c6fa13c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832059"
---
# <a name="performance-tuning-for-smb-file-servers"></a>SMB 檔案伺服器的效能微調

## <a name="smb-configuration-considerations"></a>SMB 設定考量
請勿啟用任何服務或您的檔案伺服器和用戶端不需要的功能。 這些可能包括 SMB 簽章、 用戶端快取、 檔案系統迷你-篩選、 搜尋服務、 排定的工作、 NTFS 加密，NTFS 壓縮、 IPSEC、 防火牆篩選器、 Teredo 和 SMB 加密。

請確定如有需要要設定 BIOS 和作業系統電源管理模式，其中可能包含高效能模式，或改變 C 狀態。 確定已安裝最新、 最彈性且最快速的儲存體和網路裝置驅動程式。

將檔案複製是在檔案伺服器上執行的一般作業。 Windows Server 具有數個內建的檔案複製公用程式，您可以使用命令提示字元中執行。 建議使用 Robocopy。 在 Windows Server 2008 R2 中引進 **/mt** Robocopy 的選項可大幅提升複製多個的小型檔案時，使用多個執行緒的遠端檔案傳輸的速度。 我們也建議您使用 **/log**選項可將記錄檔的重新導向到 NUL 裝置或檔案時，減少主控台輸出。 當您使用 Xcopy 時，我們建議您新增 **/q**並 **/k**至您現有的參數選項。 舊有的選項可減少 CPU 額外負荷降低主控台輸出，後者會減少網路流量。

## <a name="smb-performance-tuning"></a>SMB 效能微調


檔案伺服器的效能和可用的 tunings 依據每個用戶端與伺服器之間交涉 SMB 通訊協定以及已部署的檔案伺服器功能。 目前可用的最大通訊協定版本是 Windows Server 2016 和 Windows 10 中的 SMB 3.1.1。 您可以檢查您網路上的使用中所使用 Windows PowerShell 的 SMB 版本**Get SMBConnection**用戶端上和**Get SMBSession |佛羅里達州**在伺服器上。

### <a name="smb-30-protocol-family"></a>SMB 3.0 通訊協定家族

SMB 3.0 是 Windows Server 2012 中引進，並進一步增強了 Windows Server 2012 R2 （SMB 3.02 和） 和 Windows Server 2016 (SMB 3.1.1)。 此版本導入了可能會大幅改善效能和可用性檔案伺服器的技術。 如需詳細資訊，請參閱 < [SMB 在 Windows Server 2012 和 2012 R2 2012](https://aka.ms/smb3plus)並[SMB 3.1.1 中最新消息](https://aka.ms/smb311)。



### <a name="smb-direct"></a>SMB 直接傳輸

SMB 直接傳輸導入了低延遲及低 CPU 使用率，以達到高輸送量使用 RDMA 網路介面的能力。

只要 SMB 會偵測到一個支援 RDMA 的網路，它會自動嘗試使用 RDMA 功能。 不過，如果因為任何原因無法使用 RDMA 路徑連接 SMB 用戶端，它將只會繼續改為使用 TCP/IP 連接。 所有與 SMB 直接傳輸相容的 RDMA 介面才能也實作 TCP/IP 堆疊，而且 SMB 多重通道會注意到這一點。

SMB 直接傳輸中不需要任何 SMB 設定，但 ' s 一律建議針對需要低延遲和較低 CPU 使用率的人。

如需 SMB 直接傳輸的詳細資訊，請參閱[改善具有 SMB 直接傳輸的檔案伺服器的效能](https://aka.ms/smbdirect)。

### <a name="smb-multichannel"></a>SMB 多重通道

SMB 多重通道允許檔案伺服器同時使用多個網路連線，並提供更高的輸送量。

如需 SMB 多重通道的詳細資訊，請參閱[部署 SMB 多重通道](https://aka.ms/smbmulti)。

### <a name="smb-scale-out"></a>SMB 向外延展

SMB 向外延展允許在叢集組態中叢集的所有節點中顯示為共用的 SMB 3.0。 這個主動/主動組態就可以擴展檔案伺服器叢集更進一步，而不需要複雜的設定與多個磁碟區、 共用叢集資源。 最大共用頻寬是所有的檔案伺服器叢集節點的總頻寬。 總頻寬不再受限於單一叢集節點的頻寬，但而是取決於備份儲存體系統的功能。 您可以新增節點以增加總頻寬。

如需 SMB 向外延展的詳細資訊，請參閱 <<c0> [ 向外延展檔案伺服器的應用程式資料概觀](https://technet.microsoft.com/library/hh831349.aspx)和部落格文章[向外擴充不來向外延展，真是問題](http://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx)。

### <a name="performance-counters-for-smb-30"></a>SMB 3.0 的效能計數器

Windows Server 2012 中導入下列的 SMB 效能計數器，監視 SMB 2 和更新版本的資源使用量時，它們會考慮一組基底計數器。 效能計數器記錄到本機，原始 (.blg) 效能計數器記錄檔。 若要使用萬用字元收集所有執行個體的成本較低 (\*)，然後在後處理期間使用 Relog.exe 中擷取特定的執行個體。

-   **SMB 用戶端共用**

    這些計數器會顯示所使用的 SMB 2.0 或更新版本的用戶端正在存取的伺服器上的檔案共用的相關資訊。

    如果您 ' 熟悉在 Windows 中的一般磁碟計數器，您可能會注意到某些相似。 ' S 不是意外。 SMB 用戶端共用效能計數器可完全符合磁碟計數器。 如此一來，您可以輕鬆地重複使用任何應用程式磁碟效能微調指引您目前沒有。 如需計數器對應的詳細資訊，請參閱[每個共用用戶端效能計數器的部落格](http://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx)。

-   **SMB 伺服器共用**

    這些計數器會顯示伺服器上的 SMB 2.0 或更高版本的檔案共用的相關資訊。

-   **SMB 伺服器工作階段**

    這些計數器會顯示有關使用 SMB 2.0 或更高的 SMB 伺服器工作階段的資訊。

    開啟 伺服器端 （伺服器共用或伺服器工作階段） 的計數器可能會有顯著的效能影響的高 IO 工作負載。

-   **繼續金鑰篩選**

    這些計數器會顯示繼續金鑰篩選器的相關資訊。

-   **SMB 直接連線**

    這些計數器來測量連接活動的不同層面。 電腦可以有多個 SMB 直接傳輸的連線。 SMB 直接連線計數器代表每個為一組 IP 位址和連接埠，其中的第一個 IP 位址和連接埠代表連線的本機端點，而第二個 IP 位址和連接埠代表連線的遠端端點的連線。

-   **實體磁碟、 SMB、 CSV FS 效能計數器關聯性**

    如需如何產生關聯的實體磁碟、 SMB 或 CSV FS （檔案系統） 計數器的詳細資訊，請參閱下列部落格文章：[叢集共用磁碟區的效能計數器](http://blogs.msdn.com/b/clustering/archive/2014/06/05/10531462.aspx)。

## <a name="tuning-parameters-for-smb-file-servers"></a>SMB 檔案伺服器的微調參數


下列 REG\_DWORD 登錄設定可能會影響效能的 SMB 檔案伺服器：

-   **Smb2CreditsMin**和**Smb2CreditsMax**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMin
    ```

    ```
    HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMax
    ```

    預設值分別為 512 與 8192。 這些參數可讓節流以動態方式在指定的界限內的用戶端作業並行處理的伺服器。 某些用戶端可能會達到增加的輸送量與較高並行存取限制，例如，透過高頻寬、 高延遲連結複製的檔案。
    
    >[!TIP]
    > 在 Windows 10 和 Server 2016 之前, 的授與用戶端之間 Smb2CreditsMin 和 Smb2CreditsMax 嘗試判斷最佳的信用額度來授與數目演算法為基礎的動態各不相同的學分數根據輸入網路延遲和信用額度使用方式。 在 Windows 10 和 Server 2016 中，SMB 伺服器已變更為無條件地授與最多設定的最大數目的信用額度的要求時的信用額度。 隨著這項變更的詳細資訊，節流機制，可減少每個連接的信用額度 視窗的大小，在伺服器記憶體不足的壓力時，點數已移除。 因此記憶體不足的伺服器時所觸發的節流，核心的記憶體不足事件只收到信號 (< 幾 MB) 是毫無用處。 因為伺服器不會再縮小信用額度 windows Smb2CreditsMin 設定已不再需要和現在會被忽略。


    >[!TIP]
    > 您可以監視 SMB 用戶端共用\\來查看是否有任何問題的信用額度的信用額度停止/秒。

- **AdditionalCriticalWorkerThreads**

    ```
    HKLM\System\CurrentControlSet\Control\Session Manager\Executive\AdditionalCriticalWorkerThreads
    ```

    預設值為 0，這表示，會新增任何其他關鍵核心進行背景工作執行緒。 這個值會影響檔案系統快取會使用預先讀取和貫穿式寫入要求的執行緒的數目。 提高這個值可以讓多個已排入佇列 I/O 在儲存體子系統中，而且它可以改善 I/O 效能，特別是在許多邏輯處理器和強大的存放裝置硬體的系統上。

    >[!TIP]
    > 如果快取管理員數量已變更的資料就可增加可能需要的值 (快取的效能計數器\\中途分頁) 會增加，以佔用很大 （超過 ~ 25%)記憶體或如果系統正在進行同步大量讀取 I/o。

-   **MaxThreadsPerQueue**

    ```
    HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\MaxThreadsPerQueue
    ```

    預設值為 20。 增加此值會產生檔案伺服器可以使用服務的並行要求的執行緒的數目。 當大量的使用中的連線需要可服務，而且硬體資源，例如儲存體頻寬，就已足夠時，增加此值可以改善伺服器延展性、 效能和回應時間。

    >[!TIP]
    > 值可能需要增加的指示是如果 SMB2 工作佇列會變得非常大 (效能計數器 '伺服器工作佇列\\佇列長度\\SMB2 未封鎖\*' ~ 100 以上以一致的方式是)。

     

-   **AsynchronousCredits**

    ``` 
    HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\AsynchronousCredits
    ```

    預設值為 512。 這個參數會限制在單一連接所允許的並行非同步 SMB 命令數目。 某些情況下 (例如當沒有前端伺服器與後端的 IIS 伺服器) （適用於檔案尤其變更通知要求） 需要大量的並行存取。 以支援這種情況下，可以增加此項目的值。

### <a name="smb-server-tuning-example"></a>SMB 伺服器微調範例

下列設定可以最佳化的檔案伺服器效能，在許多情況下的電腦。 設定不是最佳或需要的所有電腦上執行。 您應該在套用之前先評估個別設定的影響。

| 參數                       | 值 | 預設 |
|---------------------------------|-------|---------|
| AdditionalCriticalWorkerThreads | 64    | 0       |
| MaxThreadsPerQueue              | 64    | 20      |


### <a name="smb-client-performance-monitor-counters"></a>SMB 用戶端效能監視器計數器

如需 SMB 用戶端計數器的詳細資訊，請參閱[Windows Server 2012 檔案伺服器秘訣：新的每個共用 SMB 用戶端的效能計數器提供絕佳的見解，](http://blogs.technet.com/b/josebda/archive/2012/11/19/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight.aspx)。
