---
title: 向外延展檔案伺服器的應用程式資料概觀 （英文）
description: Windows Server 201 R2、 Windows Server 2012 及 Windows Server 2016 的向外延展檔案伺服器功能的概觀。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 04e25e9c69062611d9d14c220614f148ac5de770
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081998"
---
# <a name="scale-out-file-server-for-application-data-overview"></a>向外延展檔案伺服器的應用程式資料概觀 （英文）

>適用於： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2016

向外延展檔案伺服器是一種功能，可提供持續可用的檔案為基礎的伺服器應用程式儲存區的向外延展檔案共用。 向外延展檔案共用提供共用相同的資料夾從多個相同叢集節點的能力。 此案例著重在如何規劃及部署向外延展檔案伺服器。

您可以部署和設定叢集的檔案伺服器使用下列方法之一：

- **向外延展檔案伺服器應用程式資料**它可讓您儲存伺服器應用程式資料，例如檔案共用上的 HYPER-V 虛擬機器檔案並取得可靠度、 可用性、 管理能力及高相似程度及 Windows Server 2012 中已導入此叢集的檔案伺服器功能您預期從儲存區域網路的效能。 所有檔案共用都同時連線的所有節點上。 這種類型的叢集的檔案伺服器相關聯的檔案共用稱為向外延展檔案共用。 這是有時稱為主動-主動。 這是建議的檔案伺服器類型透過伺服器訊息區塊 (SMB) 或 Microsoft SQL Server 部署任一 HYPER-V over SMB 時。
- **一般使用的檔案伺服器**這是已自容錯移轉叢集的簡介支援 Windows Server 中的叢集的檔案伺服器接續。 這種類型的叢集的檔案伺服器與因此所有共用叢集的檔案伺服器相關聯是線上一個節點上一次。 這是有時稱為主動-被動或雙重作用。 這種類型的叢集的檔案伺服器相關聯的檔案共用稱為叢集的檔案共用。 這是建議的檔案伺服器類型當部署資訊工作者案例。

## <a name="scenario-description"></a>案例說明

與向外延展檔案共用，您可以共用相同的資料夾從多個叢集節點。 例如，如果您有四個節點檔案伺服器叢集所使用的伺服器訊息區塊 (SMB) 向外延展，執行 Windows Server 2012 R2 或 Windows Server 2012 電腦可以從四個節點的任何存取檔案共用。 這可以透過達成運用 Windows Server 容錯移轉叢集的新特色與功能的 Windows 檔案伺服器通訊協定的 SMB 3.0。 檔案伺服器管理員可提供向外延展檔案共用及持續可用檔案服務給伺服器應用程式及快速只是將引進更多伺服器線上回應增加的要求。 所有可以選擇在實際執行環境中，並會完全透明之伺服器應用程式。

向外延展檔案伺服器中所提供的主要好處包括：

- **主動-主動檔案共用**。 所有叢集節點都可接受並提供服務的 SMB 用戶端要求。 讓檔案共用內容可透過所有叢集節點同時、 SMB 3.0 叢集和用戶端合作以提供服務計劃的維護與因的失敗期間與容錯移轉至其他叢集節點中斷。
- **增加頻寬**。 最大共用頻寬是所有檔案伺服器叢集節點的總頻寬。 與舊版的 Windows Server，不同的總頻寬不再限制在單一叢集節點; 的頻寬但備份儲存系統的功能而定義的條件約束。 您可以藉由將節點新增增加總頻寬。
- **CHKDSK 以零的停機時間**。 大幅增強 CHKDSK Windows Server 2012 的功能大幅縮短檔案系統是離線修復的時間。 叢集的共用磁碟區 (CSVs) 需要進一步這一個步驟所帶來的離線階段。 CSV 檔案系統 (CSVFS) 可以使用 CHKDSK 而不會影響使用檔案系統上開啟控點應用程式。
- **叢集共用大量快取**。 在 Windows Server 2012 中的 CSVs 引進支援可大幅提升效能在某些情況下，例如在虛擬桌面基礎結構 (VDI) 讀取快取。
- **簡化管理**。 向外延展檔案伺服器，您可以建立向外延展檔案伺服器，然後新增必要的 CSVs 和檔案共用。 不再需要先建立多個叢集的檔案伺服器各有不同的叢集磁碟，然後開發位置原則，以確保每個叢集節點上的活動。
- **自動重新平衡的向外延展檔案伺服器用戶端**。 在 Windows Server 2012 R2 中，自動重新平衡改善延展性和向外延展檔案伺服器管理性。 SMB 用戶端連線會追蹤個別的檔案共用 (而不是每個伺服器)，與用戶端會再重新導向至叢集節點以使用檔案共用大量最佳的存取權。 這可以提升效能降低檔案伺服器的節點之間的重新導向流量。 用戶端會重新導向追蹤初始連線及重新設定叢集中存放區時。

## <a name="in-this-scenario"></a>在此案例

下列主題是可用來協助您部署的向外延展檔案伺服器：

- [規劃向外延展檔案伺服器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134258(v%3dws.11)>)

  - [步驟 1： 規劃儲存在向外延展檔案伺服器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134181%28v%3dws.11%29>)
  - [步驟 2： 規劃在向外延展檔案伺服器網路](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134253%28v%3dws.11%29>)

- [部署向外延展檔案伺服器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831359%28v%3dws.11%29>)

  - [步驟 1： 安裝向外延展檔案伺服器的先決條件](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831478%28v%3dws.11%29>)
  - [步驟 2： 設定向外延展檔案伺服器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831718%28v%3dws.11%29>)
  - [步驟 3： 設定 HYPER-V 使用向外延展檔案伺服器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831463%28v%3dws.11%29>)
  - [步驟 4： 設定 Microsoft SQL Server 使用向外延展檔案伺服器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831815%28v%3dws.11%29>)

## <a name="when-to-use-scale-out-file-server"></a>向外延展檔案伺服器的使用時機

您不應該使用向外延展檔案伺服器如果您的工作負載產生過多的中繼資料作業，例如開啟檔案關閉檔案、 建立新的檔案或重新命名現有的檔案。 一般的資訊工作者會產生許多的中繼資料作業。 您應該使用向外延展檔案伺服器如果您感興趣的延展性和它便能提供的簡易性，如果您只需要在向外延展檔案伺服器所支援的技術。

下表列出在 SMB 3.0、 一般的 Windows 檔案系統、 檔案伺服器資料管理的技術及一般工作負載的功能。 您可以看到技術支援的向外延展檔案伺服器，還是需要傳統的叢集的檔案伺服器 （也稱為一般使用的檔案伺服器）。

<table>
<thead>
<tr class="header">
<th>技術區域</th>
<th>功能</th>
<th>一般使用檔案伺服器叢集</th>
<th>向外延展檔案伺服器</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>SMB</td>
<td>SMB 連續可用性</td>
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
<td>SMB 透明的容錯移轉</td>
<td>是 （如果已啟用連續可用性）</td>
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
<td>彈性檔案系統 (<a href="https://docs.microsoft.com/windows-server/storage/refs/refs-overview">ReFS</a>)</td>
<td>建議您在儲存間距直接</td>
<td>建議您在儲存間距直接</td>
</tr>
<tr class="even">
<td>檔案系統</td>
<td>叢集中共用大量檔案系統 (CSV)</td>
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
<td>資料 Deduplication (Windows Server 2012)</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="odd">
<td>檔案管理</td>
<td>資料 Deduplication (Windows Server 2012 R2)</td>
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
<td>檔案伺服器資源管理員 （畫面及配額）</td>
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
<td>動態存取控制 （宣告式存取、 限制）</td>
<td>是</td>
<td>否</td>
</tr>
<tr class="even">
<td>檔案管理</td>
<td>資料夾重新導向</td>
<td>是</td>
<td>不建議使用 *</td>
</tr>
<tr class="odd">
<td>檔案管理</td>
<td>離線檔案 （用戶端快取）</td>
<td>是</td>
<td>不建議使用 *</td>
</tr>
<tr class="even">
<td>檔案管理</td>
<td>漫遊使用者設定檔</td>
<td>是</td>
<td>不建議使用 *</td>
</tr>
<tr class="odd">
<td>檔案管理</td>
<td>主目錄</td>
<td>是</td>
<td>不建議使用 *</td>
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

\ * 資料夾重新導向、 離線檔案、 漫遊使用者設定檔或首頁目錄產生大量的必須立即寫入至磁碟 （不含緩衝） 的寫入時使用持續可用的檔案共用，以減少相較之下的效能一般用途檔案共用。 持續可用的檔案共用也包含與檔案伺服器資源管理員與執行 Windows XP 的電腦不相容。 此外，離線檔案可能未轉換成以離線模式 3-6 分鐘後使用者失去無法讓使用者不尚未使用離線檔案一律離線模式不耐煩共用的存取。

## <a name="practical-applications"></a>實際應用

向外延展檔案伺服器是理想的伺服器應用程式儲存區。 以下列出一些可以向外延展檔案共用儲存其資料的伺服器應用程式的範例：

- 網際網路資訊服務 (IIS) 網頁伺服器可以設定和資料的網站上儲存的向外延展檔案共用。 如需詳細資訊，請參閱 ＜[共用設定](http://www.iis.net/learn/manage/managing-your-configuration-settings/shared-configuration_264)。
- HYPER-V 可以向外延展檔案共用上儲存設定與 live 虛擬磁碟。 如需詳細資訊，請參閱[部署 HYPER-V over SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)。
- SQL Server 可儲存向外延展檔案共用上的即時資料庫檔案。 如需詳細資訊，請參閱[安裝 SQL Server SMB 檔案共用的儲存選項](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option)。
- Virtual Machine Manager (VMM) 可以向外延展檔案共用上儲存的文件庫共用 （其中包含虛擬機器範本及相關的檔案）。 不過，文件庫伺服器本身不能向外延展檔案伺服器 — 必須在獨立伺服器或不使用向外延展檔案伺服器叢集角色的容錯移轉叢集。

如果您使用的向外延展檔案共用作為文件庫共用時，您可以使用與向外延展檔案伺服器相容的技術。 例如，您無法使用 DFS 複寫來複寫的向外延展檔案共用上的文件庫共用。 也很重要的向外延展檔案伺服器擁有最新安裝的軟體更新。

要向外延展檔案共用作為文件庫共用，先新增文件庫伺服器 （可能為虛擬機器） 與本機共用或沒有共用所有。 然後當您新增的文件庫共用，請選擇 [向外延展檔案伺服器主控的檔案共用。 此共用應 VMM managed 屬性與以獨佔方式為使用文件庫伺服器所建立。 也請務必在向外延展檔案伺服器上安裝最新的更新。 如需新增 VMM 文件庫伺服器和共用文件庫的詳細資訊，請參閱[VMM 文件庫新增設定檔](https://docs.microsoft.com/system-center/vmm/library-profiles?view=sc-vmm-1801)。 目前可用的檔案與存放服務的 hotfix 清單，請參閱[Microsoft 知識庫文章 2899011](https://support.microsoft.com/help/2899011/list-of-currently-available-hotfixes-for-the-file-services-technologie)。

>[!NOTE]
>某些使用者，例如資訊工作者具有對效能有更大的影響的工作量。 例如，作業像是開啟及關閉檔案、 建立新的檔案並重新命名現有的檔案時執行的多個使用者對效能的影響。 檔案共用連續的可用性與啟用時，它可提供資料完整性，但它也會影響整體效能。 連續可用性需要資料寫入到磁碟以確保完整性的向外延展檔案伺服器叢集節點失敗的情況下。 因此，將數個大型檔案複製到檔案伺服器的使用者可預期持續可用的檔案共用上的效能大幅緩慢。

## <a name="features-included-in-this-scenario"></a>在此案例中包含的功能

下表列出屬於此案例中的功能，並說明所支援的方式。

<table>
<thead>
<tr class="header">
<th>功能</th>
<th>支援此案例的方式</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="failover-clustering.md">容錯移轉叢集</a></td>
<td>容錯移轉叢集可支援向外延展檔案伺服器的 Windows Server 2012 中新增下列功能： 分散式網路名稱、 向外延展檔案伺服器資源類型、 叢集共用磁碟區 (CSV) 2、 及向外延展檔案伺服器的高可用性角色。 如需這些功能的詳細資訊，請參閱<a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)">在 Windows Server 2012 [重新導向] 的容錯移轉叢集的新功能</a>。</td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831795(v%3dws.11)">伺服器訊息區塊</a></td>
<td>SMB 3.0 新增下列功能以支援 Windows Server 2012 的向外延展檔案伺服器： SMB 與容錯移轉能力、 SMB 多重通道及 SMB 直接。<br />
<br />
For Windows Server 2012 R2 中的 SMB 的最新及變更功能詳細資訊，請參閱<a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)">在 Windows Server 中的 SMB 的新功能</a>。</td>
</tr>
</tbody>
</table>

## <a name="more-information"></a>更多資訊

- [軟體定義的儲存空間設計考量指南](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt243829(v%3dws.11)>)
- [增加伺服器、 儲存及網路可用性](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [部署透過 SMB 的 Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
- [部署快速且高效率檔案伺服器的伺服器應用程式](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
- [若要向外延展或向外延展，不會是問題](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx)（部落格文章）
- [資料夾重新導向、離線檔案及漫遊使用者設定檔](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh848267(v%3dws.11)>)