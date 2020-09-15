---
title: SMB 檔案伺服器的效能微調
description: SMB 檔案伺服器的效能微調
ms.topic: article
author: phstee
ms.author: nedpyle
ms.date: 4/14/2017
ms.openlocfilehash: 2515f400f746c5e256a168d191efa842d4ba50fd
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077205"
---
# <a name="performance-tuning-for-smb-file-servers"></a>SMB 檔案伺服器的效能微調

## <a name="smb-configuration-considerations"></a>SMB 設定考慮
請勿啟用檔案伺服器和用戶端不需要的任何服務或功能。 這些可能包括 SMB 簽署、用戶端快取、檔案系統迷你篩選、搜尋服務、排程的工作、NTFS 加密、NTFS 壓縮、IPSEC、防火牆篩選器、Teredo 和 SMB 加密。

請確認 BIOS 和作業系統電源管理模式是視需要設定，其中可能包括高效能模式或改變的 C 狀態。 確定已安裝最新、最具彈性且最快速的儲存體和網路設備磁碟機。

複製檔案是在檔案伺服器上執行的常見操作。 Windows Server 擁有數個內建的檔案複製公用程式，可讓您使用命令提示字元來執行。 建議使用 Robocopy。 在 Windows Server 2008 R2 中引進，Robocopy 的 **/mt** 選項可以在複製多個小型檔案時，使用多個執行緒大幅改善遠端檔案傳輸的速度。 我們也建議使用 **/log** 選項，藉由將記錄重新導向至 NUL 裝置或檔案來減少主控台輸出。 當您使用 Xcopy 時，建議您將 **/q** 和 **/k** 選項新增至您現有的參數。 先前的選項可減少主控台輸出，而後者可減少網路流量，以降低 CPU 的額外負荷。

## <a name="smb-performance-tuning"></a>SMB 效能調整


檔案伺服器效能和可用並取決於每個用戶端與伺服器之間的協商 SMB 通訊協定，以及部署的檔案伺服器功能。 目前可用的最高通訊協定版本是 Windows Server 2016 中的 SMB 3.1.1 以及 Windows 10。 您可以使用用戶端上的 Windows PowerShell **SMBConnection** 和 SMBSession，檢查網路上正在使用的 SMB 版本 **|** 在伺服器上的佛羅里達州。

### <a name="smb-30-protocol-family"></a>SMB 3.0 通訊協定系列

SMB 3.0 是在 Windows Server 2012 中引進，並進一步增強 Windows Server 2012 R2 (SMB 3.02) 和 Windows Server 2016 (SMB 3.1.1) 。 此版本引進的技術可大幅提升檔案伺服器的效能和可用性。 如需詳細資訊，請參閱 [Windows Server 2012 和 2012 R2 2012 中的 smb](https://aka.ms/smb3plus) 和 [smb 3.1.1 的新功能](https://aka.ms/smb311)。



### <a name="smb-direct"></a>SMB 直接傳輸

SMB Direct 引進了使用 RDMA 網路介面的能力，可提供低延遲和低 CPU 使用率的高輸送量。

當 SMB 偵測到支援 RDMA 的網路時，它會自動嘗試使用 RDMA 功能。 但是，如果因為任何原因而 SMB 用戶端無法使用 RDMA 路徑進行連線，則只會繼續使用 TCP/IP 連接。 所有與 SMB Direct 相容的 RDMA 介面都必須同時執行 TCP/IP 堆疊，且 SMB 多重通道會察覺到此情況。

在任何 SMB 設定中都不需要 SMB 直接存取，但對於需要較低延遲和較低 CPU 使用率的使用者，一律建議使用它。

如需 SMB 直接存取的詳細資訊，請參閱 [使用 Smb 直接存取改善檔案伺服器的效能](https://aka.ms/smbdirect)。

### <a name="smb-multichannel"></a>SMB 多重通道

SMB 多重通道可讓檔案伺服器同時使用多個網路連線，並提供更高的輸送量。

如需有關 SMB 多重通道的詳細資訊，請參閱 [部署 Smb 多重](https://aka.ms/smbmulti)通道。

### <a name="smb-scale-out"></a>SMB 相應放大

SMB 相應放大可讓叢集設定中的 SMB 3.0 在叢集的所有節點中顯示共用。 此主動/主動設定可讓您進一步擴充檔案伺服器叢集，而不需要具有多個磁片區、共用和叢集資源的複雜設定。 共用頻寬上限是所有檔案伺服器叢集節點的總頻寬。 總頻寬不再受限於單一叢集節點的頻寬，而是取決於支援儲存體系統的功能。 您可以新增節點以增加總頻寬。

如需有關 SMB 相應放大的詳細資訊，請參閱 [應用程式資料總覽的向外延展檔案伺服器](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831349(v=ws.11)) ，以及要相應放大或不向外延展的 blog 文章 [，也就是問題](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx)所在。

### <a name="performance-counters-for-smb-30"></a>SMB 3.0 的效能計數器

下列 SMB 效能計數器是在 Windows Server 2012 中引進的，當您監視 SMB 2 和更新版本的資源使用方式時，它們會被視為一組基本的計數器。 將效能計數器記錄到本機的原始 ( .blg) 效能計數器記錄檔。 使用萬用字元 () 來收集所有實例的成本較低，然後 \* 使用 Relog.exe 在後續處理期間解壓縮特定的實例。

-   **SMB 用戶端共用**

    這些計數器會顯示在使用 SMB 2.0 或更高版本的用戶端所存取的伺服器上，檔案共用的相關資訊。

    如果您在 Windows 中熟悉一般磁片計數器，可能會注意到某些非常相似。 這不是意外的。 SMB 用戶端共用效能計數器的設計目的是完全符合磁片計數器。 如此一來，您就可以輕鬆地重複使用您目前所擁有之應用程式磁片效能調整的任何指引。 如需有關計數器對應的詳細資訊，請參閱 [每個共用用戶端效能計數器的 blog](/archive/blogs/josebda/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight)。

-   **SMB 伺服器共用**

    這些計數器會顯示伺服器上 SMB 2.0 或更高檔案共用的相關資訊。

-   **SMB 伺服器會話**

    這些計數器會顯示使用 SMB 2.0 或更高版本之 SMB 伺服器會話的相關資訊。

    在伺服器端上開啟計數器 (server 共用或伺服器會話) 可能會對高 IO 工作負載造成顯著的效能影響。

-   **繼續機碼篩選**

    這些計數器會顯示 Resume 按鍵篩選器的相關資訊。

-   **SMB 直接連接**

    這些計數器會測量連接活動的不同層面。 電腦可以有多個 SMB 直接連接。 SMB 直接連接計數器以一組 IP 位址和埠表示每個連線，其中第一個 IP 位址和埠代表連線的本機端點，而第二個 IP 位址和埠代表連線的遠端端點。

-   **實體磁片、SMB、CSV FS 效能計數器關聯性**

    如需有關實體磁片、SMB 和 CSV FS 如何 (檔案系統) 計數器相關的詳細資訊，請參閱下列 blog 文章： [叢集共用磁碟區效能計數器](https://blogs.msdn.com/b/clustering/archive/2014/06/05/10531462.aspx)。

## <a name="tuning-parameters-for-smb-file-servers"></a>調整 SMB 檔案伺服器的參數


下列 REG \_ DWORD 登錄設定可能會影響 SMB 檔案伺服器的效能：

- **Smb2CreditsMin** 和 **Smb2CreditsMax**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMin
  ```

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\Smb2CreditsMax
  ```

  預設值分別為512和8192。 這些參數可讓伺服器在指定的界限內動態節流用戶端作業並行。 某些用戶端可能會以更高的平行存取限制來達到更高的輸送量，例如，透過高頻寬、高延遲的連結來複製檔案。

  > [!TIP]
  > 在 Windows 10 和 Windows Server 2016 之前，授與用戶端的點數數量在 Smb2CreditsMin 和 Smb2CreditsMax 之間會根據演算法來動態變化，此演算法會根據網路延遲和點數使用量來決定要授與的最佳點數。 在 Windows 10 和 Windows Server 2016 中，SMB 伺服器已變更為在要求時無條件地授與點數，最多可達設定的最大點數。 在這項變更中，信用節流機制可在伺服器處於記憶體不足的壓力時，減少每個連線的點數時段大小。 只有當伺服器的記憶體不足時，才會發出觸發節流的核心記憶體不足事件 ( # A0 有幾 MB) 為無用。 因為伺服器不再縮減點數，所以不再需要 Smb2CreditsMin 設定，現在會忽略。
  >
  > 您可以監視 SMB 用戶端共用的 \\ 信用額度不到一秒，以查看是否有任何信用額度有問題。

- **AdditionalCriticalWorkerThreads**

    ```
    HKLM\System\CurrentControlSet\Control\Session Manager\Executive\AdditionalCriticalWorkerThreads
    ```

    預設值為0，表示不會加入任何其他重要的核心背景工作執行緒。 此值會影響檔案系統快取用來讀取和寫入後端要求的執行緒數目。 提高此值可允許儲存子系統中有更多佇列的 i/o，而且可以改善 i/o 效能，特別是在具有許多邏輯處理器和強大存放裝置硬體的系統上。

    >[!TIP]
    > 如果快取管理員變更資料的數量 (效能計數器快取中途分頁，則可能需要增加此值， \\) 正在成長，以耗用大量 (超過超過25% 的記憶體 ) ，或是系統正在執行大量的同步讀取 i/o。

- **MaxThreadsPerQueue**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\MaxThreadsPerQueue
  ```

  預設值為 20。 提高此值會引發檔案伺服器可用來服務並行要求的執行緒數目。 當需要處理大量作用中的連線，且硬體資源（例如儲存體頻寬）足夠時，增加價值可改善伺服器的擴充性、效能和回應時間。

  >[!TIP]
  > 如果 SMB2 工作佇列的成長很大 (效能計數器「伺服器工作佇列佇列長度 SMB2 非封鎖」的值，則可能需要增加此值，其大小 \\ \\ \* 會一直高於 ~ 100) 。

  >[!Note]
  >在 Windows 10 和 Windows Server 2016 中，無法使用 MaxThreadsPerQueue。 執行緒集區的執行緒數目會是「20 * NUMA 節點中的處理器數目」。


- **AsynchronousCredits**

  ```
  HKLM\System\CurrentControlSet\Services\LanmanServer\Parameters\AsynchronousCredits
  ```

  預設值為 512。 此參數會限制單一連接上允許的並行非同步 SMB 命令數目。 某些情況 (例如，當前端伺服器具有後端 IIS 伺服器時) 需要大量的並行 (來處理檔案變更通知要求，尤其是) 。 您可以增加這個專案的值來支援這些案例。

### <a name="smb-server-tuning-example"></a>SMB 伺服器微調範例

在許多情況下，下列設定可以針對檔案伺服器效能優化電腦。 在所有電腦上，這些設定並不是最佳或適當的。 套用個別設定之前，您應該評估其影響。

| 參數                       | 值 | 預設 |
|---------------------------------|-------|---------|
| AdditionalCriticalWorkerThreads | 64    | 0       |
| MaxThreadsPerQueue              | 64    | 20      |


### <a name="smb-client-performance-monitor-counters"></a>SMB 用戶端效能監視器計數器

如需 SMB 用戶端計數器的詳細資訊，請參閱 [Windows Server 2012 檔案伺服器提示：新增個別共用 SMB 用戶端效能計數器可提供絕佳的見解](/archive/blogs/josebda/windows-server-2012-file-server-tip-new-per-share-smb-client-performance-counters-provide-great-insight)。