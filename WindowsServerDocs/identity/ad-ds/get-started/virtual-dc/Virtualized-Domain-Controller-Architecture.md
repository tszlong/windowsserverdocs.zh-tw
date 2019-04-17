---
ms.assetid: 341614c6-72c2-444f-8b92-d2663aab7070
title: "模擬的網域控制站架構"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ac8b190df065547d82aa431761eb5c00c94a2ad6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-architecture"></a>模擬的網域控制站架構

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題涵蓋模擬的網域控制站複製和安全還原架構。 它顯示處理程序的複製和流程圖還原安全，並提供需程序的每個步驟。  
  
-   [模擬的網域控制站複製架構](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneArch)  
  
-   [模擬的網域控制站安全還原架構](../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)  
  
## <a name="BKMK_CloneArch"></a>模擬的網域控制站複製架構  
  
### <a name="overview"></a>概觀  
模擬的網域控制站複製依賴 hypervisor 平台公開識別字稱為**VM 新一代 ID**來偵測建立一樣。 AD DS 一開始將值此識別碼儲存在資料庫 (NTDS。DIT) 期間網域控制站升級。 時一樣開機時，從一樣 VM 新一代 ID 的目前值比較資料庫中值。 如果有兩個值不同，網域控制站叫用 ID 重設，並捨棄 RID 集區中，進而讓 USN 重新使用或潛在建立重複的安全性原則。 網域控制站然後看起來 DCCloneConfig.xml 檔案的位置提出的執行 「 步驟 3[複製詳細處理](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_CloneProcessDetails)。 如果 smartscreen 發現檔案 DCCloneConfig.xml，它已結束，會被部署為複本，因此為止，利用重新升級現有的 NTDS 複製到做為額外的網域控制站本身提供。從來源媒體複製 DIT 和 SYSVOL 內容。  
  
在混合的環境其中一些 hypervisors 支援 VM-GenerationID 與其他人不，這可能會不小心要在不支援 VM-GenerationID hypervisor 部署複製媒體。 DCCloneConfig.xml 檔案的指示複製 DC 系統管理意圖。 因此，如果時開機，但 VM GenerationID 找到 DCCloneConfig.xml 檔案不提供從主機，到 Directory 服務還原模式 (DSRM) 為防止任何影響的環境中的其餘部分俠複製開機。 複製媒體可以後續移到 hypervisor 支援 VM-GenerationID，且可以重試然後複製。  
  
如果複製媒體部署支援 VM-GenerationID hypervisor 上，但不是提供 DCCloneConfig.xml 檔案，在 DC 偵測 VM-GenerationID 變更其 DIT 和新 VM 從一個，它就會觸發防護功能，可防止 USN 重複使用，以避免重複的 Sid。 不過，複製將不會車載機起始，次要俠才能繼續在同一個做為來源俠身分執行。 應該從網路移除這個次要網域控制站同時最早可能避免環境中的任何不一致。 如需詳細資訊，了解如何回收這個次要網域控制站同時確保更新複寫輸出，、 看到 Microsoft 知識庫文章[2742970](https://support.microsoft.com/kb/2742970)。  
  
### <a name="BKMK_CloneProcessDetails"></a>複製詳細的處理  
下圖顯示架構初始複製操作和複製重試作業。 這些處理程序的稍後本主題中的更多詳細資料所述。  
  
**初始複製作業**  
  
![模擬的俠架構](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_InitialCloningProcess.png)  
  
**複製重試作業**  
  
![模擬的俠架構](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_CloningRetryProcess.png)  
  
下列步驟解釋程序中更多詳細資料：  
  
1.  現有一樣網域控制站在支援 VM 新一代收到 hypervisor 開機時  
  
    1.  此 VM 對其 AD DS 電腦物件升級後設定任何現有 VM 的代 ID 值。  
  
    2.  即使它無效下, 一個電腦建立將會表示該仍然複製，為新 VM 代-ID 不符。  
  
    3.  VM 代 ID 設定 DC 的下一步重新開機之後並不會複寫。  
  
2.  一樣然後讀取 VM 新一代 ID 提供 VMGenerationCounter 驅動程式。 它會比較 VM 新一代 Id 兩種。  
  
    1.  如果 Id 符合，這個新的一樣並不複製將不會繼續執行。 如果 DCCloneConfig.xml 檔案已存在，網域控制站重新命名與的時間日期戳記，以避免複製檔案。 伺服器持續通常會開機。 這是每個重新開機任何 virtual 網域控制站的 Windows Server 2012 中的運作方式。  
  
    2.  如果有兩個 Id 不符合，這是包含 NTDS 新一樣。從先前的網域控制站 DIT （或是還原的快照）。 如果 DCCloneConfig.xml 檔案已存在，電腦會繼續複製作業。 否則，請繼續執行快照還原操作。 查看[擬化檔案網域控制站安全還原架構](../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/../../../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch)。  
  
    3.  如果 hypervisor 不提供比較 VM 新一代 ID 但 DCCloneConfig.xml 檔案，來賓重新命名檔案，然後插入 DSRM 重複的網域控制站保護的網路開機。 如果有任何 dccloneconfig.xml 檔案，來賓開機通常 （可能會重複的網域控制站在網路上） 使用。 如需詳細資訊，了解如何回收此重複的網域控制站，查看 Microsoft 知識庫文章[2742970](https://support.microsoft.com/kb/2742970)。  
  
3.  （在 HKEY_Local_Machine\System\CurrentControlSet\Services\Ntds\Parameters) 登錄 VDCisCloning DWORD 值名稱值檢查 NTDS 服務。  
  
    1.  如果不存在，這是複製這個一樣的第一次嘗試。 客體實作停用本機 RID 集區與設定複製叫用編號新的網域控制站 VDC 物件 20gb 防護功能  
  
    2.  如果已經為 0x1，此為 「 重試 「 複製嘗試，先前的複製作業位置無法。 它們必須已經執行一次之前，將不必要修改來賓多次不會拍攝 VDC 物件重複的安全機制。  
  
4.  （在 Hkey_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters) 寫入登錄 IsClone DWORD 值名稱  
  
5.  NTDS 服務變更來賓開機旗標開始 DS 修復模式中的任何其他重新開機。  
  
6.  朗讀 DcCloneConfig.xml 的三個接受位置 （DSA 運作 Directory、 %windir%\ntds 或讀取/寫入卸除式媒體的磁碟機代號，在磁碟機的根順序） 會嘗試 NTDS 服務。  
  
    1.  如果檔案不存在於有效的任何位置，來賓檢查重複的 IP 位址。 如果不重複的 IP 位址，伺服器開機時一樣。 如果有重複的 IP 位址，電腦開機至 DSRM 重複的網域控制站保護的網路。  
  
    2.  如果有有效的位置，「 NTDS 服務驗證其設定。 如果是空白的檔案 （或任何特定的設定會空白） NTDS 設定自動值這些設定。  
  
    3.  如果 DcCloneConfig.xml 存在，但包含任何不正確的項目或讀取，複製失敗，且來賓開機至 Directory 服務還原模式 (DSRM)。  
  
7.  客體停用所有 DNS 自動-登記以防止誤駭客攻擊來源電腦名稱與 IP 位址。  
  
8.  客體停止為防止任何廣告或從網路 AD DS 要求的回答 Netlogon 服務。  
  
9. NTDS 驗證有不服務或不是 DefaultDCCloneAllowList.xml 或 CustomDCCloneAllowList.xml 的安裝程式  
  
    1.  如果有服務或安裝的程式無法預設排除項目中的允許清單中，自訂排除允許清單中，失敗，且來賓複製到 DSRM 重複的網域控制站保護的網路開機。  
  
    2.  如果有任何不相容，複製持續。  
  
10. 如果因為空白 DCCloneConfig.xml 網路設定時，將會使用自動 IP 位址，來賓可讓 DHCP 上取得 IP 位址租用、 網路路由及名稱解析資訊的網路介面卡。  
  
11. 客體找出並執行肯定 FSMO 角色網域控制站的連絡人。 這會使用 DNS 及 DCLocator 通訊協定。 它可 RPC 連接並呼叫 IDL_DRSAddCloneDC 複製網域控制站電腦物件的方法。  
  
    1.  如果來賓的來源電腦物件保留網域標頭延伸的權限的 「 ' 允許建立自己的複本 DC 」 然後複製進行。  
  
    2.  如果來賓的來源電腦物件不保留延伸權限，複製失敗，且客體的到 DSRM 重複的網域控制站保護的網路開機。  
  
12. AD DS 電腦物件名稱為符合指定 DCCloneConfig.xml，如果有的話，否則會自動導致 PDCE 的名稱。 NTDS 建立適當的 Active Directory 邏輯網站正確 NTDS 設定物件。  
  
    1.  如果這是 PDC 複製，來賓重新命名本機電腦，然後重新開機。 後重新開機，它會再試一次，會透過步驟 1-10，然後移至步驟 13。  
  
    2.  如果這是複本俠複製，還有這個階段不重新開機。  
  
13. 客體提供 「 DS 角色伺服器服務，資料促銷促銷設定。  
  
14. DS 角色伺服器服務會停止的所有 AD DS 相關服務 NTDS、 NTFRS 日 DFSR、 \ [KDC （DNS）。  
  
15. 客體強制 NT5DS (Windows NTP) 時間同步處理的其他網域控制站 （中的預設 Windows 時間服務階層，這表示使用 PDCE）。 客體連絡人 PDCE。 清除所有現有的 Kerberos 門票。  
  
16. 客體設定 DFSR 或 NTFRS 服務會自動執行。 客體刪除所有現有 DFSR 和 NTFRS 資料庫檔案 (預設： c:\windows\ntfrs 和 c:\system 磁碟區 information\dfsr\\*< database_GUID >*)，以時服務接下來會開始強制 SYSVOL 未經授權同步處理。 客體右鍵檔案到 SYSVOL，預先植 SYSVOL 同步處理稍後開始時。  
  
17. 重新命名來賓。 DS 角色伺服器上的服務來賓開始使用現有 NTDS AD DS 設定 （促銷）。做為來源，而不是範本資料庫中 c:\windows\system32 像是促銷通常會包含 DIT 資料庫檔案。  
  
18. 客體連絡人移除主機 FSMO 角色擁有者以取得新的 RID 集區配置。  
  
19. 升級程序會建立新的叫用 ID 並重新建立複製的網域控制站物件 NTDS 設定 （不受影響複製，這是部分的網域促銷使用現有 NTDS。DIT 資料庫）。  
  
20. NTDS 複製遺失、 更新或有較新版本的合作夥伴網域控制站物件。 NTDS。DIT 已經包含的時間來源網域控制站發生] 中的物件和這些為了複寫流量最小化使用盡可能輸入。 填入通用磁碟分割。  
  
21. DFSR 或 FRS 服務開始，因為有資料庫，SYSVOL 非系統授權同步複寫合作夥伴的輸入。 此程序重新使用現有資料，SYSVOL 資料夾，以減少複寫的網路流量。  
  
22. 現在的唯一名為電腦與網路來賓重新可讓 DNS client 登記。  
  
23. 客體執行指定 DefaultDCCloneAllowList.xml SYSPREP 模組<SysprepInformation>以快轉參考 SID 與先前的電腦名稱的項目。  
  
24. 複製升級已完成。  
  
    1.  客體移除 DSRM 開機旗標讓下一步重新開機，才能正常。  
  
    2.  它讀取再試一次在下一次開機，來賓重新命名 DCCloneConfig.xml 附加日期的頻率，使用。  
  
    3.  客體會移除在 HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters 登錄 VdcIsCloning DWORD 值名稱。  
  
    4.  客體設定 「 VdcCloningDone [DWORD 登錄值名稱底下 HKEY_Local_Machine\System\CurrentControlSet\Services\NTDS\Parameters 0x1。 Windows 不會使用此值，但改為其提供做為標記的第三方。  
  
25. 客體更新 msDS-GenerationID 屬性符合目前來賓 VM 新一代收到自己複製的網域控制站物件  
  
26. 客體重新開機。 現在是標準，網域控制站的廣告。  
  
## <a name="BKMK_SafeRestoreArch"></a>模擬的網域控制站安全還原架構  
  
### <a name="overview"></a>概觀  
AD DS 依賴 hypervisor 平台公開識別字稱為**VM 新一代 ID**來偵測一樣的快照還原。 AD DS 一開始將值此識別碼儲存在資料庫 (NTDS。DIT) 期間網域控制站升級。 當系統管理員從先前的快照還原一樣時，目前的一樣 VM 新一代 ID 值比較資料庫中值。 如果有兩個值不同，網域控制站叫用 ID 重設，並捨棄 RID 集區中，進而讓 USN 重新使用或潛在建立重複的安全性原則。 有兩種安全還原可能會發生的案例：  
  
-   當 virtual 網域控制站會開始快照已關機時還原之後  
  
-   當快照會還原執行 virtual 網域控制站  
  
    如果模擬的網域控制站在快照暫停的狀態，而不是關機時，您需要重新開機 AD DS 服務觸發新 RID 集區的要求。 您可以使用 [服務] 嵌入式管理單元，或使用 Windows PowerShell 來重新開機 AD DS 服務 (重新開機服務 NTDS-強制)。  
  
下列章節解釋安全還原中的每個案例的詳細資料。  
  
### <a name="safe-restore-detailed-processing"></a>安全還原詳細的處理  
以下流程圖顯示如何安全還原時 virtual 網域控制站開始快照已關機時還原之後，就會發生。  
  
![模擬的俠架構](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringNormalBoot.png)  
  
1.  一樣開機時向上快照還原之後，但是不會有新 VM 新一代 ID 提供 hypervisor 主機因為快照還原。  
  
2.  從一樣新 VM 新一代 ID 是相較於 VM 新一代 ID 資料庫中。 有兩個 Id 不符合，因為它會使用模擬防護功能 （看到執行 「 步驟 3 一節中）。 還原完成套用之後，以符合新的更新設定其 AD DS 電腦物件 VM GenerationID ID 提供 hypervisor 主機。  
  
3.  客體會使用透過模擬防護功能：  
  
    1.  停用本機 RID 集區。  
  
    2.  設定網域控制站資料庫新叫用來電的顯示。  
  
> [!NOTE]  
> 這部分的安全還原和重疊複製程序。 此程序後關於 virtual 網域控制站安全還原它開機時下列快照還原，雖然相同的步驟發生這種複製程序。  
  
下圖顯示如何模擬保護避免分歧快照還原執行 virtual 網域控制站時 USN 復原，導致。  
  
![模擬的俠架構](media/Virtualized-Domain-Controller-Architecture/ADDS_VDC_VirtualizationSafeguardsDuringSnapShotRestore.png)  
  
> [!NOTE]  
> 簡化上圖解釋概念。  
  
1.  時間 T1，hypervisor 系統管理員必須具備 virtual DC1 的快照。 此時 DC1 有 USN 值 (**highestCommittedUsn**實際上) 的 A 100，呼叫識別碼 （以 ID 在上圖中表示） 值 （實際上這是 GUID）。 SavedVMGID 價值，是 VM GenerationID DC 的 DIT 檔案 (儲存對電腦物件的屬性名中 DC **msDS-GenerationId**)。 VMGID 是目前的可用一樣驅動程式從 VM-GenerationId 值。 透過 hypervisor 提供這個值。  
  
2.  稍後 T2，此 dc 增加 100 使用者 (考慮使用者的更新，可能會在這個網域控制站之間執行範例時間 T1 和 T2; 這些更新實際上是混合使用者作品、 群組作品、 密碼的更新、 屬性更新等等)。 在此範例中，每個更新會消耗一個唯一 USN （但實際上使用者建立可能會消耗 USN 以上）。 執行這些更新之前, DC1 會檢查是否 VM-GenerationID 的值 (savedVMGID) 其資料庫中目前可用的驅動程式 (VMGID) 值相同。 相同時, 才不復原發生，請更新會致力和 USN 移到 200 指出下一次更新，可以使用 USN 201。 不還有呼叫識別碼、 savedVMGID 或 VMGID 任何變更。 這些更新複寫出 DC2 在下一步複寫循環。 DC2 更新時高桌面浮水印 (和**UptoDatenessVector**) 表示此處 DC1(A) 為@USN= 200。 也就是 DC2 目前正在設法 dc1 透過 USN 200 呼叫識別碼 A 環境中的所有更新。  
  
3.  時間 T3，掃瞄次 T1 適用於 DC1。 DC1 已復原，因此其 USN 回復到 100，表示它還可以使用 Usn 101 與後續的更新。 不過，此時 VMGID 的值為 hypervisors 支援 VM-GenerationID 在不同。  
  
4.  接下來，當 DC1 執行任何更新，它會檢查是否 VM GenerationId 在其資料庫 (savedVMGID) 的值為一樣驅動程式 (VMGID) 值相同。 若是如此，並不相同，讓 DC1 推斷這表示回復，以和觸發模擬保護措施;亦即，它會重設為呼叫識別碼 (ID = B) 並捨棄 RID 集區 （不會顯示在上圖中）。 它在其資料庫中儲存的 VMGID 新值，然後認可 (USN 101-250) 這些更新的部分新呼叫識別碼 b。在下一步複寫循環 DC2 完全不知道 dc1 的部分呼叫識別碼 B，讓它 DC1 呼叫識別碼 B.相關聯的所有項目要求如此一來，將會安全地涵蓋 DC1 上執行之後快照的應用程式的更新。 此外，在 T2 DC1 上執行 （，已遺失 DC1 在之後的開發進程的快照還原） 的更新設定想複製到下一個已排程的複寫 DC1 因為他們已複寫到 DC2 （（如同指示） 回到 DC1 點列）。  
  
來賓運用模擬保護措施之後，NTDS 會複寫 Active Directory 物件不同輸入非系統授權合作夥伴網域控制站。 最新的向量的目的地 directory 服務會隨之更新。 客體同步 SYSVOL:  
  
-   如果使用 FRS，來賓停止 NTFRS 的服務，並設定 D2 BURFLAGS 登錄值。 接著會開始非系統授權複寫輸入、 重新使用現有不變的 SYSVOL 資料可能的話，NTFRS 服務。  
  
-   如果使用 DFSR，來賓停止 DFSR 的服務，刪除 DFSR 資料庫檔案 (預設位置： %systemroot%\system 磁碟區 information\dfsr\\*<database GUID>*)。 接著會開始非系統授權複寫輸入、 重新使用現有不變的 SYSVOL 資料時可能 DFSR 服務。  
  
> [!NOTE]  
> -   Hypervisor 不比較 VM 新一代 ID 提供，如果 hypervisor 不支援模擬防護和來賓將操作等模擬的網域控制站執行 Windows Server 2008 R2 或更早版本。 客體實作 USN 復原隔離保護嘗試開始，已經不進階的合作夥伴俠看見一個最高 USN 過去 usn 複寫是否。 如需 USN 復原隔離保護的詳細資訊，請查看[USN 和 USN 復原](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx)  
  


