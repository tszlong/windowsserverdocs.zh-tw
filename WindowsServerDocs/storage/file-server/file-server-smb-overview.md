---
title: 在 Windows Server 中使用 SMB 3 通訊協定的檔案共用總覽
description: 概述如何將 SMB 3 通訊協定用於檔案共用，並使用 Windows Server 提供的檔案。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 01/10/2020
ms.localizationpriority: medium
ms.openlocfilehash: 416145a8c4ec20eaf46cf4b5ac88a0cdf38bdf33
ms.sourcegitcommit: 76469d1b7465800315eaca3e0c7f0438fc3939ed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/13/2020
ms.locfileid: "75919890"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>在 Windows Server 中使用 SMB 3 通訊協定的檔案共用總覽

>適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題說明 Windows Server 2019、Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中的 SMB 3 功能-這項功能的實際用法、與舊版相較之下，此版本中最重要的新功能或更新功能，以及硬體需求。 SMB 也是[軟體定義的資料中心（SDDC）](../../sddc.md)解決方案所使用的網狀架構通訊協定，例如儲存空間直接存取、儲存體複本等等。 SMB 3.0 版是隨著 Windows Server 2012 引進，並已在後續版本中以累加方式改善。

## <a name="feature-description"></a>功能描述

伺服器訊息區 (SMB) 通訊協定是網路檔案共用通訊協定，允許電腦上的應用程式讀取和寫入檔案，以及要求來自電腦網路中伺服器程式的服務。 SMB 通訊協定可以用於其 TCP/IP 通訊協定或其他網路通訊協定的最上方。 使用 SMB 通訊協定，應用程式 (或應用程式的使用者) 可以存取遠端伺服器上的檔案或其他資源。 這允許應用程式讀取、建立及更新遠端伺服器上的檔案。 SMB 也可以與任何設定為接收 SMB 用戶端要求的伺服器程式進行通訊。 SMB 是軟體定義的資料中心（SDDC）運算技術所使用的網狀架構通訊協定，例如儲存空間直接存取、儲存體複本。 如需詳細資訊，請參閱[Windows Server 軟體定義資料中心](../../sddc.md)。

## <a name="practical-applications"></a>實際應用

本節討論一些使用新 SMB 3.0 通訊協定的新實用方法。

* **適用於虛擬化的檔案存放裝置 (透過 SMB 的 Hyper-V™)**。 Hyper-V 可以透過 SMB 3.0 通訊協定，在檔案共用中儲存虛擬機器檔案，例如設定、虛擬硬碟 (VHD) 檔案及快照。 這適用於獨立式檔案伺服器和叢集檔案伺服器，這些伺服器會將 Hyper-V 與叢集的共用檔案存放裝置搭配使用。
* **透過 SMB 的 Microsoft SQL Server**。 SQL Server 可以在 SMB 檔案共用上儲存使用者資料庫檔案。 目前適用於獨立式 SQL Server 的 SQL Server 2008 R2 支援此項。 即將推出的 SQL Server 版本將新增對於叢集 SQL 伺服器和系統資料庫的支援。
* **適用於使用者資料的傳統存放裝置**。 SMB 3.0 通訊協定會為資訊工作者 (或用戶端) 工作負載提供增強功能。 這些增強功能包括透過廣域網路 (WAN) 存取資料和保護資料免於遭受竊聽攻擊時，減少分公司使用者感受到應用程式延遲。

## <a name="new-and-changed-functionality"></a>新功能和變更的功能

下列各節說明在 SMB 3 中新增的功能，以及後續的更新。

## <a name="features-added-in-windows-server-2019-and-windows-10-version-1809"></a>Windows Server 2019 和 Windows 10 版本1809中新增的功能

| 特色/功能  | 新功能或更新功能  | 摘要  |
| --------- | --------- | --------- |
| 在檔案共用上需要寫入磁片的能力，但無法持續使用 | 新的 | 若要提供一些新增的保證，以寫入檔案共用，讓寫入作業回到完成之前，將軟體和硬體堆疊到實體磁片，您可以使用 `NET USE /WRITETHROUGH` 命令或 `New-SMBMapping -UseWriteThrough` PowerShell Cmdlet，在檔案共用上啟用寫入。 使用寫入時，有一些效能會受到影響;如需進一步討論，請參閱在[SMB 中控制寫入行為](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-write-through-behaviors-in-smb/bc-p/1083417#M677)的 blog 文章。 |

## <a name="features-added-in-windows-server-version-1709-and-windows-10-version-1709"></a>Windows Server 版本1709和 Windows 10 版本1709中新增的功能

| 特色/功能  | 新功能或更新功能  | 摘要  |
| --------- | --------- | --------- |
| 已停用來賓存取檔案共用 | 新的 | SMB 用戶端不再允許下列動作：來賓帳戶對遠端伺服器的存取權;提供不正確認證之後，回復至來賓帳戶。 如需詳細資訊，請參閱[Windows 中預設停用 SMB2 中的來賓存取](https://support.microsoft.com/help/4046019/guest-access-in-smb2-disabled-by-default-in-windows-10-and-windows-ser)。 | 
| SMB 全域對應 | 新的 | 將遠端 SMB 共用對應至本機主機上所有使用者都可存取的磁碟機號，包括容器。 若要在資料磁片區上啟用容器 i/o，以進行遠端掛接點，這是必要的。 請注意，使用容器的 SMB 全域對應時，容器主機上的所有使用者都可以存取遠端共用。 在容器主機上執行的任何應用程式也都可以存取對應的遠端共用。 如需詳細資訊，請參閱[使用叢集共用磁片區（CSV）的容器儲存體支援、儲存空間直接存取、SMB 全域對應](https://techcommunity.microsoft.com/t5/failover-clustering/container-storage-support-with-cluster-shared-volumes-csv/ba-p/372140)。 |
| SMB 方言控制 | 新的 | 您現在可以設定登錄值來控制最小 SMB 版本（方言）和所使用的最大 SMB 版本。 如需詳細資訊，請參閱[控制 SMB 方言](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024)。 |

## <a name="features-added-in-smb-311-with-windows-server-2016-and-windows-10-version-1607"></a>SMB 3.11 中新增的功能與 Windows Server 2016 和 Windows 10 版本1607

| 特色/功能  | 新功能或更新功能  | 摘要  |
| --------- | --------- | --------- |
| SMB 加密     |   已更新      | 具有進階加密標準 Galois/Counter 模式（AES GCM）的 SMB 3.1.1 加密比使用 AES-CCM 的 SMB 簽署或先前的 SMB 加密更快。   |
| 目錄快取 | 新的 | SMB 3.1.1 包含目錄快取的增強功能。 Windows 用戶端現在可以快取更大的目錄，大約是500K 的專案。 Windows 用戶端會嘗試使用 1 MB 緩衝區進行目錄查詢，以減少來回行程並提升效能。 |
| 預先驗證完整性 | 新的 |  在 SMB 3.1.1 中，預先驗證完整性可讓攔截式攻擊者利用 SMB 的連線建立和驗證訊息，進行更佳的保護。 如需詳細資訊，請參閱[Windows 10 中的 SMB 3.1.1 預先驗證完整性](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10)。 |
| SMB 加密改良功能 | 新的 | SMB 3.1.1 提供一種機制來協調每個連線的加密演算法，以及 AES-128-CCM 和 AES-128-GCM 的選項。 AES-128-GCM 是新 Windows 版本的預設值，而較舊版本會繼續使用 AES-128-CCM。 |
| 滾動叢集升級支援 | 新的 | 藉由讓 SMB 能夠在升級的過程中，為叢集支援不同的 SMB 最大版本，來啟用[滾動叢集升級](../../failover-clustering/cluster-operating-system-rolling-upgrade.md)。 如需有關如何使用通訊協定的不同版本（方言）來進行 SMB 通訊的詳細資訊，請參閱[控制 SMB 方言](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024)的 blog 文章。 |
| Windows 10 中的 SMB 直接客戶支援 | 新的 | 適用于工作站的 windows 10 企業版、Windows 10 教育版和 Windows 10 專業版現在包含 SMB 直接用戶端支援。 |
| FileNormalizedNameInformation API 呼叫的原生支援 | 新的 | 新增查詢檔案之正規化名稱的原生支援。 如需詳細資訊，請參閱[FileNormalizedNameInformation](https://docs.microsoft.com/openspecs/windows_protocols/ms-fscc/20bcadba-808c-4880-b757-4af93e41edf6)。 |

如需其他詳細資訊，請參閱[Windows Server 2016 Technical Preview 2 中的](https://docs.microsoft.com/archive/blogs/josebda/whats-new-in-smb-3-1-1-in-the-windows-server-2016-technical-preview-2)的文章。

## <a name="features-added-in-smb-302-with-windows-server-2012-r2-and-windows-81"></a>SMB 3.02 中新增的功能與 Windows Server 2012 R2 和 Windows 8。1

| 特色/功能  | 新功能或更新功能  | 摘要  |
| --------- | --------- | --------- |
| 自動重新平衡向外延展檔案伺服器用戶端     |   新的      | 改善向外延展檔案伺服器的擴充性與管理能力。 SMB 用戶端連線是以每個檔案共用 (而非每個伺服器) 的方式追蹤，接著再將用戶端重新導向至最方便存取檔案共用使用之磁碟區的叢集節點。 由於檔案伺服器節點之間的重新導向流量減少，因此可以提升效率。 起始一個連線和重新設定叢集存放裝置都會將用戶端重新導向。    |
| 透過 WAN 的效能   | 已更新  | 當您從遠端電腦上的某個位置，使用 [檔案瀏覽器] 進行遠端副本，在同一部伺服器上的另一個複本時，Windows 8.1 和 Windows 10 提供 SMB 支援的改良 CopyFile SRV_COPYCHUNK。 您只會透過網路複製少量的中繼資料（每個16MiB 的檔案資料傳輸 1/2KiB）。 這會導致大幅改善效能。 這是 SMB 的 OS 層級和檔案瀏覽器層級的差異。 |
| SMB 直接傳輸     |   已更新      | 透過提高以少量 I/O 裝載工作負載的效率，改善少量 I/O 工作負載的效能 (例如虛擬機器中的線上交易處理 (OLTP) 資料庫)。 當使用較高速的網路介面時（例如 40 Gbps Ethernet 和 56 Gbps 的時間），這些增強功能很明顯。  |
| SMB 頻寬限制 | 新的 | 您現在可以使用[SmbBandwidthLimit](https://docs.microsoft.com/powershell/module/smbshare/set-smbbandwidthlimit)來設定三個類別的頻寬限制： VirtualMachine （HYPER-V over smb 流量）、LiveMigration （hyper-v 即時移轉透過 smb 的流量）或預設值（所有其他類型的 smb 流量）。

如需有關 Windows Server 2012 R2 中新的和已變更的 SMB 功能的詳細資訊，請參閱[Windows server 中 SMB 的新](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>)功能。

## <a name="features-added-in-smb-30-with-windows-server-2012-and-windows-8"></a>SMB 3.0 中新增的功能與 Windows Server 2012 和 Windows 8

| 特色/功能  | 新功能或更新功能  | 摘要  |
| --------- | --------- | --------- |
| SMB 透明容錯移轉     |   新的    | 讓系統管理員能夠在叢集檔案伺服器中執行節點的硬體或軟體維護，而不需中斷正在這些檔案共用上儲存資料的伺服器應用程式。 此外，如果叢集節點上發生硬體或軟體失敗，SMB 用戶端會以透明的方式重新連線到其他叢集節點，而不需中斷正在這些檔案共用上儲存資料的伺服器應用程式。        |
| SMB 向外延展     |   新的      | 支援向外延展檔案伺服器上的多個 SMB 實例。 使用叢集共用磁碟區 (CSV) 版本 2，系統管理員可以建立檔案共用，透過檔案伺服器叢集中的所有節點，利用直接 I/O 來提供資料檔案的同時存取。 這樣可以提供較佳的網路頻寬使用率，以及檔案伺服器用戶端的負載平衡，並將伺服器應用程式的效能最佳化。  |
| SMB 多重通道     |   新的      |  如果 SMB 用戶端與伺服器之間有多個路徑可用，則啟用網路頻寬和網路容錯的匯總。 這樣能讓伺服器應用程式可以充分利用所有可用的網路頻寬，並能彈性處理網路失敗。<br><br>相較于舊版的 SMB，SMB 3 中的 SMB 多重通道會大幅提升效能。 |
| SMB 直接傳輸     |   新的      | 支援使用含有 RDMA 功能的網路介面卡，而且在使用非常少量的 CPU 時，以非常低的延遲全速運作。 對於像是 Hyper-V 或 Microsoft SQL Server 的工作負載，這讓遠端檔案伺服器能夠類似本機存放裝置。<br><br>相較于舊版的 SMB，SMB 3 中的 SMB 直接傳輸有助於大幅增加效能。  |
| 伺服器應用程式的效能計數器     |   新的      |  新的 SMB 效能計數器提供有關輸送量、延遲和每秒 i/o 數（IOPS）的詳細每個共用資訊，讓系統管理員能夠分析儲存其資料之 SMB 檔案共用的效能。 這些計數器是特別針對伺服器應用程式而設計 (例如 Hyper-V 和 SQL Server)，它們可以在遠端檔案共用上儲存檔案。      |
| 效能最佳化    |  已更新   | SMB 用戶端和伺服器已針對小型隨機讀取/寫入 i/o 進行優化，這在伺服器應用程式（例如 SQL Server OLTP）中很常見。 此外，預設會開啟大型傳輸單元最大值 (MTU)，明顯增強大型循序傳輸的效能，例如 SQL Server 資料倉儲、資料庫備份或還原、部署或複製虛擬硬碟。 |
| SMB 特定的 Windows PowerShell Cmdlet     |   新的      |  利用適用於 SMB 的 Windows PowerShell Cmdlet，系統管理員可以從命令列，以端對端方式，在檔案伺服器上管理檔案共用。   |
| SMB 加密     |   新的      | 提供 SMB 資料的端對端加密，並保護資料以免在不受信任的網路上遭到竊聽。 不需要新的部署成本，也不需要網際網路通訊協定安全性 (IPsec)、特定的硬體或 WAN 加速器。 這可能會以每個共用為基礎進行設定，或者針對整個檔案伺服器進行設定，而且可能會針對資料周遊不受信任網路的各種案例加以啟用。 |
| SMB 目錄租用     |  新的 | 增強分公司的應用程式回應時間。 使用目錄租用，從用戶端到伺服器的來回時間會降低，因為中繼資料是從存留期間較長的目錄快取中所擷取。 快取的一致性可以維持，因為當伺服器上的目錄資料變更時，用戶端會收到通知。 目錄租用適用于主資料夾（讀取/寫入且無共用）和發行集（唯讀且有共用）的案例。    |
| 透過 WAN 的效能   | 新的   | 目錄機會鎖定（oplocks）和 oplock 租用是在 SMB 3.0 中引進。 針對一般的 office/用戶端工作負載，會顯示 oplocks/租用，以減少大約15% 的網路往返。<br><br>在 SMB 3 中，SMB 的 Windows 實行已經過調整，以改善用戶端上的快取行為，並能夠推送更高的輸送量。<br><br>SMB 3 的功能改進了 CopyFile （） API，以及相關聯的工具（例如 Robocopy），可透過網路推送更多的資料。 |
| 安全用語談判 | 新的 | 協助防止中間人嘗試降級方言的協商。 其概念是防止小人將用戶端與伺服器之間的一開始協商用語和功能降級。 如需詳細資訊，請參閱[SMB3 安全方言的協商](https://docs.microsoft.com/archive/blogs/openspecification/smb3-secure-dialect-negotiation)。 請注意，在 SMB 3.1.1 的 Windows 10 功能中， [smb 3.1.1 預先驗證完整性](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10)已取代此項。 |


## <a name="hardware-requirements"></a>硬體需求

SMB 透明容錯移轉具備下列需求：

* 執行 Windows Server 2012 或 Windows Server 2016 且至少已設定兩個節點的容錯移轉叢集。 這個叢集必須傳遞驗證精靈中所含的叢集驗證測試。
* 檔案共用必須利用持續可用性 (CA) 屬性來建立，這是預設值。
* 檔案共用必須建立於 CSV 磁碟區路徑上，以達到 SMB 向外延展。
* 用戶端電腦必須執行 Windows®8或 Windows Server 2012，這兩者都包含支援持續可用性的更新 SMB 用戶端。

> [!NOTE]
> 下層用戶端可以連接到具有 CA 屬性的檔案共用，但這些用戶端不會支援透明容錯移轉。

SMB 多重通道具備下列需求：

* 至少需要兩部執行 Windows Server 2012 的電腦。 不需要安裝任何其他的功能 - 預設會啟用這項技術。
* 如需所建議網路設定的相關資訊，請參閱這個概觀主題最後的＜另請參閱＞一節。

SMB 直接傳輸具備下列需求：

* 至少需要兩部執行 Windows Server 2012 的電腦。 不需要安裝任何其他的功能 - 預設會啟用這項技術。
* 需要含有 RDMA 功能的網路介面卡。 這些介面卡目前可用於三種不同類型：iWARP、Infiniband 或 RoCE (透過聚合式乙太網路的 RDMA)。

## <a name="more-information"></a>其他資訊

下列清單提供網路上有關 SMB 的其他資源，以及 Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2016 中的相關技術。

* [Windows Server 的儲存空間](../storage.md)
* [適用于應用程式資料的向外延展檔案伺服器](../../failover-clustering/sofs-overview.md)
* [使用 SMB 直接傳輸改善檔案伺服器的效能](smb-direct.md)
* [部署透過 SMB 的 Hyper-v](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [部署 SMB 多重通道](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [為伺服器應用程式部署快速又有效率的檔案伺服器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB：疑難排解指南](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)
