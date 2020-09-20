---
title: 用於應用程式資料的向外延展檔案伺服器概觀
description: 介紹 Windows Server 201 R2 和 Windows Server 2012 的向外延展檔案伺服器功能。
ms.topic: article
author: JasonGerend
ms.author: jgerend
manager: lizross
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 71c719bb4c148a0ff1b287011086ba75e5a3fc69
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766571"
---
# <a name="scale-out-file-server-for-application-data-overview"></a>用於應用程式資料的向外延展檔案伺服器概觀

>適用于： Windows Server 2012 R2、Windows Server 2012

「向外延展檔案伺服器」功能是為了提供向外延展檔案共用而設計的，可供檔案型伺服器應用程式存放裝置持續使用。 向外延展檔案共用提供從相同叢集的多個節點共用相同資料夾的功能。 此案例主要說明如何規劃和部署向外延展檔案伺服器。

您可以使用下列其中一種方法來部署和設定叢集檔案伺服器：

- **應用程式資料的向外延展檔案伺服器** 此叢集檔案伺服器功能是在 Windows Server 2012 中引進的，可讓您在檔案共用上儲存伺服器應用程式資料（如 Hyper-v 虛擬機器檔案），並從存放區域網路取得類似的可靠性、可用性、管理性及高效能。 所有檔案共用會在所有節點上同時上線。 與這種類型的叢集檔案伺服器相關的檔案共用，稱為向外延展檔案共用。 這有時也稱為主動/主動。 這是在部署 Hyper-V over Server Message Block (SMB) 或 Microsoft SQL Server over SMB 時建議使用的檔案伺服器類型。
- **一般用途的檔案伺服器** 這是自容錯移轉叢集推出之後，Windows Server 所支援的叢集檔案伺服器接續。 這種類型的叢集檔案伺服器一次僅在一個節點上線 (因此與這個叢集檔案伺服器相關的所有共用也是一樣)。 這有時也稱為主動/被動或雙主動。 與這種類型的叢集檔案伺服器相關的檔案共用，稱為叢集檔案共用。 這是在部署資訊工作者案例時建議使用的檔案伺服器類型。

## <a name="scenario-description"></a>案例描述

透過向外延展檔案共用，您將可共用叢集中多個節點的相同資料夾。 比方說，如果您有四個節點的檔案伺服器叢集，而且使用伺服器訊息區 (SMB) 相應放大，執行 Windows Server 2012 R2 或 Windows Server 2012 的電腦可以從四個節點中的任一個存取檔案共用。 這是利用新的 Windows Server 容錯移轉叢集功能和 Windows 檔案伺服器通訊協定 SMB 3.0 的功能所達成的。 檔案伺服器系統管理員可以提供向外延展檔案共用和持續可用的檔案服務給伺服器應用程式，而且只要使更多伺服器上線，就能回應快速增加的需求。 這全都可以在實際執行環境中進行，而且對伺服器應用程式而言是完全透明的。

向外延展檔案伺服器提供的主要好處包含：

- **主動-主動檔案共用**。 所有叢集節點都可以接受並提供 SMB 用戶端要求。 由於可同時從所有叢集節點存取檔案共用內容，在計劃中的維護期間和非計劃中的失敗導致服務中斷時，SMB 3.0 叢集和用戶端會合作以提供透明的容錯移轉給其他叢集節點。
- **增加的頻寬**。 共用頻寬上限是所有檔案伺服器叢集節點的總頻寬。 與舊版 Windows Server 不同的是，總頻寬不再僅限於單一叢集節點頻寬，而是由備份儲存系統的功能定義限制。 您可以新增節點以增加總頻寬。
- **CHKDSK 零停機**。 Windows Server 2012 中的 CHKDSK 經過大幅增強，可大幅縮短檔案系統離線修復的時間。 叢集共用磁碟區 (CSV) 進一步加以擴充，去除了離線階段。 CSV 檔案系統 (CSVFS) 可以在檔案系統上使用 CHKDSK，而不影響含有開啟控制代碼的應用程式。
- 叢集**共用磁片**區快取。 Windows Server 2012 中的 Csv 引進讀取快取支援，可以大幅改善某些案例的效能，例如虛擬桌面基礎結構 (VDI) 。
- **更簡單的管理**。 使用向外延展檔案伺服器時，您可以建立擴充檔案伺服器，然後新增必要的 Csv 和檔案共用。 不再需要建立各自擁有不同叢集磁碟的多部叢集檔案伺服器，然後開發位置原則以確保每個叢集節點上的活動。
- **自動重新平衡向外延展檔案伺服器用戶端**。 在 Windows Server 2012 R2 中，自動重新平衡可改善擴充檔案伺服器的擴充性與管理性。 SMB 用戶端連線是以每個檔案共用 (而非每個伺服器) 的方式追蹤，接著再將用戶端重新導向至最方便存取檔案共用使用之磁碟區的叢集節點。 由於檔案伺服器節點之間的重新導向流量減少，因此可以提升效率。 起始一個連線和重新設定叢集存放裝置都會將用戶端重新導向。

## <a name="in-this-scenario"></a>在這個案例中

下列主題可協助您部署向外延展檔案伺服器：

- [規劃向外延展檔案伺服器](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134258(v%3dws.11)>)

  - [步驟 1：規劃向外延展檔案伺服器中的存放裝置](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134181%28v%3dws.11%29>)
  - [Step 2: Plan for Networking in Scale-Out File Server](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134253%28v%3dws.11%29>)

- [Deploy Scale-Out File Server](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831359%28v%3dws.11%29>)

  - [步驟 1：安裝向外延展檔案伺服器的先決條件](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831478%28v%3dws.11%29>)
  - [步驟 2：設定向外延展檔案伺服器](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831718%28v%3dws.11%29>)
  - [步驟 3：設定 Hyper-V 以使用向外延展檔案伺服器](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831463%28v%3dws.11%29>)
  - [步驟 4：設定 Microsoft SQL Server 以使用向外延展檔案伺服器](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831815%28v%3dws.11%29>)

## <a name="when-to-use-scale-out-file-server"></a>向外延展檔案伺服器的使用時機

如果您的工作負載會產生大量的中繼資料操作 (例如開啟檔案、關閉檔案、建立新檔案或重新命名現有檔案)，則不應使用向外延展檔案伺服器。 典型的資訊背景工作便會產生大量的中繼資料操作。 如果您對於其所提供的延展性和易用性感興趣，而且只需要向外延展檔案伺服器支援的技術時，就應該使用向外延展檔案伺服器。

下表列出 SMB 3.0 中的功能、常見的 Windows 檔案系統、檔案伺服器資料管理技術和一般工作負載。 您可以檢視這項技術受到向外延展檔案伺服器的支援，或者是否需要傳統的叢集檔案伺服器 (也稱為一般用途的檔案伺服器)。

<table>
<thead>
<tr class="header">
<th>技術範圍</th>
<th>功能</th>
<th>檔案伺服器叢集的一般用途</th>
<th>向外延展檔案伺服器</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>SMB</td>
<td>SMB 持續可用性</td>
<td>是</td>
<td>是</td>
</tr>
<tr class="even">
<td>SMB</td>
<td>SMB 多重通道</td>
<td>是</td>
<td>是</td>
</tr>
<tr class="odd">
<td>SMB</td>
<td>SMB 直接傳輸</td>
<td>是</td>
<td>是</td>
</tr>
<tr class="even">
<td>SMB</td>
<td>SMB 加密</td>
<td>是</td>
<td>是</td>
</tr>
<tr class="odd">
<td>SMB</td>
<td>SMB 透明容錯移轉</td>
<td>是 (如果已啟用持續可用性)</td>
<td>是</td>
</tr>
<tr class="even">
<td>檔案系統</td>
<td>NTFS</td>
<td>是</td>
<td>NA</td>
</tr>
<tr class="odd">
<td>檔案系統</td>
<td>復原檔案系統 (<a href="/windows-server/storage/refs/refs-overview">ReFS</a>) </td>
<td>建議使用儲存空間直接存取</td>
<td>建議使用儲存空間直接存取</td>
</tr>
<tr class="even">
<td>檔案系統</td>
<td>叢集共用磁碟區 (CSV) 檔案系統</td>
<td>NA</td>
<td>是</td>
</tr>
<tr class="odd">
<td>檔案管理</td>
<td>BranchCache</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="even">
<td>檔案管理</td>
<td>重複資料刪除 (Windows Server 2012)</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="odd">
<td>檔案管理</td>
<td>重複資料刪除 (Windows Server 2012 R2)</td>
<td>是</td>
<td>是 (僅限 VDI)</td>
</tr>
<tr class="even">
<td>檔案管理</td>
<td>DFS 命名空間 (DFSN) 根伺服器根目錄</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="odd">
<td>檔案管理</td>
<td>DFS 命名空間 (DFSN) 資料夾目標伺服器</td>
<td>是</td>
<td>是</td>
</tr>
<tr class="even">
<td>檔案管理</td>
<td>DFS 複寫 (DFSR)</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="odd">
<td>檔案管理</td>
<td>檔案伺服器資源管理員 (畫面和配額)</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="even">
<td>檔案管理</td>
<td>檔案分類基礎結構</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="odd">
<td>檔案管理</td>
<td>動態存取控制 (以宣告為基礎的存取、CAP)</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="even">
<td>檔案管理</td>
<td>資料夾重新導向</td>
<td>是</td>
<td>不建議<em></td>
</tr>
<tr class="odd">
<td>檔案管理</td>
<td>離線檔案 (用戶端快取)</td>
<td>是</td>
<td>不建議使用</em></td>
</tr>
<tr class="even">
<td>檔案管理</td>
<td>漫遊使用者設定檔</td>
<td>是</td>
<td>不建議<em></td>
</tr>
<tr class="odd">
<td>檔案管理</td>
<td>主目錄</td>
<td>是</td>
<td>不建議使用</em></td>
</tr>
<tr class="even">
<td>檔案管理</td>
<td>工作資料夾</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="odd">
<td>NFS</td>
<td>NFS 伺服器</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="even">
<td>應用程式</td>
<td>Hyper-V</td>
<td>不建議使用</td>
<td>是</td>
</tr>
<tr class="odd">
<td>應用程式</td>
<td>Microsoft SQL Server</td>
<td>不建議使用</td>
<td>是</td>
</tr>
</tbody>
</table>

\* 資料夾重新導向、離線檔案、漫遊使用者設定檔或主目錄會產生大量寫入，在使用持續可用的檔案共用時，必須立即將大量寫入寫入磁片 (，而不需要緩衝處理) ，相較于一般用途的檔案共用，效能會降低。 持續可用的檔案共用也與檔案伺服器資源管理員和執行 Windows XP 的電腦不相容。 此外，離線檔案可能會在使用者失去共用的存取權之後，在3-6 分鐘內轉換為離線模式，這樣可能會使尚未使用離線檔案的永遠離線模式使用者感到不快。

## <a name="practical-applications"></a>實際應用

向外延展檔案伺服器十分適用於伺服器應用程式儲存。 以下列出可將其資料儲存在向外延展檔案共用的伺服器應用程式範例：

- 網際網路資訊服務 (IIS) Web 伺服器可以將網站的設定和資料儲存在向外延展檔案共用上。 如需詳細資訊，請參閱＜[共用設定](https://www.iis.net/learn/manage/managing-your-configuration-settings/shared-configuration_264)＞。
- Hyper-V 可以將設定和即時虛擬磁碟儲存在向外延展檔案共用上。 如需詳細資訊，請參閱[部署透過 SMB 的 Hyper-V](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)。
- SQL Server 可以將即時資料庫檔案儲存在向外延展檔案共用上。 如需詳細資訊，請參閱＜[將 SQL Server 與 SMB Fileshare 當做儲存選項一起安裝](/sql/database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option)＞。
- Virtual Machine Manager (VMM) 可以將程式庫共用 (其中包含虛擬機器範本和相關檔案) 儲存在向外延展檔案共用上。 不過，程式庫伺服器本身不能是向外延展檔案伺服器-它必須在獨立伺服器上，或是不使用向外延展檔案伺服器叢集角色的容錯移轉叢集。

如果您以向外延展檔案共用做為程式庫共用，您將只能使用與向外延展檔案伺服器相容的技術。 例如，您無法使用 DFS 複寫來複寫擴充檔案共用上裝載的程式庫共用。 同樣重要的是，向外延展檔案伺服器必須已安裝最新的軟體更新。

若要以向外延展檔案共用做為程式庫共用，請先新增具有本機共用或完全不具共用的程式庫伺服器 (可能是虛擬機器)。 然後，當您新增程式庫共用時，請選擇在擴充檔案伺服器上裝載的檔案共用。 此共用應受到 VMM 管理，並且是為了讓程式庫伺服器專用而建立的。 此外，請確實在向外延展檔案伺服器上安裝最新的更新。 如需新增 VMM 程式庫伺服器和程式庫共用的詳細資訊，請參閱 [將設定檔新增至 vmm 程式庫](/system-center/vmm/library-profiles?view=sc-vmm-1801)。 如需「檔案和存放服務」目前可用的 Hotfix 清單，請參閱＜[微軟知識庫文章 2899011](https://support.microsoft.com/help/2899011/list-of-currently-available-hotfixes-for-the-file-services-technologie)＞。

>[!NOTE]
>有些使用者 (例如資訊工作者) 的工作負載會受到較大的效能影響。 例如，當開啟和關閉檔案、建立新檔案和重新命名現有檔案之類的作業由多個使用者執行時，將會對效能造成影響。 檔案共用如果與持續可用性一起啟用，可提供資料完整性，但也會影響整體效能。 啟用持續可用性時，必須將資料寫入至磁碟，以確保在向外延展檔案伺服器中的叢集節點失敗時仍保有完整性。 因此，使用者若將數個大型檔案複製到檔案伺服器，可以預見持續可用的檔案共用上將發生明顯的效能減緩。

## <a name="features-included-in-this-scenario"></a>這個案例包含的功能

下表列出這個案例中的功能，並說明它們如何支援這個案例。

<table>
<thead>
<tr class="header">
<th>功能</th>
<th>如何支援本案例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="/windows-server/failover-clustering/failover-clustering-overview">容錯移轉叢集</a></td>
<td>容錯移轉叢集在 Windows Server 2012 中新增了下列功能，以支援向外延展檔案伺服器：分散式網路名稱、向外延展檔案伺服器資源類型、叢集共用磁片區 (CSV) 2，以及向外延展檔案伺服器高可用性角色。 如需這些功能的詳細資訊， <a href="/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)">&#39;請參閱 Windows Server 2012 中容錯移轉叢集的新功能 [已重新導向]</a>。</td>
</tr>
<tr class="even">
<td><a href="/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831795(v%3dws.11)">伺服器訊息區</a></td>
<td>SMB 3.0 已在 Windows Server 2012 中新增下列功能，以支援向外延展檔案伺服器： SMB 透明容錯移轉、SMB 多重通道和 SMB 直接傳輸。<br />
<br />
如需 Windows Server 2012 R2 中 SMB 的新功能和變更功能的詳細資訊，請參閱 <a href="/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)">Windows server 中的 Smb 新功能&#39;</a>。</td>
</tr>
</tbody>
</table>

## <a name="more-information"></a>更多資訊

- [軟體定義的儲存體設計考量指南](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt243829(v%3dws.11)>)
- [Increasing Server, Storage, and Network Availability](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [部署透過 SMB 的 Hyper-V](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
- [部署適用於伺服器應用程式且快速又有效率的檔案伺服器](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
- [是否要向外延展，真是難以抉擇](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx) (部落格文章)
- [資料夾重新導向、離線檔案及漫遊使用者設定檔](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh848267(v%3dws.11)>)