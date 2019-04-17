---
title: 使用容錯移轉叢集的叢集共用磁碟區
description: 如何使用中的容錯移轉叢集的叢集共用磁碟區。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f5bd0ad05bdc2573a5ea0abbe165de2d3e7f5c8f
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233514"
---
# <a name="use-cluster-shared-volumes-in-a-failover-cluster"></a>使用容錯移轉叢集的叢集共用磁碟區

>適用於： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2016

叢集共用磁碟區 (CSV) 啟用同時有讀取/寫入權限與 NTFS 磁碟區佈建相同 LUN （磁碟） 來容錯移轉叢集的多個節點。 （在 Windows Server 2012 R2、 磁碟可佈建為 NTFS 或彈性檔案系統 (ReFS)。）使用 CSV、 叢集的角色可以容錯移轉快速從一個節點至另一個節點而不需要的磁碟機擁有權變更或卸載及裝載磁碟區。 CSV 也可協助簡化可能大量 Lun 容錯移轉叢集裡的管理。

CSV 提供一般用途、 叢集檔案系統、 分層 NTFS （或 Windows Server 2012 R2 中的 ReFS） 上方。 CSV 應用程式包括：

- 叢集的叢集 HYPER-V 虛擬機器時的虛擬硬碟 (VHD) 檔案
- 若要儲存應用程式資料的向外延展檔案伺服器叢集角色的向外延展檔案共用。 此角色的應用程式資料的範例包括-HYPER-V 虛擬機器檔案和 Microsoft SQL Server 資料。 （請注意向外延展檔案伺服器不支援 ReFS）。如需向外延展檔案伺服器的詳細資訊，請參閱[應用程式資料的向外延展檔案伺服器](sofs-overview.md)。

>[!NOTE]
>CSV 不支援 SQL Server 2012 和較早版本的 SQL Server 中的 Microsoft SQL Server 叢集的工作量。

在 Windows Server 2012 中已大幅增強 CSV 功能。 例如，已移除 Active Directory 網域服務相依關係。 支援已新增**chkdsk**中的功能改良功能、 防毒及備份應用程式與互通性和整合一般儲存體功能，例如 BitLocker 加密磁碟區以及存放空間。 如需 Windows Server 2012 中已導入的 CSV 功能的概觀，請參閱[容錯移轉叢集的新功能 Windows Server 2012 \[redirected\]](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>)。

Windows Server 2012 R2 介紹其他功能，例如分散式 CSV 擁有權，增加恢復能力透過伺服器服務，提供更多彈性隨著您可以更妥善地配置 CSV 快取的實體記憶體的可用性diagnosibility，並包含 ReFS 與 deduplication 支援增強型互通性。 如需詳細資訊，請參閱[在容錯移轉叢集的新功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>)。

>[!NOTE]
>使用 CSV 資料 deduplication 虛擬桌面基礎結構 (VDI) 案例的相關資訊，請參閱[部署資料 Deduplication VDI 能夠儲存在 Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/deploying-data-deduplication-for-vdi-storage-in-windows-server-2012-r2.aspx)和[延伸資料 Deduplication 的部落格文章中的新工作負載Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/07/31/extending-data-deduplication-to-new-workloads-in-windows-server-2012-r2.aspx)。

## <a name="review-requirements-and-considerations-for-using-csv-in-a-failover-cluster"></a>檢閱需求和使用 CSV 容錯移轉叢集裡的考量

使用之前 CSV 容錯移轉叢集裡，請檢閱網路、 儲存及其他需求和考量此區段中。

### <a name="network-configuration-considerations"></a>網路組態注意事項

當您設定支援 CSV 網路時，請考慮下列。

- **多個網路和多個網路介面卡**。 若要啟用網路失敗的情況下的容錯能力，建議多個叢集網路執行 CSV 流量或您設定一起合作可以使用網路介面卡。
    
    如果不使用叢集中的網路連線叢集節點，應停用它們。 例如，我們建議您停用來防止在這些網路上的 CSV 流量的叢集使用 iSCSI 網路。 若要停用的網路中，在容錯移轉叢集管理員中，選取**網路**、 選取網路、 選取 [**屬性**] 動作，然後選取**不允許此網路上的叢集網路通訊**。 或者，您可以使用[Get ClusterNetwork](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternetwork?view=win10-ps) Windows PowerShell 指令程式來設定網路的**角色**屬性。
- **網路介面卡屬性**。 在所有執行叢集中通訊的介面卡的內容，請確定已啟用下列設定：

  - **Microsoft 網路的用戶端**及**檔案和印表機共用 Microsoft 網路的**。 這些設定支援伺服器訊息區塊 (SMB) 3.0、 是預設用來執行 CSV 各節點之間的流量。 若要啟用 SMB，也請確定 「 伺服器 」 服務與工作站服務正在執行時設定成自動啟動每個叢集節點上。

    >[!NOTE]
    >在 Windows Server 2012 R2 中，有每個容錯移轉叢集節點的多伺服器服務執行個體。 沒有預設的執行個體，可處理來自存取一般的檔案共用的 SMB 用戶端的傳入流量和只會處理間節點 CSV 流量的第二個 CSV 執行個體。 此外，如果在節點上的伺服器服務變成狀況不良，CSV 擁有權自動轉換成另一個節點。

    SMB 3.0 包含 SMB 多重通道與 SMB 直接功能，可讓 CSV 流量資料流跨多個網路叢集中的及運用支援遠端直接記憶體存取 (RDMA) 的網路介面卡。 根據預設，SMB 多重通道用於 CSV 流量。 如需詳細資訊，請參閱[伺服器訊息區塊概觀 （英文）](../storage/file-server/file-server-smb-overview.md)。
  - **Microsoft 容錯移轉叢集虛擬介面卡效能篩選**。 此設定可改善節點能夠執行時，它會需要連絡 CSV，例如時連線失敗防止節點直接連線至 CSV 磁碟 I/O 重新導向。 如需詳細資訊，請參閱本主題稍後的[相關 I/O 同步處理及 CSV 通訊中的 I/O 重新導向](#about-i/o-synchronization-and-i/o-redirection-in-csv-communication)。
- **叢集網路優先處理順序**。 一般建議您不要變更網路叢集中設定喜好設定。
- **IP 子網路組態**。 任何特定的子網路設定不是必要的網路中使用 CSV 的節點。 CSV 可支援 multisubnet 叢集。
- **原則為基礎的服務品質 (QoS)**。 我們建議使用 CSV 時設定的 QoS 優先順序原則和每個節點的網路流量的最小頻寬原則。 如需詳細資訊，請參閱[服務品質 (QoS)](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831679(v%3dws.11)>)。
- **儲存網路**。 如儲存網路建議，請檢閱儲存裝置廠商所提供的指導方針。 如需其他 CSV 的存放區的相關考量，請參閱本主題稍後的[儲存區與磁碟組態需求](#storage-and-disk-configuration-requirements)。

如需硬體、 網路與容錯移轉叢集的儲存需求的概觀，請參閱[容錯移轉叢集的硬體需求及儲存選項](clustering-requirements.md)。

#### <a name="about-io-synchronization-and-io-redirection-in-csv-communication"></a>關於 I/O 同步處理及 CSV 通訊中的 I/O 重新導向

- **I/O 同步處理**： CSV 可讓多個節點至相同的共用儲存裝置的同時讀取/寫入存取權。 節點在執行時磁碟輸入/輸出 (I/O) CSV 磁碟區上，節點直接與儲存區，例如會透過進行通訊儲存區域網路 (SAN)。 不過，在任何時候，單一節點 （稱為協調者節點）"擁有 「 LUN 相關聯的實體磁碟資源。 CSV 磁碟區的協調者節點會顯示在 [容錯移轉叢集管理員為**擁有者節點**底下**的磁碟**。 它也會出現在[取得 ClusterSharedVolume](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustersharedvolume?view=win10-ps) Windows PowerShell cmdlet 的輸出。

  >[!NOTE]
  >在 Windows Server 2012 R2 中，根據每個節點擁有 CSV 磁碟區數目的容錯移轉叢集節點之間平均分散 CSV 擁有權。 此外，擁有權會自動重新平衡有 CSV 容錯移轉等情況、 節點 rejoins 叢集、 您新增節點至叢集、 重新啟動叢集節點或具有已關閉之後開始的容錯移轉叢集時。

  CSV 磁碟區上的檔案系統中發生特定的細微變更、 時此中繼資料都必須在每個存取 LUN，不只在單一協調員節點上的實體節點進行同步處理。 例如入門、 建立或刪除的 CSV 磁碟區上的虛擬機器時或移轉虛擬機器時，這項資訊必須在每個存取虛擬機器的實體節點進行同步處理。 這些中繼資料的 update 作業在使用 SMB 3.0 同時發生跨叢集網路。 這些作業不需要與共用存放裝置進行通訊的所有實體節點。

- **I/O 重新導向**： 儲存連線失敗，以及某些儲存作業可防止在指定的節點直接與儲存區進行通訊。 時節點不通訊的儲存區維護函數節點會透過叢集網路磁碟 I/O 重新導向至目前掛磁碟上的 [協調器] 節點。 如果目前的協調者節點遭受儲存連線失敗，所有的磁碟 I/O 作業排入佇列暫時時協調員節點以建立新的節點。

伺服器會使用其中一個下列 I/O 重新導向模式，根據情況而定：

- **檔案系統重新導向**重新導向做為每個磁碟區 — 例如時 CSV 快照集所時所採取的備份的應用程式 CSV 大量手動處於重新導向的 I/O 模式。
- **封鎖重新導向**重新導向做為層級的檔案封鎖 — 例如時儲存連線是以大量遺失。 封鎖重新導向做為比檔案系統重新導向大幅快。

在 Windows Server 2012 R2 中，您可以針對每個節點逐一檢視 CSV 大量的狀態。 例如，您可以看到 I/O 是直接或重新導向，或是否 CSV 磁碟區無法使用。 如果 CSV 大量 I/O 重新導向模式，您也可以檢視原因。 使用 Windows PowerShell 指令程式**取得 ClusterSharedVolumeState**檢視此資訊。

>[!NOTE]
> * 在 Windows Server 2012] 項改進功能在 CSV 設計 CSV 執行多個作業中直接 I/O 模式比發生在 Windows Server 2008 R2。
> * SMB 3.0 功能，例如 SMB 多重通道和 SMB 直接與 CSV 整合，因為重新導向的 I/O 流量可以跨多個叢集網路資料流。
> * 您應該規劃您可用於潛在增加協調員節點的網路流量期間 I/O 重新導向的叢集網路。

### <a name="storage-and-disk-configuration-requirements"></a>儲存與磁碟設定需求

若要使用 CSV，儲存空間及磁碟必須符合下列需求：

- **檔案系統格式**。 在 Windows Server 2012 R2 CSV 大量磁碟或儲存空間必須基本磁碟分割與 NTFS 或 ReFS。 在 Windows Server 2012 CSV 大量磁碟或儲存空間必須與 NTFS 分割基本磁碟。

  CSV 有下列額外需求：
    
  - 在 Windows Server 2012 R2 中，您無法使用的格式是 FAT 或 FAT32 CSV 的磁碟。
  - 在 Windows Server 2012，則無法使用的磁碟來的格式是 FAT、 FAT32 或 ReFS CSV。
  - 如果您想要用於 CSV 儲存空間，您可以設定簡單空格或鏡像空格。 在 Windows Server 2012 R2 中，您也可以設定同位空格。 （在 Windows Server 2012 CSV 不支援同位空格。）
  - CSV 能作為仲裁見證磁碟。 如需叢集仲裁的詳細資訊，請參閱[了解仲裁中直接儲存空間](../storage/storage-spaces/understand-quorum.md)。
  - 新增磁碟以 csv 格式之後，將它指定 CSVFS 格式 （適用於 CSV 檔案系統）。 如此叢集和來區分 CSV 儲存來自其他 NTFS 或 ReFS 儲存其他軟體。 一般而言，CSVFS 支援 NTFS 或 ReFS 相同的功能。 不過，某些功能不支援。 例如，在 Windows Server 2012 R2 中，您無法在線上啟用壓縮 CSV。 在 Windows Server 2012，就無法啟用資料 deduplication 或在 CSV 壓縮。
- **叢集中的資源類型**。 CSV 數量，您必須使用實體磁碟資源類型。 根據預設，會自動設定新增叢集儲存至磁碟或儲存空間以這種方式。
- **選擇的 CSV 磁碟或在叢集中存放區中的其他磁碟**。 選擇一個或多個磁碟的叢集的虛擬機器時, 請考慮每個磁碟的使用方式。 如果磁碟用來儲存檔案所建立的 HYPER-V，如 VHD 檔案或設定檔，您可以選擇從 CSV 磁碟或在叢集中存放區中其他可用的磁碟。 如果磁碟都是直接是實體磁碟附加至虛擬機器 （也稱為傳遞磁碟）、 無法選擇 CSV 磁碟，而您必須選擇從在叢集中存放區中其他可用的磁碟。
- **識別磁碟的路徑名稱**。 磁碟 csv 檔案中的所識別的路徑名稱。 每個路徑看起來節點的系統磁碟機上為**\\ClusterStorage**資料夾下的編號磁碟區。 此路徑是檢視從叢集中的任何節點時相同。 您可以視來重新命名磁碟區。

CSV 的儲存需求，請檢閱儲存裝置廠商所提供的指導方針。 額外儲存空間的 CSV 的規劃考量，請參閱本主題稍後的[計劃使用容錯移轉叢集裡的 CSV](#plan-to-use-csv-in-a-failover-cluster) 。

### <a name="node-requirements"></a>節點需求

若要使用 CSV，您節點必須符合下列需求：

- **系統磁碟的磁碟機代號**。 所有節點上的系統磁碟機代號必須相同。
- **驗證通訊協定**。 必須在所有節點上啟用 NTLM 通訊協定。 這是預設啟用。

## <a name="plan-to-use-csv-in-a-failover-cluster"></a>使用 CSV 容錯移轉叢集裡的計劃

本節列出的規劃考量及使用 CSV 中執行 Windows Server 2012 R2 或 Windows Server 2012 的容錯移轉叢集的建議。

>[!IMPORTANT]
>尋求建議的儲存裝置廠商有關如何設定為 CSV 您特定的儲存區的單位。 本主題中的資訊的不同的存放裝置廠商的建議，如果使用中儲存裝置廠商的建議。

### <a name="arrangement-of-luns-volumes-and-vhd-files"></a>Lun、 磁碟區及 VHD 檔案的排列方式

若要讓 CSV 提供儲存的叢集的虛擬機器時的最佳用法，相當有用檢閱您如何將排列 Lun （磁碟） 當您設定實體伺服器。 當您設定相對應的虛擬機器時時，請嘗試以類似方式排列的 VHD 檔案。

請考慮實體伺服器的您將組織的磁碟和檔案，如下所示：

- 包括] 頁面上的檔案，一部實體磁碟上的系統檔案
- 在另一個實體磁碟上的資料檔案

對等的叢集虛擬機器，您應該以類似方式組織的磁碟區和檔案：

- 上一個 CSV VHD 檔案中包含頁面檔案的系統檔案
- 在另一個 CSV VHD 檔案中的資料檔案

如果您新增其他虛擬機器，請儘可能，您應該保留相同的排列方式的 Vhd 該虛擬機器上。

### <a name="number-and-size-of-luns-and-volumes"></a>數目與大小 Lun 和磁碟區

當您規劃使用 CSV 容錯移轉叢集的儲存設定時，請考慮下列建議：

- 若要決定多少個 Lun 設定，請洽詢儲存裝置廠商。 例如儲存裝置廠商可能會建議您具有一個分割區設定每個 LUN 並在其上放置一個 CSV 大量。
- 有可支援在單一 CSV 磁碟區的虛擬機器數目沒有限制。 但是，您應該考慮想要有叢集和每部虛擬機器的工作負載 （每秒 I/O 操作） 中的虛擬機器數目。 請考量以下範例：

  - 一個組織已部署會支援虛擬桌面基礎結構 (VDI) 這是較淺工作負載的虛擬機器。 叢集中使用高效能儲存。 叢集系統管理員之後諮詢的存放裝置廠商，以決定會放置在每個 CSV 磁碟區的虛擬機器較大數目。
  - 其他組織已部署可支援使用頻繁資料庫應用程式，這是加重工作負載的虛擬機器的數目。 叢集中使用較低執行儲存。 叢集系統管理員之後諮詢的存放裝置廠商，以決定會放置在較小的數字的每個 CSV 磁碟區的虛擬機器。
- 當您規劃在特定的虛擬機器儲存設定時，請考慮磁碟需求的服務、 應用程式或支援虛擬機器的角色。 了解這些需求可協助您避免可能會導致效能不佳的磁碟爭用情形。 虛擬機器儲存設定密切應該類似您可以使用相同的服務、 應用程式或角色執行實體伺服器的儲存設定。 如需詳細資訊，請參閱本主題中前面的[排列方式的 Lun、 磁碟區及 VHD 檔案](#arrangement-of-luns,-volumes,-and-vhd-files)。

    您也可以藉由儲存獨立的實體硬碟的數目與降低磁碟爭用情形。 據以，選擇您儲存的硬體和洽詢以最佳化效能的存放裝置廠商。
- 依據叢集工作量和其 I/O 作業需要，您可以考慮設定只能存取每個 LUN、 其他虛擬機器不具連線以及會改用專用計算作業時的虛擬機器的百分比。

## <a name="add-a-disk-to-csv-on-a-failover-cluster"></a>新增至 CSV 的磁碟上的容錯移轉叢集

在 [容錯移轉叢集的預設會啟用 CSV 功能。 若要新增 CSV 磁碟，您必須將磁碟新增至叢集中 （如果未加入） 的**可用儲存**群組與叢集上再新增至 CSV 的磁碟。 您可以使用容錯移轉叢集管理員] 或 [容錯移轉叢集 Windows PowerShell cmdlet 來執行這些程序。

### <a name="add-a-disk-to-available-storage"></a>將磁碟新增至可用的儲存裝置

1. 在 [容錯移轉叢集管理員] 中的主控台樹狀目錄展開叢集的名稱，然後展開 [**存放區**。
2. 以滑鼠右鍵按一下**磁碟**，然後再選取 [**新增磁碟**。 顯示可用於容錯移轉叢集裡新增磁碟出現的清單。
3. 選取磁碟或磁碟您想要新增，然後再選取 **[確定]**。

    磁碟現在已指派給**可用儲存**群組。

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-available-storage"></a>Windows PowerShell 相等命令 （將磁碟新增至可用的儲存裝置）

下列 Windows PowerShell cmdlet 或指令程式執行前述的程序相同的功能。 雖然它們可能會出現 word 進行三重包裝跨以下幾行因為格式化的條件約束，在單一行中，輸入每個指令程式。

下面範例說明準備好要新增至該叢集，磁碟，然後將它們新增至**可用儲存**群組。

```PowerShell
Get-ClusterAvailableDisk | Add-ClusterDisk
```

### <a name="add-a-disk-in-available-storage-to-csv"></a>將磁碟中可用的儲存空間新增至 CSV

1. 在容錯移轉叢集管理員] 中的主控台樹狀目錄中，依序展開 [叢集的名稱、 展開 [**儲存**]，然後選取 [**磁碟**。
2. 選取下列其中一個或多個指派給**可用儲存空間**的磁碟以滑鼠右鍵按一下選取項目，並再選取 [**新增至叢集共用磁碟區**。

    磁碟現在已指派給在叢集中的**叢集共用大量**群組。 磁碟公開到每個叢集節點以 %systemdisk%ClusterStorage 資料夾下編號磁碟區 （掛接點為單位）。 磁碟區出現在 CSVFS 檔案系統中。

>[!NOTE]
>您可以重新命名 CSV 磁碟區 %systemdisk%ClusterStorage 資料夾中。

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-csv"></a>Windows PowerShell 相等命令 （為 CSV 新增磁碟）

下列 Windows PowerShell cmdlet 或指令程式執行前述的程序相同的功能。 雖然它們可能會出現 word 進行三重包裝跨以下幾行因為格式化的條件約束，在單一行中，輸入每個指令程式。

下列範例會新增*叢集磁碟 1*中**可用儲存**CSV 本機叢集上。

```PowerShell
Add-ClusterSharedVolume –Name "Cluster Disk 1"
```

## <a name="enable-the-csv-cache-for-read-intensive-workloads-optional"></a>啟用 CSV 快取的密集讀取工作負載 （選用）

CSV 快取提供的寫入透過快取為配置系統記憶體 (RAM) 快取區塊層級的唯讀未緩衝的 I/O 作業。 （未緩衝的 I/O 作業是未快取快取管理員）。這可以提升效能的應用程式，例如 HYPER-V，這會存取 VHD 時未緩衝的 I/O 作業。 CSV 快取可以提升效能的讀取要求不含快取寫入要求。 啟用 CSV 快取十分有用的向外延展檔案伺服器案例。

>[!NOTE]
>我們建議您啟用所有叢集的 HYPER-V 和向外延展檔案伺服器部署的 CSV 快取。

在 Windows Server 2012 中預設會停用 CSV 快取。 在 Windows Server 2012 R2 中，預設會啟用 CSV 快取。 不過，您仍必須配置齊備封鎖快取的大小。

下表說明控制 CSV 快取的兩個組態設定。

<table>
<thead>
<tr class="header">
<th>在 Windows Server 2012 R2 中的屬性名稱</th>
<th>在 Windows Server 2012 的屬性名稱</th>
<th>描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>BlockCacheSize</strong></td>
<td><strong>SharedVolumeBlockCacheSizeInMB</strong></td>
<td>這是叢集的一般屬性可讓您以預定 CSV 快取叢集中的每個節點上定義保留的記憶體 （以 mb 為單位）。 例如，定義的值為 512，則 512 MB 的系統記憶體會保留在每個節點上。 （在許多叢集配備 512 MB 是建議的值）。預設值是 0 （停用）。</td>
</tr>
<tr class="even">
<td><strong>EnableBlockCache</strong></td>
<td><strong>CsvEnableBlockCache</strong></td>
<td>這是私人叢集實體磁碟資源的屬性。 它可讓您啟用 CSV 快取會新增至 CSV 個別的磁碟上。 在 Windows Server 2012 中的預設設定是 0 （停用）。 若要啟用 CSV 快取磁碟上的，設定的值是 1。 根據預設，在 Windows Server 2012 R2 中，會啟用此設定。</td>
</tr>
</tbody>
</table>

您可以透過新增下**叢集 CSV 大量快取**計數器監視效能監視器] 中的 CSV 快取。

#### <a name="configure-the-csv-cache"></a>設定 CSV 快取

1. 以管理員身分啟動 Windows PowerShell。
2. 若要定義*512* MB 保留在每個節點的快取，請輸入下列命令：

    - Windows Server 2012 r2：

        ```PowerShell
        (Get-Cluster).BlockCacheSize = 512  
        ```

    - Windows server 2012：

        ```PowerShell
        (Get-Cluster).SharedVolumeBlockCacheSizeInMB = 512  
        ```
3. 在 Windows Server 2012] 以啟用上名為*叢集磁碟 1*CSV CSV 快取，請輸入下列項目：

    ```PowerShell
    Get-ClusterSharedVolume "Cluster Disk 1" | Set-ClusterParameter CsvEnableBlockCache 1
    ```

>[!NOTE]
> * 您可以在 Windows Server 2012] 配置僅 20%的總的實體 RAM 的 CSV 快取。 在 Windows Server 2012 R2 中，您可以配置 80%。 向外延展檔案伺服器通常不受限制的記憶體，因為您可以完成大型的效能提升 CSV 快取使用額外記憶體。
> * 若要避免資源爭用的情形，您應該之後重新啟動每個節點叢集中的修改配置給 CSV 快取的記憶體。 在 Windows Server 2012 R2 中，則不再需要重新啟動。
> * 啟用或停用在個別的磁碟，才會生效，設定 CSV 快取後您必須離線取得實體磁碟資源，以及使重新連線。 （根據預設，在 Windows Server 2012 R2 CSV 快取是已啟用）。 
> * 如需包含效能計數器的相關資訊的 CSV 快取的詳細資訊，請參閱部落格文章[如何啟用 CSV 快取](https://blogs.msdn.microsoft.com/clustering/2013/07/19/how-to-enable-csv-cache/)。

## <a name="back-up-csv"></a>備份 CSV

有多個方法以備份儲存在 CSV 容錯移轉叢集裡的資訊。 您可以使用 Microsoft 備份應用程式或非 Microsoft 應用程式。 一般而言，CSV 不會強制特殊的備份需求以外的叢集與 NTFS 或 ReFS 格式化的儲存空間。 CSV 備份也不會干擾其他 CSV 儲存作業。

當您選取 [備份應用系統和備份排程 CSV，應考慮下列因素：

- CSV 大量的磁碟區層級備份可執行從任何連接至 CSV 大量的節點。
- 您備份的應用程式可以使用軟體快照或硬體快照集。 根據您備份的應用程式以支援它們的能力，備份可以使用應用程式一致且一致損毀的磁碟區陰影複製服務 (VSS) 快照。
- 如果您要備份 CSV 具有多個執行的虛擬機器時，通常應該選擇 [管理作業系統型備份方法。 如果您備份的應用程式支援、 多個虛擬機器可以備份同時。
- CSV 支援 Windows Server 2012 備份 R2、 Windows Server 2012 Backup 或 Windows Server 2008 R2 Backup 正在執行的備份要求者。 不過，Windows Server Backup 通常提供僅限基本備份解決方案，可能不適合具有較大的叢集的組織。 Windows Server Backup 不支援應用程式一致的虛擬機器備份 CSV 上。 它支援一致損毀的磁碟區層級備份僅。 （如果您將損毀一致的備份還原、 虛擬機器會在同一個狀態與其如果虛擬機器鎖損毀確切可用的備份。）將會成功的 CSV 磁碟區上的虛擬機器的備份，但會指出本功能不支援記錄錯誤事件。
- 您可能需要系統管理認證時備份的容錯移轉叢集。

>[!IMPORTANT]
>請務必謹慎檢閱什麼資料應用程式備份會備份和還原它支援哪些 CSV 功能並在每個應用程式的資源需求叢集節點。

>[!WARNING]
>如果您需要還原到 CSV 大量的備份資料，請注意功能及限制的備份應用系統來維護和不同的叢集節點都還原應用程式一致的資料。 例如，與某些應用程式如果 CSV 還原其中 CSV 大量已備份的節點與不同節點上可能不經意會覆寫其中還原正在進行的應用程式狀態] 節點上的相關的重要資料。

## <a name="more-information"></a>更多資訊

- [容錯移轉叢集](failover-clustering.md)
- [部署叢集的存放空間](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)