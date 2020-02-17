---
title: 在 Windows Server 中使用 SMB 3 通訊協定的檔案共用概觀
description: 概述如何在 Windows Server 中使用 SMB 3 通訊協定進行檔案共用和檔案提供。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 01/10/2020
ms.localizationpriority: medium
ms.openlocfilehash: 416145a8c4ec20eaf46cf4b5ac88a0cdf38bdf33
ms.sourcegitcommit: 76469d1b7465800315eaca3e0c7f0438fc3939ed
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/13/2020
ms.locfileid: "75919890"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>在 Windows Server 中使用 SMB 3 通訊協定的檔案共用概觀

>適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題說明 Windows Server 2019、Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中的 SMB 3 功能 — 功能的實際用法、此版本中相較於舊版最重要的新功能或更新功能，以及硬體需求。 SMB 也是[軟體定義資料中心 (SDDC)](../../sddc.md) 解決方案 (例如儲存空間直接存取、儲存體複本等) 所使用的網狀架構通訊協定。 SMB 3.0 版是在 Windows Server 2012 中導入的，且在後續版本中持續以累加方式改進。

## <a name="feature-description"></a>功能說明

伺服器訊息區 (SMB) 通訊協定是網路檔案共用通訊協定，允許電腦上的應用程式讀取和寫入檔案，以及要求來自電腦網路中伺服器程式的服務。 SMB 通訊協定可以用於其 TCP/IP 通訊協定或其他網路通訊協定的最上方。 使用 SMB 通訊協定，應用程式 (或應用程式的使用者) 可以存取遠端伺服器上的檔案或其他資源。 這允許應用程式讀取、建立及更新遠端伺服器上的檔案。 SMB 也可以與任何設定用來接收 SMB 用戶端要求的伺服器程式進行通訊。 SMB 是軟體定義資料中心 (SDDC) 計算技術 (例如儲存空間直接存取、儲存體複本) 所使用的網狀架構通訊協定。 如需詳細資訊，請參閱 [Windows Server 軟體定義資料中心](../../sddc.md)。

## <a name="practical-applications"></a>實際應用

本節討論一些使用新 SMB 3.0 通訊協定的新實用方法。

* **適用於虛擬化的檔案存放裝置 (透過 SMB 的 Hyper-V™)** 。 Hyper-V 可以透過 SMB 3.0 通訊協定，在檔案共用中儲存虛擬機器檔案，例如設定、虛擬硬碟 (VHD) 檔案及快照。 這適用於獨立式檔案伺服器和叢集檔案伺服器，這些伺服器會將 Hyper-V 與叢集的共用檔案存放裝置搭配使用。
* **透過 SMB 的 Microsoft SQL Server**。 SQL Server 可以在 SMB 檔案共用上儲存使用者資料庫檔案。 目前適用於獨立式 SQL Server 的 SQL Server 2008 R2 支援此項。 即將推出的 SQL Server 版本將新增對於叢集 SQL 伺服器和系統資料庫的支援。
* **適用於使用者資料的傳統存放裝置**。 SMB 3.0 通訊協定會為資訊工作者 (或用戶端) 工作負載提供增強功能。 這些增強功能包括透過廣域網路 (WAN) 存取資料和保護資料免於遭受竊聽攻擊時，減少分公司使用者感受到應用程式延遲。

## <a name="new-and-changed-functionality"></a>新功能和變更的功能

以下幾節說明在 SMB 3 中新增的功能以及後續的更新。

## <a name="features-added-in-windows-server-2019-and-windows-10-version-1809"></a>在 Windows Server 2019 和 Windows 10 (1809 版) 中新增的功能

| 特色/功能  | 新功能或更新功能  | 摘要  |
| --------- | --------- | --------- |
| 在無法持續使用的檔案共用上要求直接寫入磁碟的能力 | 新增 | 為了進一步確保對檔案共用的寫入可一路經由軟體和硬體堆疊寫入至實體磁碟，然後才將寫入作業回報為完成，您可以使用 `NET USE /WRITETHROUGH` 命令或 `New-SMBMapping -UseWriteThrough` PowerShell Cmdlet 在檔案共用上啟用直接寫入。 使用直接寫入時，效能會受到一些影響；如需進一步的討論，請參閱部落格文章[控制 SMB 中的直接寫入行為](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-write-through-behaviors-in-smb/bc-p/1083417#M677)。 |

## <a name="features-added-in-windows-server-version-1709-and-windows-10-version-1709"></a>Windows Server (1709 版) 和 Windows 10 (1709 版) 中新增的功能

| 特色/功能  | 新功能或更新功能  | 摘要  |
| --------- | --------- | --------- |
| 已停用對檔案共用的來賓存取 | 新增 | SMB 用戶端已不再允許下列動作：來賓帳戶對遠端伺服器的存取；在提供無效的認證後，會回復為來賓帳戶。 如需詳細資訊，請參閱[在 Windows 中依預設停用 SMB2 中的來賓存取](https://support.microsoft.com/help/4046019/guest-access-in-smb2-disabled-by-default-in-windows-10-and-windows-ser)。 | 
| SMB 全域對應 | 新增 | 將遠端 SMB 共用對應至本機主機 (包括容器) 上的所有使用者都可存取的磁碟機代號。 若要讓資料磁碟區上的容器 I/O 能夠周遊遠端掛接點，則必須進行此作業。 請注意，對容器使用 SMB 全域對應時，容器主機上的所有使用者都可以存取遠端共用。 容器主機上執行的任何應用程式也都可以存取對應的遠端共用。 如需詳細資訊，請參閱[叢集共用磁碟區 (CSV)、儲存空間直接存取、SMB 全域對應的相關容器儲存空間支援](https://techcommunity.microsoft.com/t5/failover-clustering/container-storage-support-with-cluster-shared-volumes-csv/ba-p/372140)。 |
| SMB 方言控制 | 新增 | 現在，您可以設定登錄值來控制使用的最低 SMB 版本 (方言) 和最高 SMB 版本。 如需詳細資訊，請參閱[控制 SMB 方言](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024)。 |

## <a name="features-added-in-smb-311-with-windows-server-2016-and-windows-10-version-1607"></a>在 Windows Server 2016 和 Windows 10 (1607 版) 的 SMB 3.11 中新增的功能

| 特色/功能  | 新功能或更新功能  | 摘要  |
| --------- | --------- | --------- |
| SMB 加密     |   已更新      | 具有進階加密標準 Galois/計數器模式 (AES-GCM) 的 SMB 3.1.1 加密，比使用 AES-CCM 的 SMB 簽署或舊版 SMB 加密更為快速。   |
| 目錄快取 | 新增 | SMB 3.1.1 包含目錄快取的增強功能。 Windows 用戶端現在可快取的目錄比以往大得多，約 500K 個項目。 Windows 用戶端會嘗試以 1 MB 的緩衝區進行目錄查詢，以減少來回行程並提升效能。 |
| 預先驗證完整性 | 新增 |  在 SMB 3.1.1 中，預先驗證完整性可進一步防止攔截式攻擊者利用 SMB 的連線建立和驗證訊息進行竄改。 如需詳細資訊，請參閱 [Windows 10 中的 SMB 3.1.1 預先驗證完整性](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10)。 |
| SMB 加密的改進 | 新增 | SMB 3.1.1 提供了相關機制，可透過 AES-128-CCM 和 AES-128-GCM 的選項來交涉每個連線的加密演算法。 AES-128-GCM 是新版 Windows 的預設值，而較舊版本會繼續使用 AES-128-CCM。 |
| 輪流叢集升級支援 | 新增 | 藉由讓 SMB 能夠在升級的過程中對叢集支援不同的 SMB 最高版本，來啟用[輪流叢集升級](../../failover-clustering/cluster-operating-system-rolling-upgrade.md)。 如需讓 SMB 使用不同版本 (方言) 的通訊協定進行通訊的詳細資訊，請參閱部落格文章[控制 SMB 方言](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024)。 |
| Windows 10 中的 SMB 直接傳輸用戶端支援 | 新增 | Windows 10 企業版、Windows 10 教育版和 Windows 10 工作站專業版現在包含 SMB 直接傳輸用戶端支援。 |
| FileNormalizedNameInformation API 呼叫的原生支援 | 新增 | 新增查詢標準化檔案名稱的原生支援。 如需詳細資訊，請參閱 [FileNormalizedNameInformation](https://docs.microsoft.com/openspecs/windows_protocols/ms-fscc/20bcadba-808c-4880-b757-4af93e41edf6)。 |

如需其他詳細資訊，請參閱部落格文章 [Windows Server 2016 Technical Preview 2的 SMB 3.1.1 中的新功能](https://docs.microsoft.com/archive/blogs/josebda/whats-new-in-smb-3-1-1-in-the-windows-server-2016-technical-preview-2)。

## <a name="features-added-in-smb-302-with-windows-server-2012-r2-and-windows-81"></a>在 Windows Server 2012 R2 和 Windows 8.1 的 SMB 3.02 中新增的功能

| 特色/功能  | 新功能或更新功能  | 摘要  |
| --------- | --------- | --------- |
| 自動重新平衡向外延展檔案伺服器用戶端     |   新增      | 改善向外延展檔案伺服器的延展性與管理性。 SMB 用戶端連線是以每個檔案共用 (而非每個伺服器) 的方式追蹤，接著再將用戶端重新導向至最方便存取檔案共用使用之磁碟區的叢集節點。 由於檔案伺服器節點之間的重新導向流量減少，因此可以提升效率。 起始一個連線和重新設定叢集存放裝置都會將用戶端重新導向。    |
| 透過 WAN 的效能   | 已更新  | 當您使用檔案總管從遠端電腦上的某個位置對相同伺服器上的另一個複本進行遠端複製時，可以利用 Windows 8.1 和 Windows 10 透過 SMB 支援提供的改良式 CopyFile SRV_COPYCHUNK。 您只需透過網路複製少量的中繼資料 (每 16MiB 只需傳輸 1/2KiB 的檔案資料)。 這將可大幅改善效能。 這是 SMB 在 OS 層級和檔案總管層級上的差異。 |
| SMB 直接傳輸     |   已更新      | 透過提高以少量 I/O 裝載工作負載的效率，改善少量 I/O 工作負載的效能 (例如虛擬機器中的線上交易處理 (OLTP) 資料庫)。 這些改良在使用較高速的網路介面時很明顯，例如 40 Gbps 乙太網路與 56 Gbps InfiniBand。  |
| SMB 頻寬限制 | 新增 | 現在，您可以使用 [Smb-BandwidthLimit](https://docs.microsoft.com/powershell/module/smbshare/set-smbbandwidthlimit) 來設定三種類別的頻寬限制：VirtualMachine (透過 SMB 的 Hyper-V 流量)、LiveMigration (透過 SMB 的 Hyper-V 即時移轉流量) 或預設值 (所有其他類型的 SMB 流量)。

若要進一步了解 Windows Server 2012 R2 中新增和變更的 SMB 功能，請參閱 [Windows Server 中新增的 SMB 功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>)。

## <a name="features-added-in-smb-30-with-windows-server-2012-and-windows-8"></a>在 Windows Server 2012 和 Windows 8 的 SMB 3.0 中新增的功能

| 特色/功能  | 新功能或更新功能  | 摘要  |
| --------- | --------- | --------- |
| SMB 透明容錯移轉     |   新增    | 讓系統管理員能夠在叢集檔案伺服器中執行節點的硬體或軟體維護，而不需中斷正在這些檔案共用上儲存資料的伺服器應用程式。 此外，如果叢集節點上發生硬體或軟體失敗，SMB 用戶端會以透明的方式重新連線到其他叢集節點，而不需中斷正在這些檔案共用上儲存資料的伺服器應用程式。        |
| SMB 向外延展     |   新增      | 在一部向外延展檔案伺服器上支援多個 SMB 執行個體。 使用叢集共用磁碟區 (CSV) 版本 2，系統管理員可以建立檔案共用，透過檔案伺服器叢集中的所有節點，利用直接 I/O 來提供資料檔案的同時存取。 這樣可以提供較佳的網路頻寬使用率，以及檔案伺服器用戶端的負載平衡，並將伺服器應用程式的效能最佳化。  |
| SMB 多重通道     |   新增      |  如果 SMB 用戶端與伺服器之間有多個路徑可供使用，則能彙總網路頻寬和網路容錯性。 這樣能讓伺服器應用程式可以充分利用所有可用的網路頻寬，並能彈性處理網路失敗。<br><br>相較於舊版的 SMB，SMB 3 中的 SMB 多重通道可促使效能大幅提升。 |
| SMB 直接傳輸     |   新增      | 支援使用含有 RDMA 功能的網路介面卡，而且在使用非常少量的 CPU 時，以非常低的延遲全速運作。 對於像是 Hyper-V 或 Microsoft SQL Server 的工作負載，這讓遠端檔案伺服器能夠類似本機存放裝置。<br><br>相較於舊版的 SMB，SMB 3 中的 SMB 直接傳輸可促使效能大幅提升。  |
| 伺服器應用程式的效能計數器     |   新增      |  新的 SMB 效能計數器可以針對每個共用提供有關輸送量、延遲及每秒 I/O (IOPS) 的詳細資訊，讓系統管理員能夠分析儲存資料所在的 SMB 檔案共用的效能。 這些計數器是特別針對伺服器應用程式而設計 (例如 Hyper-V 和 SQL Server)，它們可以在遠端檔案共用上儲存檔案。      |
| 效能最佳化    |  已更新   | SMB 用戶端和伺服器都已針對小型隨機讀取/寫入 I/O 進行最佳化，這在像是 SQL Server OLTP 的伺服器應用程式中很常見。 此外，預設會開啟大型傳輸單元最大值 (MTU)，明顯增強大型循序傳輸的效能，例如 SQL Server 資料倉儲、資料庫備份或還原、部署或複製虛擬硬碟。 |
| SMB 特定的 Windows PowerShell Cmdlet     |   新增      |  利用適用於 SMB 的 Windows PowerShell Cmdlet，系統管理員可以從命令列，以端對端方式，在檔案伺服器上管理檔案共用。   |
| SMB 加密     |   新增      | 提供 SMB 資料的端對端加密，並保護資料以免在不受信任的網路上遭到竊聽。 不需要新的部署成本，也不需要網際網路通訊協定安全性 (IPsec)、特定的硬體或 WAN 加速器。 這可能會以每個共用為基礎進行設定，或者針對整個檔案伺服器進行設定，而且可能會針對資料周遊不受信任網路的各種案例加以啟用。 |
| SMB 目錄租用     |  新增 | 增強分公司的應用程式回應時間。 使用目錄租用，從用戶端到伺服器的來回時間會降低，因為中繼資料是從存留期間較長的目錄快取中所擷取。 快取的一致性可以維持，因為當伺服器上的目錄資料變更時，用戶端會收到通知。 目錄租用適用於主資料夾 (讀取/寫入且無共用) 和發行集 (唯讀且有共用) 的案例。    |
| 透過 WAN 的效能   | 新增   | 目錄隨機鎖定 (oplock) 和 oplock 租用是在 SMB 3.0 中導入的。 針對一般的辦公室/用戶端工作負載，oplock/租用可減少約 15% 的網路來回行程。<br><br>在 SMB 3 中，SMB 的 Windows 實作已經過調整，而可改善用戶端上的快取行為，並且能夠推送更高的輸送量。<br><br>SMB 3 改進了 CopyFile() API 以及相關聯的工具 (例如 Robocopy)，可透過網路推送遠多於以往的資料。 |
| 安全方言交涉 | 新增 | 有助於防止攔截式攻擊者試圖降級方言交涉。 其概念是防止不法人士將用戶端與伺服器之間最初交涉的方言和功能降級。 如需詳細資訊，請參閱 [SMB3 安全方言交涉](https://docs.microsoft.com/archive/blogs/openspecification/smb3-secure-dialect-negotiation)。 請注意，此功能在 SMB 3.1.1 中已取代為 [Windows 10 中的 SMB 3.1.1 預先驗證完整性](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10)功能。 |


## <a name="hardware-requirements"></a>硬體需求

SMB 透明容錯移轉具備下列需求：

* 執行 Windows Server 2012 或 Windows Server 2016 的容錯移轉叢集，且至少已設定兩個節點。 這個叢集必須傳遞驗證精靈中所含的叢集驗證測試。
* 檔案共用必須利用持續可用性 (CA) 屬性來建立，這是預設值。
* 檔案共用必須建立於 CSV 磁碟區路徑上，以達到 SMB 向外延展。
* 用戶端電腦必須執行 Windows® 8 或 Windows Server 2012，且兩者都必須包含支援持續可用性的更新 SMB 用戶端。

> [!NOTE]
> 下層用戶端可連線至含有 CA 屬性的檔案共用，但是這些用戶端將不支援透明容錯移轉。

SMB 多重通道具備下列需求：

* 至少有兩部執行 Windows Server 2012 的電腦。 不需要安裝任何其他的功能 - 預設會啟用這項技術。
* 如需所建議網路設定的相關資訊，請參閱這個概觀主題最後的＜另請參閱＞一節。

SMB 直接傳輸具備下列需求：

* 至少有兩部執行 Windows Server 2012 的電腦。 不需要安裝任何其他的功能 - 預設會啟用這項技術。
* 需要含有 RDMA 功能的網路介面卡。 這些介面卡目前可用於三種不同類型：iWARP、Infiniband 或 RoCE (透過聚合式乙太網路的 RDMA)。

## <a name="more-information"></a>其他資訊

下列清單提供網路上有關於 Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2016 中的 SMB 和相關技術的資源。

* [Windows Server 的儲存空間](../storage.md)
* [用於應用程式資料的向外延展檔案伺服器](../../failover-clustering/sofs-overview.md)
* [使用 SMB 直接傳輸改善檔案伺服器的效能](smb-direct.md)
* [部署透過 SMB 的 Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [部署 SMB 多重通道](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [部署適用於伺服器應用程式且快速又有效率的檔案伺服器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB：疑難排解指南](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)
