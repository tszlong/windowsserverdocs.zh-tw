---
Title: DFS 複寫：常見問題集 (FAQ)
ms.date: 06/18/2014
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 3782667e54f5e6b52c07645704b95fc9e7409a27
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476065"
---
# <a name="dfs-replication-frequently-asked-questions-faq"></a>DFS 複寫：常見問題集 (FAQ)


更新日期：2019 年 4 月 30日日

適用於：Windows Server 2019，Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

此常見問題集會回答有關適用於 Windows Server （也稱為 DFS-R 或 DFSR） 的分散式檔案系統 (DFS) 複寫的問題。

如需 DFS 命名空間的詳細資訊，請參閱[DFS 命名空間：常見問題集](https://technet.microsoft.com/en-us/library/ee404780)。

如需什麼是新的 DFS 複寫，請參閱下列主題：

  - [DFS 命名空間和 DFS 複寫概觀](http://technet.microsoft.com/en-us/library/jj127250)（在 Windows Server 2012)  
      
  - [What's New in 分散式檔案系統](https://technet.microsoft.com/en-us/library/ee307957)中的主題[從 Windows Server 2008 到 Windows Server 2008 R2 的功能變更](https://technet.microsoft.com/en-us/library/dd391932)  
      
  - [分散式檔案系統](https://technet.microsoft.com/en-us/library/cc753479)中的主題[從 Windows Server 2003 SP1 到 Windows Server 2008 的功能變更](https://technet.microsoft.com/en-us/library/cc753208)  
      

如需近期對本主題所做變更的清單，請參閱本主題的＜[變更歷程記錄](#change-history)＞一節。

      

## <a name="interoperability"></a>互通性

### <a name="can-dfs-replication-communicate-with-frs"></a>DFS 複寫可以與 FRS 通訊？

資料分割 DFS 複寫不通訊與檔案複寫服務 (FRS)。 DFS 複寫和 FRS 可以在相同伺服器上同時執行，但它們必須永遠不會設定為複寫相同的資料夾或子資料夾，因為這樣做可能會導致資料遺失。

### <a name="can-dfs-replication-replace-frs-for-sysvol-replication"></a>DFS 複寫進行 SYSVOL 複寫取代 FRS 可以嗎

是，DFS 複寫可在執行 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008 伺服器上的 SYSVOL 複寫取代 FRS。 執行 Windows Server 2003 R2 的伺服器不支援使用 DFS 複寫來複寫 SYSVOL 資料夾。

如需有關使用 DFS 複寫來複寫 SYSVOL 的詳細資訊，請參閱[SYSVOL 複寫移轉指南：FRS 到 DFS 複寫](https://technet.microsoft.com/en-us/library/dd640019)。

### <a name="can-i-upgrade-from-frs-to-dfs-replication-without-losing-configuration-settings"></a>可以升級從 FRS 到 DFS 複寫不會遺失組態設定？

是的。 若要將複寫從 FRS 移轉至 DFS 複寫，請參閱下列文件：

  - 若要移轉的 SYSVOL 資料夾以外之資料夾的複寫，請參閱[DFS 操作指南：從 FRS 移轉至 DFS 複寫](http://go.microsoft.com/fwlink/?linkid=192776)並[FRS2DFSR – DFSR 移轉公用程式 FRS](http://go.microsoft.com/fwlink/?linkid=195437) (http://go.microsoft.com/fwlink/?LinkID=195437)。  
      
  - 若要移轉至 DFS 複寫的 SYSVOL 資料夾的複寫，請參閱[SYSVOL 複寫移轉指南：FRS 到 DFS 複寫](https://technet.microsoft.com/en-us/library/dd640019)。  
      

### <a name="can-i-use-dfs-replication-in-a-mixed-windowsunix-environment"></a>可以使用 DFS 複寫在混合的 Windows/UNIX 環境嗎？

是的。 雖然 DFS 複寫僅支援執行 Windows Server 的伺服器之間複寫的內容，但是 UNIX 用戶端可以存取的 Windows 伺服器上的檔案共用。 若要這樣做，請 DFS 複寫伺服器上安裝 Services for Network File 系統 (NFS)。

您也可以使用許多 UNIX 用戶端直接存取 Windows 檔案共用，雖然這項功能通常是有限，或需要修改 （例如使用停用 SMB 簽章的 Windows 環境中包含的 SMB/CIFS 用戶端功能群組原則）。

執行 Windows Server 作業系統中，在伺服器上互通搭配 NFS 的 DFS 複寫，但您無法將複寫的 NFS 掛接點。

### <a name="can-i-use-the-volume-shadow-copy-service-with-dfs-replication"></a>可以與 DFS 複寫使用磁碟區陰影複製服務嗎？

是的。 DFS 複寫支援磁碟區陰影複製服務 (VSS) 磁碟區上，且可以成功還原先前的快照集，與先前版本的用戶端。

### <a name="can-i-use-windowsbackup-ntbackupexe-to-remotely-back-up-a-replicated-folder"></a>可以使用 Windows 備份 (Ntbackup.exe) 從遠端備份複寫的資料夾嗎？

否，在執行 Windows Server 2003 的電腦上或稍早備份執行 Windows Server 2012 的電腦上的複寫資料夾的內容，請使用 Windows 備份 (Ntbackup.exe)，Windows Server 2008 R2 或 Windows Server 2008 不支援。

若要備份會儲存在複寫資料夾的檔案，請使用 Windows Server Backup 或 Microsoft® System Center Data Protection Manager。 Windows Server 2008 R2 和 Windows Server 2008 中的備份和復原功能的相關資訊，請參閱[備份和復原](https://technet.microsoft.com/en-us/library/Cc754097)。 如需詳細資訊，請參閱 < [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=182261) (http://go.microsoft.com/fwlink/?LinkId=182261)。

### <a name="do-file-system-policies-impact-dfs-replication"></a>檔案系統原則會影響 DFS 複寫？

是的。 請勿在複寫資料夾上設定檔案系統原則。 檔案系統原則會在每個群組原則重新整理間隔，重新套用 NTFS 權限。 這會導致共用違規，因為開啟的檔案不會複寫，直到關閉檔案。

### <a name="does-dfs-replication-replicate-mailboxes-hosted-on-microsoft-exchange-server"></a>DFS 複寫是否複寫裝載在 Microsoft Exchange Server 上的信箱？

資料分割 DFS 複寫無法用來複寫裝載在 Microsoft Exchange Server 上信箱中。

### <a name="does-dfs-replication-support-file-screens-created-by-file-server-resource-manager"></a>DFS 複寫是否支援在 「 檔案伺服器資源管理員 」 所建立的檔案檢測？

是的。 不過，檔案伺服器資源管理員 (FSRM) 檔案檢測設定必須符合複寫的兩端。 此外，DFS 複寫有其本身的篩選機制，如檔案及資料夾可用來從複寫中排除特定檔案和檔案類型。

以下是實作檔案檢測或配額的最佳作法：

  - 隱藏的 DfsrPrivate 資料夾不能受到配額或檔案檢測。  
      
  - 經過篩選的檔案必須存在於任何複寫的資料夾，啟用檢測之前。  
      
  - 沒有資料夾可能會超過配額，才能啟用配額。  
      
  - 您必須小心使用固定配額。 就可以維持在複寫之前的配額內，但超出它，檔案會在複寫時的複寫群組的個別成員。 比方說，如果使用者複製到伺服器 A 上 10 mb (MB) 檔案 （也就是再在固定的限制），另一位使用者 5 MB 會將檔案複製到伺服器 B 上發生下一步 的複寫時，兩部伺服器會超過配額的 5 mb。 這會造成 DFS 複寫，持續重試複寫檔案，導致版本向量和可能的效能問題的漏洞。  
      

### <a name="is-dfs-replication-cluster-aware"></a>是 DFS 複寫的叢集感知嗎？

是，Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2008 R2 中 DFS 複寫包括複寫群組的成員身分新增容錯移轉叢集的能力。 如需詳細資訊，請參閱 <<c0> [ 新增到複寫群組的容錯移轉叢集](http://go.microsoft.com/fwlink/?linkid=155085)(http://go.microsoft.com/fwlink/?LinkId=155085)。 DFS 複寫服務的 Windows Server 2008 R2 之前的 Windows 版本上不設計成協調容錯移轉叢集，以及服務不會容錯移轉到另一個節點。


> [!NOTE]
> DFS 複寫不支援叢集共用磁碟區上的複寫檔案。 
<br>


### <a name="is-dfs-replication-compatible-with-data-deduplication"></a>是 DFS 複寫相容於重複資料刪除？

是，DFS 複寫都可以複寫在 Windows Server 中使用重複資料刪除的磁碟區上的資料夾。

### <a name="is-dfs-replication-compatible-with-ris-and-wds"></a>是 DFS 複寫與 RIS，WDS 相容？

是的。 DFS 複寫會將啟用單一執行個體儲存體 (SIS) 所在的磁碟區。 遠端安裝服務 (RIS)、 Windows 部署服務 (WDS) 與 Windows Storage Server 會使用 SIS。

### <a name="is-it-possible-to-use-dfs-replication-with-offline-files"></a>是否可以使用 DFS 複寫，離線檔案？

您可以安全地使用 DFS 複寫 」 和 「 離線檔案一起在案例中一次寫入檔案只有一位使用者時。 這是適用於兩個分公司之間移動，並想要能夠存取其檔案在任一分支，或在使用者離線。 離線檔案快取供離線使用的本機檔案和 DFS 複寫每個分公司之間複寫資料。

請勿使用 DFS 複寫使用離線檔案在多使用者環境中因為 DFS 複寫不提供任何分散式鎖定機制或檔案簽出功能。 如果兩位使用者在不同的伺服器上同時修改相同的檔案，DFS 複寫會將較舊的檔案移至 DfsrPrivate\\ConflictandDeleted 資料夾 （位於複寫資料夾的本機路徑下） 的下一步 的複寫過程。

### <a name="what-antivirus-applications-are-compatible-with-dfs-replication"></a>與 DFS 複寫相容哪些防毒應用程式？

防毒應用程式會造成過多的複寫，若其掃描活動同時修改了複寫的資料夾中的檔案。 如需詳細資訊，[測試防毒應用程式互通性與 DFS 複寫](http://go.microsoft.com/fwlink/?linkid=73990)(http://go.microsoft.com/fwlink/?LinkId=73990)。

### <a name="what-are-the-benefits-of-using-dfs-replication-instead-of-windows-sharepoint-services"></a>使用 DFS 複寫，而不 Windows SharePoint Services 的優點有哪些？

Windows® SharePoint® Services DFS 複寫 」 所沒有的檔案簽出功能的形式提供緊密的一致性。 如果您擔心多人編輯相同的檔案時，我們建議使用 Windows SharePoint Services。 Windows SharePoint Services 2.0 搭配 Service Pack 2 可做為 Windows Server 2003 R2 的一部分。 從 Microsoft 網站上，您可以下載 Windows SharePoint Services它不會包含在較新版本的 Windows Server。 不過，如果您跨多個站台複寫資料，而且使用者不會同時編輯相同的檔案，DFS 複寫會提供更高的頻寬和更簡易的管理。

## <a name="limitations-and-requirements"></a>限制和需求

### <a name="can-dfs-replication-replicate-between-branch-offices-without-a-vpn-connection"></a>DFS 複寫可以複寫不需要透過 VPN 連線的分公司之間嗎？

是，假設是私人的廣域網路 (WAN) 連結 （而非透過網際網路） 連線的分公司。 不過，您也必須在外部防火牆開啟適當的連接埠。 DFS 複寫會使用 RPC 端點對應程式 （連接埠 135） 和大於 1024年的隨機指派暫時連接埠。 您可以使用**Dfsrdiag**命令列工具，可指定靜態連接埠，而不是暫時的連接埠。 如需如何指定 RPC 端點對應程式的詳細資訊，請參閱[文章 154596](http://go.microsoft.com/fwlink/?linkid=73991)在 Microsoft Knowledge Base (http://go.microsoft.com/fwlink/?LinkId=73991)。

### <a name="can-dfs-replication-replicate-files-encrypted-with-the-encrypting-file-system"></a>DFS 複寫可以複寫使用加密檔案系統加密的檔案嗎？

資料分割 DFS 複寫不會複寫會使用加密檔案系統 (EFS) 加密的檔案或資料夾。 如果使用者進行加密的檔案，先前已複寫，DFS 複寫的複寫群組的所有其他成員刪除的檔案。 這可確保檔案的唯一可用的複本伺服器上的加密的版本。

### <a name="can-dfs-replication-replicate-outlook-pst-or-microsoft-office-access-database-files"></a>DFS 複寫可以複寫 Outlook.pst 或 Microsoft Office Access 資料庫檔案嗎？

DFS 複寫都可以安全地複寫 Microsoft Outlook 個人資料夾檔案 (.pst) 和 Microsoft Access 檔案只有當儲存，以供封存之用，而且不會透過網路存取利用 （若要開啟.pst 或存取例如 Outlook 或存取用戶端檔案，先將檔案複製到本機存放裝置）。 此情況的原因如下所示：

  - 透過網路連接開啟.pst 檔案可能會導致資料損毀.pst 檔案中。 如需有關為什麼.pst 檔案無法安全地存取從跨網路的詳細資訊，請參閱[文章 297019](http://go.microsoft.com/fwlink/?linkid=125363)在 Microsoft Knowledge Base (http://go.microsoft.com/fwlink/?LinkId=125363)。  
      
  - .pst 和 Access 檔案通常會在長的時間，例如 Outlook 或 Office Access 的用戶端正在存取的同時開啟的狀態。 這可防止 DFS 複寫在複寫這些檔案，直到它們關閉。  
      

### <a name="can-i-use-dfs-replication-in-a-workgroup"></a>可以使用 DFS 複寫在工作群組嗎？

資料分割 DFS 複寫會依賴 Active Directory® 網域服務組態。 它只會在網域中運作。

### <a name="can-more-than-one-folder-be-replicated-on-a-single-server"></a>可以在單一伺服器上複寫多個資料夾嗎？

是的。 DFS 複寫都可以複寫伺服器之間的許多資料夾。 請確定每個複寫資料夾都有唯一根的路徑和，它們不會重疊。 比方說，d:\\Sales 和 d:\\計量可以是兩個複寫的資料夾，但 d： 的根目錄路徑\\銷售和 d:\\銷售\\報表不能兩個複寫資料夾的根目錄路徑。

### <a name="does-dfs-replication-require-dfs-namespaces"></a>DFS 複寫需要 DFS 命名空間嗎？

資料分割 合併或分開，可以使用 DFS 複寫及 DFS 命名空間。 此外，使用 DFS 複寫，複寫獨立 DFS 命名空間，不被使用 FRS

### <a name="does-dfs-replication-require-time-synchronization-between-servers"></a>DFS 複寫需要伺服器之間的時間同步處理嗎？

資料分割 DFS 複寫不會明確要求的伺服器之間的時間同步處理。 不過，DFS 複寫需要伺服器的時鐘與密切相符。 伺服器的時鐘必須彼此 （依預設） 的五分鐘內設定 Kerberos 驗證，才能正確運作。 例如，DFS 複寫會使用時間戳記來判斷哪些檔案會在發生衝突的優先。 精確的時間也是很重要的記憶體回收、 排程和其他功能。

### <a name="does-dfs-replication-support-replicating-an-entire-volume"></a>支援複寫整個磁碟區 DFS 複寫？

是的。 不過，您必須先安裝 Windows Server 2003 Service Pack 2 或 hotfix。 如需詳細資訊，請參閱 <<c0> [ 文章 920335](http://go.microsoft.com/fwlink/?linkid=76776)在 Microsoft Knowledge Base (http://go.microsoft.com/fwlink/?LinkId=76776)。 此外，將整個磁碟區複寫可能會造成下列問題：

  - 如果磁碟區會包含 Windows 分頁檔，複寫就會失敗，並在系統事件記錄檔中記錄 DFSR 事件 4312。  
      
  - DFS 複寫的複寫資料夾的目的地伺服器上設定的系統和隱藏屬性。 這是因為 Windows 預設會套用 系統 和 隱藏屬性的磁碟區根資料夾。 如果目的地伺服器上的複寫資料夾的本機路徑也是磁碟區根目錄，任何進一步的變更會不對資料夾屬性。  
      
  - 當複寫包含 Windows 系統資料夾的磁碟區，DFS 複寫可辨識的 %WINDIR%資料夾，並不會複寫。 不過，DFS 複寫會複寫非 Microsoft 應用程式，可能是導致失敗的應用程式於目的地伺服器上，如果應用程式具有互通性問題，使用 DFS 複寫所使用的資料夾。  
      

### <a name="does-dfs-replication-support-rpc-over-http"></a>DFS 複寫支援 RPC over HTTP？

資料分割

### <a name="does-dfs-replication-work-across-wireless-networks"></a>DFS 複寫運作在無線網路？

是的。 DFS 複寫無關的連接類型。

### <a name="does-dfs-replication-work-on-refs-or-fat-volumes"></a>DFS 複寫會在 ReFS 或 FAT 磁碟區上運作？

資料分割 DFS 複寫可支援使用; NTFS 檔案系統格式化的磁碟區不支援復原檔案系統 (ReFS) 和 FAT 檔案系統。 DFS 複寫需要 NTFS，因為它會使用 NTFS 變更日誌，以及其他功能的 NTFS 檔案系統。

### <a name="does-dfs-replication-work-with-sparse-files"></a>DFS 複寫是否使用的疏鬆檔案中？

是的。 您可以複寫疏鬆檔案。 **Sparse**屬性會保留接收成員。

### <a name="do-i-need-to-log-in-as-administrator-to-replicate-files"></a>我需要將檔案複寫的系統管理員身分登入嗎？

資料分割 DFS 複寫是本機系統帳戶執行，因此您不需要複寫的系統管理員身分登入的服務。 不過，您必須是網域系統管理員或受影響的檔案伺服器的本機系統管理員至 DFS 複寫組態進行變更。

如需詳細資訊，請參閱 「 DFS 複寫安全性需求和委派 」 中[委派管理 DFS 複寫的能力](http://go.microsoft.com/fwlink/?linkid=182294)(http://go.microsoft.com/fwlink/?LinkId=182294)。

### <a name="how-can-i-upgrade-or-replace-a-dfs-replication-member"></a>如何升級或取代 DFS 複寫的成員？

若要升級或取代 DFS 複寫的成員，請參閱此部落格文章 「 詢問目錄服務小組部落格：[取代 DFSR 成員硬體或 OS](http://blogs.technet.com/b/askds/archive/2010/09/10/series-wrap-up-and-downloads-replacing-dfsr-member-hardware-or-os.aspx)。

### <a name="is-dfs-replication-suitable-for-replicating-roaming-profiles"></a>適合用 DFS 複寫來複寫漫遊設定檔嗎？

是的。 複寫漫遊使用者設定檔時，會支援特定案例。 如需支援的案例，請參閱[Microsoft 的支援陳述式周圍複寫使用者設定檔資料](http://go.microsoft.com/fwlink/?linkid=201282)(http://go.microsoft.com/fwlink/?LinkId=201282)。

### <a name="is-there-a-file-character-limit-or-limit-to-the-folder-depth"></a>有檔案字元的限制或資料夾深度限制嗎？

Windows 和 DFS 複寫可支援最多 32 千個字元的資料夾路徑。 DFS 複寫不限於資料夾路徑為 260 個字元。

### <a name="must-members-of-a-replication-group-reside-in-the-same-domain"></a>複寫群組的成員必須位於相同的網域嗎？

資料分割 複寫群組可橫跨單一樹系內跨網域，但不是會跨不同樹系。

### <a name="what-are-the-supported-limits-of-dfs-replication"></a>DFS 複寫的支援的限制有哪些？

下列清單提供一組經過 Microsoft Windows Server 2012 R2 上的延展性指導方針：

  - 在伺服器上的所有複寫檔案的大小：100 tb。  
      
  - 複寫的磁碟區上的檔案數目：70 萬個。  
      
  - 最大檔案大小：250 gb 為單位。  
      


> [!IMPORTANT]
> 使用大型的數字或檔案的大小建立複寫群組時我們建議匯出資料庫複本，並使用預先植入的技術，在初始複寫的持續時間降至最低。 如需詳細資訊，請參閱<A href="http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx">DFS 複寫初始同步處理 Windows Server 2012 R2 中：進攻</A>。 
<br>


下列清單提供一組經過 Microsoft Windows Server 2012、 Windows Server 2008 R2 和 Windows Server 2008 的延展性指導方針：

  - 在伺服器上的所有複寫檔案的大小：10 tb。  
      
  - 複寫的磁碟區上的檔案數目：11 萬個。  
      
  - 最大檔案大小：64 gb。  
      


> [!NOTE]
> 不再是複寫群組、 複寫的資料夾、 連線或複寫群組成員的數目限制。 
<br>


如需經過 Microsoft Windows Server 2003 R2 的延展性指導方針，請參閱[DFS 複寫延展性指導方針](http://go.microsoft.com/fwlink/?linkid=75043)(http://go.microsoft.com/fwlink/?LinkId=75043)。

### <a name="when-should-i-not-use-dfs-replication"></a>何時應該不使用 DFS 複寫？

請勿在多個使用者更新，或修改相同的檔案，同時在不同的伺服器上的環境中使用 DFS 複寫。 這樣會造成 DFS 複寫將衝突檔案的複本移至 隱藏 DfsrPrivate\\ConflictandDeleted 資料夾。

當多個使用者需要在不同的伺服器上同時修改相同的檔案，請使用檔案簽出功能的 Windows SharePoint Services，以確保只有一個使用者正在處理檔案。 Windows SharePoint Services 2.0 搭配 Service Pack 2 可做為 Windows Server 2003 R2 的一部分。 從 Microsoft 網站上，您可以下載 Windows SharePoint Services它不會包含在較新版本的 Windows Server。

### <a name="why-is-a-schema-update-required-for-dfs-replication"></a>為什麼是 DFS 複寫所需的結構描述更新？

DFS 複寫會使用 Active Directory 網域服務網域命名內容中新的物件，來儲存組態資訊。 當您更新 Active Directory 網域服務結構描述時，會建立這些物件。 如需詳細資訊，請參閱 <<c0> [ 檢閱 DFS 複寫的需求](http://go.microsoft.com/fwlink/?linkid=182264)(http://go.microsoft.com/fwlink/?LinkId=182264)。

## <a name="monitoring-and-management-tools"></a>監視和管理工具

### <a name="can-i-automate-the-health-report-to-receive-warnings"></a>可以自動化的健康情況報告，以收到警告嗎？

是的。 有三種方式可將自動化的健康情況報告：

  - 您可以使用 包含在 Windows Server 2012 R2 或 DfsrAdmin.exe 中排定的工作搭配 DFSR Windows PowerShell 模組來定期產生 健全狀況報表。 如需詳細資訊，請參閱 <<c0> [ 自動化 DFS 複寫健康情況報告](http://go.microsoft.com/fwlink/?linkid=74010)(http://go.microsoft.com/fwlink/?LinkId=74010)。  
      
  - 您可以使用 DFS 複寫管理組件，System Center Operations manager，建立以指定的條件為基礎的警示。  
      
  - 使用 DFS 複寫 WMI 提供者，若要編寫警示。  
      

### <a name="can-i-use-microsoft-system-center-operations-manager-to-monitor-dfs-replication"></a>可以使用 Microsoft System Center Operations Manager 監視 DFS 複寫嗎？

是的。 如需詳細資訊，請參閱 < [System Center Operations Manager 2007 的 DFS 複寫管理組件](http://go.microsoft.com/fwlink/?linkid=182265)在 Microsoft Download Center (http://go.microsoft.com/fwlink/?LinkId=182265)。

### <a name="does-dfs-replication-support-remote-management"></a>DFS 複寫是否支援遠端管理？

是的。 DFS 複寫 」 支援使用 DFS 管理主控台遠端管理，**新增複寫群組**命令。 比方說，在伺服器 A 上，您可以連線到具有做為成員伺服器 A 和 B 的樹系中所定義的複寫群組。

DFS 管理是隨附於 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008 和 Windows Server 2003 R2。 若要管理 DFS 複寫，從其他版本的 Windows，使用 遠端桌面或[遠端伺服器管理工具的 Windows 7](https://technet.microsoft.com/en-us/library/Ee449475)。


> [!IMPORTANT]
> 若要檢視或管理包含唯讀複寫的資料夾或容錯移轉叢集成員的複寫群組，您必須使用隨附於 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、的DFS管理的版本<a href="http://go.microsoft.com/fwlink/p/?linkid=238560">適用於 Windows 8 的遠端伺服器管理工具</a>，或<a href="https://technet.microsoft.com/en-us/library/ee449475">Windows 7 的遠端伺服器管理工具</a>。 
<br>


### <a name="do-ultrasound-and-sonar-work-with-dfs-replication"></a>Ultrasound 和 sonar 查看複寫工作與 DFS 複寫？

資料分割 DFS 複寫有它自己的監視和診斷工具組。 Ultrasound 和 sonar 查看複寫只能夠監視 FRS

### <a name="how-can-files-be-recovered-from-the-conflictanddeleted-or-preexisting-folders"></a>檔案，要如何修復從 ConflictAndDeleted 或 PreExisting 資料夾？

若要復原遺失的檔案，從檔案系統資料夾或使用 [檔案歷程記錄中的共用的資料夾還原檔案**還原舊版**命令在檔案總管] 中，或從備份還原檔案。 若要直接從 ConflictAndDeleted 或 PreExisting 資料夾復原檔案，請使用`Get-DfsrPreservedFiles`並`Restore-DfsrPreservedFiles`Windows PowerShell cmdlet （包含 Windows Server 2012 R2 中的 [DFSR] 模組），或有[RestoreDFSR](http://code.msdn.microsoft.com/restoredfsr)從 MSDN Code Gallery 的範例指令碼。 此指令碼僅適用於災害復原，並提供 AS-是，不提供擔保。

### <a name="is-there-a-way-to-know-the-state-of-replication"></a>有辦法知道的複寫狀態嗎？

是的。 有數種方式來監視複寫：

  - DFS 複寫有提供主動式監視的 System Center Operations Manager 管理組件。  
      
  - DFS 管理具有內建的診斷報告複寫待處理項目、 複寫效率和指定的複寫群組中檔案和資料夾的數目。  
      
  - Windows Server 2012 R2 中的 DFSR Windows PowerShell 模組包含 cmdlet 來啟動傳播測試和寫入傳播和健康情況報告。 如需詳細資訊，請參閱 < [Windows PowerShell 中分散式檔案系統複寫 Cmdlet](http://technet.microsoft.com/library/dn296601.aspx)。  
      
  - Dfsrdiag.exe 是可以在待處理項目計數或觸發程序傳播測試所產生的命令列工具。 同時顯示複寫的狀態。 傳播會顯示是否檔案會被複寫到所有節點。 待處理項目會顯示多少檔案仍需要複寫前兩部電腦都保持同步。待處理項目計數是複寫群組成員尚未處理的更新數目。 在電腦上執行 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2，Dfsrdiag.exe 也可以顯示目前正在複寫 DFS 複寫的更新。  
      
  - 指令碼可以使用 WMI 來收集待處理項目資訊，以手動方式或透過 MOM。  
      

## <a name="performance"></a>效能

### <a name="does-dfs-replication-support-dial-up-connections"></a>DFS 複寫是否支援撥接連線？

雖然 DFS 複寫會以撥接速度運作，但它可以取得待處理項目有大量的複寫變更。 如果現有的檔案進行小變更，DFS 複寫使用遠端的差異壓縮 (RDC) 會提供效能遠高於直接複製檔案。

### <a name="does-dfs-replication-perform-bandwidth-sensing"></a>不會執行頻寬感應 DFS 複寫？

資料分割 DFS 複寫不會執行感應頻寬。 您可以設定要使用的頻寬數量有限 （頻寬節流設定） 以每個連接為基礎的 DFS 複寫。 不過，DFS 複寫不會進一步減少頻寬使用率如果網路介面會呈現飽和狀態，而且 DFS 複寫可以 saturate 期限很短的連結。 頻寬節流設定與 DFS 複寫不完全精確，因為 DFS 複寫節流頻寬節流的 RPC 呼叫。 如此一來，較低層級 （包括 RPC） 的網路堆疊中的各種緩衝區可能會影響，導致暴增的網路流量。

### <a name="does-dfs-replication-throttle-bandwidth-per-schedule-per-server-or-per-connection"></a>DFS 複寫啟用依排程每一部伺服器，或每個連線的頻寬節流設定嗎？

如果您設定頻寬節流設定，指定排程時，該複寫群組的所有連線會都使用該設定頻寬節流設定。 頻寬節流設定可以也設定為使用 DFS 管理的連接層級設定。

### <a name="does-dfs-replication-use-active-directory-domain-services-to-calculate-site-links-and-connection-costs"></a>DFS 複寫會計算網站連結和連線成本使用 Active Directory 網域服務嗎？

資料分割 DFS 複寫會使用系統管理員，也就是獨立的 Active Directory Domain Services 站台的成本計算方式定義的拓撲。

### <a name="how-can-i-improve-replication-performance"></a>如何提升複寫效能？

若要深入了解不同的調整複寫效能的方法，請參閱[DFSR 在微調複寫效能](http://blogs.technet.com/b/askds/archive/2010/03/31/tuning-replication-performance-in-dfsr-especially-on-win2008-r2.aspx)上[詢問目錄服務小組部落格](http://blogs.technet.com/b/askds/)。

### <a name="how-does-dfs-replication-avoid-saturating-a-connection"></a>DFS 複寫如何避免飽和，讓連線？

DFS 複寫在您設定您想要在連接之後，使用的最大頻寬和服務會維護該層級的網路使用量。 這是不同的背景智慧型傳送服務 (BITS)，和 DFS 複寫不會不 saturate 連接，如果您加以適當設定。

然而，頻寬節流設定不正確的 100%，而且 DFS 複寫可以使連結飽和期限很短的時間。 這是因為 DFS 複寫節流頻寬節流的 RPC 呼叫。 因為此程序依賴各種緩衝區中的網路堆疊，包括 RPC、 較低層級的複寫流量通常會以高載，這有時可能會使飽和的網路連結。

在 Windows Server 2008 的 DFS 複寫包含數個效能增強功能，如所述[分散式檔案系統](https://technet.microsoft.com/en-us/library/Cc753479)中的主題[從 Windows Server 2003 SP1 到 Windows Server 的功能變更2008](https://technet.microsoft.com/en-us/library/cc753208)。

### <a name="how-does-dfs-replication-performance-compare-with-frs"></a>DFS 複寫的效能與使用 FRS 如何比較的？

DFS 複寫是 FRS，比快很多，尤其是對大型檔案進行小變更和 RDC 已啟用。 比方說，使用 RDC，2 MB PowerPoint® 簡報小幅變更可能會導致只有 60 (kb) 透過網路傳送，以位元組為單位傳送的 97 折。

RDC 上未使用的檔案小於 64KB 且可能無法幫助高速的 Lan 上不多的網路頻寬爭用。 RDC 可停用，使用 DFS 管理每個連線為基礎。

### <a name="how-frequently-does-dfs-replication-replicate-data"></a>DFS 複寫頻率複寫資料？

根據您設定的排程，複寫資料。 比方說，您可以設定排程，每隔 15 分鐘到一週七天。 在這些間隔中，已啟用複寫。 只有在偵測到檔案變更時 （通常是在秒） 後，隨即會開始複寫。

連線排程設定為接收成員的當地時間時，複寫群組排程可能會設定為標準時間協調 (UTC)。 當複寫群組跨越多個時區時，請考慮這點。 當地時間表示的時間，裝載輸入的連線的成員。 排程設定為本地時間時，顯示輸入的連線和對應的輸出連線的排程會反映時區差異。

### <a name="how-much-of-my-servers-system-resources-will-dfs-replication-consume"></a>我的伺服器系統資源中有多少將 DFS 複寫使用嗎？

磁碟、 記憶體和 DFS 複寫所使用的 CPU 資源取決於許多因素，包括的數目和大小的檔案、 變更、 的複寫群組的成員，以及複寫資料夾數目的速率。 此外，某些資源來估計是很難。 比方說，「 DFS 複寫資料庫使用的可延伸儲存引擎 (ESE) 技術可能會耗用大量百分比的可用記憶體，它會釋放依需求。 DFS 複寫以外的應用程式可以裝載在相同的伺服器，根據伺服器組態。 不過，當裝載多個應用程式或在單一伺服器上的伺服器角色中，務必您測試這項設定，在生產環境中實作之前。

### <a name="what-happens-if-a-wan-link-fails-during-replication"></a>如果在複寫期間的 WAN 連結失敗，發生什麼事？

如果連線已關閉時，DFS 複寫會嘗試複寫排程為開啟時。 也會在 DFS 複寫事件記錄檔，可以使用 MOM （主動透過警示） 和 DFS 複寫健康情況報告 （被動，例如當系統管理員身分執行它時） 原創作品中記下的連線錯誤。

## <a name="remote-differential-compression-details"></a>遠端差異壓縮詳細資料

### <a name="are-changes-compressed-before-being-replicated"></a>為壓縮之前正在複寫的變更嗎？

是的。 為所有檔案所示 （它已經有壓縮） 以外的類型在傳送之前先壓縮檔案的變更的部分：.wma、.wmv、.zip、.jpg、.mpg、.mpeg、.m1v，.mp2、.mp3、.mpa、.cab、.wav、.snd、.au、.asf、.wm、.avi、...z、.gz、.tgz 和.frx。 不是可在 Windows Server 2003 R2 中設定這些檔案類型的壓縮設定。

### <a name="can-an-administrator-turn-off-rdc-or-change-the-threshold"></a>系統管理員可以關閉 RDC 或變更的臨界值？

是的。 您可以透過指定連接的屬性頁面來關閉 RDC。 停用 RDC 可以減少 CPU 使用率和複寫延遲主要包含小於 64KB 的檔案的複寫群組或沒有頻寬限制的快速的區域網路 (LAN) 連結。 如果您選擇停用連接上的 RDC，測試複寫效率，以確認您有改善複寫效能變更之前和之後。

您可以使用，以變更 RDC 大小閾值**Dfsradmin 連接設定**命令時，DFS 複寫 WMI 提供者，或以手動方式編輯組態 XML 檔案。

### <a name="does-rdc-work-on-all-file-types"></a>RDC 會在所有檔案類型上運作？

是的。 RDC 會計算在區塊層級，無論檔案資料類型的差異。 不過，RDC 的某些檔案類型，例如 Word 文件、 PST 檔案和 VHD 映像會更有效率。

### <a name="how-does-rdc-work-on-a-compressed-file"></a>RDC 如何壓縮的檔案？

DFS 複寫使用 RDC，會計算檔案中已經變更，並透過網路傳送只會將這些區塊的區塊。 DFS 複寫不需要知道任何關於檔案的內容，只會將哪些區塊已變更。

### <a name="is-cross-file-rdc-enabled-when-upgrading-to-windows-server-enterprise-edition-or-datacenter-edition"></a>是跨檔案 RDC 時升級到 Windows Server Enterprise Edition 或 Datacenter Edition 啟用嗎？

Windows Server Standard 版本不支援跨檔案 RDC。 不過，它會自動啟用或當您升級到版本，支援跨檔案 RDC，如果複寫連接的成員所執行的受支援的版本。 如需支援跨檔案 RDC 的版本中，請參閱哪些版本的 Windows 作業系統支援跨檔案 RDC 嗎？

### <a name="is-rdc-true-block-level-replication"></a>是 RDC，則為 true 的區塊層級複寫？

資料分割 這是壓縮檔案傳輸的一般用途通訊協定。 DFS 複寫會在檔案層級，而不是磁碟區塊層級的區塊，使用 RDC。 RDC 會將檔案分割成區塊。 針對在檔案中每個區塊，它會計算簽章，也就是一個小型可代表大區塊的位元組數。 簽章組是從伺服器傳輸到用戶端。 用戶端會比到其自有的伺服器簽章。 用戶端，然後伺服器傳送資料的用戶端上還沒有簽章的要求。

### <a name="what-happens-if-i-rename-a-file"></a>如果我重新命名檔案發生什麼事？

DFS 複寫的複寫群組的所有其他成員上的檔案重新命名的下一步 的複寫過程。 檔案包括追蹤使用的唯一識別碼，所以重新命名檔案和移動的檔案複本中沒有任何作用，DFS 複寫的複寫檔案的能力。

### <a name="what-is-cross-file-rdc"></a>什麼是跨檔案 RDC？

跨檔案 RDC 可讓 DFS 複寫使用 RDC，即使不存在具有相同名稱的檔案，這是在用戶端。 跨檔案 RDC 會使用啟發學習法來判斷檔案類似，受到要複寫所需要的檔案，並經由 WAN 傳輸相同的資料量降到最低的複寫檔案的類似檔案會使用區塊。 跨檔案 RDC 可使用此程序最多五個類似的檔案區塊。

若要使用跨檔案 RDC，複寫連接的其中一個成員必須執行的 Windows 版本支援跨檔案 RDC。 如需支援跨檔案 RDC 的版本中，請參閱哪些版本的 Windows 作業系統支援跨檔案 RDC 嗎？

### <a name="what-is-rdc"></a>RDC 是什麼？

遠端差異壓縮 (RDC) 是可用來有效率地透過有限頻寬網路更新檔案的用戶端-伺服器通訊協定。 RDC 會偵測插入、 移除和重新排列的資料檔案中，啟用 DFS 複寫來複寫的變更，檔案更新時。 RDC 預設為只能用於 64 KB 的檔案或更大。 RDC 可使用較舊版本的檔案具有相同名稱的複寫資料夾中，或在 DfsrPrivate\\ConflictandDeleted 資料夾 （位於複寫資料夾的本機路徑下）。

### <a name="when-is-rdc-used-for-replication"></a>RDC 何時使用複寫？

當檔案超過大小下限臨界值時，會使用 RDC。 此大小臨界值預設為 64 KB。 已複寫的檔案超過該臨界值之後，更新的版本的檔案一律會使用 RDC，除非 RDC 已停用或變更大部分的檔案。

### <a name="which-editions-of-the-windows-operating-system-support-cross-file-rdc"></a>哪些版本的 Windows 作業系統支援跨檔案 RDC？

若要使用跨檔案 RDC，複寫連接的其中一個成員必須執行的 Windows 作業系統版本支援跨檔案 RDC。 下表顯示哪些版本的 Windows 作業系統支援跨檔案 RDC。

### <a name="cross-file-rdc-availability-in-editions-of-the-windows-operating-system"></a>跨檔案 RDC 版本的 Windows 作業系統的可用性

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>作業系統版本</th>
<th>Standard Edition</th>
<th>企業版</th>
<th>Datacenter Edition</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>Windows Server 2012 R2</p></td>
<td><p>是*</p></td>
<td><p>沒有</p></td>
<td><p>是*</p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012</p></td>
<td><p>是</p></td>
<td><p>沒有</p></td>
<td><p>是</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2008 R2</p></td>
<td><p>否</p></td>
<td><p>是</p></td>
<td><p>是</p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2008</p></td>
<td><p>否</p></td>
<td><p>是</p></td>
<td><p>否</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2003 R2</p></td>
<td><p>否</p></td>
<td><p>是</p></td>
<td><p>否</p></td>
</tr>
</tbody>
</table>

\* 您可以選擇性地停用跨檔案 RDC，Windows Server 2012 R2 上的。

## <a name="replication-details"></a>複寫詳細資料

### <a name="can-i-change-the-path-for-a-replicated-folder-after-it-is-created"></a>在建立之後可以變更複寫資料夾的路徑嗎？

資料分割 如果您需要變更的複寫資料夾的路徑，您必須刪除在 DFS 管理，並將它新增為新複寫資料夾。 然後，DFS 複寫會使用遠端差異壓縮 (RDC) 來執行判斷資料是否在傳送和接收成員上相同的同步處理。 它不會再次複寫資料夾中的所有資料。

### <a name="can-i-configure-which-file-attributes-are-replicated"></a>我可以設定哪些檔案的屬性會複寫嗎？

否，您無法設定 DFS 複寫會將哪些檔案屬性。

如需屬性值和其描述的清單，請參閱 <<c0> [ 檔案屬性](http://go.microsoft.com/fwlink/?linkid=182268)MSDN 上 (http://go.microsoft.com/fwlink/?LinkId=182268)。

利用下列的屬性值設為`SetFileAttributes dwFileAttributes`函式，而且它們會複寫由 「 DFS 複寫。 這些屬性的值所做的變更會觸發複寫的屬性。 除非內容也會變更，不會複寫檔案的內容。 如需詳細資訊，請參閱 < [SetFileAttributes 函式](http://go.microsoft.com/fwlink/?linkid=182269)在 MSDN library (http://go.microsoft.com/fwlink/?LinkId=182269)。

  - 檔案\_屬性\_隱藏  
      
  - 檔案\_屬性\_READONLY  
      
  - 檔案\_屬性\_系統  
      
  - 檔案\_屬性\_不\_內容\_索引  
      
  - 檔案\_屬性\_離線  
      

DFS 複寫所複寫的下列屬性值，但不是會觸發複寫。

  - 檔案\_屬性\_封存  
      
  - 檔案\_屬性\_正常  
      

下列的檔案屬性值也會觸發複寫，雖然無法設定使用`SetFileAttributes`函式 (使用`GetFileAttributes`函式，若要檢視的屬性值)。

  - 檔案\_屬性\_重新分析\_點  
      

> [!NOTE]
> DFS 複寫不會複寫重新分析點的屬性值，除非重新分析標記是 IO_REPARSE_TAG_SYMLINK。 具有 IO_REPARSE_TAG_DEDUP、 IO_REPARSE_TAG_SIS 或 IO_REPARSE_TAG_HSM 重新分析標記的檔案會複寫的一般檔案。 不過，重新分析標記和重新分析資料緩衝區不會複寫到其他伺服器因為重新分析點僅適用於本機系統上。 
<br>

  - 檔案\_屬性\_壓縮  
      
  - 檔案\_屬性\_加密  
      

> [!NOTE]
> DFS 複寫不會複寫會使用加密檔案系統 (EFS) 加密的檔案。 DFS 複寫會複寫檔案已加密，使用非 Microsoft 軟體，但前提是它不會在檔案上設定 FILE_ATTRIBUTE_ENCRYPTED 屬性值。 
<br>

  - 檔案\_屬性\_SPARSE\_檔案  
      
  - 檔案\_屬性\_目錄  
      

DFS 複寫不會複寫檔案\_屬性\_暫存值。

### <a name="can-i-control-which-member-is-replicated"></a>我可以控制哪些成員複寫嗎？

是的。 當您建立複寫群組，您可以選擇的拓撲。 或者您可以選取**沒有拓樸**和建立複寫群組之後，手動設定連線。

### <a name="can-i-seed-a-replication-group-member-with-data-prior-to-the-initial-replication"></a>我可以植入複寫群組成員之前的初始複寫的資料嗎？

是的。 DFS 複寫 」 支援複製檔案到之前的初始複寫的複寫群組成員。 此 「 預備 」 可以大幅降低複寫在初始複寫期間的資料量。

在初始複寫不需要複寫內容，當檔案的差異是只有實際的屬性或時間戳記。 實際的屬性是 Win32 函式可以設定的屬性`SetFileAttributes`。 如需詳細資訊，請參閱 < [SetFileAttributes 函式](http://go.microsoft.com/fwlink/?linkid=182269)在 MSDN library (http://go.microsoft.com/fwlink/?LinkId=182269)。 如果兩個檔案不同的其他屬性，例如壓縮，則檔案的內容複寫。

若要預先設置的複寫群組成員，將檔案複製到目的地伺服器上適當的資料夾，建立複寫群組，，然後選擇主要成員。 請選擇具有最新的檔案，您想要複寫，因為主要成員的內容會被視為 「 授權 」。 這表示，在初始複寫，主要成員的檔案一定會覆寫其他版本的複寫群組的其他成員上的檔案。

如需預先植入，並複製下 DFSR 資料庫的資訊，請參閱[DFS 複寫初始同步處理 Windows Server 2012 R2 中：進攻](http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx)。

如需有關在初始複寫的詳細資訊，請參閱[建立複寫群組](https://technet.microsoft.com/en-us/library/cc725893)。

### <a name="does-dfs-replication-overcome-common-file-replication-service-issues"></a>DFS 複寫是否會解決常見的檔案複寫服務問題？

是的。 DFS 複寫克服了三種常見的 FRS 問題：

  - 日誌換行：DFS 複寫會復原從即時日誌換行。 將標示為 journalWrap 每個現有的檔案或資料夾，並再次啟用複寫之前，針對檔案系統驗證。 在復原期間，此磁碟區不是可用於任一方向的複寫。  
      
  - 過多的複寫：若要避免過多的複寫，DFS 複寫會使用系統的信用額度。  
      
  - 已變形的資料夾：若要避免已變形的資料夾名稱，DFS 複寫將衝突資料儲存在隱藏 DfsrPrivate\\ConflictandDeleted 資料夾 （位於複寫資料夾的本機路徑下）。 例如，使用相同的名稱，在使用 FRS 複寫的不同伺服器上，同時建立多個資料夾會導致重新命名舊的資料夾的 FRS。 相反地，DFS 複寫會將較舊的資料夾移至本機的 「 因衝突而刪除 」 資料夾。  
      

### <a name="does-dfs-replication-replicate-files-in-chronological-order"></a>DFS 複寫是否複寫依時間先後順序的檔案？

資料分割 檔案可能會複寫順序。

### <a name="does-dfs-replication-replicate-files-that-are-being-used-by-another-application"></a>DFS 複寫是否複寫正由另一個應用程式的檔案？

如果應用程式開啟檔案，而且它 （使其無法開啟時，要供其他應用程式） 上建立檔案鎖定，DFS 複寫不會複寫檔案關閉為止。 如果應用程式以讀取共用存取權，開啟檔案，檔案仍然可以進行複製。

### <a name="does-dfs-replication-replicate-ntfs-file-permissions-alternate-data-streams-hard-links-and-reparse-points"></a>複寫 NTFS 檔案權限、 替代資料流、 永久連結，與重新分析點 DFS 複寫？

  - DFS 複寫會複寫 NTFS 檔案權限和替代資料流。  
      
  - Microsoft 不支援複寫資料夾中建立 NTFS 永久連結來或檔案，這樣做可能會導致複寫問題與受影響的檔案。 永久連結檔案由 「 DFS 複寫會忽略，而且不會複寫。 連接點也不會複寫，及 DFS 複寫記錄檔事件 4406 遇到的每個連接點。  
      
  - DFS 複寫所複寫的只重新分析點是採用 IO\_重新分析\_標記\_符號連結標記; 不過，DFS 複寫並不保證符號連結的目標也會複寫。 如需詳細資訊，請參閱 <<c0> [ 詢問目錄服務小組部落格](http://blogs.technet.com/b/askds/archive/2011/09/30/friday-mail-sack-super-slo-mo-edition.aspx)。  
      
  - 檔案 IO\_重新分析\_標記\_重複資料刪除、 IO\_重新分析\_標記\_SIS 或 IO\_重新分析\_標記\_複寫 HSM 重新分析標記一般檔案。 重新分析標記和重新分析資料緩衝區不會複寫到其他伺服器因為重新分析點僅適用於本機系統上。 因此，DFS 複寫都可以複寫在使用 Windows Server 2012 或單一執行個體儲存體 (SIS) 中的重複資料刪除的磁碟區上的資料夾，不過，資料重複資料刪除資訊會分別維護每個角色服務已啟用的伺服器。  
      

### <a name="does-dfs-replication-replicate-timestamp-changes-if-no-other-changes-are-made-to-the-file"></a>沒有 DFS 複寫複寫時間戳記的變更，如果對檔案不進行任何其他變更嗎？

否，DFS 複寫不會複寫的檔案的唯一變更的時間戳記的變更。 此外，已變更的時間戳記不會複寫到複寫群組的其他成員除非對檔案進行其他變更。

### <a name="does-dfs-replication-replicate-updated-permissions-on-a-file-or-folder"></a>DFS 複寫進行複寫更新的權限的檔案或資料夾嗎？

是的。 DFS 複寫會將檔案和資料夾的權限變更。 只有部分相關聯的存取控制清單 (ACL) 的檔案會複寫，雖然 DFS 複寫到臨時區域仍然必須讀取整個檔案。


> [!NOTE]
> 變更大量檔案的 Acl 會對複寫效能的影響。 不過，當使用 RDC，資料傳輸數量成正比的 Acl，而非整個檔案的大小的大小。 進出快取資料夾，就必須讀取的檔案是，仍然與檔案的大小成正比的磁碟流量。 
<br>


### <a name="does-dfs-replication-support-merging-text-files-in-the-event-of-a-conflict"></a>DFS 複寫是否支援合併的文字檔案，在發生衝突？

DFS 複寫不會合併檔案，就會發生衝突時。 不過，它會嘗試保留舊版的檔案中隱藏 DfsrPrivate\\ConflictandDeleted 資料夾的電腦上已偵測到這個衝突。

### <a name="does-dfs-replication-use-encryption-when-transmitting-data"></a>傳輸資料時，不會 DFS 複寫使用加密？

是的。 DFS 複寫會使用遠端程序呼叫 (RPC) 連線，並加密。

### <a name="is-it-possible-to-disable-the-use-of-encrypted-rpc"></a>是否可以停用加密的 RPC 使用？

資料分割 DFS 複寫服務會透過 TCP 使用遠端程序呼叫 (RPC)，來複寫資料。 若要保護在網際網路上的資料傳輸，DFS 複寫服務設計為一律使用 驗證等級常數`RPC_C_AUTHN_LEVEL_PKT_PRIVACY`。 這可確保一律會加密在網際網路上的 RPC 通訊。 因此，不能夠停用加密的 RPC 使用 DFS 複寫服務。

如需詳細資訊，請參閱下列 Microsoft 網站：

  - [RPC 技術參考](http://go.microsoft.com/fwlink/?linkid=182278)  
      
  - [關於遠端差異壓縮](http://go.microsoft.com/fwlink/?linkid=182279)  
      
  - [驗證等級常數](http://go.microsoft.com/fwlink/?linkid=182280)  
      

### <a name="how-are-simultaneous-replications-handled"></a>如何處理同時的複寫？

沒有每個複寫資料夾的一個更新管理員。 更新管理員彼此的工作。

根據預設，最多 16 (四個在 Windows Server 2003 R2) 的同時下載數目由所有連接和複寫群組共用。 連線和複寫群組的更新會序列化，因為接收到更新沒有特定順序。 如果已開啟兩個排程，通常收到並安裝更新從這兩個連線一次。

### <a name="how-do-i-force-replication-or-polling"></a>如何強制複寫或輪詢？

您可以立即強制複寫使用 DFS 管理中所述[編輯複寫排程](https://technet.microsoft.com/en-us/library/Cc732278)。 您也可以使用 強制複寫`Sync-DfsReplicationGroup`所導入的 Windows Server 2012 R2，DFSR PowerShell 模組內含的 cmdlet 或**Dfsrdiag SyncNow**命令。 您可以使用，以強制輪詢`Update-DfsrConfigurationFromAD`cmdlet，或有**Dfsrdiag pollad /** 命令。

### <a name="is-it-possible-to-configure-a-quiet-time-between-replications-for-files-that-change-frequently"></a>是否可以設定複寫經常變更的檔案之間的無訊息的時間？

資料分割 如果排程已開啟，DFS 複寫會複寫變更，如發現它們。 設定檔的靜止時間沒有辦法。

### <a name="is-it-possible-to-configure-one-way-replication-with-dfs-replication"></a>是否可以使用 DFS 複寫設定單向複寫？

是的。 如果您使用 Windows Server 2012 或 Windows Server 2008 R2，您可以建立唯讀的複寫的資料夾複寫透過單向連線的內容。 如需詳細資訊，請參閱 <<c0> [ 的特定成員上進行複寫資料夾設為唯讀](http://go.microsoft.com/fwlink/?linkid=156740)(http://go.microsoft.com/fwlink/?LinkId=156740)。

我們不支援使用 Windows Server 2008 或 Windows Server 2003 R2 中 DFS 複寫建立單向複寫連線。 如此一來，可能會造成許多的問題包括健康情況檢查拓撲錯誤、 執行問題，以及 DFS 複寫資料庫的問題。

如果您使用 Windows Server 2008 或 Windows Server 2003 R2，您可以模擬單向連線，藉由執行下列動作：

  - 訓練系統管理員只能在您想要指定為主要伺服器的伺服器上進行變更。 然後讓變更複寫到目的地伺服器。  
      
  - 在目的地伺服器上設定共用權限的使用者沒有寫入權限。 如果不允許變更分支在伺服器上，就沒有資料複寫回來，模擬單向連線，並降低 WAN 使用量。  
      

### <a name="is-there-a-way-to-force-a-complete-replication-of-all-files-including-unchanged-files"></a>是否有強制完整的複寫，包括未變更的檔案的所有檔案的方法？

資料分割 如果 DFS 複寫會將檔案視為相同，它不會複寫它們。 如果尚未複寫變更的檔案，DFS 複寫會自動將它們複寫時設定為執行這項操作。 若要覆寫設定的排程，請使用 WMI 方法**ForceReplicate()** 。 不過，這是只覆寫的排程，並不會強制複寫變更或完全相同的檔案。

### <a name="what-happens-if-the-primary-member-suffers-a-database-loss-during-initial-replication"></a>如果主要成員資料庫遺失初始複寫期間發生什麼事？

在初始複寫，主要成員的檔案將會一律優先如果接收成員的不同版本的主要成員上的檔案就會發生的衝突解決中。 指定的主要成員會儲存在 Active Directory 網域服務，與主要成員已準備好要複製之後, 但在所有成員的複寫都群組複寫之前，會清除指定的位置。

如果在初始複寫失敗，或 DFS 複寫服務重新啟動在複寫期間，主要成員就會看見本機的 DFS 複寫資料庫中指定的主要成員，然後重試初始複寫。 如果清除 指定 Active Directory 網域服務中的主要位置後，主要成員的 DFS 複寫資料庫會遺失，但複寫群組的所有成員都完成初始複寫之前，所有的複寫群組的成員無法複寫資料夾，因為沒有伺服器指定為主要的成員。 如果發生這種情況，使用**Dfsradmin 成員資格/設定 /isprimary:true**命令手動還原主要成員指定主要成員伺服器上。

如需初始複寫的詳細資訊，請參閱[建立複寫群組](https://technet.microsoft.com/en-us/library/cc725893)。


> [!WARNING]
> 主要成員指定是只在初始複寫期間使用。 如果您使用<STRONG>Dfsradmin</STRONG>命令，以指定主要成員複寫資料夾複寫之後完成時，DFS 複寫不會無法將伺服器指定為 Active Directory 網域服務的主要成員。 不過，如果伺服器上的 DFS 複寫資料庫接著會因無法復原的損毀或資料遺失，伺服器會嘗試與主要成員，而不是從另一個成員的複寫復原其資料執行的初始複寫群組。 基本上，伺服器會變成 rogue 主要伺服器，可能會造成衝突。 基於這個理由，主要成員手動指定只有您是否確定無可挽回的嚴重失敗的初始複寫。 
<br>


### <a name="what-happens-if-the-replication-schedule-closes-while-a-file-is-being-replicated"></a>如果這個複寫排程關閉複寫檔案時，發生什麼事？

如果在連接上啟用遠端差異壓縮 (RDC)，請輸入大於開始複寫之前的排程結束的 64 KB 的檔案複寫 (或變更為**沒有頻寬**) 會繼續時排程會開啟 (或變更加入到項目以外**沒有頻寬**)。 複寫會繼續複寫已停止時的狀態。

如果 RDC 已關閉，則 DFS 複寫完全重新啟動檔案傳輸。 這可以延遲接收成員，您可以使用檔案時。

### <a name="what-happens-when-two-users-simultaneously-update-the-same-file-on-different-servers"></a>當兩位使用者同時更新不同伺服器上相同的檔案時，會發生什麼事？

當 DFS 複寫偵測到衝突時，它會使用上一次儲存檔案的版本。 它將另一個檔案移到 DfsrPrivate\\ConflictandDeleted 資料夾 （在解決衝突的電腦上的複寫資料夾的本機路徑）。 它那里會一直因衝突而刪除資料夾清除 [因衝突而刪除] 資料夾超過設定的大小或 DFS 複寫發生的磁碟空間的錯誤訊息不足時，就會發生。 因衝突而刪除 」 資料夾不在複寫中，與這種衝突解決方法可避免在 FRS 變形目錄的問題

發生衝突時，DFS 複寫會將告知性事件記錄到 DFS 複寫事件記錄檔。 此事件不需要使用者動作，原因如下：

  - 它看不到使用者 （它是只有伺服器系統管理員可以看到）。  
      
  - DFS 複寫會視為快取中的 [因衝突而刪除] 資料夾。 當達到配額閾值時，它會清除掉某些資料這些檔案。 不沒有衝突的檔案將會儲存任何保證。  
      
  - 衝突可能位在不同於衝突的來源伺服器上。  
      

## <a name="staging"></a>執行

### <a name="does-dfs-replication-continue-staging-files-when-replication-is-disabled-by-a-schedule-or-bandwidth-throttling-quota-or-when-a-connection-is-manually-disabled"></a>沒有 DFS 複寫會繼續複寫為停用的排程或頻寬節流設定配額，或手動停用連線時，暫存檔案嗎？

資料分割 如果尚未超過的頻寬節流的配額，或連線已停用 DFS 複寫不會繼續進行排定的複寫時間之外的暫存檔案。

### <a name="does-dfs-replication-prevent-other-applications-from-accessing-a-file-during-staging"></a>DFS 複寫是否防止其他應用程式接移期間存取檔案？

資料分割 DFS 複寫不會封鎖使用者或應用程式的 replication 資料夾中開啟檔案的方式開啟檔案。 此方法稱為 「 隨機鎖定 」。

### <a name="is-it-possible-to-change-the-location-of-the-staging-folder-with-the-dfs-management-tool"></a>是否可以變更執行資料夾使用 DFS 管理工具的位置？

是的。 暫存資料夾位置上設定**進階**索引標籤**屬性**對話方塊中，針對每個複寫群組成員。

### <a name="when-are-files-staged"></a>何時暫存檔案？

當接收成員要求檔案，將會分段傳送成員上的檔案 (除非檔案是 64 KB 或更小) 下, 表所示。 遠端差異壓縮 (RDC) 已停用連接上，檔案會分段，除非它是 256 KB 或更小。 檔案也暫存接收成員，才傳送它們是否小於 64 KB 的大小，雖然您可以設定這項設定介於 16 KB 到 1 MB。 如果排程已關閉，無法暫存檔案。

### <a name="the-minimum-file-sizes-for-staging-files"></a>暫存檔案的最小的檔案大小

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>RDC 已啟用</th>
<th>RDC 停用</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>傳送的成員</p></td>
<td><p>64 KB</p></td>
<td><p>256 KB</p></td>
</tr>
<tr class="odd">
<td><p>接收成員</p></td>
<td><p>預設的 64 KB</p></td>
<td><p>預設的 64 KB</p></td>
</tr>
</tbody>
</table>

### <a name="what-happens-if-a-file-is-changed-after-it-is-staged-but-before-it-is-completely-transmitted-to-the-remote-site"></a>如果它接移後，但它完全傳輸至遠端站台的檔案有所變更，發生什麼事？

如果已在傳送檔案的任何部分，DFS 複寫會繼續傳輸。 如果 DFS 複寫可讓您開始將檔案傳輸之前，會變更的檔案，則會傳送檔案的較新版本。

## <a name="change-history"></a>變更記錄


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Date</th>
<th>描述</th>
<th>`Reason`</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>2018 年 11 月 15 日</p></td>
<td><p>更新適用於 Windows Server 2019。</p></td>
<td><p>新的作業系統。</p></td>
</tr>
<tr class="even">
<td><p>2013 年 10 月 9 日</p></td>
<td><p>已更新的支援的限制為何 DFS 複寫的？從 Windows Server 2012 R2 上的測試結果區段。</p></td>
<td><p>最新版本的 Windows Server 更新</p></td>
</tr>
<tr class="odd">
<td><p>2013 年 1 月 30日日</p></td>
<td><p>新增不 DFS 複寫繼續複寫為停用的排程或頻寬節流設定配額，或手動停用連線時，暫存檔案嗎？項目。</p></td>
<td><p>客戶的問題</p></td>
</tr>
<tr class="even">
<td><p>2012 年 10 月 31日日</p></td>
<td><p>編輯功能是支援的 DFS 複寫的限制嗎？若要增加磁碟區上的複寫檔案的測試的數目的項目。</p></td>
<td><p>客戶回函</p></td>
</tr>
<tr class="odd">
<td><p>2012 年 8 月 15日日</p></td>
<td><p>編輯未 DFS 複寫進行複寫 NTFS 檔案權限、 替代資料流、 永久連結，以及重新分析點嗎？項目，進一步釐清 DFS 複寫會永久連結的處理和重新分析點。</p></td>
<td><p>在客戶支援服務的意見反應</p></td>
</tr>
<tr class="even">
<td><p>2012 年 6 月 13日日</p></td>
<td><p>編輯 ReFS 或 FAT 磁碟區上的沒有 DFS 複寫工作嗎？要加入的 ReFS 討論的項目。</p></td>
<td><p>客戶回函</p></td>
</tr>
<tr class="odd">
<td><p>2012 年 4 月 25日日</p></td>
<td><p>編輯未 DFS 複寫進行複寫 NTFS 檔案權限、 替代資料流、 永久連結，以及重新分析點嗎？若要釐清 DFS 複寫的永久連結的處理方式的項目。</p></td>
<td><p>減少潛在的混淆</p></td>
</tr>
<tr class="even">
<td><p>2011 年 3 月 30日日</p></td>
<td><p>編輯複寫 Outlook.pst 可以 DFS 複寫或 Microsoft Office Access 資料庫檔案嗎？若要更正使用.pst 和 Access 檔案中的 DFS 複寫的潛在影響的項目。</p>
<p>新增如何提升複寫效能？</p></td>
<td><p>客戶先前的項目，這項設定不正確地表示複寫.pst 或存取的檔案可能損毀的 DFS 複寫的資料庫有關的問題。</p></td>
</tr>
<tr class="odd">
<td><p>2011 年 1 月 26日日</p></td>
<td><p>新增如何可以復原檔案從 ConflictAndDeleted 或預先存在的資料夾嗎？</p></td>
<td><p>客戶回函</p></td>
</tr>
<tr class="even">
<td><p>2010 年 10 月 20日日</p></td>
<td><p>新增方式可以升級或取代 DFS 複寫的成員？</p></td>
<td><p>客戶回函</p></td>
</tr>
</tbody>
</table>

