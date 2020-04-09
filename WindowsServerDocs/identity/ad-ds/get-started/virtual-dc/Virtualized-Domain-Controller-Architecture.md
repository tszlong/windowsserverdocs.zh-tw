---
ms.assetid: 341614c6-72c2-444f-8b92-d2663aab7070
title: 虛擬網域控制站架構
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: fa8645198374d91911f8ec7dc15f04bea4865e38
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824441"
---
# <a name="virtualized-domain-controller-architecture"></a>虛擬網域控制站架構

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

此主題涵蓋虛擬網域控制站複製與安全還原的架構。 本文以流程圖顯示複製與安全還原的程序，並提供程序中每個步驟的詳細說明。  
  
-   [虛擬網域控制站複製架構](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch)  
  
-   [虛擬網域控制站安全還原架構](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)  
  
## <a name="virtualized-domain-controller-cloning-architecture"></a><a name="BKMK_CloneArch"></a>虛擬網域控制站複製架構  
  
### <a name="overview"></a>概觀  
虛擬網域控制站複製依賴 Hypervisor 平台公開稱為「VM 世代識別碼」 的識別碼來偵測虛擬機器的建立。 在網域控制站升級期間，AD DS 一開始會將此識別碼的值儲存在其資料庫 (NTDS.DIT)。 當虛擬機器開機時，會比較目前來自虛擬機器之「VM 世代識別碼」的值與資料庫中的值。 如果兩個值不同，網域控制站會重設「引動過程識別碼」並捨棄 RID 集區，因此可以防止重複使用 USN 或可能會建立重複安全性主體的問題。 接著，網域控制站會在[複製的詳細程序](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneProcessDetails)之步驟 3 所述的位置中尋找 DCCloneConfig.xml 檔案。 如果發現 DCCloneConfig.xml 檔案，它會做出被部署為複本的結論，因此它會起始複製程序，使用現有的 NTDS.DIT 與從來源媒體複製的 SYSVOL 內容重新升級，將自己佈建為其他網域控制站。  
  
在有些 Hypervisor 支援「VM 世代識別碼」，而有些不支援的混合環境中，可能會不小心在不支援「VM 世代識別碼」的 Hypervisor 上部署複製媒體。 DCCloneConfig.xml 檔案存在表示系統管理員意圖複製 DC。 因此，如果在開機期間發現 DCCloneConfig.xml 檔案，但主機未提供「VM 世代識別碼」，表示複製 DC 開機進入目錄服務還原模式 (DSRM)，以防止環境中的其他部分受影響。 接著，您可以將複製媒體移動到支援「VM 世代識別碼」的 Hypervisor，然後重試複製。  
  
如果複製媒體部署在支援「VM 世代識別碼」的 Hypervisor 上，但未提供 DCCloneConfig.xml 檔案，當 DC 偵測到來自其 DIT 的「VM 世代識別碼」與來自新 VM 的「VM 世代識別碼」不同，就會觸發防護功能，以防止重複使用 USN 並避免 SID 重複。 不過，將不會起始複製程序，因此次要 DC 會以和來源 DC 相同的身分識別繼續執行。 您應該儘快將此次要 DC 從網路移除，以避免環境中的不一致。 如需如何回收此次要 DC 同時確保更新可輸出複寫的詳細資訊，請參閱 Microsoft 知識庫文章 [2742970](https://support.microsoft.com/kb/2742970)。  
  
### <a name="cloning-detailed-processing"></a><a name="BKMK_CloneProcessDetails"></a>複製詳細的處理  
下圖顯示初始複製操作和複製重試操作的架構。 本主題稍後將詳細說明這些程序。  
  
**初始複製作業**  
  
![虛擬化的 DC 架構](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_InitialCloningProcess.png)  
  
**正在複製重試作業**  
  
![虛擬化的 DC 架構](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_CloningRetryProcess.png)  
  
下列步驟詳細說明此程序：  
  
1.  現有的虛擬機器網域控制站在支援「VM 世代識別碼」的 Hypervisor 中開機。  
  
    1.  此 VM 升級之後，在它的 AD DS 電腦物件上沒有設定現有的「VM 世代識別碼」值。  
  
    2.  即使它是空值，下一次建立電腦時將表示它依然會複製，因為新的「VM 世代識別碼」不會符合。  
  
    3.  DC 在下一次重新開機後會設定「VM 世代識別碼」，且不會複寫。  
  
2.  接著，虛擬機器會讀取 VMGenerationCounter 驅動程式提供的「VM 世代識別碼」。 它會比較兩個「VM 世代識別碼」。  
  
    1.  若兩個識別碼相符合，表示這不是新的虛擬機器，且複製作業不會繼續執行。 如果 DCCloneConfig.xml 檔案存在，網域控制站會以時間日期戳記重新命名該檔案，以防止複製。 伺服器會繼續正常開機。 這是 Windows Server 2012 中的虛擬網域控制站在每次重新開機時的運作方式。  
  
    2.  若兩個識別碼不相符，表示這是新的虛擬機器，其包含來自先前之虛擬網域控制站 (或其還原的快照) 的 NTDS.DIT。 如果 DCCloneConfig.xml 檔案存在，網域控制站會繼續執行複製操作。 如果沒有，它會繼續執行快照還原操作。 請參閱[虛擬網域控制站安全還原架構](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)。  
  
    3.  若 Hypervisor 沒有提供「VM 世代識別碼」供比較，但有 DCCloneConfig.xml 檔案存在，客體會重新命名該檔案，並重新開機進入 DSRM 以避免網路上出現重複的網域控制站。 如果沒有 dccloneconfig.xml 檔案，客體會正常開機 (可能會在網路上產生重複的網域控制站)。 如需有關如何回收此重複網域控制站的詳細資訊，請參閱 Microsoft 知識庫文章 [2742970](https://support.microsoft.com/kb/2742970)。  
  
3.  NTDS 服務會檢查 VDCisCloning DWORD 登錄值名稱 (位在 HKEY_Local_Machine\System\CurrentControlSet\Services\Ntds\Parameters 下) 的值。  
  
    1.  若該值不存在，表示這是第一次嘗試複製此虛擬機器。 客體會實作 VDC 物件重複防護 (將本機 RID 集區判定為無效，並為網域控制站設定新的複寫引動過程識別碼  
  
    2.  若它已設定為 0x1，表示這是先前複製作業失敗而「重試」的複製嘗試。 VDC 物件重複安全措施不被視為必須至少執行過一次，而且將不必要地多次修改客體。  
  
4.  IsClone DWORD 登錄值名稱會被寫入到登錄中 (位在 Hkey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters 下)  
  
5.  NTDS 服務會變更客體的開機旗標，以在未來重新開機時進入 DS 修復模式。  
  
6.  NTDS 服務會嘗試讀取三個可接受位置 (DSA 工作目錄、%windir%\NTDS 或卸除式讀/寫媒體的根目錄 (依磁碟機代號順序)) 其中之一的 DcCloneConfig.xml。  
  
    1.  如果該檔案不存在於任何有效位置中，客體會檢查 IP 位址是否重複。 如果 IP 位址沒有重複，伺服器會正常開機。 如果有重複的 IP 位址，電腦會開機進入 DSRM 以避免網路上出現重複的網域控制站。  
  
    2.  如果該檔案存在於有效位置，NTDS 服務會驗證其設定。 如果該檔案是空白的 (或任何特定設定是空白的)，NTDS 會自動設定那些設定的值。  
  
    3.  如果 DcCloneConfig.xml 存在，但包含任何無效的項目或無法讀取，複製會失敗，且客體會開機進入目錄服務還原模式 (DSRM)。  
  
7.  客體會停用所有 DNS 自動登錄，以防止意外劫持來源電腦名稱與 IP 位址。  
  
8.  客體會停止 Netlogon 服務，以防止通告或回應來自用戶端的網路 AD DS 要求。  
  
9. NTDS 會驗證沒有安裝任何不屬於 DefaultDCCloneAllowList.xml 或 CustomDCCloneAllowList.xml 的服務或程式。  
  
    1.  如果安裝的服務或程式不在預設排除允許清單或自訂排除允許清單中，複製會失敗，且客體會開機進入 DSRM 以避免網路上出現重複的網域控制站。  
  
    2.  如果沒有任何不相容性，複製會繼續執行。  
  
10. 若因為 DCCloneConfig.xml 網路設定為空白而將使用自動 IP 定址，客體會啟用網路介面卡上的 DHCP，以取得 IP 位址租用、網路路由與名稱解析資訊。  
  
11. 客體會找出並連絡執行 PDC 模擬器 FSMO 角色的網域控制站。 此程序使用 DNS 與 DCLocator 通訊協定。 它會建立 RPC 連線，並呼叫 IDL_DRSAddCloneDC 方法以複製網域控制站電腦物件。  
  
    1.  若客體的來源電腦物件持有「允許 DC 建立本身的複製品」的網域最上層節點延伸權限，則複製會繼續執行。  
  
    2.  若客體的來源電腦物件沒有該延伸權限，則複製會失敗，且客體會開機進入 DSRM 以避免網路上出現重複的網域控制站。  
  
12. 若在 DCCloneConfig.xml 中有指定的名稱，AD DS 電腦物件名稱會設定為與其相同，若沒有則會在 PDCE 上自動產生。 NTDS 會為適當的 Active Directory 邏輯站台建立正確的 NTDS 設定物件。  
  
    1.  如果這是 PDC 複製，客體會重新命名本機電腦，然後重新開機。 重新開機之後，它會再次執行步驟 1-10，然後移至步驟13。  
  
    2.  如果這是複本 DC 複製，則此階段不會重新開機。  
  
13. 客體會提供升級設定給開始升級的「DS 角色伺服器」服務。  
  
14. 「DS 角色伺服器」服務會停止所有 AD DS 相關服務 (NTDS、NTFRS/DFSR、KDC、DNS)。  
  
15. 客體會強制 NT5DS (Windows NTP) 的時間與另一部網域控制站同步 (在預設的「Windows 時間服務」階層中，這表示使用 PDCE)。 客體會連絡 PDCE。 所有現有的 Kerberos 票證都會被排清。  
  
16. 客體會將 DFSR 或 NTFRS 服務設定為自動執行。 來賓會刪除所有現有的 DFSR 和 NTFRS 資料庫檔案（預設值： c:\windows\ntfrs 和 c:\system volume information\dfsr\\ *< database_GUID >* ），以便在下次啟動服務時，強制執行 SYSVOL 的非權威同步處理。 稍後當同步開始後，客體不會刪除 SYSVOL 的檔案內容以先植 SYSVOL。  
  
17. 系統會重新命名客體。 客體上的「DS 角色伺服器」服務會開始進行 AD DS 設定 (升級)，並使用現有的 NTDS.DIT 資料庫檔案做為來源，而不是使用位在 c:\windows\system32 的範本資料庫做為來源 (升級程序通常會這樣做)。  
  
18. 客體會與 RID 主機 FSMO 角色持有者連絡，以取得新的 RID 集區配置。  
  
19. 升級程序會建立新的引動過程識別碼，並為複製的網域控制站重新建立 NTDS 設定物件 (不考慮複製，這是使用現有 NTDS.DIT 資料庫進行網域升級的一部分)。  
  
20. NTDS 會從夥伴網域控制站複寫遺失的物件、較新的物件或具有較高之版本的物件。 NTDS.DIT 已包含來源網域控制站離線時的物件，且系統會儘可能使用這些物件以減少連入複寫流量。 系統會填入通用類別目錄分割。  
  
21. DFSR 或 FRS 服務會啟動，且因為資料庫不存在，所以 SYSVOL 會以非權威方式從複寫協力電腦同步。 此程序會重複使用 SYSVOL 資料夾中的現有資料，以減少網路複寫流量。  
  
22. 既然該電腦已具有唯一名稱並已連線到網路，客體會重新啟用 DNS 用戶端登錄。  
  
23. 客體會執行 DefaultDCCloneAllowList.xml <SysprepInformation> 元素指定的 SYSPREP 模組，以刪除對舊電腦名稱與 SID 的參考。  
  
24. 複製升級完成。  
  
    1.  客體會移除 DSRM 開機旗標，因此下次能正常重新開機。  
  
    2.  客體會重新命名 DCCloneConfig.xml 並附加日期時間戳記，使它在下次開機時不會被讀取。  
  
    3.  客體會移除 HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters 下的 VdcIsCloning DWORD 登錄值名稱。  
  
    4.  客體會將 HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters 下的 "VdcCloningDone" DWORD 登錄值名稱設定為 0x1。 Windows 不會使用此值，但會提供給第三方做為標記。  
  
25. 客體會更新自己複製之網域控制站物件的 msDS-GenerationID 屬性，以符合目前的客體「VM 世代識別碼」。  
  
26. 客體會重新啟動。 它現在是標準的通告網域控制站。  
  
## <a name="virtualized-domain-controller-safe-restore-architecture"></a><a name="BKMK_SafeRestoreArch"></a>虛擬網域控制站安全還原架構  
  
### <a name="overview"></a>概觀  
AD DS 依賴 Hypervisor 平台公開稱為「VM 世代識別碼」 的識別碼來偵測虛擬機器的快照還原。 在網域控制站升級期間，AD DS 一開始會將此識別碼的值儲存在其資料庫 (NTDS.DIT)。 當系統管理員從先前的快照還原虛擬機器時，會比較目前來自虛擬機器的「VM 世代識別碼」值與資料庫中的值。 如果兩個值不同，網域控制站會重設「引動過程識別碼」並捨棄 RID 集區，因此可以防止重複使用 USN 或可能會建立重複安全性主體的問題。 有兩種情況可能發生安全還原：  
  
-   當虛擬網域控制站在快照還原 (在它關閉時) 後啟動。  
  
-   當快照在執行中的虛擬網域控制站上還原。  
  
    如果快照中的虛擬網域控制站是暫停而不是關閉的狀態，您需要重新啟動 AD DS 服務以觸發新的 RID 集區要求。 您可以使用 [服務] 嵌入式管理單元或 Windows PowerShell (Restart-Service NTDS -force) 來重新啟動 AD DS 服務。  
  
以下各節詳細說明每個情況中的安全還原。  
  
### <a name="safe-restore-detailed-processing"></a>安全還原的詳細程序  
下列流程圖顯示當虛擬網域控制站在快照還原 (在它關閉時) 後啟動時，安全還原如何進行。  
  
![虛擬化的 DC 架構](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringNormalBoot.png)  
  
1.  當虛擬機器在快照還原後開機，它會因快照還原而有 Hypervisor 主機提供的新「VM 世代識別碼」。  
  
2.  系統會比較來自虛擬機器的新「VM 世代識別碼」與資料庫中的「VM 世代識別碼」。 因為兩個識別碼不相符，它會採用「虛擬化防護」(請參閱前一節中的步驟 3)。 還原完成套用後，設定在其 AD DS 電腦物件上的「VM 世代識別碼」會更新，以符合 Hypervisor 主機所提供的新識別碼。  
  
3.  客體會以下列方式採用「虛擬化防護」：  
  
    1.  將本機 RID 集區判定為無效。  
  
    2.  為網域控制站資料庫設定新的引動過程識別碼。  
  
> [!NOTE]  
> 此部分安全還原程序與複製程序相同。 雖然此程序是關於在虛擬網域控制站在快照還原 (在它關閉時) 後啟動時發生的安全還原，相同的步驟也在複製程序中發生。  
  
下圖顯示當快照在執行中的虛擬網域控制站上還原時，「虛擬化防護」如何防止 USN 復原引起的分歧。  
  
![虛擬化的 DC 架構](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringSnapShotRestore.png)  
  
> [!NOTE]  
> 上圖經過簡化以說明概念。  
  
1.  在時間點 T1，Hypervisor 系統管理員建立虛擬機器 DC1 的快照。 此時 DC1 的 USN 值 (實際為 **highestCommittedUsn**) 為 100、InvocationId (在上圖中以 ID 表示) 的值為 A (實務上為 GUID)。 savedVMGID 值是 DC 之 DIT 檔案中的「VM 世代識別碼」(儲存在 DC 的電腦物件中名為 **msDS-GenerationId**的屬性)。 VMGID 是目前「VM 世代識別碼」的值 (來自虛擬機器驅動程式)。 這個值是由 Hypervisor 所提供。  
  
2.  在稍後的時間點 T2，100 位使用者被新增至此 DC (將使用者視為在 T1 與 T2 時間點間執行更新的範例，這些更新實際上可以是使用者建立、群組建立、密碼更新、屬性更新等等的混合)。 在此範例中，每一個更新都會使用一個唯一的 USN (但實際上使用者建立可能使用多個 USN)。 認可這些更新之前，DC1 會檢查其資料庫中的「VM 世代識別碼」(savedVMGID) 與目前來自驅動程式的值 (VMGID) 是否相同。 兩個值相同 (因為尚未發生復原)，因此更新已被認可，且 USN 提升至 200，表示下次更新時可以使用 USN 201。 InvocationId、savedVMGID 或 VMGID 都沒有變更。 這些更新會在下一個複寫週期複寫至 DC2。 DC2 以 DC1 （A） @USN = 200 來更新它在這裡所表示的高水位線（和**UptoDatenessVector**）。 也就是說，DC2 知道來自 DC1 的所有更新 (透過 USN 200 的 InvocationId A 內容)。  
  
3.  在時間點 T3，在時間點 T1 所建立的快照會套用到 DC1。 DC1 已復原，因此它的 USN 復原到 100，表示它可以從 USN 101 開始使用，來與後續的更新關聯。 但此時 VMGID 的值會與支援「VM 世代識別碼」的 Hypervisor 不同。  
  
4.  此後，當 DC1 執行任何更新時，它會檢查其資料庫中的「VM 世代識別碼」的值 (savedVMGID) 與來自虛擬機器驅動程式的值 (VMGID) 是否相同。 在此例中識別碼不相同，因此 DC1 會推斷這表示復原，並觸發「虛擬化防護」；換句話說，它會重設其 InvocationId (ID = B) 並捨棄 RID 集區 (未顯示在上圖中)。 然後，它會將 VMGID 的新值儲存在其資料庫中，並認可新 InvocationId B 內容中的這些更新（USN 101-250）。在下一個複寫週期中，DC2 在 InvocationId B 的內容中不知道 DC1，因此它會要求 DC1 與 InvocationID B 相關的所有專案。如此一來，在 DC1 上執行的快照集後續的更新將會安全地融合。 此外，DC1 在 T2 時執行的更新組 (這些更新在 DC1 的快照還原後遺失) 會在排定的下一次複寫時間會複寫回 DC1，因為這些更新已被複寫至 DC2 (用連回 DC1 的虛線表示)。  
  
在客體採用「虛擬化防護」後，NTDS 會以非權威方式將 Active Directory 物件差異從夥伴網域控制站複寫回來。 「目的地目錄服務」的最新狀態向量也會一併更新。 接著，客體會同步 SYSVOL：  
  
-   若使用 FRS，客體會停止 NTFRS 服務，並設定 D2 BURFLAGS 登錄值。 接著，它會啟動 NTFRS 服務，此服務會以非權威方式進行複寫，並盡可能地重複使用未變更的 SYSVOL 資料。  
  
-   若使用 DFSR，來賓會停止 DFSR 服務並刪除 DFSR 資料庫檔案（預設位置：%systemroot%\system volume volume information\dfsr\\ *<database GUID>* ）。 接著，它會啟動 DFSR 服務，此服務會以非權威方式進行複寫，並盡可能地重複使用現有且未變更的 SYSVOL 資料。  
  
> [!NOTE]  
> -   如果 Hypervisor 未提供「VM 世代識別碼」供比較，表示 Hypervisor 不支援「虛擬化防護」，客體將以執行 Windows Server 2008 R2 或更早版本之虛擬網域控制站的方式運作。 若嘗試用非夥伴 DC 所見過的上一個最高 USN 啟用複寫，客體會實作 USN 復原隔離保護。 如需有關 USN 復原隔離保護的詳細資訊，請參閱＜ [USN 和 USN 復原](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx)＞。  
  


