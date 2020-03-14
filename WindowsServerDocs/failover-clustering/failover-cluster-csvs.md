---
title: 在容錯移轉叢集中使用叢集共用磁片區
description: 如何在容錯移轉叢集中使用叢集共用磁片區。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.openlocfilehash: da0f541c34c7f8687822bec365364fdd406fa3c3
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322690"
---
# <a name="use-cluster-shared-volumes-in-a-failover-cluster"></a>在容錯移轉叢集中使用叢集共用磁片區

>適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012、Windows Server 2012 R2

叢集共用磁碟區 (CSV) 可讓容錯移轉叢集中的多個節點，同時對佈建為 NTFS 磁碟區的相同 LUN (磁碟) 具有讀寫存取權。 （在 Windows Server 2012 R2 中，磁片可以布建為 NTFS 或復原檔案系統（ReFS））。有了 CSV，叢集角色就可以從一個節點快速故障切換至另一個節點，而不需要變更磁片磁碟機擁有權，或卸載和重新掛接磁片區。 CSV 也有助於簡化容錯移轉中潛在之大量 LUN 的管理。

CSV 提供一般用途的叢集檔案系統，其層高於 NTFS （或 Windows Server 2012 R2 中的 ReFS）。 CSV 應用程式包括：

- 叢集 Hyper-V 虛擬機器的叢集虛擬硬碟 (VHD) 檔案
- 向外延展檔案共用來儲存向外延展檔案伺服器叢集角色的應用程式資料。 此角色之應用程式資料的範例包括 Hyper-V 虛擬機器檔案和 Microsoft SQL Server 資料。 （請注意，向外延展檔案伺服器不支援 ReFS）。如需向外延展檔案伺服器的詳細資訊，請參閱[適用于應用程式資料的向外延展檔案伺服器](sofs-overview.md)。

> [!NOTE]
> Csv 不支援 SQL Server 2012 和舊版 SQL Server 中的 Microsoft SQL Server 叢集工作負載。

在 Windows Server 2012 中，CSV 功能已大幅增強。 例如，已移除 Active Directory 網域服務的相依性。 新增的支援包括對 **chkdsk** 中功能的改進、與防毒和備份應用程式的互通性，以及與一般的存放裝置功能 (例如 BitLocker 加密磁碟區) 與儲存空間的整合。 如需 Windows Server 2012 中所引進 CSV 功能的總覽，請參閱[Windows server 2012 中容錯移轉叢集的新功能 \[重新導向\]](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>)。

Windows Server 2012 R2 引進額外的功能，例如分散式 CSV 擁有權、透過伺服器服務的可用性提升復原能力、可配置給 CSV 快取的實體記憶體數量有更大的彈性。; 和增強的互通性，包括對 ReFS 和重復資料刪除的支援。 如需詳細資訊，請參閱[容錯移轉叢集的新功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)>)。

> [!NOTE]
> 如需針對虛擬桌面基礎結構 (VDI) 情況，在 CSV 上使用重複資料刪除的相關資訊，請參閱部落格文章 [將重複資刪除部署至 Windows Server 2012 R2 中的 VDI 存放裝置](https://blogs.technet.com/b/filecab/archive/2013/07/31/deploying-data-deduplication-for-vdi-storage-in-windows-server-2012-r2.aspx) 和 [將重複資料刪除延伸至 Windows Server 2012 R2 中新的工作負載](https://blogs.technet.com/b/filecab/archive/2013/07/31/extending-data-deduplication-to-new-workloads-in-windows-server-2012-r2.aspx).。

## <a name="review-requirements-and-considerations-for-using-csv-in-a-failover-cluster"></a>檢閱在容錯移轉叢集中使用 CSV 的需求與考量

在容錯移轉叢集中使用 CSV 之前，請檢閱本節中所述的網路、存放裝置及其他需求和考量。

### <a name="network-configuration-considerations"></a>網路設定考量

設定支援 CSV 的網路時，請考慮下列各項。

- **多個網路與多個網路介面卡**。 若要在發生網路失敗時能夠容錯，我們建議您使用多個叢集網路承載 CSV 流量或設定網路介面卡小組。
    
    如果叢集節點連線到不應由叢集使用的網路，您應該將它們停用。 例如，我們建議您停用 iSCSI 網路來防止叢集使用，以避免 CSV 流量佔用那些網路。 若要停用網路，請在容錯移轉叢集管理員中選取 [**網路**]，選取 [網路]，選取 [內容 **] 動作，** 然後選取 [**此網路上不允許叢集網路通訊**]。 或者，您可以使用[Get-clusternetwork](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternetwork?view=win10-ps) Windows PowerShell Cmdlet 來設定網路的**Role**屬性。
- **網路介面卡內容**。 在執行叢集通訊之所有介面卡的內容中，請確定已啟用下列設定：

  - [Microsoft 網路用戶端] 和 [Microsoft 網路檔案及印表機共用] **File 和 [Microsoft 網路檔案及印表機共用] Printer Sharing for Microsoft Networks**。 這些設定支援伺服器訊息區 (SMB) 3.0，預設用來承載節點之間的 CSV 流量。 若要啟用 SMB，也請確定伺服器服務和工作站服務正在執行，而且它們設定為在每個叢集節點上自動啟動。

    >[!NOTE]
    >在 Windows Server 2012 R2 中，每個容錯移轉叢集節點都有多個伺服器服務實例。 有一個預設執行個體會處理來自於存取一般檔案共用的 SMB 用戶端的連入流量，還有第二個 CSV 執行個體只處理節點間 CSV 流量。 此外，如果節點上的伺服器服務變成健康情況不良，CSV 擁有權會自動轉換到另一個節點。

    SMB 3.0 包括 SMB 多重通道與 SMB 直接傳輸兩個功能，可讓 CSV 流量在叢集中多個網路之間串流，並利用支援遠端直接記憶體存取 (RMDA) 的網路介面卡 根據預設，CSV 流量會使用 SMB 多重通道 。 如需詳細資訊，請參閱[伺服器訊息區概觀](../storage/file-server/file-server-smb-overview.md)。
  - **Microsoft 容錯移轉叢集虛擬介面卡效能篩選**。 這個設定可以改善節點能夠在必須連接 CSV 時執行 I/O 重新導向的能力，例如，當連線失敗可防止節點直接連接至 CSV 磁碟時。 如需詳細資訊，請參閱本主題稍後的[關於 CSV 通訊中的 i/o 同步處理和 i/o](#about-io-synchronization-and-io-redirection-in-csv-communication)重新導向。
- **叢集網路優先順序**。 我們通常會建議您不要變更網路的叢集設定喜好設定。
- **IP 子網路設定**。 網路中使用 CSV 的節點不需要特定的子網路組態。 CSV 可以支援多重子網路叢集。
- **以原則為依據的服務品質 (QoS)** 。 我們建議您在使用 CSV 時針對每個節點的網路流量設定 QoS 優先順序原則與最小頻寬原則。 如需詳細資訊，請參閱[服務品質（QoS）](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831679(v%3dws.11)>)。
- **存放網路**。 對於適用於存放網路的建議，請檢視存放裝置廠商提供的指導方針。 如需有關 CSV 存放裝置的其他考慮，請參閱本主題稍後的[儲存體和磁片設定需求](#storage-and-disk-configuration-requirements)。

如需容錯移轉叢集的硬體、網路與存放裝置需求概觀，請參閱[容錯移轉叢集的硬體需求及存放選項](clustering-requirements.md)。

#### <a name="about-io-synchronization-and-io-redirection-in-csv-communication"></a>關於 CSV 通訊中 I/O 同步處理和 I/O 重新導向

- **I/o 同步**處理： CSV 可讓多個節點同時讀寫存取相同的共用存放裝置。 當節點在 CSV 磁碟區中執行磁碟輸入/輸出 (I/O) 時，節點是與存放裝置直接通訊，例如，透過存放區域網路 (SAN)。 不過，在任何時間，單一節點 (稱為協調器節點) 會「擁有」與 LUN 關聯的實體磁碟資源。 CSV 磁碟區的協調器節點在 [容錯移轉叢集管理員] 是顯示為 [磁碟] 底下的 [擁有者節點]。 它也會出現在[Get-clustersharedvolume](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustersharedvolume?view=win10-ps) Windows PowerShell Cmdlet 的輸出中。

  >[!NOTE]
  >在 Windows Server 2012 R2 中，CSV 擁有權會根據每個節點所擁有的 CSV 磁片區數目，平均分散到容錯移轉叢集節點之間。 此外，出現 CSV 容錯移轉、 節點重新加入叢集、新增節點到叢集、重新啟動叢集節點，或在關閉容錯移轉叢集後啟動容錯移轉叢集之類的情況時，會自動重新平衡擁有權。

  CSV 磁碟區上的檔案系統中發生特定微小變更時，這個中繼資料必須在存取 LUN 的每個實體節點上同步化，而不是只在單一協調器節點上同步化。 例如，當 CSV 磁碟機上的虛擬機器啟動、建立或刪除時，需要在存取虛擬機器的每個實體節點上同步化該資訊。 這些中繼資料更新作業會使用 SMB 3.0 在叢集網路上並行發生。 這些作業不需要所有實體節點與共用存放裝置通訊。

- **I/o**重新導向：存放裝置連線失敗與特定儲存作業可以防止指定節點直接與存放裝置通訊。 若要在節點沒有與存放裝置通訊時仍然維持運作，節點會透過叢集網路將磁碟 I/O 重新導向到目前掛接磁碟的協調器節點。 如果目前的協調器節點碰到存放裝置連線失敗，則會暫時佇列所有磁碟 I/O 作業，同時建立新的節點做為協調器節點。

伺服器會視情況使用下列其中一個 I/O 重新導向模式：

- **檔案系統重新導向** 重新導向是每個磁碟區個別進行，例如，當備份應用程式拍攝 CSV 快照時，當 CSV 磁碟區以手動方式放置在重新導向的 I/O 模式時。
- **區塊重新導向** 重新導向位於檔案區塊層級，例如，存放裝置與磁碟區的連線遺失時。 區塊重新導向會比檔案系統重新導向快很多。

在 Windows Server 2012 R2 中，您可以根據每個節點來查看 CSV 磁片區的狀態。 例如，您可以看到 I/O 是直接或重新導向，或者 CSV 磁碟區是否無法使用。 如果 CSV 磁碟區處於 I/O 重新導向模式中，您也可以檢視其原因。 使用 Windows PowerShell Cmdlet **Get-ClusterSharedVolumeState** 來檢視此資訊。

> [!NOTE]
> * 在 Windows Server 2012 中，由於 CSV 設計方面的改進，CSV 在直接 i/o 模式中執行的作業比 Windows Server 2008 R2 中所發生的還要多。
> * 因為 CSV 與 SMB 3.0 功能 (例如 SMB 多重通道和 SMB 直接傳輸) 已經整合，所以重新導向的 I/O 流量可以跨多個叢集網路來串流。
> * 您應該規劃叢集網路能夠在 I/O 重新導向期間允許對協調器節點的網路流量增加。

### <a name="storage-and-disk-configuration-requirements"></a>存放裝置和磁碟設定需求

若要使用 CSV，您的存放裝置和磁碟必須符合以下需求：

- **檔案系統格式**。 在 Windows Server 2012 R2 中，CSV 磁片區的磁片或儲存空間必須是使用 NTFS 或 ReFS 分割的基本磁碟。 在 Windows Server 2012 中，CSV 磁片區的磁片或儲存空間必須是使用 NTFS 分割的基本磁碟。

  CSV 有下列額外需求：
    
  - 在 Windows Server 2012 R2 中，您無法將磁片用於使用 FAT 或 FAT32 格式化的 CSV。
  - 在 Windows Server 2012 中，您無法將磁片用於使用 FAT、FAT32 或 ReFS 格式化的 CSV。
  - 如果要將儲存空間用於 CSV，您可以設定簡單空間或鏡像空間。 在 Windows Server 2012 R2 中，您也可以設定同位空間。 （在 Windows Server 2012 中，CSV 不支援同位空間）。
  - CSV 無法做為仲裁見證磁碟使用。 如需叢集仲裁的詳細資訊，請參閱[瞭解儲存空間直接存取中的仲裁](../storage/storage-spaces/understand-quorum.md)。
  - 將磁碟新增為 CSV 之後，它會指定 CSVFS 格式 (適用於 CSV 檔案系統)。 這可讓叢集和其他軟體區別 CSV 存放裝置與其他 NTFS 或 ReFS 存放裝置。 一般而言，CSVFS 與 NTFS 或 ReFS 支援相同的功能。 不過，不支援某些功能。 例如，在 Windows Server 2012 R2 中，您無法在 CSV 上啟用壓縮。 在 Windows Server 2012 中，您無法在 CSV 上啟用重復資料刪除或壓縮。
- **叢集中的資源類型**。 對於 CSV 磁碟區，您必須使用實體磁碟資源類型。 根據預設，新增到叢集存放區的磁碟或儲存空間會自動以這種方式設定。
- **在叢集存放區中選用 CSV 磁碟或其他磁碟**。 為叢集虛擬機器選擇一或多個磁碟時，請考慮每個磁碟的用途。 如果磁碟將用來儲存 Hyper-V 建立的檔案 (例如 VHD 檔案或設定檔)，那麼您可以在磁碟存放區中選擇 CSV 磁碟或其他可用磁碟。 如果磁碟是直接連接到虛擬機器的實體磁碟 (也稱為穿通磁碟)，您就無法選擇 CSV 磁碟，而必須選擇叢集存放區中的其他可用磁碟。
- **用來識別磁碟的路徑名稱**。 CSV 中的磁碟是以路徑名稱加以識別。 每個路徑似乎都位於節點的系統磁片磁碟機上，做為 **\\ClusterStorage**資料夾下的編號磁片區。 從叢集中的任何節點檢視時，這個路徑都是相同的。 如果需要，您可以重新命名磁碟區。

如要了解 CSV 的存放裝置需求，請檢視存放裝置廠商提供的指導方針。 如要了解 CSV 的其他存放裝置規劃考量，請參閱本主題稍後的[計劃在容錯移轉叢集中使用 CSV](#plan-to-use-csv-in-a-failover-cluster)。

### <a name="node-requirements"></a>節點需求

若要使用 CSV，您的節點必須符合以下需求：

- **系統磁碟的磁碟機代號**。 在所有節點上，系統磁碟的磁碟機代號都必須相同。
- **驗證通訊協定**。 在所有節點上都必須啟用 NTLM 通訊協定。 預設為啟用。

## <a name="plan-to-use-csv-in-a-failover-cluster"></a>計劃在容錯移轉叢集中使用 CSV

本節列出在執行 Windows Server 2012 R2 或 Windows Server 2012 的容錯移轉叢集中使用 CSV 的規劃考慮和建議。

> [!IMPORTANT]
> 請詢問您的存放裝置廠商，了解如何設定 CSV 之特定存放裝置的相關建議。 如果存放裝置廠商的建議與本主題中的資訊不同，請遵循存放裝置廠商提供的建議。

### <a name="arrangement-of-luns-volumes-and-vhd-files"></a>LUN、磁碟區以及 VHD 檔案的安排方式

若要善用 CSV 來為叢集虛擬機器提供存放裝置，最好在設定實體伺服器時檢視您要如何安排 LUN (磁碟)。 當您設定相對應的虛擬機器時，請嘗試以類似方式安排 VHD 檔案。

請在您要於其中組織磁碟和檔案的實體伺服器中考量下列事項：

- 將系統檔案 (包含分頁檔) 放在某一個實體磁碟上
- 將資料檔案放在另一個實體磁碟上

對於對等的叢集虛擬機器，您應該以類似方式安排磁碟區和檔案：

- 將系統檔案 (包含分頁檔) 放在某個 CSV 上的 VHD 檔案
- 將資料檔案放在另一個 CSV 上的 VHD 檔案

如果您新增其他虛擬機器，可能的話，您應該對該虛擬機器上的 VHD 保持相同的安排方式。

### <a name="number-and-size-of-luns-and-volumes"></a>LUN 和磁碟區的數量和大小

當您為使用 CSV 的容錯移轉叢集規劃存放裝置設定時，請考慮下列建議：

- 若要決定需設定多少個 LUN，請洽詢存放裝置廠商。 例如，存放裝置廠商可能會建議您每個 LUN 設定一個磁碟分割，並在上面放置一個 CSV 磁碟區。
- 單一 CSV 磁碟區上支援的虛擬機器數目沒有限制。 不過，您應該考慮叢集中計劃擁有的虛擬機器數，以及每個虛擬機器的工作負載 (每秒的 I/O 作業)。 請考慮以下範例：

  - 一個組織正在部署將支援虛擬桌面基礎結構 (VDI) 的虛擬機器，這是相對較低的工作負載。 叢集使用高效能的存放裝置。 叢集系統管理員諮詢存放裝置廠商之後, 決定在每個 CSV 磁碟區上放置相對大量的虛擬機器。
  - 另一個組織要部署大量的虛擬機器，它們將支援重度使用的資料庫應用程式，這是較高的工作負載。 叢集使用較低效能的存放裝置。 叢集系統管理員諮詢存放裝置廠商之後, 決定在每個 CSV 磁碟區上放置相對小量的虛擬機器。
- 在您規劃特定虛擬機器的存放裝置設定時，請考慮虛擬機器將支援的服務、 應用程式或角色的磁碟需求。 了解這些需求可協助您避免發生可能會導致效能不佳的磁碟爭用情況。 虛擬機器的存放裝置設定應該要和您用於執行相同服務、 應用程式或角色之實體伺服器的存放裝置設定類似。 如需詳細資訊，請參閱本主題稍早所述[的 lun、磁片區和 VHD 檔案的相片順序](#arrangement-of-luns-volumes-and-vhd-files)。

    您也可以透過讓存放裝置擁有大量獨立實體硬碟的方式來降低磁碟爭用機率。 請據此選擇儲存硬體，並諮詢廠商以將存放裝置的效能最佳化。
- 根據您的叢集工作負載以及它們對 I/O 作業的需要而定，您可以考慮只設定某個比例的虛擬機器來存取每個 LUN，同時其他虛擬機器則不連線而且專門用來執行運算作業。

## <a name="add-a-disk-to-csv-on-a-failover-cluster"></a>在容錯移轉叢集上新增磁碟至 CSV

容錯移轉叢集中預設會啟用 CSV 功能。 若要新增磁碟到 CSV，您必須將磁碟新增至叢集的 [可用存放裝置] 群組 (如果尚未新增)，然後將磁碟新增至叢集中的 CSV。 您可以使用容錯移轉叢集管理員或容錯移轉叢集 Windows PowerShell Cmdlet 來執行這些程式。

### <a name="add-a-disk-to-available-storage"></a>將磁片新增至可用的存放裝置

1. 在 [容錯移轉叢集管理員] 的主控台樹狀目錄中，展開叢集的名稱，然後展開 [存放裝置]。
2. 以滑鼠右鍵按一下 [**磁片**]，然後選取 [**新增磁片**]。 此時會顯示可新增至容錯移轉叢集中使用的磁碟。
3. 選取您要新增的磁片或磁片，然後選取 **[確定]** 。

    磁碟會立即被指派給 [可用存放裝置] 群組。

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-available-storage"></a>Windows PowerShell 對等命令（將磁片新增至可用的存放裝置）

下列 Windows PowerShell 指令程式會執行與前述程序相同的功能。 請逐行各輸入一個指令程式，儘管有些指令程式可能因為受制於內文格式而自動換行拆成好幾行。

以下範例會識別已經可以新增至叢集的磁碟，然後將它們新增至 [可用存放裝置] 群組。

```PowerShell
Get-ClusterAvailableDisk | Add-ClusterDisk
```

### <a name="add-a-disk-in-available-storage-to-csv"></a>將可用的存放裝置中的磁片新增至 CSV

1. 在容錯移轉叢集管理員的主控台樹中，展開叢集的名稱，展開 [**存放裝置**]，然後選取 [**磁片**]。
2. 選取一或多個已指派給 [**可用存放裝置**] 的磁片，以滑鼠右鍵按一下選取範圍，然後選取 [**新增至叢集共用磁片**區]。

    磁碟會立即指派給叢集中的 [叢集共用磁碟區] 群組。 每個磁碟會向每個叢集節點公開，並在 %SystemDisk%ClusterStorage 資料夾底下顯示為已編號的磁碟區 (掛接點)。 磁碟區會顯示在 CSVFS 檔案系統中。

>[!NOTE]
>您可以重新命名 %SystemDisk%ClusterStorage 資料夾中的 CSV 磁碟區。

#### <a name="windows-powershell-equivalent-commands-add-a-disk-to-csv"></a>Windows PowerShell 對等命令（將磁片新增至 CSV）

下列 Windows PowerShell 指令程式會執行與前述程序相同的功能。 請逐行各輸入一個指令程式，儘管有些指令程式可能因為受制於內文格式而自動換行拆成好幾行。

以下範例會將 [可用存放裝置] 中的 **[叢集磁碟 1]** 新增到本機叢集上的 CSV。

```PowerShell
Add-ClusterSharedVolume –Name "Cluster Disk 1"
```

## <a name="enable-the-csv-cache-for-read-intensive-workloads-optional"></a>為大量讀取的工作負載啟用 CSV 快取 (選擇性)

CSV 快取會將系統記憶體 (RAM) 配置為直接寫入式快取，以便在唯讀未緩衝處理 I/O 作業的區塊層級提供快取。 （快取管理員不會快取未緩衝的 i/o 作業。）這可以改善應用程式的效能，例如 Hyper-v，這會在存取 VHD 時執行未緩衝的 i/o 作業。 CSV 快取可以在不快取寫入要求的情況下提升讀取要求的效能。 啟用 CSV 快取功能對於向外延展檔案伺服器情況也很有用。

>[!NOTE]
>我們建議您為所有叢集的 Hyper-V 與向外延展檔案伺服器部署啟用 CSV 快取。

根據預設，在 Windows Server 2012 中，會停用 CSV 快取。 在 Windows Server 2012 R2 和更新版本中，預設會啟用 CSV 快取。 不過，您仍然必須配置要保留的區塊快取大小。

下表描述控制 CSV 快取的兩個組態設定。

| Windows Server 2012 R2 和更新版本 |  Windows Server 2012                 | 描述 |
| -------------------------------- | ------------------------------------ | ----------- |
| BlockCacheSize                   | SharedVolumeBlockCacheSizeInMB       | 這是叢集一般屬性，可讓您定義要為叢集中每個節點上 CSV 快取保留多少記憶體 (MB)。 例如，如果將值定義為 512，每個節點上就會保留 512 MB 的系統記憶體。 （在許多群集中，512 MB 是建議的值）。預設設定為0（表示停用）。 |
| EnableBlockCache                 | CsvEnableBlockCache                  | 這是叢集實體磁碟資源的私用屬性。 它可讓您在新增到 CSV 的個別磁碟上啟用 CSV 快取。 在 Windows Server 2012 中，預設設定為0（表示停用）。 若要在磁碟上啟用 CSV 快取，請將值設為 1。 根據預設，在 Windows Server 2012 R2 中，會啟用此設定。 |

您可以在 [叢集 CSV 磁碟區快取] 底下新增計數器，以在效能監視器中監視 CSV 快取。

#### <a name="configure-the-csv-cache"></a>設定 CSV 快取

1. 以系統管理員身分啟動 Windows PowerShell。
2. 若要定義在每個節點上保留 *512* MB 的快取，請輸入下列命令：

    - 若為 Windows Server 2012 R2 和更新版本：

        ```PowerShell
        (Get-Cluster).BlockCacheSize = 512  
        ```

    - 若是 Windows Server 2012：

        ```PowerShell
        (Get-Cluster).SharedVolumeBlockCacheSizeInMB = 512  
        ```
3. 在 Windows Server 2012 中，若要在名為叢集*磁片 1*的 csv 上啟用 csv 快取，請輸入下列內容：

    ```PowerShell
    Get-ClusterSharedVolume "Cluster Disk 1" | Set-ClusterParameter CsvEnableBlockCache 1
    ```

>[!NOTE]
> * 在 Windows Server 2012 中，您只能將總實體 RAM 的20% 配置給 CSV 快取。 在 Windows Server 2012 R2 和更新版本中，您最多可以配置80%。 因為向外延展檔案伺服器通常沒有限制記憶體，所以您可以為 CSV 快取使用額外的記憶體來大幅度提升效能。
> * 為避免資源爭用，您應該在修改配置給 CSV 快取的記憶體之後，重新開機叢集中的每個節點。 在 Windows Server 2012 R2 和更新版本中，已不再需要重新開機。
> * 啟用或停用個別磁碟上的 CSV 快取之後，若要讓設定生效, 您必須讓實體磁碟資源離線，然後讓它恢復上線。 （根據預設，在 Windows Server 2012 R2 和更新版本中，已啟用 CSV 快取）。 
> * 如需 CSV 快取的相關詳細資訊 (包含效能計數器的相關資訊)，請參閱部落格文章 [如何啟用 CSV 快取](https://blogs.msdn.microsoft.com/clustering/2013/07/19/how-to-enable-csv-cache/)

## <a name="backing-up-csvs"></a>備份 Csv

有多種方法可備份儲存在容錯移轉叢集中 Csv 的資訊。 您可以使用 Microsoft 備份應用程式或非 Microsoft 應用程式。 一般而言，除了以 NTFS 或 ReFS 格式化之叢集存放裝置的備份需求以外，CSV 不需要特殊的備份需求。 CSV 備份也不會中斷其他 CSV 存放裝置運作。

當您為 CSV 選取備份應用程式和備份排程時，應該考量下列因素：

- CSV 磁碟區的磁碟區層級備份可以從任何連線至 CSV 磁碟區的節點執行。
- 備份應用程式可以使用軟體快照或硬體快照。 視備份應用程式支援它們的能力而定，備份可以使用應用程式一致且當機時保持一致的磁碟區陰影複製服務 (VSS) 快照。
- 如果您正在備份有多個執行中虛擬機器的 CSV，您通常應該選擇以管理作業系統為基礎的備份方法。 如果您的備份應用程式有支援，就可以同時備份多部虛擬機器。
- CSV 支援執行 Windows Server 2012 R2 備份、Windows Server 2012 備份或 Windows Server 2008 R2 備份的備份要求者。 不過，Windows Server Backup 通常只提供基本備份解決方案，可能不適合具有較大叢集的組織。 Windows Server Backup 不支援在 CSV 上執行應用程式一致的虛擬機器備份。 它只支援當機時保持一致的磁碟區層級備份。 （如果您還原當機時保持一致的備份，虛擬機器的狀態將會與虛擬機器在建立備份的時間點損毀時相同）。CSV 磁片區上的虛擬機器備份將會成功，但會記錄錯誤事件，指出不支援這種情況。
- 備份容錯移轉叢集時，您可能需要系統管理認證。

> [!IMPORTANT]
>請務必小心檢閱備份應用程式會備份及還原哪些資料、它支援哪些 CSV 功能，以及每個叢集節點上應用程式的資源需求。

> [!WARNING]
> 如果您需要將備份資料還原到 CSV 磁碟區，請注意備份應用程式的功能和限制，以便在叢集節點上保持並還原應用程式一致的資料。 以某些應用程式為例，如果還原 CSV 的節點與備份 CSV 磁碟區的節點不同，您可能會不小心覆寫執行還原之節點上與應用程式狀態有關的重要資料。

## <a name="more-information"></a>其他資訊

- [容錯移轉叢集](failover-clustering.md)
- [部署叢集儲存空間](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)