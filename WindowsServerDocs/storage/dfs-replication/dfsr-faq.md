---
title: DFS 複寫：常見問題集 (FAQ)
ms.date: 06/18/2014
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 92fe505c3ae7d76f7a8d5bd9d2ed0ce845159fde
ms.sourcegitcommit: de71970be7d81b95610a0977c12d456c3917c331
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2019
ms.locfileid: "71940753"
---
# <a name="dfs-replication-frequently-asked-questions-faq"></a>DFS 複寫：常見問題集 (FAQ)


更新日期：2019年4月30日

適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

此常見問題會回答適用于 Windows Server 的分散式檔案系統（DFS）複寫（也稱為 DFS-R 或 DFSR）相關問題。

如需 dfs 命名空間的相關[資訊，請參閱 dfs 命名空間：](https://technet.microsoft.com/library/ee404780)常見問題。

如需 DFS 複寫新功能的詳細資訊，請參閱下列主題：

  - [DFS 命名空間和 DFS 複寫總覽](https://technet.microsoft.com/library/jj127250)（在 Windows Server 2012 中）  
      
  - [Windows server 2008 到 Windows server 2008 R2 的功能變更](https://technet.microsoft.com/library/dd391932)中[分散式檔案系統主題的新](https://technet.microsoft.com/library/ee307957)功能  
      
  - [Windows server 2003 SP1 到 Windows server 2008 的功能變更](https://technet.microsoft.com/library/cc753208)中的[分散式檔案系統](https://technet.microsoft.com/library/cc753479)主題  
      

如需近期對本主題所做變更的清單，請參閱本主題的＜[變更歷程記錄](#change-history)＞一節。

      

## <a name="interoperability"></a>互通性

### <a name="can-dfs-replication-communicate-with-frs"></a>DFS 複寫可以與 FRS 通訊嗎？

資料分割 DFS 複寫不會與檔案複寫服務（FRS）通訊。 DFS 複寫和 FRS 可以同時在同一部伺服器上執行，但絕不能將它們設定為複寫相同的資料夾或子資料夾，因為這樣做可能會造成資料遺失。

### <a name="can-dfs-replication-replace-frs-for-sysvol-replication"></a>可以 DFS 複寫取代適用于 SYSVOL 複寫的 FRS

是，在執行 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 的伺服器上，DFS 複寫可以取代適用于 SYSVOL 複寫的 FRS。 執行 Windows Server 2003 R2 的伺服器不支援使用 DFS 複寫來複寫 SYSVOL 資料夾。

如需使用 DFS 複寫複寫 SYSVOL 的詳細資訊，請參閱[SYSVOL 複寫遷移指南：要 DFS 複寫](https://technet.microsoft.com/library/dd640019)的 FRS。

### <a name="can-i-upgrade-from-frs-to-dfs-replication-without-losing-configuration-settings"></a>我可以從 FRS 升級至 DFS 複寫，而不會遺失設定嗎？

是的。 若要將複寫從 FRS 遷移至 DFS 複寫，請參閱下列檔：

  - 若要遷移 SYSVOL 資料夾以外的資料夾複寫，請參閱[DFS 操作指南：從 frs 遷移到 DFS 複寫](http://go.microsoft.com/fwlink/?linkid=192776)和[FRS2DFSR – frs 到 DFSR 遷移公用程式](http://go.microsoft.com/fwlink/?linkid=195437)（ http://go.microsoft.com/fwlink/?LinkID=195437) ）。  
      
  - 若要將 SYSVOL 資料夾的複寫遷移到 DFS 複寫， [請參閱 sysvol 複寫遷移指南：要 DFS 複寫](https://technet.microsoft.com/library/dd640019)的 FRS。  
      

### <a name="can-i-use-dfs-replication-in-a-mixed-windowsunix-environment"></a>我可以在混合式 Windows/UNIX 環境中使用 DFS 複寫嗎？

是的。 雖然 DFS 複寫僅支援在執行 Windows Server 的伺服器之間複寫內容，但 UNIX 用戶端可以存取 Windows 伺服器上的檔案共用。 若要這麼做，請在 DFS 複寫伺服器上安裝適用于網路檔案系統（NFS）的服務。

您也可以使用許多 UNIX 用戶端內含的 SMB/CIFS 用戶端功能來直接存取 Windows 檔案共用，雖然這項功能通常受限或需要修改 Windows 環境（例如，使用來停用 SMB 簽署）群組原則）。

DFS 複寫在執行 Windows Server 作業系統的伺服器上與 NFS 交互操作，但您無法複寫 NFS 掛接點。

### <a name="can-i-use-the-volume-shadow-copy-service-with-dfs-replication"></a>我可以搭配 DFS 複寫使用磁碟區陰影複製服務嗎？

是的。 磁碟區陰影複製服務（VSS）磁片區支援 DFS 複寫，而且先前的快照集可以與舊版用戶端順利還原。

### <a name="can-i-use-windowsbackup-ntbackupexe-to-remotely-back-up-a-replicated-folder"></a>我可以使用 Windows Backup （Ntbackup）從遠端備份複寫資料夾嗎？

否，在執行 windows Server 2003 或更早版本的電腦上使用 Windows Backup （Ntbackup）來備份執行 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 的電腦上複寫資料夾的內容。

若要備份儲存在複寫資料夾中的檔，請使用 Windows Server Backup 或 Microsoft® System Center Data Protection Manager。 如需有關 Windows Server 2008 R2 和 Windows Server 2008 中備份和復原功能的詳細資訊，請參閱[備份和](https://technet.microsoft.com/library/Cc754097)復原。 如需詳細資訊，請參閱[System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=182261) （ http://go.microsoft.com/fwlink/?LinkId=182261) 。

### <a name="do-file-system-policies-impact-dfs-replication"></a>檔案系統原則是否會影響 DFS 複寫？

是的。 請勿在複寫資料夾上設定檔案系統原則。 檔案系統原則會在每個群組原則重新整理間隔重新套用 NTFS 許可權。 這可能會導致共用違規，因為在檔案關閉之前，不會複寫已開啟的檔案。

### <a name="does-dfs-replication-replicate-mailboxes-hosted-on-microsoft-exchange-server"></a>DFS 複寫複寫裝載在 Microsoft Exchange Server 上的信箱嗎？

資料分割 DFS 複寫不能用來複寫裝載在 Microsoft Exchange Server 上的信箱。

### <a name="does-dfs-replication-support-file-screens-created-by-file-server-resource-manager"></a>DFS 複寫是否支援檔案伺服器 Resource Manager 所建立的檔案檢測？

是的。 但是，檔案伺服器 Resource Manager （FSRM）檔案檢測設定必須符合複寫的兩端。 此外，DFS 複寫有自己的檔案和資料夾篩選機制，可讓您用來從複寫中排除特定檔案和檔案類型。

以下是用來執行檔案檢測或配額的最佳作法：

  - 隱藏的 DfsrPrivate 資料夾不得受限於配額或檔案檢測。  
      
  - 篩選後的檔案不能存在於任何已複寫的資料夾中，因此必須啟用檢測。  
      
  - 在啟用配額之前，沒有任何資料夾可能會超過配額。  
      
  - 您必須謹慎使用硬體配額。 複寫群組的個別成員可能會在複寫之前維持在配額內，但在複寫檔案時，則會超出此限制。 例如，如果使用者將 10 mb 的檔案複製到伺服器 A （這是固定限制），而另一位使用者將 5 MB 的檔案複製到伺服器 B，則在下次複寫發生時，這兩部伺服器將會超過 5 mb 的配額。 這可能會導致 DFS 複寫持續重試複寫檔案，而造成版本向量中的漏洞，以及可能的效能問題。  
      

### <a name="is-dfs-replication-cluster-aware"></a>DFS 複寫叢集感知嗎？

是，Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2008 R2 中的 DFS 複寫包括將容錯移轉叢集新增為複寫群組成員的功能。 如需詳細資訊，請參閱[將容錯移轉叢集新增至](http://go.microsoft.com/fwlink/?linkid=155085)複寫 http://go.microsoft.com/fwlink/?LinkId=155085) 群組（。 Windows Server 2008 R2 之前的 Windows 版本上的 DFS 複寫服務不是設計來與容錯移轉叢集協調，而且服務也不會容錯移轉到另一個節點。


> [!NOTE]
> DFS 複寫不支援複寫叢集共用磁片區上的檔案。 
<br>


### <a name="is-dfs-replication-compatible-with-data-deduplication"></a>DFS 複寫與重復資料刪除相容嗎？

是，DFS 複寫可以在 Windows Server 中使用重復資料刪除的磁片區上複寫資料夾。

### <a name="is-dfs-replication-compatible-with-ris-and-wds"></a>DFS 複寫與 RIS 和 WDS 相容嗎？

是的。 DFS 複寫複寫已啟用單一實例儲存體（SIS）的磁片區。 SIS 是由遠端安裝服務（RIS）、Windows 部署服務（WDS）和 Windows Storage Server 所使用。

### <a name="is-it-possible-to-use-dfs-replication-with-offline-files"></a>是否可以搭配離線檔案使用 DFS 複寫？

當寫入檔案時，如果一次只有一個使用者，您就可以安全地在案例中使用 DFS 複寫和離線檔案。 這適用于在兩個分公司之間移動，而且想要能夠在任一分支或離線存取其檔案的使用者。 離線檔案在本機快取檔案以供離線使用，並 DFS 複寫在每個分公司之間複寫資料。

請勿在多使用者環境中搭配離線檔案使用 DFS 複寫，因為 DFS 複寫不會提供任何分散式鎖定機制或檔案簽出功能。 如果兩個使用者在不同的伺服器上同時修改相同的檔案，DFS 複寫在下一次複寫期間，\\將較舊的檔案移至 DfsrPrivate ConflictandDeleted 資料夾（位於複寫資料夾的本機路徑底下）。

### <a name="what-antivirus-applications-are-compatible-with-dfs-replication"></a>哪些防毒軟體應用程式與 DFS 複寫相容？

如果防毒軟體應用程式的掃描活動改變了複寫資料夾中的檔案，則可能會造成過多的複寫。 如需詳細資訊，請 DFS 複寫（ http://go.microsoft.com/fwlink/?LinkId=73990) ）[測試防毒軟體應用程式的互通性](http://go.microsoft.com/fwlink/?linkid=73990)。

### <a name="what-are-the-benefits-of-using-dfs-replication-instead-of-windows-sharepoint-services"></a>使用 DFS 複寫而不是 Windows SharePoint Services 有哪些優點？

Windows® SharePoint®服務以檔簽出功能的形式提供緊密的一致性，DFS 複寫不會這樣做。 如果您擔心多人編輯相同的檔案，建議使用 Windows SharePoint Services。 Windows SharePoint Services 2.0 （含 Service Pack 2）可作為 Windows Server 2003 R2 的一部分。 您可以從 Microsoft 網站下載 Windows SharePoint Services;這不包含在較新版本的 Windows Server 中。 不過，如果您要跨多個網站複寫資料，而且使用者不會同時編輯相同的檔案，DFS 複寫提供更大的頻寬和更簡單的管理。

## <a name="limitations-and-requirements"></a>限制和需求

### <a name="can-dfs-replication-replicate-between-branch-offices-without-a-vpn-connection"></a>不需要 VPN 連線，就可以 DFS 複寫在分公司之間進行複寫嗎？

是-假設有一個私人廣域網路（WAN）連結（而不是網際網路）連接分公司。 不過，您必須在外部防火牆中開啟適當的埠。 DFS 複寫使用 RPC 端點對應程式（埠135）和超過1024的隨機指派暫時埠。 您可以使用**Dfsrdiag**命令列工具來指定靜態埠，而不是暫時埠。 如需如何指定 RPC 端點對應程式的詳細資訊，請參閱 Microsoft 知識庫中的[文章 154596](http://go.microsoft.com/fwlink/?linkid=73991) （ http://go.microsoft.com/fwlink/?LinkId=73991) 。

### <a name="can-dfs-replication-replicate-files-encrypted-with-the-encrypting-file-system"></a>DFS 複寫可以複寫以加密檔案系統加密的檔案嗎？

資料分割 DFS 複寫不會複寫使用加密檔案系統（EFS）加密的檔案或資料夾。 如果使用者加密先前複寫的檔案，DFS 複寫會將檔案從複寫群組的所有其他成員中刪除。 這可確保唯一可用的檔案複本是伺服器上的加密版本。

### <a name="can-dfs-replication-replicate-outlook-pst-or-microsoft-office-access-database-files"></a>DFS 複寫可以複寫 Outlook .pst 或 Microsoft Office Access 資料庫檔案嗎？

DFS 複寫可以安全地複寫 Microsoft Outlook 個人資料夾（.pst）和 Microsoft Access 檔案（僅在儲存以供封存之用），而且不能透過網路使用 Outlook 或 Access 等用戶端存取（以開啟 .pst 或存取）檔案中，先將檔案複製到本機存放裝置。 這種情況的原因如下：

  - 透過網路連線開啟 .pst 檔案可能會導致 .pst 檔案中的資料損毀。 如需有關為何無法安全地從網路存取 pst 檔案的詳細資訊，請參閱 Microsoft 知識庫中的[文章 297019](http://go.microsoft.com/fwlink/?linkid=125363) （ http://go.microsoft.com/fwlink/?LinkId=125363) 。  
      
  - .pst 和 Access 檔案通常會在用戶端（例如 Outlook 或 Office Access）存取時保持開啟一段很長的時間。 這可防止 DFS 複寫複寫這些檔案，直到它們關閉為止。  
      

### <a name="can-i-use-dfs-replication-in-a-workgroup"></a>我可以在工作組中使用 DFS 複寫嗎？

資料分割 DFS 複寫依賴 Active Directory®網域服務進行設定。 它只會在網域中工作。

### <a name="can-more-than-one-folder-be-replicated-on-a-single-server"></a>可以在單一伺服器上複寫一個以上的資料夾嗎？

是的。 DFS 複寫可以在伺服器之間複寫多個資料夾。 請確定每個複寫資料夾都有唯一的根路徑，而且不會重迭。 例如，d：\\sales 和 d：\\Accounting 可以是兩個複寫資料夾的根路徑，但 d：\\sales 和 d：\\sales\\報表不能是兩個複寫資料夾的根路徑。

### <a name="does-dfs-replication-require-dfs-namespaces"></a>DFS 複寫需要 DFS 命名空間嗎？

資料分割 DFS 複寫和 DFS 命名空間可以單獨或一起使用。 此外，DFS 複寫可以用來複寫獨立的 DFS 命名空間，這不是 FRS 的可能情況。

### <a name="does-dfs-replication-require-time-synchronization-between-servers"></a>DFS 複寫需要伺服器之間的時間同步處理嗎？

資料分割 DFS 複寫不會明確要求伺服器之間的時間同步處理。 不過，DFS 複寫會要求伺服器時鐘必須密切符合。 伺服器時鐘必須設定在彼此的五分鐘內（根據預設），Kerberos 驗證才能正常運作。 例如，DFS 複寫會使用時間戳來判斷當發生衝突時，會優先處理哪一個檔案。 精確的時間對於垃圾收集、排程和其他功能也很重要。

### <a name="does-dfs-replication-support-replicating-an-entire-volume"></a>DFS 複寫是否支援複寫整個磁片區？

是的。 不過，您必須先安裝 Windows Server 2003 Service Pack 2 或此修補程式。 如需詳細資訊，請參閱 Microsoft 知識庫中的[文章 920335](http://go.microsoft.com/fwlink/?linkid=76776) （ http://go.microsoft.com/fwlink/?LinkId=76776) 。 此外，複寫整個磁片區可能會造成下列問題：

  - 如果磁片區包含 Windows 分頁檔案，複寫就會失敗，並在系統事件記錄檔中記錄 DFSR 事件4312。  
      
  - DFS 複寫會在目的地伺服器上的複寫資料夾上設定系統和隱藏的屬性。 這是因為 Windows 預設會將系統和隱藏的屬性套用至磁片區根資料夾。 如果目的地伺服器上複寫資料夾的本機路徑也是磁片區根目錄，則不會對資料夾屬性進行進一步的變更。  
      
  - 當複寫包含 Windows 系統資料夾的磁片區時，DFS 複寫會辨識% WINDIR% 資料夾，而不會將它複寫。 不過，DFS 複寫會複寫非 Microsoft 應用程式所使用的資料夾，如果應用程式具有 DFS 複寫的互通性問題，可能會導致目的地伺服器上的應用程式失敗。  
      

### <a name="does-dfs-replication-support-rpc-over-http"></a>DFS 複寫是否支援透過 HTTP 的 RPC？

資料分割

### <a name="does-dfs-replication-work-across-wireless-networks"></a>DFS 複寫可以在無線網路上運作嗎？

是的。 DFS 複寫與連線類型無關。

### <a name="does-dfs-replication-work-on-refs-or-fat-volumes"></a>DFS 複寫在 ReFS 或 FAT 磁片區上運作嗎？

資料分割 DFS 複寫僅支援使用 NTFS 檔案系統格式化的磁片區;不支援復原檔案系統（ReFS）和 FAT 檔案系統。 DFS 複寫需要 NTFS，因為它使用 ntfs 變更日誌和 NTFS 檔案系統的其他功能。

### <a name="does-dfs-replication-work-with-sparse-files"></a>DFS 複寫使用稀疏檔案嗎？

是的。 您可以複寫稀疏檔案。 **Sparse**屬性會保留在接收成員上。

### <a name="do-i-need-to-log-in-as-administrator-to-replicate-files"></a>我需要以系統管理員身分登入才能複寫檔案嗎？

資料分割 DFS 複寫是在本機系統帳戶下執行的服務，因此您不需要以系統管理員身分登入來進行複寫。 不過，您必須是受影響檔案伺服器的網域系統管理員或本機系統管理員，才能對 DFS 複寫設定進行變更。

如需詳細資訊，請參閱[委派管理 DFS 複寫的能力](http://go.microsoft.com/fwlink/?linkid=182294)（ http://go.microsoft.com/fwlink/?LinkId=182294) ）中的「DFS 複寫安全性需求和委派」。

### <a name="how-can-i-upgrade-or-replace-a-dfs-replication-member"></a>如何升級或取代 DFS 複寫成員？

若要升級或取代 DFS 複寫的成員，請參閱詢問目錄服務小組 blog 的此 blog 文章：[取代 DFSR 成員硬體或 OS](http://blogs.technet.com/b/askds/archive/2010/09/10/series-wrap-up-and-downloads-replacing-dfsr-member-hardware-or-os.aspx)。

### <a name="is-dfs-replication-suitable-for-replicating-roaming-profiles"></a>DFS 複寫適合用來複寫漫遊設定檔嗎？

是的。 複寫漫遊使用者設定檔時，支援特定案例。 如需有關支援案例的詳細資訊，請參閱 Microsoft 對複寫[之使用者設定檔資料的支援聲明](http://go.microsoft.com/fwlink/?linkid=201282)（ http://go.microsoft.com/fwlink/?LinkId=201282) 。

### <a name="is-there-a-file-character-limit-or-limit-to-the-folder-depth"></a>資料夾深度是否有檔案字元限制或限制？

Windows 和 DFS 複寫支援最多32000個字元的資料夾路徑。 DFS 複寫不限於260個字元的資料夾路徑。

### <a name="must-members-of-a-replication-group-reside-in-the-same-domain"></a>複寫群組的成員必須位於相同的網域中嗎？

資料分割 複寫群組可以跨越單一樹系中的網域，但不能跨越不同的樹系。

### <a name="what-are-the-supported-limits-of-dfs-replication"></a>DFS 複寫支援的限制為何？

下列清單提供一組已由 Microsoft 測試並適用于 Windows Server 2012 R2、Windows Server 2016 和 Windows Server 2019 的擴充性指導方針。

  - 伺服器上所有已複寫檔案的大小：100 tb。  
      
  - 磁片區上已複寫的檔案數目：70000000。  
      
  - 檔案大小上限：250 gb。  
      


> [!IMPORTANT]
> 建立具有大量或大小檔案的複寫群組時，我們建議您匯出資料庫複製並使用預先植入技術，將初始複寫的持續時間降至最低。 如需詳細資訊， [請參閱 DFS 複寫 Windows Server 2012 R2 中的初始同步處理：複製](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/DFS-Replication-Initial-Sync-in-Windows-Server-2012-R2-Attack-of/ba-p/424877)的攻擊。 
<br>


下列清單提供一組 Microsoft 在 Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008 上測試過的擴充性指導方針：

  - 伺服器上所有已複寫檔案的大小：10 tb。  
      
  - 磁片區上已複寫的檔案數目：11000000。  
      
  - 檔案大小上限：64 gb。  
      


> [!NOTE]
> 複寫群組、複寫資料夾、連接或複寫群組成員的數目已不再有限制。 
<br>


如需 Microsoft 針對 Windows Server 2003 R2 測試過的擴充性指導方針清單，請參閱[DFS 複寫擴充性指導方針](http://go.microsoft.com/fwlink/?linkid=75043)（ http://go.microsoft.com/fwlink/?LinkId=75043) ）。

### <a name="when-should-i-not-use-dfs-replication"></a>我何時應該不要使用 DFS 複寫？

請勿在多個使用者在不同伺服器上同時更新或修改相同檔案的環境中使用 DFS 複寫。 這麼做可能會導致 DFS 複寫將衝突的檔案複本移動到隱藏的 DfsrPrivate\\ConflictandDeleted 資料夾。

當多個使用者同時需要在不同的伺服器上同時修改相同的檔案時，請使用 Windows SharePoint Services 的檔案簽出功能，以確保只有一個使用者正在處理某個檔案。 Windows SharePoint Services 2.0 （含 Service Pack 2）可作為 Windows Server 2003 R2 的一部分。 您可以從 Microsoft 網站下載 Windows SharePoint Services;這不包含在較新版本的 Windows Server 中。

### <a name="why-is-a-schema-update-required-for-dfs-replication"></a>為什麼需要 DFS 複寫的架構更新？

DFS 複寫在 Active Directory Domain Services 的網域命名內容中使用新物件來儲存設定資訊。 當您更新 Active Directory Domain Services 架構時，就會建立這些物件。 如需詳細資訊，請參閱[DFS 複寫的審查需求](http://go.microsoft.com/fwlink/?linkid=182264)（ http://go.microsoft.com/fwlink/?LinkId=182264) 。

## <a name="monitoring-and-management-tools"></a>監視與管理工具

### <a name="can-i-automate-the-health-report-to-receive-warnings"></a>我可以將健康狀態報表自動化以接收警告嗎？

是的。 有三種方式可自動執行健康情況報告：

  - 使用包含在 Windows Server 2012 R2 或 DfsrAdmin 中的 DFSR Windows PowerShell 模組搭配排程工作，定期產生健康情況報告。 如需詳細資訊，請參閱[自動化 DFS 複寫健康狀態報表](http://go.microsoft.com/fwlink/?linkid=74010)（ http://go.microsoft.com/fwlink/?LinkId=74010) ）。  
      
  - 使用 System Center Operations Manager 的 DFS 複寫管理元件來建立以指定條件為基礎的警示。  
      
  - 使用 DFS 複寫 WMI 提供者來編寫警示的腳本。  
      

### <a name="can-i-use-microsoft-system-center-operations-manager-to-monitor-dfs-replication"></a>我可以使用 Microsoft System Center Operations Manager 來監視 DFS 複寫嗎？

是的。 如需詳細資訊，請參閱 Microsoft 下載中心的[System Center Operations Manager 2007 的 DFS 複寫管理元件](http://go.microsoft.com/fwlink/?linkid=182265)（ http://go.microsoft.com/fwlink/?LinkId=182265) ）。

### <a name="does-dfs-replication-support-remote-management"></a>DFS 複寫是否支援遠端系統管理？

是的。 DFS 複寫支援使用 DFS 管理主控台和 [**新增複寫群組**] 命令進行遠端系統管理。 例如，在伺服器 A 上，您可以使用伺服器 A 和 B 做為成員，連接到樹系中定義的複寫群組。

DFS 管理包含在 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008 和 Windows Server 2003 R2 中。 若要管理其他 Windows 版本的 DFS 複寫，請使用遠端桌面或[適用于 windows 7 的遠端伺服器管理工具](https://technet.microsoft.com/library/Ee449475)。


> [!IMPORTANT]
> 若要查看或管理包含唯讀複寫資料夾或容錯移轉叢集成員的複寫群組，您必須使用 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、<a href="http://go.microsoft.com/fwlink/p/?linkid=238560">遠端適用于 Windows 8 的伺服器管理工具</a>，或<a href="https://technet.microsoft.com/library/ee449475">適用于 windows 7 的遠端伺服器管理工具</a>。 
<br>


### <a name="do-ultrasound-and-sonar-work-with-dfs-replication"></a>Ultrasound 和 Sonar 會與 DFS 複寫搭配使用嗎？

資料分割 DFS 複寫有自己的一組監視和診斷工具。 Ultrasound 和 Sonar 僅能夠監視 FRS。

### <a name="how-can-files-be-recovered-from-the-conflictanddeleted-or-preexisting-folders"></a>如何從 ConflictAndDeleted 或預先存在的資料夾復原檔案？

若要復原遺失的檔案，請使用 [檔案歷程記錄]、[**還原先前的版本**] 命令，或從備份還原檔案，從檔系統資料夾或共用資料夾還原檔案。 若要直接從 ConflictAndDeleted 或預先存在的資料夾復原檔案， `Get-DfsrPreservedFiles`請`Restore-DfsrPreservedFiles`使用和 windows PowerShell Cmdlet （隨附于 windows Server 2012 R2 中的 DFSR 模組），或 MSDN 的[RestoreDFSR](http://code.msdn.microsoft.com/restoredfsr)範例腳本程式碼庫。 此腳本僅供嚴重損壞修復之用，並依原樣提供（不含擔保）。

### <a name="is-there-a-way-to-know-the-state-of-replication"></a>是否有方法可以得知複寫的狀態？

是的。 有數種方式可以監視複寫：

  - DFS 複寫具有提供主動式監視之 System Center Operations Manager 的管理元件。  
      
  - DFS 管理具有可供複寫待處理專案、複寫效率，以及指定複寫群組中的檔案和資料夾數目的內建診斷報告。  
      
  - Windows Server 2012 R2 中的 DFSR Windows PowerShell 模組包含用於啟動傳播測試和寫入傳播和健康狀態報表的 Cmdlet。 如需詳細資訊，請參閱[Windows PowerShell 中的分散式檔案系統 Replication Cmdlet](https://technet.microsoft.com/library/dn296601.aspx)。  
      
  - Dfsrdiag 是一種命令列工具，可產生待處理專案計數或觸發傳播測試。 兩者都會顯示覆寫的狀態。 傳播會顯示檔案是否複寫到所有節點。 待處理專案會顯示兩部電腦同步之前，仍需要複寫的檔案數目。待處理專案計數是複寫群組成員尚未處理的更新數目。 在執行 Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 的電腦上，Dfsrdiag 也可以顯示 DFS 複寫目前正在複寫的更新。  
      
  - 腳本可以使用 WMI 來收集待處理專案資訊（以手動方式或透過 MOM）。  
      

## <a name="performance"></a>效能

### <a name="does-dfs-replication-support-dial-up-connections"></a>DFS 複寫是否支援撥號連線？

雖然 DFS 複寫會在撥號速度中運作，但如果有大量的變更要複寫，它可能會受到累積。 如果對現有檔案進行小型變更，使用遠端差異壓縮（RDC） DFS 複寫會提供比直接複製檔案更高的效能。

### <a name="does-dfs-replication-perform-bandwidth-sensing"></a>DFS 複寫執行頻寬檢測嗎？

資料分割 DFS 複寫不會執行頻寬檢測。 您可以設定 DFS 複寫，以每個連線為基礎（頻寬節流）使用有限的頻寬量。 不過，如果網路介面變得飽和，DFS 複寫不會進一步降低頻寬使用率，而且 DFS 複寫可以短時間將連結飽和。 DFS 複寫的頻寬節流不完全精確，因為 DFS 複寫藉由節流 RPC 呼叫來調節頻寬。 因此，網路堆疊較低層級（包括 RPC）的各種緩衝區可能會干擾，因而導致網路流量激增。

### <a name="does-dfs-replication-throttle-bandwidth-per-schedule-per-server-or-per-connection"></a>DFS 複寫依排程、每一伺服器或每個連線來節流頻寬嗎？

如果您在指定排程時設定頻寬節流，該複寫群組的所有連接都會使用該設定進行頻寬節流。 您也可以使用 DFS 管理，將頻寬節流設定為連線層級的設定。

### <a name="does-dfs-replication-use-active-directory-domain-services-to-calculate-site-links-and-connection-costs"></a>DFS 複寫是否使用 Active Directory Domain Services 來計算站台連結和連接成本？

資料分割 DFS 複寫會使用系統管理員所定義的拓撲，這與 Active Directory Domain Services 網站成本無關。

### <a name="how-can-i-improve-replication-performance"></a>如何改善複寫效能？

若要深入瞭解微調複寫效能的不同方法，請參閱[詢問目錄服務小組的 blog](http://blogs.technet.com/b/askds/)，以[深入調整 DFSR 中](http://blogs.technet.com/b/askds/archive/2010/03/31/tuning-replication-performance-in-dfsr-especially-on-win2008-r2.aspx)的複寫效能。

### <a name="how-does-dfs-replication-avoid-saturating-a-connection"></a>DFS 複寫如何避免連接飽和？

在 DFS 複寫您會設定您想要在連線上使用的最大頻寬，而此服務會維護該層級的網路使用量。 這與背景智慧型傳送服務（BITS）不同，而且如果您適當地設定連線，DFS 複寫就不會將它飽和。

不過，頻寬節流不是 100% 精確，DFS 複寫可以在短時間內讓連結飽和。 這是因為 DFS 複寫藉由節流 RPC 呼叫來調節頻寬。 由於此程式依賴網路堆疊較低層級的各種緩衝區，包括 RPC，因此複寫流量通常會在高載中行進，而這可能會導致網路連結飽和。

Windows Server 2008 中的 DFS 複寫包含幾項效能增強功能（如[分散式檔案系統](https://technet.microsoft.com/library/Cc753479)所述），這是[WINDOWS server 2003 SP1 到 windows Server 2008 的功能變更](https://technet.microsoft.com/library/cc753208)主題。

### <a name="how-does-dfs-replication-performance-compare-with-frs"></a>DFS 複寫效能與 FRS 的比較如何？

DFS 複寫比 FRS 快得多，特別是當對大型檔案和 RDC 進行小型變更時。 例如，使用 RDC，對 2 MB PowerPoint®簡報的一項小變更，可能只會在網路上傳送 60 kb （以傳輸的位元組節省 97%）。

RDC 不會用於小於 64 KB 的檔案，而且對於未爭用網路頻寬的高速 Lan 而言，可能不會有任何好處。 您可以使用 DFS 管理，以每個連線為基礎來停用 RDC。

### <a name="how-frequently-does-dfs-replication-replicate-data"></a>DFS 複寫複寫資料的頻率為何？

資料會根據您設定的排程進行複寫。 例如，您可以將排程設定為15分鐘的間隔，一周七天。 在這些間隔期間，會啟用複寫。 在偵測到檔案變更（通常會在幾秒內）之後，複寫就會立即開始。

當連接排程設定為接收成員的本機時間時，複寫群組排程可能會設定為 [通用時間協調（UTC）]。 當複寫群組跨越多個時區時，請將此納入考慮。 本地時程表示裝載輸入連接之成員的時間。 當排程設定為本地時間時，輸入連接和對應的輸出連線所顯示的排程會反映時區差異。

### <a name="how-much-of-my-servers-system-resources-will-dfs-replication-consume"></a>我的伺服器的系統資源 DFS 複寫會耗用多少？

DFS 複寫使用的磁片、記憶體和 CPU 資源取決於數個因素，包括檔案的數目和大小、變更的速率、複寫群組成員的數目，以及複寫的資料夾數目。 此外，某些資源較難預估。 例如，用於 DFS 複寫資料庫的可延伸儲存引擎（ESE）技術可能會耗用大量百分比的可用記憶體，視需要進行釋放。 DFS 複寫以外的應用程式可以裝載在相同的伺服器上，視伺服器設定而定。 不過，在單一伺服器上裝載多個應用程式或伺服器角色時，請務必先測試此設定，然後再于生產環境中執行。

### <a name="what-happens-if-a-wan-link-fails-during-replication"></a>如果 WAN 連結在複寫期間失敗，會發生什麼事？

如果連線中斷，DFS 複寫會在排程開啟時繼續嘗試複寫。 在 DFS 複寫事件記錄檔中，也會注明可使用 MOM 搜集到的連線錯誤（主動通過警示）和 DFS 複寫健康情況報告（例如，當系統管理員執行時）。

## <a name="remote-differential-compression-details"></a>遠端差異壓縮詳細資料

### <a name="are-changes-compressed-before-being-replicated"></a>在複寫之前，會先壓縮變更嗎？

是的。 已變更的檔案部分會在傳送給所有檔案類型之前進行壓縮，但下列除外（已壓縮）：。 wma、.wmv、.zip、.jpg、mpg、mpeg、. .m1v、. .mp2、mp3、mpa、.cab、.wav、operators.snd、澳大利亞、.asf、wm、.avi、. z、. gz、tgz 和 frx 的格式為。 這些檔案類型的壓縮設定無法在 Windows Server 2003 R2 中設定。

### <a name="can-an-administrator-turn-off-rdc-or-change-the-threshold"></a>系統管理員是否可以關閉 RDC 或變更閾值？

是的。 您可以透過指定連接的屬性頁來關閉 RDC。 停用 RDC 可減少沒有頻寬限制的快速區域網路（LAN）連結上的 CPU 使用率和複寫延遲，或包含主要檔案小於 64 KB 的複寫群組。 如果您選擇停用連接上的 RDC，請在變更之前和之後測試複寫效率，以確認您已改善複寫效能。

您可以使用**Dfsradmin Connection Set**命令、DFS 複寫 WMI 提供者，或手動編輯設定 XML 檔案，以變更 RDC 大小閾值。

### <a name="does-rdc-work-on-all-file-types"></a>RDC 適用于所有檔案類型嗎？

是的。 RDC 會計算區塊層級的差異，而不考慮檔案資料類型。 不過，RDC 在某些檔案類型（例如 Word 檔、PST 檔案和 VHD 映射）上的運作效率更高。

### <a name="how-does-rdc-work-on-a-compressed-file"></a>RDC 如何在壓縮檔案上運作？

DFS 複寫使用 RDC，其會計算檔案中已變更的區塊，並只透過網路傳送那些區塊。 DFS 複寫不需要知道檔案內容的任何專案，只會變更哪些區塊。

### <a name="is-cross-file-rdc-enabled-when-upgrading-to-windows-server-enterprise-edition-or-datacenter-edition"></a>升級到 Windows Server Enterprise Edition 或 Datacenter Edition 時是否啟用跨檔案 RDC？

Standard Edition 的 Windows Server 不支援跨檔案 RDC。 不過，當您升級至支援跨檔案 RDC 的版本，或複寫連線的成員正在執行支援的版本時，會自動啟用此功能。 如需支援跨檔案 RDC 的版本清單，請參閱哪些版本的 Windows 作業系統支援跨檔案 RDC？

### <a name="is-rdc-true-block-level-replication"></a>RDC 真正的區塊層級複寫嗎？

資料分割 RDC 是用來壓縮檔案傳輸的一般用途通訊協定。 DFS 複寫在檔案層級的區塊上使用 RDC，而不是在磁片區塊層級。 RDC 會將檔案分割成區塊。 對於檔案中的每個區塊，它會計算簽章，這是可代表較大區塊的少數位節數目。 簽章集合會從伺服器傳送到用戶端。 用戶端會將伺服器簽章與自己的簽章進行比較。 接著，用戶端會要求伺服器只傳送尚未存在於用戶端上的簽章資料。

### <a name="what-happens-if-i-rename-a-file"></a>如果我重新命名檔案，會發生什麼事？

DFS 複寫在下一次複寫期間，會在複寫群組的所有其他成員上重新命名檔案。 檔案是使用唯一識別碼進行追蹤，因此在複本中重新命名檔案並移動檔案，並不會影響 DFS 複寫複寫檔案的能力。

### <a name="what-is-cross-file-rdc"></a>什麼是跨檔案 RDC？

跨檔案 RDC 可讓 DFS 複寫使用 RDC，即使用戶端上不存在具有相同名稱的檔案。 跨檔案 RDC 會使用啟發學習法來判斷類似于需要複寫之檔案的檔案，並使用與複寫檔案相同的類似檔案區塊，將透過 WAN 傳送的資料量降到最低。 跨檔案 RDC 可以在此進程中使用最多五個類似檔案的區塊。

若要使用跨檔案 RDC，複寫連接的其中一個成員必須執行支援跨檔案 RDC 的 Windows 版本。 如需支援跨檔案 RDC 的版本清單，請參閱哪些版本的 Windows 作業系統支援跨檔案 RDC？

### <a name="what-is-rdc"></a>什麼是 RDC？

遠端差異壓縮（RDC）是一種用戶端伺服器通訊協定，可用來透過有限頻寬的網路有效率地更新檔案。 RDC 會偵測檔案中的資料插入、移除和重排所以，讓 DFS 複寫只能在檔案更新時複寫變更。 RDC 僅用於預設為 64 KB 或更大的檔案。 RDC 可以使用複寫資料夾或 DfsrPrivate\\ConflictandDeleted 資料夾（位於複寫資料夾的本機路徑底下）中相同名稱的舊版本檔案。

### <a name="when-is-rdc-used-for-replication"></a>RDC 何時用於複寫？

當檔案超過最小大小閾值時，會使用 RDC。 此大小閾值預設為 64 KB。 複寫超過該臨界值的檔案之後，檔案的更新版本一律會使用 RDC，除非檔案的大型部分已變更或 RDC 已停用。

### <a name="which-editions-of-the-windows-operating-system-support-cross-file-rdc"></a>哪些版本的 Windows 作業系統支援跨檔案 RDC？

若要使用跨檔案 RDC，複寫連接的其中一個成員必須執行支援跨檔案 RDC 的 Windows 作業系統版本。 下表顯示哪些版本的 Windows 作業系統支援跨檔案 RDC。

### <a name="cross-file-rdc-availability-in-editions-of-the-windows-operating-system"></a>Windows 作業系統版本中的跨檔案 RDC 可用性

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
<td><p>是<em></p></td>
<td><p>沒有</p></td>
<td><p>是</em></p></td>
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

\*您可以選擇性地停用 Windows Server 2012 R2 上的跨檔案 RDC。

## <a name="replication-details"></a>複寫詳細資料

### <a name="can-i-change-the-path-for-a-replicated-folder-after-it-is-created"></a>我可以在建立複寫資料夾之後變更其路徑嗎？

資料分割 如果您需要變更複寫資料夾的路徑，您必須在 [DFS 管理] 中將它刪除，然後將它新增回做為新的複寫資料夾。 DFS 複寫接著會使用遠端差異壓縮（RDC）來執行同步處理，以判斷傳送和接收成員上的資料是否相同。 它不會再次複寫資料夾中的所有資料。

### <a name="can-i-configure-which-file-attributes-are-replicated"></a>我可以設定要複寫哪些檔案屬性？

否，您無法設定 DFS 複寫複寫的檔案屬性。

如需屬性值及其描述的清單，請參閱 MSDN 上的[檔案屬性](http://go.microsoft.com/fwlink/?linkid=182268)（ http://go.microsoft.com/fwlink/?LinkId=182268) 。

下列屬性值是使用`SetFileAttributes dwFileAttributes`函式所設定，並由 DFS 複寫進行複寫。 這些屬性值的變更會觸發屬性的複寫。 除非內容也變更，否則不會複寫檔案的內容。 如需詳細資訊，請參閱 MSDN library 中的[SetFileAttributes 函數](http://go.microsoft.com/fwlink/?linkid=182269)（ http://go.microsoft.com/fwlink/?LinkId=182269) 。

  - 隱藏\_的\_檔案屬性  
      
  - 檔\_屬性\_READONLY  
      
  - 檔\_屬性\_系統  
      
  - 檔\_屬性\_未編制\_索引內容\_  
      
  - 檔\_屬性\_離線  
      

下列屬性值會由 DFS 複寫進行複寫，但不會觸發複寫。

  - 檔\_屬性\_封存  
      
  - 檔\_屬性\_正常  
      

下列檔案屬性值也會觸發複寫，雖然無法使用`SetFileAttributes`函式來設定（ `GetFileAttributes`使用函數來查看屬性值）。

  - 檔\_屬性\_重新分析\_點  
      

> [!NOTE]
> 除非 IO_REPARSE_TAG_SYMLINK 重新分析標記，否則 DFS 複寫不會複寫重新分析點屬性值。 具有 IO_REPARSE_TAG_DEDUP、IO_REPARSE_TAG_SIS 或 IO_REPARSE_TAG_HSM 重新分析標記的檔案會複寫為一般檔案。 不過，重新分析標記和重新分析資料緩衝區不會複寫到其他伺服器，因為重新分析點只適用于本機系統。 
<br>

  - 檔\_屬性\_已壓縮  
      
  - 已\_加密\_檔案屬性  
      

> [!NOTE]
> DFS 複寫不會複寫使用加密檔案系統（EFS）加密的檔案。 DFS 複寫會複寫使用非 Microsoft 軟體加密的檔案，但只有在未設定檔案的 FILE_ATTRIBUTE_ENCRYPTED 屬性值時，才會進行複寫。 
<br>

  - 檔\_屬性\_SPARSE檔案\_  
      
  - 檔\_屬性\_目錄  
      

DFS 複寫不會複寫檔\_屬性\_暫存值。

### <a name="can-i-control-which-member-is-replicated"></a>我可以控制要複寫的成員嗎？

是的。 當您建立複寫群組時，可以選擇拓撲。 或者，您也可以選取 [**無拓撲**]，並在建立複寫群組之後手動設定連線。

### <a name="can-i-seed-a-replication-group-member-with-data-prior-to-the-initial-replication"></a>我可以在初始複寫之前，使用資料來植入複寫群組成員嗎？

是的。 DFS 複寫支援在初始複寫之前將檔案複製到複寫群組成員。 這項「預先設置」可以大幅減少初始複寫期間複寫的資料量。

當檔案只有實際的屬性或時間戳記不同時，初始複寫不需要複寫內容。 Real 屬性是可由 Win32 函數`SetFileAttributes`設定的屬性。 如需詳細資訊，請參閱 MSDN library 中的[SetFileAttributes 函數](http://go.microsoft.com/fwlink/?linkid=182269)（ http://go.microsoft.com/fwlink/?LinkId=182269) 。 如果兩個檔案與其他屬性（例如壓縮）不同，則會複寫檔案的內容。

若要預先設置複寫群組成員，請將檔案複製到目的地伺服器上的適當資料夾、建立複寫群組，然後選擇主要成員。 選擇您想要複寫的最新檔案成員，因為主要成員的內容會被視為「授權」。 這表示在初始複寫期間，主要成員的檔案一律會覆寫複寫群組其他成員上的其他檔案版本。

如需有關預先植入和複製 DFSR 資料庫的詳細資訊[，請參閱 DFS 複寫 Windows Server 2012 R2 中的初始同步處理：複製](http://blogs.technet.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx)的攻擊。

如需初始複寫的詳細資訊，請參閱[建立複寫群組](https://technet.microsoft.com/library/cc725893)。

### <a name="does-dfs-replication-overcome-common-file-replication-service-issues"></a>DFS 複寫克服常見的檔案複寫服務問題嗎？

是的。 DFS 複寫克服了三個常見的 FRS 問題：

  - 日誌包裝：DFS 複寫從日誌即時復原。 在再次啟用複寫之前，每個現有的檔案或資料夾都會標示為 journalWrap，並針對檔案系統進行驗證。 在復原期間，此磁片區無法在任一方向進行複寫。  
      
  - 複寫過多：若要避免過多複寫，DFS 複寫使用點數的系統。  
      
  - [仿] 資料夾：若要避免將資料夾名稱設為 DfsrPrivate\\，DFS 複寫將衝突的資料儲存在隱藏的 ConflictandDeleted 資料夾中（位於複寫資料夾的本機路徑底下）。 例如，在使用 FRS 複寫的不同伺服器上，同時使用相同的名稱來建立多個資料夾，會導致 FRS 重新命名舊的資料夾。 DFS 複寫改為將較舊的資料夾移至 [本機衝突] 和 [已刪除] 資料夾。  
      

### <a name="does-dfs-replication-replicate-files-in-chronological-order"></a>DFS 複寫依時間順序複寫檔案嗎？

資料分割 檔案可能會依序複寫。

### <a name="does-dfs-replication-replicate-files-that-are-being-used-by-another-application"></a>DFS 複寫是否會複寫另一個應用程式正在使用的檔案？

如果應用程式開啟檔案並在其上建立檔案鎖定（防止其他應用程式在開啟時使用），DFS 複寫在關閉檔案之前，不會複寫該檔案。 如果應用程式開啟具有讀取共用存取權的檔案，仍然可以複寫該檔案。

### <a name="does-dfs-replication-replicate-ntfs-file-permissions-alternate-data-streams-hard-links-and-reparse-points"></a>DFS 複寫會複寫 NTFS 檔案許可權、替代資料流程、永久連結和重新分析點嗎？

  - DFS 複寫複寫 NTFS 檔案許可權和替代的資料流程。  
      
  - Microsoft 不支援對複寫資料夾中的檔案建立 NTFS 永久連結，這麼做可能會造成受影響檔案的複寫問題。 DFS 複寫會忽略永久連結檔案，而且不會複寫。 連接點也不會複寫，而且 DFS 複寫記錄每個遇到之連接點的事件4406。  
      
  - DFS 複寫所複寫的唯一重新分析點，就是使用 IO\_重新\_分析\_標記符號的標記; 不過，DFS 複寫不保證也會複寫符號的目標。 如需詳細資訊，請參閱[詢問目錄服務小組的 blog](http://blogs.technet.com/b/askds/archive/2011/09/30/friday-mail-sack-super-slo-mo-edition.aspx)。  
      
  - 已複寫具有 IO\_重新\_ \_分析標籤重複\_資料\_刪除\_、io 重新分析\_標籤\_SIS 或 io 重新分析\_標籤 HSM 重新分析標記的檔案做為一般檔案。 重新分析標記和重新分析資料緩衝區不會複寫到其他伺服器，因為重新分析點只適用于本機系統。 因此，DFS 複寫可以在 Windows Server 2012 或儲存單一版本（SIS）中使用重復資料刪除的磁片區上複寫資料夾，不過，重復資料刪除資訊是由啟用角色服務的每部伺服器分別維護。  
      

### <a name="does-dfs-replication-replicate-timestamp-changes-if-no-other-changes-are-made-to-the-file"></a>如果沒有對檔案進行任何其他變更，DFS 複寫複寫時間戳記變更嗎？

否，DFS 複寫不會複寫只有時間戳記變更的檔案。 此外，變更的時間戳記不會複寫到複寫群組的其他成員，除非對檔案進行其他變更。

### <a name="does-dfs-replication-replicate-updated-permissions-on-a-file-or-folder"></a>DFS 複寫是否會在檔案或資料夾上複寫更新的許可權？

是的。 DFS 複寫複製檔案和資料夾的許可權變更。 只有與存取控制清單（ACL）相關聯之檔案的部分會進行複寫，不過 DFS 複寫仍然必須將整個檔案讀取到臨時區域中。


> [!NOTE]
> 變更大量檔案的 Acl 可能會影響複寫效能。 不過，使用 RDC 時，傳輸的資料量會與 Acl 的大小成正比，而不是整個檔案的大小。 磁片流量的數量仍然與檔案大小成正比，因為檔案必須在執行資料夾中讀取。 
<br>


### <a name="does-dfs-replication-support-merging-text-files-in-the-event-of-a-conflict"></a>當發生衝突時，DFS 複寫是否支援合併文字檔？

當發生衝突時，DFS 複寫不會合並檔案。 不過，它會嘗試在偵測到衝突的電腦上，于隱藏的 DfsrPrivate\\ConflictandDeleted 資料夾中保留較舊版本的檔案。

### <a name="does-dfs-replication-use-encryption-when-transmitting-data"></a>傳輸資料時 DFS 複寫使用加密嗎？

是的。 DFS 複寫搭配加密使用遠端程序呼叫（RPC）連線。

### <a name="is-it-possible-to-disable-the-use-of-encrypted-rpc"></a>是否可以停用加密的 RPC？

資料分割 DFS 複寫服務會透過 TCP 使用遠端程序呼叫（RPC）來複寫資料。 為了保護跨網際網路的資料傳輸，DFS 複寫服務設計為一律使用驗證層級常數`RPC_C_AUTHN_LEVEL_PKT_PRIVACY`。 這可確保網際網路上的 RPC 通訊一律會加密。 因此，DFS 複寫服務無法停用加密的 RPC。

如需詳細資訊，請參閱下列 Microsoft 網站：

  - [RPC 技術參考](http://go.microsoft.com/fwlink/?linkid=182278)  
      
  - [關於遠端差異壓縮](http://go.microsoft.com/fwlink/?linkid=182279)  
      
  - [驗證層級常數](http://go.microsoft.com/fwlink/?linkid=182280)  
      

### <a name="how-are-simultaneous-replications-handled"></a>如何處理同步複寫？

每個複寫資料夾都有一個更新管理員。 更新管理員彼此獨立工作。

根據預設，最多16個（Windows Server 2003 R2 中的四個）並行下載會在所有連線和複寫群組之間共用。 因為連接和複寫群組更新未序列化，所以沒有收到更新的特定順序。 如果開啟兩個排程，通常會從兩個連接同時接收並安裝更新。

### <a name="how-do-i-force-replication-or-polling"></a>如何? 強制複寫或輪詢？

您可以使用 DFS 管理來立即強制複寫，如[編輯](https://technet.microsoft.com/library/Cc732278)複寫排程中所述。 您也可以使用`Sync-DfsReplicationGroup` Cmdlet （隨附于 Windows Server 2012 R2 所引進的 DFSR PowerShell 模組）或**Dfsrdiag SyncNow**命令來強制複寫。 您可以使用`Update-DfsrConfigurationFromAD` Cmdlet 或**Dfsrdiag PollAD**命令來強制輪詢。

### <a name="is-it-possible-to-configure-a-quiet-time-between-replications-for-files-that-change-frequently"></a>是否可以為經常變更的檔案設定複寫之間的無訊息時間？

資料分割 如果排程已開啟，DFS 複寫會在它注意到變更時進行複寫。 沒有任何方法可以設定檔案的無訊息時間。

### <a name="is-it-possible-to-configure-one-way-replication-with-dfs-replication"></a>是否可以使用 DFS 複寫設定單向複寫？

是的。 如果您使用 Windows Server 2012 或 Windows Server 2008 R2，您可以建立唯讀複寫資料夾，透過單向連線來複寫內容。 如需詳細資訊，請參閱將[特定成員的複寫資料夾設為唯讀](http://go.microsoft.com/fwlink/?linkid=156740)（ http://go.microsoft.com/fwlink/?LinkId=156740) 。

我們不支援使用 Windows Server 2008 或 Windows Server 2003 R2 中的 DFS 複寫來建立單向複寫連線。 這麼做可能會造成許多問題，包括健康情況檢查拓撲錯誤、臨時問題，以及 DFS 複寫資料庫的問題。

如果您使用 Windows Server 2008 或 Windows Server 2003 R2，您可以執行下列動作來模擬單向連線：

  - 將系統管理員定型，只在您想要指定為主伺服器的伺服器上進行變更。 然後讓變更複寫到目的地伺服器。  
      
  - 在目的地伺服器上設定共用許可權，讓終端使用者不具有寫入權限。 如果不允許在分支伺服器上進行任何變更，則不會複寫回任何內容，模擬單向連線並讓 WAN 使用率降低。  
      

### <a name="is-there-a-way-to-force-a-complete-replication-of-all-files-including-unchanged-files"></a>是否有方法可以強制完成所有檔案的完整複寫，包括未變更的檔案？

資料分割 如果 DFS 複寫將檔案視為相同，就不會複寫它們。 如果尚未複寫變更的檔案，DFS 複寫會在設定時自動複寫檔案。 若要覆寫已設定的排程，請使用 WMI 方法**ForceReplicate （）** 。 不過，這只是排程覆寫，而且不會強制複寫未變更或相同的檔案。

### <a name="what-happens-if-the-primary-member-suffers-a-database-loss-during-initial-replication"></a>如果主要成員在初始複寫期間造成資料庫遺失，會發生什麼事？

在初始複寫期間，如果接收成員在主要成員上有不同版本的檔案，主要成員的檔案一律會優先于衝突解決。 主要成員指定會儲存在 Active Directory Domain Services 中，而指定會在主要成員準備好複寫後，但在複寫群組的所有成員複寫之前清除。

如果初始複寫失敗或 DFS 複寫服務在複寫期間重新開機，則主要成員會看到本機 DFS 複寫資料庫中的主要成員指定，並重試初始複寫。 如果在 Active Directory Domain Services 中清除主要指定之後，主要成員的 DFS 複寫資料庫遺失，但是在複寫群組的所有成員完成初始複寫之前，複寫群組的所有成員都將無法複寫資料夾，因為未指定任何伺服器做為主要成員。 如果發生這種情況，請在主要成員伺服器上使用**Dfsradmin 成員/set/isprimary： true**命令，以手動方式還原主要成員指定。

如需初始複寫的詳細資訊，請參閱[建立複寫群組](https://technet.microsoft.com/library/cc725893)。


> [!WARNING]
> 主要成員指定只會在初始複寫過程中使用。 當複寫完成之後，如果您使用<STRONG>Dfsradmin</STRONG>命令來指定複寫資料夾的主要成員，DFS 複寫不會將伺服器指定為 Active Directory Domain Services 中的主要成員。 不過，如果伺服器上的 DFS 複寫資料庫導致無法復原的損毀或資料遺失，伺服器就會嘗試執行初始複寫做為主要成員，而不是從複寫的另一個成員復原其資料。小組. 基本上，伺服器會變成 rogue 主伺服器，這可能會造成衝突。 基於這個理由，只有在您確定初始複寫已無可挽回嚴重失敗時，才手動指定主要成員。 
<br>


### <a name="what-happens-if-the-replication-schedule-closes-while-a-file-is-being-replicated"></a>如果複寫排程在複寫檔案時關閉，會發生什麼事？

如果已在連線上啟用遠端差異壓縮（RDC），則在排程關閉之前立即開始複寫的檔案（或變更為「**無頻寬**」）的輸入複寫會繼續64執行（或**不是頻寬**的變更）。 複寫停止時，複寫會從其所在的狀態繼續。

如果 RDC 已關閉，DFS 複寫會完全重新開機檔案傳輸。 當接收成員上有檔案可供使用時，這可能會延遲。

### <a name="what-happens-when-two-users-simultaneously-update-the-same-file-on-different-servers"></a>當兩個使用者同時更新不同伺服器上的相同檔案時，會發生什麼事？

當 DFS 複寫偵測到衝突時，它會使用最後儲存的檔案版本。 它會將另一個檔案移到\\DfsrPrivate ConflictandDeleted 資料夾中（在解決衝突之電腦上複寫資料夾的本機路徑底下）。 它會一直留在 [衝突] 和 [已刪除] 資料夾清除中，這會在 [因衝突而刪除] 資料夾超過設定的大小，或 DFS 複寫遇到磁碟空間不足的錯誤時發生。 [衝突] 和 [已刪除] 資料夾不會複寫，而且這種衝突解決方法可避免在 FRS 中可能發生的仿型目錄問題。

發生衝突時，DFS 複寫會將參考事件記錄到 DFS 複寫的事件記錄檔。 基於下列原因，此事件不需要使用者動作：

  - 使用者看不到它（只有伺服器系統管理員可以看見）。  
      
  - DFS 複寫會將 [衝突] 和 [已刪除] 資料夾視為快取。 達到配額臨界值時，會清除其中一些檔案。 不保證會儲存衝突的檔案。  
      
  - 衝突可能位於與衝突來源不同的伺服器上。  
      

## <a name="staging"></a>預備環境

### <a name="does-dfs-replication-continue-staging-files-when-replication-is-disabled-by-a-schedule-or-bandwidth-throttling-quota-or-when-a-connection-is-manually-disabled"></a>當複寫已由排程或頻寬節流配額停用時，或手動停用連接時，DFS 複寫繼續暫存檔案？

資料分割 DFS 複寫不會繼續在排程的複寫時間外暫存檔案，如果已超過頻寬節流配額，或已停用連接。

### <a name="does-dfs-replication-prevent-other-applications-from-accessing-a-file-during-staging"></a>DFS 複寫是否會在暫存期間防止其他應用程式存取檔案？

資料分割 DFS 複寫以不會封鎖使用者或應用程式開啟 [複寫] 資料夾中檔案的方式來開啟檔案。 這個方法稱為「有機會鎖定」。

### <a name="is-it-possible-to-change-the-location-of-the-staging-folder-with-the-dfs-management-tool"></a>是否可以使用 DFS 管理工具來變更執行資料夾的位置？

是的。 在複寫群組的每個**成員的 [內容] 對話方塊的**[ **Advanced** ] 索引標籤上，設定了預備資料夾位置。

### <a name="when-are-files-staged"></a>檔案何時暫存？

當接收成員要求檔案（除非檔案是 64 KB 或更小的檔案）時，檔案就會在傳送成員上暫存，如下表所示。 如果已在連線上停用遠端差異壓縮（RDC），則會暫存該檔案，除非它是 256 KB 或更小。 如果檔案的大小小於 64 KB，檔案也會在接收成員上暫存，雖然您可以在 16 KB 到 1 MB 之間設定這項設定。 如果排程已關閉，則不會暫存檔案。

### <a name="the-minimum-file-sizes-for-staging-files"></a>暫存檔案的檔案大小下限

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th> </th>
<th>已啟用 RDC</th>
<th>已停用 RDC</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>正在傳送成員</p></td>
<td><p>64 KB</p></td>
<td><p>256 KB</p></td>
</tr>
<tr class="odd">
<td><p>接收成員</p></td>
<td><p>預設為 64 KB</p></td>
<td><p>預設為 64 KB</p></td>
</tr>
</tbody>
</table>

### <a name="what-happens-if-a-file-is-changed-after-it-is-staged-but-before-it-is-completely-transmitted-to-the-remote-site"></a>如果檔案在暫存後，但在完全傳輸到遠端網站之前變更，會發生什麼事？

如果已傳輸檔案的任何部分，DFS 複寫會繼續傳輸。 如果在 DFS 複寫開始傳輸檔案之前，檔案已變更，則會傳送較新版本的檔案。

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
<td><p>已更新 Windows Server 2019。</p></td>
<td><p>新的作業系統。</p></td>
</tr>
<tr class="even">
<td><p>2013 年 10 月 9 日</p></td>
<td><p>已更新 DFS 複寫支援的限制為何？包含來自 Windows Server 2012 R2 測試之結果的區段。</p></td>
<td><p>最新版 Windows Server 的更新</p></td>
</tr>
<tr class="odd">
<td><p>2013年1月30日</p></td>
<td><p>已新增 DFS 複寫在排程或頻寬節流配額停用複寫時繼續暫存檔案，或在手動停用連接時繼續執行檔案？上場.</p></td>
<td><p>客戶問題</p></td>
</tr>
<tr class="even">
<td><p>2012年10月31日</p></td>
<td><p>已編輯 DFS 複寫支援的限制為何？專案，以增加磁片區上已測試的已複寫檔案數目。</p></td>
<td><p>客戶回函</p></td>
</tr>
<tr class="odd">
<td><p>2012年8月15日</p></td>
<td><p>已編輯 DFS 複寫複寫 NTFS 檔案許可權、替代資料流程、永久連結和重新分析點嗎？進一步瞭解 DFS 複寫如何處理永久連結和重新分析點的專案。</p></td>
<td><p>客戶支援服務的意見反應</p></td>
</tr>
<tr class="even">
<td><p>2012年6月13日</p></td>
<td><p>編輯 DFS 複寫在 ReFS 或 FAT 磁片區上運作嗎？加入 ReFS 討論的專案。</p></td>
<td><p>客戶回函</p></td>
</tr>
<tr class="odd">
<td><p>2012年4月25日</p></td>
<td><p>已編輯 DFS 複寫複寫 NTFS 檔案許可權、替代資料流程、永久連結和重新分析點嗎？用來闡明 DFS 複寫如何處理永久連結的專案。</p></td>
<td><p>減少可能的混淆</p></td>
</tr>
<tr class="even">
<td><p>2011年3月30日</p></td>
<td><p>編輯可以 DFS 複寫複寫 Outlook .pst 還是 Microsoft Office Access 資料庫檔案？以更正將 DFS 複寫與 .pst 和 Access 檔案搭配使用的潛在影響專案。</p>
<p>新增了如何改善複寫效能？</p></td>
<td><p>關於前一個專案的客戶問題，錯誤地指出複寫 .pst 或 Access 檔案可能損毀 DFS 複寫資料庫。</p></td>
</tr>
<tr class="odd">
<td><p>2011年1月26日</p></td>
<td><p>新增了如何從 ConflictAndDeleted 或預先存在的資料夾復原檔案？</p></td>
<td><p>客戶回函</p></td>
</tr>
<tr class="even">
<td><p>2010年10月20日</p></td>
<td><p>已新增如何升級或取代 DFS 複寫成員？</p></td>
<td><p>客戶回函</p></td>
</tr>
</tbody>
</table>

