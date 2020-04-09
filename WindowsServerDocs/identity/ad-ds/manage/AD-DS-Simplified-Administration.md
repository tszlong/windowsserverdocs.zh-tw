---
ms.assetid: f74eec9a-2485-4ee0-a0d8-cce01250a294
title: AD DS 簡化的系統管理
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e1989630cadd7d63f8ed041174135722d568484f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824421"
---
# <a name="ad-ds-simplified-administration"></a>AD DS 簡化的系統管理

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題說明 Windows Server 2012 網域控制站部署和管理的功能和優點，以及先前的作業系統 DC 部署和新的 Windows Server 2012 執行之間的差異。  
  
Windows Server 2012 引進新一代的 Active Directory Domain Services 簡化的系統管理，而且是自 Windows 2000 伺服器以來最基本的網域重新構想。 AD DS 簡化的系統管理參考 Active Directory 十二年的經驗，為架構設計人員和系統管理員提供更耐久、更有彈性、更直覺的系統管理經驗。 這意味著以現有技術建立新版本，以及擴充 Windows Server 2008 R2 中所發行元件的功能。  
  
AD DS 簡化的系統管理是網域部署的重新構思。  
  
- AD DS 角色部署現在是新伺服器管理員架構的一部分，並允許遠端安裝。  
- AD DS 部署和設定引擎現在是 Windows PowerShell，即使使用新的 AD DS 設定精靈也一樣。  
- 在升級網域控制站的過程中，會自動進行結構描述延伸、樹系準備及網域準備工作，而且不再需要於特殊的伺服器 (如架構主機) 上執行特殊工作。  
- 升級現在包含先決條件檢查，可驗證樹系和網域對新網域控制站的整備度，以減少升級失敗的機會。  
- Windows PowerShell 的 Active Directory 模組現在包含複寫拓撲管理、 動態存取控制及其他操作的 Cmdlet。  
- Windows Server 2012 樹系功能等級不會實作新功能，而且只有新的 Kerberos 功能子集需要網域功能等級，系統管理員就不會經常需要同質性的網域控制站環境。  
- 新增了對虛擬網域控制站的完整支援，以包含自動化的部署和復原保護。  
   - 如需虛擬網域控制站的詳細資訊，請參閱[Active Directory Domain Services &#40;AD DS&#41;虛擬&#40;化層&#41;級100的簡介](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md)。

此外，還有許多系統管理和維護改良功能：  

- Active Directory 管理中心包含圖形化 Active Directory 資源回收筒、更細緻的密碼原則管理和 Windows PowerShell 歷程記錄檢視器。
- 新的伺服器管理員有 AD DS 特定的介面，可提供效能監視、最佳做法分析、重要服務以及事件記錄檔。  
- 群組受管理的服務帳戶可支援多部使用相同安全性主體的電腦。  
- 成熟的 Active Directory 網域中相關的識別元 (RID) 發行和監視的改良功能，可提供更好的管理性。  

從 Windows Server 2012 中包含的其他新功能 AD DS 收益，例如：  

- NIC 小組和資料中心橋接  
- DNS 安全性和開機後能更快速地使用 AD 整合區域  
- Hyper-V 的可靠性與延展性改進功能  
- BitLocker 網路解除鎖定  
- 其他 Windows PowerShell 元件管理模組  

## <a name="adprep-integration"></a>ADPREP 整合

Active Directory 樹系結構描述延伸和網域準備現已整合到網域控制站設定程序。 如果您將新的網域控制站升級到現有的樹系，程序會偵測升級狀態，並自動進行結構描述延伸和網域準備階段。 安裝第一個 Windows Server 2012 網域控制站的使用者必須仍是 Enterprise Admin 與 Schema Admin，或是提供有效的替代認證。  
  
Adprep.exe 保留在 DVD 上是為了個別的樹系與網域準備。 隨附於 Windows Server 2012 的工具版本可相容於 Windows Server 2008 x64 與 Windows Server 2008 R2。 Adprep.exe 也支援遠端 forestprep 及 domainprep，就像 ADDSDeployment 型的網域控制站設定工具一樣。  
  
如需 Adprep 與先前作業系統樹系準備的詳細資訊，請參閱 [執行 Adprep (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd464018(WS.10).aspx)。  

## <a name="server-manager-ad-ds-integration"></a>伺服器管理員 AD DS 整合

![簡化的系統管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_Dashboard.png)  
  
伺服器管理員是伺服器管理工作的集線器。 其儀表板樣式外觀會定期重新整理已安裝角色和遠端伺服器群組的檢視。 伺服器管理員提供本機與遠端伺服器的集中管理，而且不需存取主控台。  
  
Active Directory Domain Services 是其中一個中樞角色;藉由在網域控制站或 Windows 8 上的遠端伺服器管理工具上執行伺服器管理員，您會看到樹系中網域控制站上最近的重要問題。  
  
這些檢視包括：  
  
- 伺服器可用性  
- 高 CPU 和記憶體使用量的效能監視器警示  
- AD DS 特定的 Windows 服務的狀態  
- 事件記錄檔中最近的目錄服務相關警告與錯誤項目  
- 根據一組 Microsoft 建議規則的網域控制站最佳做法分析  

## <a name="active-directory-administrative-center-recycle-bin"></a>Active Directory 管理中心資源回收筒

![簡化的系統管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_ADAC.png)  
  
Windows Server 2008 R2 引進 Active Directory 資源回收筒，可復原已刪除的 Active Directory 物件，而不需要從備份還原、重新啟動 AD DS 服務，或重新啟動網域控制站。  
  
Windows Server 2012 增強了現有的 Windows PowerShell 還原功能，在 Active Directory 管理中心有全新的圖形化介面。 這可讓系統管理員啟用資源回收筒，並在樹系的網域內容中找出或還原已刪除的物件，而不需要直接執行 Windows PowerShell Cmdlet。 Active Directory 管理中心和 Active Directory 資源回收筒仍然使用 Windows PowerShell，因此，前一個指令碼與程序仍相當重要。  
  
如需 Active Directory 資源回收筒的詳細資訊，請參閱 [Active Directory 資源回收筒逐步指南 (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd392261(WS.10).aspx)。  
  
## <a name="active-directory-administrative-center-fine-grained-password-policy"></a>Active Directory 管理中心更細緻的密碼原則

![簡化的系統管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_FGPP.png)  
  
Windows Server 2008 中引進更細緻的密碼原則，可讓系統管理員依網域設定多個密碼及帳戶鎖定原則。 這可讓網域依據使用者和群組，彈性選擇強制執行較嚴格或較寬鬆的密碼規則。 它有沒有管理介面，需要系統管理員使用 Ldp.exe 或 Adsiedit.msc 來設定。 Windows Server 2008 R2 中引進 Windows PowerShell 的 Active Directory 模組，為系統管理員提供使用 FGPP 的命令列介面。  
  
Windows Server 2012 則引進更細緻的密碼原則的圖形化介面。 Active Directory 管理中心是這個新對話方塊的首頁，為所有系統管理員提供簡化的 FGPP 管理。  
  
如需有關更細緻的密碼原則之資訊，請參閱 [AD DS 更細緻的密碼與帳戶鎖定原則逐步指南 (Windows Server 2008 R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx)。  
  
## <a name="active-directory-administrative-center-windows-powershell-history-viewer"></a>Active Directory 管理中心 Windows PowerShell 歷程記錄檢視器

![簡化的系統管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_HistoryViewer.png)  
  
Windows Server 2008 R2 引進 Active Directory 管理中心，取代 Windows 2000 中舊版的 Active Directory 使用者和電腦嵌入式管理單元。 Active Directory 管理中心會建立一個圖形化的管理介面到 Windows PowerShell 的新 Active Directory 模組。  
  
由於 Active Directory 模組包含 100 多個 Cmdlet，系統管理員可能並不容易學習。 Windows PowerShell 大量整合到 Windows 系統管理策略，因此，Active Directory 管理中心現在包含一個檢視器，可讓您在圖形化介面中查看 Cmdlet 執行狀況。 您可以透過一個簡單的介面來搜尋、複製、清除歷程記錄以及新增附註。 目的是讓系統管理員使用圖形化介面來建立和修改物件，然後在歷程記錄檢視器中檢閱它們，以深入了解 Windows PowerShell 指令碼處理和修改範例。  

## <a name="ad-replication-windows-powershell"></a>AD 複寫 Windows PowerShell

![簡化的系統管理](media/AD-DS-Simplified-Administration/ADDS_PSNewADReplSite.png)  
  
Windows Server 2012 新增額外的 Active Directory 複寫 Cmdlet 到 Active Directory Windows PowerShell 模組。 它們可用來設定新的或現有的站台、子網路、連線、站台連結與橋接器。 它們也會傳回 Active Directory 複寫中繼資料、複寫狀態、佇列和最新版向量資訊。 複寫 Cmdlet 的引進，再加上部署和其他現有的 AD DS Cmdlet，讓您只要使用 Windows PowerShell 即可管理樹系。 這使得想要佈建和管理 Windows Server 2012 的系統管理員不需使用圖形化介面即能達成目的，並能減少作業系統的攻擊面和服務的需求。 將伺服器部署到高安全性的網路 (例如機密網際網路通訊協定路由器 (SIPR) 與公司 DMZ) 時，這一點尤其重要。  
  
如需 AD DS 站台拓撲和複寫的詳細資訊，請參閱 [Windows Server 技術參照](https://technet.microsoft.com/library/cc739127(WS.10).aspx)。  

## <a name="rid-management-and-issuance-improvements"></a>RID 管理及發行的改良功能

Windows 2000 Active Directory 引進 RID 主機，其可將相關的識別元集區發行到網域控制站，以建立安全性信任者 (如使用者、群組及電腦) 的安全性識別碼 (SID)。  根據預設，這個全域的 RID 空間大小限制為 2<sup>30</sup> (或網域中共建立 1,073,741,823 個 SID) 。 SID 無法傳回集區或重新發行。 經過一段時間，大型網域的 RID 可能會開始不足，或意外可能導致不必要的 RID 消耗，最後終究匱乏。  
  
Windows Server 2012 可解決自 1999 年首次建立 Active Directory 網域以來，隨著 AD DS 成熟而有許多客戶和 Microsoft 客戶支援未發現的 RID 發行與管理問題。 這些地方包括：  

- 定期的 RID 消耗警告會寫入事件記錄檔  
- 當系統管理員使 RID 集區失效時會產生事件記錄檔  
- 現在會強制執行 RID 原則 RID 區塊大小的最大限度  
- 當全域 RID 空間不足時，現在會強制執行人為設定的 RID 最大限度並做記錄，讓系統管理員可在全域空間耗盡之前採取動作
- 全域 RID 空間現在可以增加 1 個位元，使大小增加為原本的 2 倍：2<sup>31</sup> (2147483648 個 SID)  

如需有關 RID 與 RID 主機的詳細資訊，請檢閱 [安全性識別碼的運作方式](https://technet.microsoft.com/library/cc778824(WS.10).aspx)。  
  
## <a name="ad-ds-role-deployment-and-management-architecture"></a>AD DS 角色部署和管理架構

伺服器管理員和 ADDSDeployment Windows PowerShell 部署或管理 AD DS 角色時，依賴下列功能核心組件：  

- Microsoft.ADroles.Aspects.dll  
- Microsoft.ADroles.Instrumentation.dll  
- Microsoft.ADRoles.ServerManager.Common.dll  
- Microsoft.ADRoles.UI.Common.dll  
- Microsoft.DirectoryServices.Deployment.Types.dll  
- Microsoft.DirectoryServices.ServerManager.dll  
- Addsdeployment.psm1  
- Addsdeployment.psd1  

同時依賴 Windows PowerShell 和其遠端叫用命令，以執行遠端角色安裝和設定。  

![簡化的系統管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_DepDLLs.png)  

Windows Server 2012 也會將出自 LSASS.EXE 的一些先前的升級作業重構，以做為下列的一部分：  

- DS 角色伺服器服務 (DsRoleSvc)  
- DSRoleSvc.dll (由 DsRoleSvc 服務載入)  

此服務必須存在並且正在執行，才能夠升級、降級或複製虛擬網域控制站。 AD DS 角色安裝會新增這項服務，且預設會將啟動類型設為「手動」。 請勿停用此服務。  

## <a name="adprep-and-prerequisite-checking-architecture"></a>ADPrep 與先決條件檢查架構

Adprep 不再需要於架構主機上執行。 它可以從執行 Windows Server 2008 x64 或更新版本的電腦遠端執行。  
  
> [!NOTE]  
> Adprep 使用 LDAP 匯入 Schxx.ldf 檔案，如果匯入過程中與架構主機的連線中斷，並不會自動重新連線。 在匯入過程中，架構主機是設定在特定的模式，並且會停用自動重新連線功能，因為如果 LDAP 在斷線後重新連線，重新建立的連線不會設定在特定的模式。 在此情況下，架構無法正確更新。  
  
先決條件檢查可確保某些條件是相符的。 如果要順利安裝 AD DS 安裝，這些條件是必要的。 如果某些必要條件不相符，就可以在繼續安裝之前先解決。 它也會偵測樹系或網域是否尚未準備就緒，讓 Adprep 部署程式碼可以自動執行。  

### <a name="adprep-executables-dlls-ldfs-files"></a>ADPrep 可執行檔、DLL、LDF、檔案

- ADprep.dll  
- Ldifde.dll  
- Csvde.dll  
- Sch14.ldf - Sch56.ldf  
- Schupgrade.cat  
- *dcpromo.csv  

先前在 ADprep.exe 中的 AD 準備程式碼已重構為 adprep.dll。 這可讓 ADPrep.exe 和 ADDSDeployment Windows PowerShell 模組使用媒體櫃執行相同的工作，並具備相同的功能。 Adprep.exe 隨附於安裝媒體中，但自動化程序不會直接呼叫它 - 只有系統管理員可以手動執行。 它只能在 Windows Server 2008 x 64 及更新版本的作業系統上執行。 Ldifde.exe 和 csvde.exe 也有 DLL 格式的重構版本，由準備程序載入。 結構描述延伸仍會使用簽章驗證的 LDF 檔案，就像在先前的作業系統版本中一樣。  
  
![簡化的系統管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_AdprepDLLs.png)  
  
> [!IMPORTANT]  
> Windows Server 2012 沒有 32 位元的 Adprep32.exe 工具。 您必須至少擁有一部 Windows Server 2008 x64、Windows Server 2008 R2 或 Windows Server 2012 電腦執行為網域控制站、成員伺服器或在工作群組中執行，才能夠準備樹系和網域。 Adprep.exe 無法在 Windows Server 2003 x64 的作業系統上執行。  
  
## <a name="prerequisite-checking"></a><a name="BKMK_PrereuisiteChecking"></a>先決條件檢查

ADDSDeployment Windows PowerShell Managed 程式碼內建的先決條件檢查系統會依據作業而有不同的運作模式。 下表描述當其使用時的每個測試，並說明其驗證方式與內容。 如果有驗證失敗但錯誤不足以疑難排解問題的情況，這些表格可能會很有用。  
  
這些測試記錄在工作類別 **Core** 底下的 **DirectoryServices-Deployment**作業事件記錄檔通道中，事件識別碼一律為 **103**。  
  
### <a name="prerequisite-windows-powershell"></a>先決條件 Windows PowerShell

所有網域控制站部署 Cmdlet 都有適用的 ADDSDeployment Windows PowerShell Cmdlet。 它們與相關的 Cmdlet 有幾乎相同的引數。  

- Test-ADDSDomainControllerInstallation  
- Test-ADDSDomainControllerUninstallation  
- Test-ADDSDomainInstallation  
- Test-ADDSForestInstallation  
- Test-ADDSReadOnlyDomainControllerAccountCreation  

通常不需要執行這些 Cmdlet，根據預設，它們已隨著部署 Cmdlet 自動執行。  

#### <a name="prerequisite-tests"></a><a name="BKMK_ADDSInstallPrerequisiteTests"></a>先決條件測試

||||  
|-|-|-|  
|測試名稱|通訊協定<p>已使用|說明與附註|  
|VerifyAdminTrusted<p>ForDelegationProvider|LDAP|驗證您在現有的協力廠商網域控制站上具有 [讓電腦及使用者帳戶受信賴，以進行委派] (SeEnableDelegationPrivilege) 權限。 這需要您所建構的 tokenGroups 屬性的存取權。<p>在連線 Windows Server 2003 網域控制站時不會使用。 在升級之前，您必須手動確認此權限。|  
|VerifyADPrep<p>先決條件 (樹系)|LDAP|使用 rootDSE namingContexts 屬性和結構描述命名內容 fsmoRoleOwner 屬性探索並連線架構主機。 判斷 AD DS 安裝需要哪些準備作業 (forestprep、domainprep 或 rodcprep)。 驗證結構描述 objectVersion 是否為預期的值，及其是否需要進一步的延伸。|  
|VerifyADPrep<p>先決條件 (網域和 RODC)|LDAP|使用 rootDSE namingContexts 屬性和基礎架構容器 fsmoRoleOwner 屬性探索並連線基礎架構主機。 如果是 RODC 安裝，這項測試會探索網域命名主機並確定其在線上。|  
|CheckGroup<p>成員資格|LDAP、<p>RPC over SMB (LSARPC)|視作業而定，驗證使用者是否為 Domain Admins 或 Enterprise Admins 群組的成員 (新增或降級網域控制站為 DA，新增或移除網域為 EA)|  
|CheckForestPrep<p>GroupMembership|LDAP、<p>RPC over SMB (LSARPC)|驗證使用者是否為 Schema Admins 和 Enterprise Admins 群組的成員，而且對現有的網域控制站具有管理稽核及安全性事件記錄檔 (SesScurityPrivilege) 的權限|  
|CheckDomainPrep<p>GroupMembership|LDAP、<p>RPC over SMB (LSARPC)|驗證使用者是否為 Domain Admins 群組的成員，而且對現有的網域控制站具有管理稽核及安全性事件記錄檔 (SesScurityPrivilege) 的權限|  
|CheckRODCPrep<p>GroupMembership|LDAP、<p>RPC over SMB (LSARPC)|驗證使用者是否為 Enterprise Admins 群組的成員，而且對現有的網域控制站具有管理稽核及安全性事件記錄檔 (SesScurityPrivilege) 的權限|  
|VerifyInitSync<p>AfterReboot|LDAP|透過在 rootDSE 屬性 becomeSchemaMaster 設定虛擬值，以驗證架構主機自重新啟動後是否至少複寫過一次|  
|VerifySFUHotFix<p>已套用|LDAP|驗證現有的樹系架構未包含 OID 為 1.2.840.113556.1.4.7000.187.102 的 UID 屬性的已知問題 SFU2 延伸<p>（[https://support.microsoft.com/kb/821732](https://support.microsoft.com/kb/821732)）|  
|VerifyExchange<p>SchemaFixed|LDAP、WMI、DCOM、RPC|驗證現有的樹系架構是否仍未包含問題 Exchange 2000 延伸模組-Ms-exch-assistant-name-Assistant-Name、Ms-exch-assistant-name-LabeledURI 和 ms-chap-Ms-exch-assistant-name-內部識別碼（[https://support.microsoft.com/kb/314649](https://support.microsoft.com/kb/314649)）|  
|VerifyWin2KSchema<p>一致性|LDAP|驗證現有的樹系架構是否有一致 (未經其他廠商不當修改) 的核心屬性及類別。|  
|DCPromo|DRSR over RPC、<p>LDAP、<p>DNS<p>RPC over SMB (SAMR)|驗證命令列語法已傳送到升級程式碼和測試升級。 如果是新建樹系或網域，驗證其是否尚未存在|  
|VerifyOutbound<p>ReplicationEnabled|LDAP、DRSR over SMB、RPC over SMB (LSARPC)|檢查 NTDS 設定物件的選項屬性是否為 NTDSDSA_OPT_DISABLE_OUTBOUND_REPL (0x00000004)，以驗證指定為複寫協力電腦的現有網域控制站是否已啟用連出複寫|  
|VerifyMachineAdmin<p>Password|DRSR over RPC、<p>LDAP、<p>DNS<p>RPC over SMB (SAMR)|驗證為 DSRM 設定的安全模式密碼符合網域複雜性需求。|  
|VerifySafeModePassword|*不適用*|驗證設定的本機系統管理員密碼符合電腦的安全性原則複雜性需求。|  
