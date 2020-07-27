---
title: DFS 複寫 - 常見問題集 (FAQ)
ms.date: 06/18/2014
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 4c4d5310fa6cf47945483c9ee7a3f89afd313da9
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966130"
---
# <a name="dfs-replication-frequently-asked-questions-faq"></a>DFS 複寫：常見問題集 (FAQ)


更新日期：2019 年 4 月 30 日

適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

此常見問題會回答適用於 Windows Server 的分散式檔案系統 (DFS) 複寫 (也稱為 DFS-R 或 DFSR) 的相關問題。

如需 DFS 命名空間的相關資訊，請參閱 [DFS 命名空間：常見問題集](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee404780(v=ws.10))。

如需 DFS 複寫新功能的相關資訊，請參閱下列主題：

  - [DFS 命名空間和 DFS 複寫概觀](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v=ws.11)) (Windows Server 2012)  
      
  - [從 Windows Server 2008 到 Windows Server 2008 R2 的功能變更](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd391932(v=ws.10))中的[分散式檔案系統新功能](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee307957(v=ws.10))主題  
      
  - [從 Windows Server 2003 with SP1 到 Windows Server 2008 的功能變更](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753208(v=ws.10))中的[分散式檔案系統](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753479(v=ws.10))主題  
      

如需近期對本主題所做變更的清單，請參閱本主題的＜ [變更歷程記錄](#change-history) ＞一節。

      

## <a name="interoperability"></a>互通性

### <a name="can-dfs-replication-communicate-with-frs"></a>DFS 複寫可以與 FRS 通訊嗎？

不可以。 DFS 複寫不會與檔案複寫服務 (FRS) 通訊。 DFS 複寫和 FRS 可以同時在同一部伺服器上執行，但絕不能將它們設定為複寫相同的資料夾或子資料夾，因為這樣做可能會造成資料遺失。

### <a name="can-dfs-replication-replace-frs-for-sysvol-replication"></a>DFS 複寫可以取代適用於 SYSVOL 的 FRS 複寫嗎

可以，在執行 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 的伺服器上，DFS 複寫可以取代適用於 SYSVOL 的 FRS 複寫。 執行 Windows Server 2003 R2 的伺服器不支援使用 DFS 複寫來複寫 SYSVOL 資料夾。

如需使用 DFS 複寫將 SYSVOL 複寫的詳細資訊，請參閱 [SYSVOL 複寫移轉指南：從 FRS 到 DFS 複寫](./migrate-sysvol-to-dfsr.md)。

### <a name="can-i-upgrade-from-frs-to-dfs-replication-without-losing-configuration-settings"></a>我可以從 FRS 升級至 DFS 複寫，而不會遺失組態設定嗎？

是。 若要將複寫從 FRS 遷移至 DFS 複寫，請參閱下列文件：

  - 若要遷移 SYSVOL 資料夾以外的資料夾複寫，請參閱 [DFS 操作指南：從 FRS 遷移至 DFS 複寫](https://go.microsoft.com/fwlink/?linkid=192776)和 [FRS2DFSR – FRS 到 DFSR 移轉公用程式](https://go.microsoft.com/fwlink/?linkid=195437) (https://go.microsoft.com/fwlink/?LinkID=195437) 。  
      
  - 若要將 SYSVOL 資料夾的複寫遷移到 DFS 複寫，請參閱 [SYSVOL 複寫移轉指南：從 FRS 到 DFS 複寫](./migrate-sysvol-to-dfsr.md)。  
      

### <a name="can-i-use-dfs-replication-in-a-mixed-windowsunix-environment"></a>我可以在混合式 Windows/UNIX 環境中使用 DFS 複寫嗎？

是。 雖然 DFS 複寫僅支援在執行 Windows Server 的伺服器之間複寫內容，但 UNIX 用戶端可以存取 Windows 伺服器上的檔案共用。 若要這麼做，請在 DFS 複寫伺服器上安裝適用於網路檔案系統 (NFS) 的服務。

您也可以使用許多 UNIX 用戶端中內含的 SMB/CIFS 用戶端功能，直接存取 Windows 檔案共用；雖然這項功能經常受限或需要根據 Windows 環境修改 (例如使用群組原則以停用 SMB 簽署)。

DFS 複寫會在執行 Windows Server 作業系統的伺服器上與 NFS 交互操作，但您無法複寫 NFS 掛接點。

### <a name="can-i-use-the-volume-shadow-copy-service-with-dfs-replication"></a>我可以搭配 DFS 複寫使用磁碟區陰影複製服務嗎？

是。 磁碟區陰影複製服務 (VSS) 磁碟區支援 DFS 複寫，而且可搭配舊版用戶端順利還原先前的快照集。

### <a name="can-i-use-windowsbackup-ntbackupexe-to-remotely-back-up-a-replicated-folder"></a>我可以使用 Windows 備份 (Ntbackup.exe) 從遠端備份複寫資料夾嗎？

不可以，系統不支援在執行 windows Server 2003 或更早版本的電腦上，使用 Windows 備份 (Ntbackup.exe) 備份執行 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008 的電腦上複寫資料夾的內容。

若要備份儲存在複寫資料夾中的檔案，請使用 Windows Server Backup 或 Microsoft&reg; System Center Data Protection Manager。 如需 Windows Server 2008 R2 及 Windows Server 2008 中備份與復原功能的相關資訊，請參閱[備份和復原](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754097(v=ws.10))＞。 如需詳細資訊，請參閱 [System Center Data Protection Manager](https://go.microsoft.com/fwlink/?linkid=182261) (https://go.microsoft.com/fwlink/?LinkId=182261) 。

### <a name="do-file-system-policies-impact-dfs-replication"></a>檔案系統原則是否會影響 DFS 複寫？

是。 請勿在複寫資料夾上設定檔案系統原則。 檔案系統原則會在每個群組原則重新整理間隔重新套用 NTFS 權限。 這可能會導致共用違規，因為在檔案關閉之前，不會複寫已開啟的檔案。

### <a name="does-dfs-replication-replicate-mailboxes-hosted-on-microsoft-exchange-server"></a>DFS 複寫可以複寫裝載在 Microsoft Exchange Server 上的信箱嗎？

不可以。 DFS 複寫不能用來複寫裝載在 Microsoft Exchange Server 上的信箱。

### <a name="does-dfs-replication-support-file-screens-created-by-file-server-resource-manager"></a>DFS 複寫是否支援由檔案伺服器資源管理員所建立的檔案檢測？

是。 但是，檔案伺服器資源管理員 (FSRM) 檔案檢測設定必須符合複寫的兩個方面。 此外，DFS 複寫有自己的檔案和資料夾篩選機制，可讓您從複寫中排除特定檔案和檔案類型。

以下是用來實作檔案檢測或配額的最佳做法：

  - 隱藏的 DfsrPrivate 資料夾不得受限於配額或檔案檢測。  
      
  - 在啟用檢測之前，所有已複寫的資料夾中不得包含檢測過的檔案。  
      
  - 在啟用配額之前，資料夾均不會超過配額。  
      
  - 請務必謹慎使用固定配額。 在複寫之前，複寫群組的個別成員可能會維持在配額內，但在複寫檔案卻會超出配額。 例如，如果使用者將 10 百萬位元組 (MB) 的檔案複製到伺服器 A 中 (當時具有固定限制)，而另一位使用者將 5 MB 的檔案複製到伺服器 B 中；那麼在下次複寫時，這兩部伺服器都會超過 5 MB 的配額。 這可能會導致 DFS 複寫持續重試複寫檔案，而造成版本向量中的漏洞，以及可能出現的效能問題。  
      

### <a name="is-dfs-replication-cluster-aware"></a>DFS 複寫叢集能夠感知嗎？

可以，Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2008 R2 中的 DFS 複寫納入了將容錯移轉叢集新增為複寫群組成員的功能。 如需詳細資訊，請參閱[將容錯移轉叢集新增到複寫群組](https://go.microsoft.com/fwlink/?linkid=155085) (https://go.microsoft.com/fwlink/?LinkId=155085) 。 Windows Server 2008 R2 之前的 Windows 版本上的 DFS 複寫服務並非設計來與容錯移轉叢集協調，而且服務也不會容錯移轉到另一個節點。


> [!NOTE]
> DFS 複寫不支援複寫在叢集共用磁碟區的檔案。 
<br>


### <a name="is-dfs-replication-compatible-with-data-deduplication"></a>DFS 複寫與重複資料刪除相容嗎？

是的，DFS 複寫可以複寫在 Windows Server 中使用重複資料刪除的磁碟區資料夾。

### <a name="is-dfs-replication-compatible-with-ris-and-wds"></a>DFS 複寫與 RIS 和 WDS 相容嗎？

是。 DFS 複寫會複寫已啟用單一執行個體儲存體 (SIS) 的磁碟區。 SIS 是由遠端安裝服務 (RIS)、Windows 部署服務 (WDS) 和 Windows Storage Server 所使用。

### <a name="is-it-possible-to-use-dfs-replication-with-offline-files"></a>DFS 複寫是否可以搭配離線檔案使用？

如果一次只有一個使用者寫入檔案，您就可以安全地使用 DFS 複寫和離線檔案。 這項功能對於在兩個分公司之間移動，而且想要能夠在任一分公司或離線存取其檔案的使用者來說非常實用。 離線檔案會在本機快取檔案以供離線使用，且 DFS 複寫會在每個分公司之間複寫資料。

請勿在多使用者環境中搭配離線檔案使用 DFS 複寫，因為 DFS 複寫不提供任何分散式鎖定機制或檔案簽出功能。 如果有兩位使用者在不同的伺服器上同時修改相同的檔案，DFS 複寫會在下一次複寫期間，將較舊的檔案移到 DfsrPrivate\\ConflictandDeleted 資料夾 (位於複寫資料夾的本機路徑下) 中。

### <a name="what-antivirus-applications-are-compatible-with-dfs-replication"></a>哪些防毒軟體應用程式與 DFS 複寫相容？

如果防毒軟體應用程式的掃描活動改變了複寫資料夾中的檔案，則可能會造成過多的複寫。 如需詳細資訊，請[測試防毒應用程式與 DFS 複寫的互通性](https://go.microsoft.com/fwlink/?linkid=73990) (https://go.microsoft.com/fwlink/?LinkId=73990) 。

### <a name="what-are-the-benefits-of-using-dfs-replication-instead-of-windows-sharepoint-services"></a>使用 DFS 複寫而不是 Windows SharePoint Services 有哪些優點？

有別於 DFS 複寫，Windows&reg; SharePoint&reg; Services 以檔案簽出功能的形式提供緊密的一致性。 如果您擔心多人編輯相同的檔案，建議使用 Windows SharePoint Services。 Windows Server 2003 R2 中會隨附 Windows SharePoint Services 2.0 (含 Service Pack 2)。 您可以從 Microsoft 網站下載 Windows SharePoint Services；較新版本的 Windows 伺服器中並未包含 Windows SharePoint Services。 不過，如果您要跨多個網站複寫資料，而且使用者不會同時編輯相同的檔案，DFS 複寫能提供更大的頻寬和更簡單的管理。

## <a name="limitations-and-requirements"></a>限制和需求

### <a name="can-dfs-replication-replicate-between-branch-offices-without-a-vpn-connection"></a>DFS 複寫是否不需要 VPN 連線，就可以在分公司之間進行複寫？

是的；前提是有一個私人廣域網路 (WAN) 連結 (不是網際網路) 與各個分公司連線。 不過，您必須在外部防火牆中開啟適當的連接埠。 DFS 複寫使用 RPC 端點對應程式 (連接埠 135) 和隨機指派的暫時連接埠 (1024 以上)。 您可以使用 **Dfsrdiag** 命令列工具指定靜態連接埠而非暫時連接埠。 如需如何指定 RPC 端點的詳細資訊，請參閱 Microsoft 知識庫中的[文章 154596](https://go.microsoft.com/fwlink/?linkid=73991) (https://go.microsoft.com/fwlink/?LinkId=73991) 。

### <a name="can-dfs-replication-replicate-files-encrypted-with-the-encrypting-file-system"></a>DFS 複寫可以複寫由加密檔案系統加密的檔案嗎？

不可以。 DFS 複寫不會複寫使用加密檔案系統 (EFS) 加密的檔案或資料夾。 如果使用者加密先前已複寫的檔案，DFS 複寫會將檔案從複寫群組的所有其他成員中刪除。 如此可確保伺服器上的加密版本是唯一可用的檔案複本。

### <a name="can-dfs-replication-replicate-outlook-pst-or-microsoft-office-access-database-files"></a>DFS 複寫可以複寫 Outlook .pst 或 Microsoft Office Access 資料庫檔案嗎？

DFS 複寫可以安全地複寫僅儲存供封存之用的 Microsoft Outlook 個人資料夾 (.pst) 和 Microsoft Access 檔案，而且不能透過使用 Outlook 或 Access 等用戶端以跨網路方式存取 (若要開啟 .pst 或 Access 檔案，請先將檔案複製到本機存放區裝置)。 原因如下：

  - 透過網路連線開啟 .pst 檔案可能會導致 .pst 檔案中的資料損毀。 如需有關為何無法安全地從網路存取 .pst 檔案的詳細資訊，請參閱 Microsoft 知識庫中的 [文章 297019](https://go.microsoft.com/fwlink/?linkid=125363) (https://go.microsoft.com/fwlink/?LinkId=125363) 。  
      
  - .pst 和 Access 檔案通常會在 Outlook 或 Office Access 等用戶端存取時保持開啟一段很長的時間。 這可防止 DFS 複寫在檔案關閉之前複寫這些檔案。  
      

### <a name="can-i-use-dfs-replication-in-a-workgroup"></a>我可以在工作組中使用 DFS 複寫嗎？

不可以。 DFS 複寫依賴 Active Directory&reg; Domain Services 進行組態設定。 它只能在網域中運作。

### <a name="can-more-than-one-folder-be-replicated-on-a-single-server"></a>可以在單一伺服器上複寫一個以上的資料夾嗎？

是。 DFS 複寫可以在伺服器之間複寫多個資料夾。 請確定每個已複寫資料夾都有唯一的根路徑，而且不會重疊。 例如，D:\\Sales 和 D:\\Accounting 可以是兩個已複寫資料夾的根路徑，但 D:\\Sales 和 D:\\Sales\\報表不能是兩個已複寫資料夾的根路徑。

### <a name="does-dfs-replication-require-dfs-namespaces"></a>DFS 複寫需要 DFS 命名空間嗎？

不可以。 DFS 複寫和 DFS 命名空間可以單獨或一起使用。 此外，DFS 複寫可以用來複寫獨立的 DFS 命名空間，FRS 則無法做到這點。

### <a name="does-dfs-replication-require-time-synchronization-between-servers"></a>DFS 複寫需要伺服器之間進行時間同步處理嗎？

不可以。 DFS 複寫不會明確要求伺服器之間進行時間同步處理。 不過，DFS 複寫會要求各個伺服器時鐘必須儘量一致。 各個伺服器時鐘彼此設定的時間誤差必須在五分鐘內 (根據預設)，Kerberos 驗證才能正常運作。 例如，DFS 複寫會使用時間戳記來判斷在發生衝突時，會優先處理哪一個檔案。 準確的時間對於垃圾資訊收集、排程和其他功能也很重要。

### <a name="does-dfs-replication-support-replicating-an-entire-volume"></a>DFS 複寫是否支援複寫整個磁碟區？

是。 不過，您必須先安裝 Windows Server 2003 Service Pack 2 或 Hotfix。 如需詳細資訊，請參閱 Microsoft 知識庫的[文章 920335](https://go.microsoft.com/fwlink/?linkid=76776) (https://go.microsoft.com/fwlink/?LinkId=76776) 。 此外，複寫整個磁碟區可能會造成下列問題：

  - 如果磁碟區包含 Windows 分頁檔案，複寫就會失敗，並在系統事件記錄檔中記錄 DFSR 事件 4312。  
      
  - DFS 複寫會在目的地伺服器的複寫資料夾上設定「系統」和「隱藏」屬性。 這是因為 Windows 預設會將「系統」和「隱藏」屬性套用至磁碟區根資料夾。 如果目的地伺服器上複寫資料夾的本機路徑也是磁碟區根目錄，則不會進一步變更資料夾屬性。  
      
  - 當複寫包含 Windows 系統資料夾的磁碟區時，DFS 複寫會辨識 %WINDIR% 資料夾，而不會將它複寫。 不過，DFS 複寫會複寫非 Microsoft 應用程式所使用的資料夾；如果應用程式具有與 DFS 複寫的互通性問題，可能會導致目的地伺服器上的應用程式失敗。  
      

### <a name="does-dfs-replication-support-rpc-over-http"></a>DFS 複寫是否支援 RPC over HTTP？

不可以。

### <a name="does-dfs-replication-work-across-wireless-networks"></a>DFS 複寫可以在無線網路上運作嗎？

是。 DFS 複寫與連線類型無關。

### <a name="does-dfs-replication-work-on-refs-or-fat-volumes"></a>DFS 複寫可以在 ReFS 或 FAT 磁碟區上運作嗎？

不可以。 DFS 複寫僅支援使用 NTFS 檔案系統格式化的磁碟區；不支援彈性檔案系統 (ReFS) 和 FAT 檔案系統。 DFS 複寫需要 NTFS，因為它使用 NTFS 變更日誌和 NTFS 檔案系統的其他功能。

### <a name="does-dfs-replication-work-with-sparse-files"></a>DFS 複寫可以複寫疏鬆檔案嗎？

是。 您可以複寫疏鬆檔案。 接收成員會保留**疏鬆**屬性。

### <a name="do-i-need-to-log-in-as-administrator-to-replicate-files"></a>我需要以系統管理員身分登入才能複寫檔案嗎？

不可以。 DFS 複寫是在本機系統帳戶下執行的服務，因此不需要以系統管理員身分登入就能複寫。 不過，您必須是受影響檔案伺服器的網域系統管理員或本機系統管理員，才能變更 DFS 複寫組態。

如需詳細資訊，請參閱[委派管理 DFS 複寫功能](https://go.microsoft.com/fwlink/?linkid=182294) (https://go.microsoft.com/fwlink/?LinkId=182294) 中的「DFS 複寫安全性需求和委派」。

### <a name="how-can-i-upgrade-or-replace-a-dfs-replication-member"></a>如何升級或取代 DFS 複寫成員？

若要升級或取代 DFS 複寫的成員，請參閱 Ask the Directory Services Team 部落格的此篇文章：[取代 DFSR 成員硬體或 OS](/archive/blogs/askds/series-wrap-up-and-downloads-replacing-dfsr-member-hardware-or-os)。

### <a name="is-dfs-replication-suitable-for-replicating-roaming-profiles"></a>DFS 複寫適合用來複寫漫遊設定檔嗎？

是。 在特定情況下，支援複寫漫遊使用者設定檔。 如需有關受支援情況的詳細資訊，請參閱 [Microsoft 對於複寫使用者設定檔資料的支援聲明](https://go.microsoft.com/fwlink/?linkid=201282) (https://go.microsoft.com/fwlink/?LinkId=201282) 。

### <a name="is-there-a-file-character-limit-or-limit-to-the-folder-depth"></a>是否有檔案字元限制或資料夾深度限制？

Windows 和 DFS 複寫支援最多 32000 個字元的資料夾路徑。 DFS 複寫不受於 260 個字元的資料夾路徑。

### <a name="must-members-of-a-replication-group-reside-in-the-same-domain"></a>複寫群組的成員必須位於相同的網域中嗎？

不可以。 複寫群組可以位於跨越單一樹系中的網域，但不能跨越不同樹系。

### <a name="what-are-the-supported-limits-of-dfs-replication"></a>DFS 複寫支援的限制為何？

下列清單提供一組已由 Microsoft 測試並適用於 Windows Server 2012 R2、Windows Server 2016 和 Windows Server 2019 的擴充性指導方針

  - 伺服器上所有已複寫檔案的大小：100 TB。  
      
  - 磁碟區上已複寫的檔案數目：7000 萬。  
      
  - 檔案大小上限：250 GB。  
      


> [!IMPORTANT]
> 建立具有大量檔案個數或大型檔案的複寫群組時，建議您匯出資料庫複製並使用預先植入技術，將初始複寫的持續時間降至最低。 如需詳細資訊，請參閱 [Windows Server 2012 R2 中的 DFS 複寫初始同步：複製的攻擊](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/DFS-Replication-Initial-Sync-in-Windows-Server-2012-R2-Attack-of/ba-p/424877)。 
<br>


下列清單提供一組已由 Microsoft 測試且適用於 Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008 的擴充性指導方針：

  - 伺服器上所有已複寫檔案的大小：10 TB。  
      
  - 磁碟區上已複寫的檔案數目：7100 萬。  
      
  - 檔案大小上限：64 GB。  
      


> [!NOTE]
> 複寫群組、複寫資料夾、連線或複寫群組成員的數目已不再受限。 
<br>


如需已由 Microsoft 針對 Windows Server 2003 R2 測試過的擴充性指導方針清單，請參閱 [DFS 複寫擴充性指導方針](https://go.microsoft.com/fwlink/?linkid=75043) (https://go.microsoft.com/fwlink/?LinkId=75043) 。

### <a name="when-should-i-not-use-dfs-replication"></a>不應該使用 DFS 複寫的時機為何？

請勿在有多個使用者在不同伺服器上同時更新或修改相同檔案的環境中使用 DFS 複寫。 這樣可能會導致 DFS 複寫將衝突的檔案複本移到隱藏的 DfsrPrivate\\ConflictandDeleted 資料夾中。

當多個使用者同時需要在不同的伺服器上同時修改相同的檔案時，請使用 Windows SharePoint Services 的檔案簽出功能，以確保只有一次只有一個使用者能處理檔案。 Windows Server 2003 R2 中會隨附 Windows SharePoint Services 2.0 (含 Service Pack 2)。 您可以從 Microsoft 網站下載 Windows SharePoint Services；較新版本的 Windows 伺服器中並未包含 Windows SharePoint Services。

### <a name="why-is-a-schema-update-required-for-dfs-replication"></a>為什麼 DFS 複寫需要結構描述更新？

DFS 複寫在 Active Directory Domain Services 的網域命名內容中使用新物件來儲存組態資訊。 當您更新 Active Directory Domain Services 結構描述時，就會建立這些物件。 如需詳細資訊，請參閱[檢閱DFS 複寫的需求](https://go.microsoft.com/fwlink/?linkid=182264) (https://go.microsoft.com/fwlink/?LinkId=182264) 。

## <a name="monitoring-and-management-tools"></a>監視與管理工具

### <a name="can-i-automate-the-health-report-to-receive-warnings"></a>我可以自動執行健康情況報告接收警告的功能嗎？

是。 有三種方式可自動執行健康情況報告：

  - 使用包含在 Windows Server 2012 R2 或 DfsrAdmin.exe 中的 DFSR Windows PowerShell 模組搭配排程工作，定期產生健康情況報告。 如需詳細資訊，請參閱[自動執行 DFS 複寫健康情況報告](https://go.microsoft.com/fwlink/?linkid=74010) (https://go.microsoft.com/fwlink/?LinkId=74010) 。  
      
  - 使用 System Center Operations Manager 的 DFS 複寫管理封包來建立以指定條件為基礎的警示。  
      
  - 使用 DFS 複寫 WMI 提供者編寫警示指令碼。  
      

### <a name="can-i-use-microsoft-system-center-operations-manager-to-monitor-dfs-replication"></a>我可以使用 Microsoft System Center Operations Manager 來監視 DFS 複寫嗎？

是。 如需詳細資訊，請參閱 Microsoft 下載中心中[適用於 System Center Operations Manager 2007 的 DFS 複寫管理封包](https://go.microsoft.com/fwlink/?linkid=182265) (https://go.microsoft.com/fwlink/?LinkId=182265) 。

### <a name="does-dfs-replication-support-remote-management"></a>DFS 複寫是否支援遠端管理？

是。 DFS 複寫支援使用 DFS 管理主控台和**新增複寫群組**命令進行遠端系統管理。 例如，在伺服器 A 上，您可以使用伺服器 A 和 B 做為成員，與樹系中定義的複寫群組連線。

Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008 及 Windows Server 2003 R2 均隨附 DFS 管理功能。 若要管理其他 Windows 版本的 DFS 複寫，請使用遠端桌面或[適用於 Windows 7 的遠端伺服器管理工具](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee449475(v=ws.10))。


> [!IMPORTANT]
> 若要查看或管理包含唯讀複寫資料夾或容錯移轉叢集成員的複寫群組，您必須使用 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows 8、<a href="https://go.microsoft.com/fwlink/p/?linkid=238560">適用於 Windows 8 的遠端伺服器管理工具</a>，或是<a href="https://technet.microsoft.com/library/ee449475">適用於 Windows 7 的遠端伺服器管理工具</a>隨附的 DFS 管理版本。 
<br>


### <a name="do-ultrasound-and-sonar-work-with-dfs-replication"></a>Ultrasound 和 Sonar 可與 DFS 複寫搭配使用嗎？

不可以。 DFS 複寫有自己的一組監視和診斷工具。 Ultrasound 和 Sonar 只能監視 FRS。

### <a name="how-can-files-be-recovered-from-the-conflictanddeleted-or-preexisting-folders"></a>如何從 ConflictAndDeleted 或 PreExisting 資料夾中復原檔案？

若要復原遺失的檔案，請使用檔案歷程記錄、檔案總管中的**還原先前的版本**命令，或從備份還原檔案等方式，還原檔案系統資料夾或共用資料夾中的檔案。 若要直接從 ConflictAndDeleted 或 PreExisting 資料夾復原檔案，請使用 `Get-DfsrPreservedFiles` 和 `Restore-DfsrPreservedFiles`Windows PowerShell Cmdlet (隨附於 Windows Server 2012 R2 中的 DFSR 模組)，或 MSDN 程式碼庫中的 [RestoreDFSR](https://code.msdn.microsoft.com/restoredfsr) 範例指令碼。 此指令碼僅供災害復原修復之用，並依原樣提供，不含擔保。

### <a name="is-there-a-way-to-know-the-state-of-replication"></a>是否有方法可以得知複寫的狀態？

是。 有數種方式可以監視複寫：

  - DFS 複寫具有 System Center Operations Manager 的管理元件，可進行主動監視。  
      
  - DFS 管理可提供複寫待處理項目、複寫效率，以及指定複寫群組中檔案和資料夾數目的內建診斷報告。  
      
  - Windows Server 2012 R2 中的 DFSR Windows PowerShell 模組包含用於啟動傳播測試和寫入傳播和健康情況報告的 Cmdlet。 如需詳細資訊，請參閱 [Windows PowerShell 中的分散式檔案系統複寫 Cmdlet](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee449475(v=ws.10))。  
      
  - Dfsrdiag.exe 是一種命令列工具，可產生待處理項目計數或觸發傳播測試。 兩者都會顯示覆寫狀態。 傳播會顯示是否已將檔案複寫到所有節點。 待處理項目會顯示兩部電腦同步之前，仍需要複寫的檔案數目。待處理項目計數是複寫群組成員尚未處理的更新數目。 在執行 Windows Server 2012 R2、Windows Server 2012 或 Windows Server 2008 R2 的電腦上，Dfsrdiag.exe 也可以顯示 DFS 複寫目前正在複寫的更新。  
      
  - 指令碼可以手動或透過 MOM，使用 WMI 來收集待處理項目資訊。  
      

## <a name="performance"></a>效能

### <a name="does-dfs-replication-support-dial-up-connections"></a>DFS 複寫是否支援撥號連線？

雖然 DFS 複寫可在撥號速度中運作，但如果有大量的變更要複寫，可能會累積許多待處理的項目。 如果只對現有檔案進行些微變更，使用具有遠端差異壓縮 (RDC) 的 DFS 複寫會提供比直接複製檔案更高的效能。

### <a name="does-dfs-replication-perform-bandwidth-sensing"></a>DFS 複寫會執行頻寬感測嗎？

不可以。 DFS 複寫不會執行頻寬感測。 您可以設定 DFS 複寫，以每個連線為基礎使用有限的頻寬量 (頻寬節流)。 不過，如果網路介面變得飽和，DFS 複寫不會進一步降低頻寬使用率，而且 DFS 複寫可以在短時間內讓連結處於飽和狀態。 DFS 複寫的頻寬節流並非絕對準確，因為 DFS 複寫會藉由節流 RPC 呼叫來調節頻寬。 因此，網路堆疊較低層級 (包括 RPC) 的各種緩衝區可能會干擾，導致網路流量激增。

### <a name="does-dfs-replication-throttle-bandwidth-per-schedule-per-server-or-per-connection"></a>DFS 複寫是否依排程、伺服器或連線讓頻寬節流？

如果您在指定排程時設定頻寬節流，該複寫群組的所有連線都會使用該設定進行頻寬節流。 您也可以使用 DFS 管理，將頻寬節流設定為連線層級的設定。

### <a name="does-dfs-replication-use-active-directory-domain-services-to-calculate-site-links-and-connection-costs"></a>DFS 複寫是否使用 Active Directory Domain Services 來計算網站連結和連線成本？

不可以。 DFS 複寫會使用系統管理員所定義的拓撲，這與 Active Directory Domain Services 網站成本無關。

### <a name="how-can-i-improve-replication-performance"></a>如何改善複寫效能？

若要瞭解微調複寫效能的不同方法，請參閱 [Ask the Directory Services Team 部落格](/archive/blogs/askds/)的[微調 DFSR 中的複寫效能](/archive/blogs/askds/tuning-replication-performance-in-dfsr-especially-on-win2008-r2)。

### <a name="how-does-dfs-replication-avoid-saturating-a-connection"></a>DFS 複寫如何避免連線飽和？

您可在 DFS 複寫設定希望連線能夠使用的最大頻寬，而服務會持續使用設定的網路使用量等級。 這與背景智慧型傳送服務 (BITS) 不同，而且如果適當地設定連線，DFS 複寫就不會讓連線處於飽和狀態。

不過，頻寬節流並非 100% 準確，DFS 複寫可以在短時間內讓連結飽和。 這是因為 DFS 複寫會藉由節流 RPC 呼叫來調節頻寬。 由於此程序依賴網路堆疊較低層級的各種緩衝區 (包括 RPC)，因此複寫流量通常會以高載速度行進，而這可能會導致網路連結飽和。

Windows Server 2008 中的 DFS 複寫包含幾項效能增強功能，如 [Windows Server 2003 with SP1 到 WIndows Server 2008 的功能變更](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753208(v=ws.10))中的[分散式檔案系統](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753479(v=ws.10))主題所述。

### <a name="how-does-dfs-replication-performance-compare-with-frs"></a>相較於 FRS，DFS 複寫的效能表現如何？

DFS 複寫比 FRS 更快，特別是對大型檔案和 RDC 進行些微變更時。 例如，使用 RDC 對 2 MB PowerPoint&reg; 簡報進行些微變更時，可能只會在網路上傳送 60 KB 的流量 (可節省 97% 的位元組傳輸量)。

RDC 不會用於小於 64 KB 的檔案，而且對於未爭用網路頻寬的高速 Lan 而言，可能不會有任何好處。 您可以使用 DFS 管理，以每個連線為基礎來停用 RDC。

### <a name="how-frequently-does-dfs-replication-replicate-data"></a>DFS 複寫的資料複寫頻率為何？

資料會根據您設定的排程進行複寫。 例如，您可以將排程設定為間隔 15 分鐘，一周七天。 在這些間隔期間，會啟用複寫。 在偵測到檔案變更後，複寫就會立即開始 (通常會在幾秒內)。

當連線排程設定為接收成員的本機時間時，複寫群組排程可能會設定為通用協調時間 (UTC)。 當複寫群組跨越多個時區時，請將此納入考慮。 本機時間表示裝載傳入連線之成員的時間。 當排程設定為本機時間時，傳入連線和對應的傳出連線所顯示的排程會反映時區差異。

### <a name="how-much-of-my-servers-system-resources-will-dfs-replication-consume"></a>DFS 複寫會耗用多少伺服器的系統資源？

DFS 複寫使用的磁碟、記憶體和 CPU 資源取決於數種因素，包括檔案數目和大小、變更速率、複寫群組成員數目，以及複寫資料夾數目。 此外，某些資源較難預估。 例如，用於 DFS 複寫資料庫的可延伸儲存引擎 (ESE) 技術可能會耗用大量的可用記憶體百分比，並依需要加以釋出。 視伺服器組態而定，DFS 複寫以外的應用程式可以裝載在相同的伺服器上。 不過，在單一伺服器上裝載多個應用程式或伺服器角色時，請務必先測試此組態，再於生產環境中執行。

### <a name="what-happens-if-a-wan-link-fails-during-replication"></a>如果 WAN 連結在複寫期間失敗，會發生什麼事？

如果連線中斷，DFS 複寫會在排程開啟時繼續嘗試複寫。 在 DFS 複寫事件記錄檔中，也會註明可使用 MOM 搜集到的連線錯誤 (通過警示主動通報) 和 DFS 複寫健康情況報告 (例如由系統管理員執行)。

## <a name="remote-differential-compression-details"></a>遠端差異壓縮詳細資料

### <a name="are-changes-compressed-before-being-replicated"></a>在複寫之前，會先壓縮變更嗎？

是。 已變更的檔案部分會在傳送給所有檔案類型之前進行壓縮，但下列格式除外 (因為已進行壓縮)：wma、.wmv、.zip、.jpg、.mpg、.mpeg、.m1v、.mp2、.mp3、.mpa、.cab、.wav、.snd、au、.asf、.wm、.avi、.z、.gz、.tgz 和 .frx。 您無法在 Windows Server 2003 R2 中設定這些檔案類型的壓縮設定。

### <a name="can-an-administrator-turn-off-rdc-or-change-the-threshold"></a>系統管理員是否可以關閉 RDC 或變更閾值？

是。 您可以透過指定連線的屬性頁來關閉 RDC。 停用 RDC 可針對沒有頻寬限制的快速區域網路 (LAN) 連結，或多數檔案小於 64 KB 的複寫群組，減少 CPU 使用率和複寫延遲。 如果您選擇停用連線上的 RDC，請在變更前後測試複寫效率，以確認已改善複寫效能。

您可以使用 **Dfsradmin 連線集**命令、DFS 複寫 WMI 提供者，或手動編輯組態 XML 檔案，變更 RDC 大小閾值。

### <a name="does-rdc-work-on-all-file-types"></a>RDC 適用於所有檔案類型嗎？

是。 RDC 會計算區塊層級的差異，而不考慮檔案資料類型。 不過，RDC 在某些檔案類型 (例如 Word 檔案、PST 檔案和 VHD 映像) 上的運作效率更高。

### <a name="how-does-rdc-work-on-a-compressed-file"></a>RDC 如何在壓縮檔案上運作？

DFS 複寫使用 RDC 來計算檔案中已變更的區塊，並透過網路只傳送那些區塊。 DFS 複寫不需要知道檔案內容，只需知道變更的區塊。

### <a name="is-cross-file-rdc-enabled-when-upgrading-to-windows-server-enterprise-edition-or-datacenter-edition"></a>升級到 Windows Server Enterprise Edition 或 Datacenter Edition 時是否會啟用跨檔案 RDC？

Windows Server Standard Edition 不支援跨檔案 RDC。 不過，當您升級至支援跨檔案 RDC 的版本，或複寫連線的成員正在執行支援的版本時，會自動啟用此功能。 如需支援跨檔案 RDC 的版本清單，請參閱「哪些版本的 Windows 作業系統支援跨檔案 RDC？」一文

### <a name="is-rdc-true-block-level-replication"></a>RDC 可執行區塊層級的複寫嗎？

不可以。 RDC 是用來壓縮檔案傳輸的一般用途通訊協定。 DFS 複寫會在檔案層級的區塊上使用 RDC，而不是在磁碟區塊層級。 RDC 會將檔案分割成區塊， 並計算檔案中每個區塊的簽章，這是可代表較大區塊的少量位元組。 簽章集合會從伺服器傳送到用戶端。 用戶端會將伺服器簽章與自己的簽章進行比較。 接著，用戶端會要求伺服器只傳送尚未存在於用戶端上的簽章資料。

### <a name="what-happens-if-i-rename-a-file"></a>如果重新命名檔案會如何？

DFS 複寫會在下一次複寫期間，將複寫群組的所有其他成員上的檔案重新命名。 系統會使用唯一識別碼追蹤檔案，因此在複本中將檔案重新命名並移動，並不會影響 DFS 的檔案複寫能力。

### <a name="what-is-cross-file-rdc"></a>什麼是跨檔案 RDC？

跨檔案 RDC 可讓 DFS 複寫使用 RDC，即使用戶端上不存在具有相同名稱的檔案。 跨檔案 RDC 會使用啟發學習法來判斷與需要複寫之檔案類似的檔案，並使用與複寫檔案相同的類似檔案區塊，將透過 WAN 傳送的資料量降到最低。 跨檔案 RDC 可以在此程序中使用最多五個類似檔案的區塊。

若要使用跨檔案 RDC，複寫連線的其中一個成員必須執行支援跨檔案 RDC 的 Windows 版本。 如需支援跨檔案 RDC 的版本清單，請參閱「哪些版本的 Windows 作業系統支援跨檔案 RDC？」一文

### <a name="what-is-rdc"></a>什麼是 RDC？

遠端差異壓縮 (RDC) 是一種用戶端伺服器通訊協定，可用來透過有限頻寬的網路有效率地更新檔案。 RDC 會偵測檔案中的資料插入、移除與重新排列等變更，讓 DFS 複寫僅需複寫檔案的變更部分。 RDC 僅用於預設為 64 KB 或更大的檔案。 RDC 可以使用複寫資料夾或 DfsrPrivate\\ConflictandDeleted 資料夾 (位於複寫資料夾的本機路徑下) 中具有相同名稱的舊版本檔案。

### <a name="when-is-rdc-used-for-replication"></a>何時會使用 RDC 進行複寫？

當檔案超過大小閾值下限時，會使用 RDC。 此大小閾值預設為 64 KB。 複寫超過該閾值的檔案之後，檔案的更新版本一律會使用 RDC，除非大部分的檔案已變更或 RDC 已停用。

### <a name="which-editions-of-the-windows-operating-system-support-cross-file-rdc"></a>哪些版本的 Windows 作業系統支援跨檔案 RDC？

若要使用跨檔案 RDC，複寫連線的其中一個成員必須執行支援跨檔案 RDC 的 Windows 作業系統版本。 下表顯示支援跨檔案 RDC 的 Windows 版本。

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
<td><p>無法使用</p></td>
<td><p>是</em></p></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012</p></td>
<td><p>是</p></td>
<td><p>無法使用</p></td>
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

\* 您可以選擇性地停用 Windows Server 2012 R2 上的跨檔案 RDC。

## <a name="replication-details"></a>複寫詳細資料

### <a name="can-i-change-the-path-for-a-replicated-folder-after-it-is-created"></a>我可以在建立複寫資料夾之後變更其路徑嗎？

不可以。 如果您需要變更複寫資料夾的路徑，必須在 DFS 管理中將原來的路徑刪除，然後新增路徑以作為新的複寫資料夾。 DFS 複寫接著會使用遠端差異壓縮 (RDC) 來執行同步處理，以判斷傳送和接收成員上的資料是否相同。 此舉不會再次複寫資料夾中的所有資料。

### <a name="can-i-configure-which-file-attributes-are-replicated"></a>我可以設定要複寫的檔案屬性嗎？

不可以，您無法設定 DFS 複寫要複寫的檔案屬性。

如需屬性值及其描述的清單，請參閱 MSDN 上的[檔案屬性](https://go.microsoft.com/fwlink/?linkid=182268) (https://go.microsoft.com/fwlink/?LinkId=182268) 。

下列屬性值是使用 `SetFileAttributes dwFileAttributes` 函式所設定，而且是由 DFS 複寫進行複寫。 變更這些屬性值會觸發屬性的複寫。 除非也變更了內容，否則系統不會複寫檔案內容。 如需詳細資訊，請參閱 MSDN 程式庫中的 [ 函式](https://go.microsoft.com/fwlink/?linkid=182269) (https://go.microsoft.com/fwlink/?LinkId=182269) 。

  - FILE\_ATTRIBUTE\_HIDDEN  
      
  - FILE\_ATTRIBUTE\_READONLY  
      
  - FILE\_ATTRIBUTE\_SYSTEM  
      
  - FILE\_ATTRIBUTE\_NOT\_CONTENT\_INDEXED  
      
  - FILE\_ATTRIBUTE\_OFFLINE  
      

下列屬性值是由 DFS 複寫所複寫，但不會觸發複寫。

  - FILE\_ATTRIBUTE\_ARCHIVE  
      
  - FILE\_ATTRIBUTE\_NORMAL  
      

下列檔案屬性值也會觸發複寫，但無法使用 `SetFileAttributes` 函式來設定 (使用 `GetFileAttributes` 函式檢視屬性值)。

  - FILE\_ATTRIBUTE\_REPARSE\_POINT  
      

> [!NOTE]
> 除非 IO_REPARSE_TAG_SYMLINK 重新剖析標記，否則 DFS 複寫不會複寫重新剖析點的屬性值。 具有 IO_REPARSE_TAG_DEDUP、IO_REPARSE_TAG_SIS 或 IO_REPARSE_TAG_HSM 重新剖析標記的檔案會複寫為一般檔案。 不過，重新剖析標記和重新剖析資料緩衝區不會複寫到其他伺服器，因為重新剖析點只可在本機系統運作。 
<br>

  - FILE\_ATTRIBUTE\_COMPRESSED  
      
  - FILE\_ATTRIBUTE\_ENCRYPTED  
      

> [!NOTE]
> DFS 複寫不會複寫使用加密檔案系統 (EFS) 加密的檔案。 DFS 複寫會複寫使用非 Microsoft 軟體加密的檔案，但只有在未在檔案上設定 FILE_ATTRIBUTE_ENCRYPTED 屬性值時，才會進行複寫。 
<br>

  - FILE\_ATTRIBUTE\_SPARSE\_FILE  
      
  - FILE\_ATTRIBUTE\_DIRECTORY  
      

DFS 複寫不會複寫 FILE\_ATTRIBUTE\_TEMPORARY 值。

### <a name="can-i-control-which-member-is-replicated"></a>我可以控制要複寫的成員嗎？

是。 建立複寫群組時，您可以選擇拓撲。 或者，您可以選取 [沒有拓撲]  並在建立複寫群組之後手動設定連線。

### <a name="can-i-seed-a-replication-group-member-with-data-prior-to-the-initial-replication"></a>我可以在初始複寫之前，將資料植入複寫群組成員嗎？

是。 DFS 複寫支援在初始複寫之前將檔案複製到複寫群組成員。 這項「預先設置」可以大幅減少初始複寫期間複寫的資料量。

若只有檔案的實際屬性或時間戳記不同，則初始複寫不需要複寫內容。 實際屬性是可由 Win32 函式 `SetFileAttributes` 設定的屬性。 如需詳細資訊，請參閱 MSDN 程式庫中的 [ 函式](https://go.microsoft.com/fwlink/?linkid=182269) (https://go.microsoft.com/fwlink/?LinkId=182269) 。 如果兩個檔案有相異的其他屬性 (例如壓縮)，則會複寫檔案內容。

若要預先設置複寫群組成員，請將檔案複製到目的地伺服器上適當的資料夾、建立複寫群組，然後選擇主要成員。 因為主要成員的內容會被視為具有「系統授權」，所以請選擇擁有您要複寫之最新檔案的成員。 這表示在初始複寫期間，主要成員的檔案一律會覆寫其他複寫群組成員上的其他檔案版本。

如需有關預先植入和複製 DFSR 資料庫的詳細資訊，請參閱[Windows Server 2012 R2 中的 DFS 複寫初始同步：複製的攻擊](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB)。

如需初始複寫的詳細資訊，請參閱[建立複寫群組](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc725893(v=ws.11))。

### <a name="does-dfs-replication-overcome-common-file-replication-service-issues"></a>DFS 複寫是否解決了常見的檔案複寫服務問題？

是。 DFS 複寫克服了三個常見的 FRS 問題：

  - 日誌包裝：DFS 複寫會從日誌包裝即時復原。 再次啟用複寫之前，每個現有的檔案或資料夾都會標示為 journalWrap，並針對檔案系統進行驗證。 在復原期間，無法針對此磁碟區從任一方向進行複寫。  
      
  - 超量複寫：為了避免超量複寫，DFS 複寫會使用點數系統。  
      
  - 轉化資料夾：為了避免資料夾名稱轉化，DFS 複寫會將衝突的資料儲存在隱藏的 DfsrPrivate\\ConflictandDeleted 資料夾中 (位於複寫資料夾的本機路徑下)。 例如，在使用 FRS 複寫的不同伺服器上，同時使用相同的名稱來建立多個資料夾，會導致 FRS 將較舊的資料夾重新命名。 因此，DFS 複寫改為將較舊的資料夾移至本機的「因衝突而刪除」資料夾。  
      

### <a name="does-dfs-replication-replicate-files-in-chronological-order"></a>DFS 複寫會依時間順序複寫檔案嗎？

不可以。 檔案可能會不依序複寫。

### <a name="does-dfs-replication-replicate-files-that-are-being-used-by-another-application"></a>DFS 複寫是否會複寫正由另一個應用程式使用的檔案？

如果應用程式開啟檔案並在其上建立檔案鎖定 (防止其他應用程式在檔案開啟時使用)，在關閉檔案之前，DFS 複寫不會複寫該檔案。 如果應用程式開啟具有讀取共用存取權的檔案，則仍然可以複寫該檔案。

### <a name="does-dfs-replication-replicate-ntfs-file-permissions-alternate-data-streams-hard-links-and-reparse-points"></a>DFS 複寫會複寫 NTFS 檔案權限、替代資料串流、永久連結和重新剖析點嗎？

  - DFS 複寫會複寫 NTFS 檔案權限和替代資料串流。  
      
  - Microsoft 不支援對存入或來自複寫資料夾中的檔案建立 NTFS 永久連結，這麼做可能會讓受影響檔案發生複寫問題。 DFS 複寫會忽略永久連結檔案，而且不會複寫。 連接點也不會複寫，而且 DFS 複寫會對每個遇到的連接點記錄事件 4406。  
      
  - DFS 複寫只會複寫使用 IO\_REPARSE\_TAG\_SYMLINK 標記的重新剖析點；不過，DFS 複寫不保證也會一併複寫符號連結目標。 如需詳細資訊，請參閱 [Ask the Directory Services Team 部落格](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc725893(v=ws.11))。  
      
  - 具有 IOIO\_REPARSE\_TAG\_DEDUP、IO\_REPARSE\_TAG\_SIS 或 IO\_REPARSE\_TAG\_HSM 標記的檔案會複寫為一般檔案。 重新剖析標記和重新剖析資料緩衝區不會複寫到其他伺服器，因為重新剖析點只可在本機系統運作。 因此，DFS 複寫可以在 Windows Server 2012 或單一執行個體儲存體 (SIS) 中使用重複資料刪除的磁碟區上複寫資料夾；不過，重複資料刪除資訊是由啟用角色服務的每部伺服器個別維護。  
      

### <a name="does-dfs-replication-replicate-timestamp-changes-if-no-other-changes-are-made-to-the-file"></a>如果沒有對檔案進行任何其他變更，DFS 複寫的複寫時間戳記會變更嗎？

不會，DFS 複寫不會針對只有時間戳記變更的檔案進行複寫。 此外，除非對檔案進行其他變更，否則變更的時間戳記不會複寫到複寫群組的其他成員。

### <a name="does-dfs-replication-replicate-updated-permissions-on-a-file-or-folder"></a>DFS 複寫是否會複寫檔案或資料夾上更新的權限？

是。 DFS 複寫會複寫檔案和資料夾的權限變更。 只有與存取控制清單 (ACL) 相關聯的檔案部分會進行複寫，但是 DFS 複寫仍然必須將整個檔案讀取到暫存區域中。


> [!NOTE]
> 變更大量檔案的 ACL 可能會影響複寫效能。 不過，使用 RDC 時，資料傳輸量會與 ACL 的大小成正比，而不是整個檔案的大小。 磁碟流量仍然與檔案大小成正比，因為系統必須將檔案讀取到複寫用快取資料夾，或從複寫用快取資料夾中讀取檔案。 
<br>


### <a name="does-dfs-replication-support-merging-text-files-in-the-event-of-a-conflict"></a>發生衝突時，DFS 複寫是否支援合併文字檔？

發生衝突時，DFS 複寫不會合併檔案。 不過，它會嘗試在偵測到衝突的電腦上，將較舊版本的檔案保留在隱藏的 DfsrPrivate\\ConflictandDeleted 資料夾中。

### <a name="does-dfs-replication-use-encryption-when-transmitting-data"></a>DFS 複寫是否會在傳輸資料時使用加密？

是。 DFS 複寫使用遠端程序呼叫 (RPC) 連線進行加密。

### <a name="is-it-possible-to-disable-the-use-of-encrypted-rpc"></a>是否可以停用加密的 RPC？

不可以。 DFS 複寫服務會透過 TCP，使用遠端程序呼叫 (RPC) 來複寫資料。 為了保護跨網際網路的資料傳輸安全性，DFS 複寫服務設計為一律使用驗證層級常數，`RPC_C_AUTHN_LEVEL_PKT_PRIVACY`。 這可確保網際網路上的 RPC 通訊一律會加密。 因此，DFS 複寫服務無法停用加密的 RPC。

若需詳細資訊，請參閱下列 Microsoft 網站：

  - [RPC 技術參考](https://go.microsoft.com/fwlink/?linkid=182278)  
      
  - [有關遠端差異壓縮](https://go.microsoft.com/fwlink/?linkid=182279)  
      
  - [驗證層級常數](https://go.microsoft.com/fwlink/?linkid=182280)  
      

### <a name="how-are-simultaneous-replications-handled"></a>如何處理同步複寫？

每個複寫資料夾都有一個更新管理員。 更新管理員可以彼此獨立作業。

根據預設，最多 16 個 (其中 4 個位於 Windows Server 2003 R2 中) 並行下載會在所有連線和複寫群組之間共用。 因為連線和複寫群組更新尚未序列化，因此不會有收到更新的特定順序。 如果開啟兩個排程，通常會從兩個連線同時接收並安裝更新。

### <a name="how-do-i-force-replication-or-polling"></a>如何強制複寫或輪詢？

您可以使用 DFS 管理立即強制複寫，如[編輯複寫排程](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732278(v=ws.11))中所述。 也可以使用 `Sync-DfsReplicationGroup` Cmdlet (隨附於 Windows Server 2012 R2 所引進的 DFSR PowerShell 模組) 或 **Dfsrdiag SyncNow** 命令來強制複寫。 您可以使用 `Update-DfsrConfigurationFromAD` Cmdlet 或 **Dfsrdiag PollAD** 命令來強制輪詢。

### <a name="is-it-possible-to-configure-a-quiet-time-between-replications-for-files-that-change-frequently"></a>是否可以為經常變更的檔案設定複寫間的靜止時間？

不可以。 如果排程已開啟，DFS 複寫會在注意到變更時進行複寫。 沒有任何方法可以設定檔案的靜止時間。

### <a name="is-it-possible-to-configure-one-way-replication-with-dfs-replication"></a>是否可以使用 DFS 複寫設定單向複寫？

是。 如果您使用 Windows Server 2012 或 Windows Server 2008 R2，可以建立唯讀複寫資料夾，透過單向連線來複寫內容。 如需詳細資訊，請參閱[將特定成員的複寫資料夾設為唯讀](https://go.microsoft.com/fwlink/?linkid=156740) (https://go.microsoft.com/fwlink/?LinkId=156740) 。

我們不支援使用 Windows Server 2008 或 Windows Server 2003 R2 中的 DFS 複寫來建立單向複寫連線。 這麼做可能會造成許多問題，包括健康情況檢查拓撲錯誤、暫存問題，以及 DFS 複寫資料庫的問題。

如果您使用 Windows Server 2008 或 Windows Server 2003 R2，可以執行下列動作來模擬單向連線：

  - 訓練系統管理員只在想要指定為主要伺服器的伺服器上進行變更， 然後將變更複寫到目的地伺服器。  
      
  - 在目的地伺服器上設定共用權限，如此使用者就不需要具有寫入權限。 如果分支伺服器上不允許變更，就沒有資料可複寫回來，模擬單向連線可維持較低的廣域網路使用率。  
      

### <a name="is-there-a-way-to-force-a-complete-replication-of-all-files-including-unchanged-files"></a>是否有方法可以強制所有檔案進行完整複寫，包括未變更的檔案？

不可以。 如果 DFS 複寫將檔案視為相同，就不會複寫。 如果尚未複寫變更的檔案，DFS 複寫會依設定自動複寫檔案。 若要覆寫已設定的排程，請使用 WMI 方法 **ForceReplicate()** 。 不過，這只是排程覆寫，而且不會強制複寫未變更或相同的檔案。

### <a name="what-happens-if-the-primary-member-suffers-a-database-loss-during-initial-replication"></a>如果主要成員在初始複寫期間造成資料庫遺失，會發生什麼事？

在初始複寫期間，當接收成員的檔案版本與主要成員的不同，主要成員的檔案一律會在衝突解決中獲得優先權。 主要成員指定會儲存在 Active Directory Domain Services 中，而指定會在主要成員準備好複寫後，但在複寫群組的所有成員複寫之前清除。

如果初始複寫失敗或 DFS 複寫服務在複寫期間重新啟動，則主要成員會看到本機 DFS 複寫資料庫中的主要成員指定，並重試初始複寫。 如果在 Active Directory Domain Services 中清除主要指定之後，主要成員的 DFS 複寫資料庫遺失，但在複寫群組的所有成員完成初始複寫之前，複寫群組的所有成員都將無法複寫資料夾，因為未指定任何伺服器做為主要成員。 如果發生這種情況，請使用主要成員伺服器上的 **Dfsradmin membership /set /isprimary:true** 命令，手動還原主要成員指定。

如需初始複寫的詳細資訊，請參閱[建立複寫群組](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc725893(v=ws.11))。


> [!WARNING]
> 主要成員指定只會在初始複寫程序中使用。 如果您在複寫完成後，使用 <STRONG>Dfsradmin</STRONG> 命令指定複寫資料夾的主要成員，DFS 複寫不會將伺服器指定為 Active Directory Domain Services 中的主要成員。 不過，如果之後伺服器上的 DFS 複寫資料庫發生無法復原的損毀或資料遺失，伺服器就會嘗試執行主要成員的初始複寫，而不是將複寫群組的其他成員資料復原。 基本上，伺服器會變成 Rogue 主要伺服器，這可能會造成衝突。 基於這個理由，只有在您確定初始複寫已造成無可挽回的嚴重失敗時，才手動指定主要成員。 
<br>


### <a name="what-happens-if-the-replication-schedule-closes-while-a-file-is-being-replicated"></a>如果複寫排程在複寫檔案時關閉，會發生什麼事？

如果已在連線上啟用遠端差異壓縮 (RDC)，則在排程關閉之前 (或變更為**沒有頻寬**) 針對大於 64KB 的檔案立即開始傳入複寫的作業會繼續進行，並會在排程開啟時繼續進行 (或變更為**沒有頻寬**以外的狀態)。 複寫會從其停止時的狀態繼續進行。

如果 RDC 已關閉，DFS 複寫會完整重新啟動檔案傳輸。 當接收成員中有可供使用的檔案時，可能會造成延遲。

### <a name="what-happens-when-two-users-simultaneously-update-the-same-file-on-different-servers"></a>當兩個使用者同時更新不同伺服器上的相同檔案時，會發生什麼事？

當 DFS 複寫偵測到衝突時，會使用最後儲存的檔案版本。 如此會將其他檔案移到 DfsrPrivate\\ConflictandDeleted 資料中 (位於解決衝突之電腦的複寫資料夾的本機路徑下)。 系統會一直保留這些檔案，直到「因衝突而刪除」資料夾清除，這會在「因衝突而刪除」資料夾超過設定的大小，或 DFS 複寫遇到磁碟空間不足的錯誤時發生。 系統不會複寫「因衝突而刪除」資料夾，而且這種衝突解決方法可避免在 FRS 中可能發生的目錄轉化問題。

發生衝突時，DFS 複寫會將參考事件記錄到 DFS 複寫的事件記錄檔中。 基於下列原因，此事件不需要使用者採取動作：

  - 使用者看不到相關訊息 (只有伺服器系統管理員可以看見)。  
      
  - DFS 複寫會將「因衝突而刪除」資料夾視為快取。 達到配額閾值時，DFS 複寫會清除其中一些檔案。 不保證會儲存衝突的檔案。  
      
  - 衝突可能發生在與衝突來源不同的伺服器上。  
      

## <a name="staging"></a>執行

### <a name="does-dfs-replication-continue-staging-files-when-replication-is-disabled-by-a-schedule-or-bandwidth-throttling-quota-or-when-a-connection-is-manually-disabled"></a>當複寫已由排程或頻寬節流配額停用，或手動停用連線時，DFS 複寫仍然繼續暫存檔案嗎？

不可以。 如果已超過頻寬節流配額，或已停用連線，則DFS 複寫不會繼續在排程的複寫時間外暫存檔案。

### <a name="does-dfs-replication-prevent-other-applications-from-accessing-a-file-during-staging"></a>DFS 複寫是否會在暫存期間防止其他應用程式存取檔案？

不可以。 DFS 複寫開啟檔案的方式，不會阻檔使用者或應用程式開啟複寫資料夾中的檔案。 這個方法稱為「機會鎖定」。

### <a name="is-it-possible-to-change-the-location-of-the-staging-folder-with-the-dfs-management-tool"></a>是否可以使用 DFS 管理工具來變更複寫用快取資料夾的位置？

是。 在每個複寫群組成員之 [屬性]  對話方塊的 [進階]  索引標籤上，設定複寫用快取資料夾位置。

### <a name="when-are-files-staged"></a>何時會暫存檔案？

當接收成員要求檔案 (除非檔案為 64 KB 或更小的檔案) 時，就會在傳送成員上暫存，如下表所示。 如果已在連線上停用遠端差異壓縮 (RDC)，則會暫存該檔案，除非檔案為 256 KB 或更小。 如果檔案大小小於 64 KB，也會在接收成員上暫存，但您可以將其設定為 16 KB 到 1 MB 之間。 如果排程已關閉，則不會暫存檔案。

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
<th>啟用 RDC</th>
<th>停用 RDC</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td><p>傳送成員</p></td>
<td><p>64 KB</p></td>
<td><p>256 KB</p></td>
</tr>
<tr class="odd">
<td><p>接收成員</p></td>
<td><p>預設 64 KB</p></td>
<td><p>預設 64 KB</p></td>
</tr>
</tbody>
</table>

### <a name="what-happens-if-a-file-is-changed-after-it-is-staged-but-before-it-is-completely-transmitted-to-the-remote-site"></a>如果檔案在暫存後，但完全傳輸到遠端網站之前變更，會發生什麼事？

如果已傳輸檔案的任何部分，DFS 複寫會繼續傳輸。 如果在 DFS 複寫開始傳輸檔案之前，檔案就已變更，則會傳送較新版本的檔案。

## <a name="change-history"></a>變更歷程記錄


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>日期</th>
<th>說明</th>
<th>原因</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>2018 年 11 月 15 日</p></td>
<td><p>Windows Server 2019 更新。</p></td>
<td><p>新版作業系統。</p></td>
</tr>
<tr class="even">
<td><p>2013 年 10 月 9 日</p></td>
<td><p>更新了「DFS 複寫支援的限制為何？」一節中的內容，納入 Windows Server 2012 R2 中測試結果。</p></td>
<td><p>最新版本 Windows Server 中有哪些更新</p></td>
</tr>
<tr class="odd">
<td><p>2013 年 1 月 30 日</p></td>
<td><p>新增了「當複寫已由排程或頻寬節流配額停用，或手動停用連線時，DFS 複寫仍然繼續暫存檔案嗎？」項目。</p></td>
<td><p>客戶問題</p></td>
</tr>
<tr class="even">
<td><p>2012 年 10 月 31 日</p></td>
<td><p>編輯了「DFS 複寫支援的限制為何？」項目，增加磁碟區上已測試的已複寫檔案數目。</p></td>
<td><p>客戶意見反應</p></td>
</tr>
<tr class="odd">
<td><p>2012 年 8 月 15 日</p></td>
<td><p>編輯了「FS 複寫會複寫 NTFS 檔案權限、替代資料串流、永久連結和重新剖析點嗎？」項目，進一步釐清 DFS 複寫如何處理永久連結和重新剖析點。</p></td>
<td><p>客戶支援服務的意見反應</p></td>
</tr>
<tr class="even">
<td><p>2012 年 6 月 13 日</p></td>
<td><p>編輯了「DFS 複寫可以在 ReFS 或 FAT 磁碟區上運作嗎？」項目，新增 ReFS 的相關討論。</p></td>
<td><p>客戶意見反應</p></td>
</tr>
<tr class="odd">
<td><p>2012 年 4 月 25 日</p></td>
<td><p>編輯了「FS 複寫會複寫 NTFS 檔案權限、替代資料串流、永久連結和重新剖析點嗎？」項目，釐清 DFS 複寫如何處理永久連結。</p></td>
<td><p>減少可能的混淆</p></td>
</tr>
<tr class="even">
<td><p>2011 年 3 月 30 日</p></td>
<td><p>編輯了「DFS 複寫可以複寫 Outlook .pst 或 Microsoft Office Access 資料庫檔案嗎？」項目，以更正將 DFS 複寫與 .pst 和 Access 檔案搭配使用的潛在影響。</p>
<p>新增了「如何改善複寫效能？」</p></td>
<td><p>客戶對前一個項目的問題有疑問，項目內容錯誤地指出複寫 .pst 或 Access 檔案可能損毀 DFS 複寫資料庫。</p></td>
</tr>
<tr class="odd">
<td><p>2011 年 1 月 26 日</p></td>
<td><p>新增了「如何從 ConflictAndDeleted 或 PreExisting 資料夾中復原檔案？」</p></td>
<td><p>客戶意見反應</p></td>
</tr>
<tr class="even">
<td><p>2010 年 10 月 20 日</p></td>
<td><p>新增了「如何升級或取代 DFS 複寫成員？」</p></td>
<td><p>客戶意見反應</p></td>
</tr>
</tbody>
</table>
