---
ms.assetid: f74eec9a-2485-4ee0-a0d8-cce01250a294
title: "AD DS 簡化管理"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6232e281c47f3b5b4627bc9d8ccf53269aafc390
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-simplified-administration"></a>AD DS 簡化管理

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題解釋的新功能和 Windows Server 2012 網域控制站部署及管理和之前的作業系統俠部署新的 Windows Server 2012 實作不同的好處。  
  
Windows Server 2012 導入下一代 Active Directory Domain 服務簡化管理的而且最根本網域重新構想自 Windows 2000 Server。 AD DS 簡化管理拍下全家 Active directory 12 年來，並讓更多可支援、更具彈性、更直覺管理體驗 architects 和系統管理員。 這是建立現有的技術，以及擴充功能的元件在 Windows Server 2008 R2 推出的最新版本。  
  
AD DS 簡化管理是 reimagining 網域部署。  
  
-   AD DS 角色部署現在已經成為伺服器管理員架構新的一部分，並允許遠端安裝  
  
-   AD DS 部署和設定引擎時，現在 Windows PowerShell，甚至使用新的 AD DS 設定精靈  
  
-   架構延伸模組、樹系準備，網域準備就會自動網域控制站升級的一部分並不需要特殊伺服器例如架構主機上不同的工作  
  
-   立即升級包含必要條件查看確認新的網域控制站，降低失敗促銷活動的機會樹系和網域整備  
  
-   Windows PowerShell 中的 active Directory 模組現在包含 cmdlet 複寫拓撲管理、動態存取控制和其他作業  
  
-   Windows Server 2012 樹系層級尚未實作新功能和網域功能等級為僅針對新 Kerberos 功能，減輕常用的系統管理員子集需要的功能需要質網域控制站環境  
  
-   新增擬化檔案網域控制站，包含自動化的部署和復原保護完整支援  
  
如需有關模擬的網域控制站的詳細資訊，請查看[Active Directory Domain Services 和 #40; 簡介 AD DS 和 #41;模擬與 #40;層級 100 和 #41;](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).  
  
除此之外，有許多系統及維護改良功能：  
  
-   Active Directory 管理中心包含的圖形 Active Directory 資源回收筒]、Fine-Grained 密碼原則管理及 Windows PowerShell 歷史檢視器  
  
-   新的伺服器管理員已監視效能、最佳做法分析、重要的服務，以及事件登 AD DS 特定介面  
  
-   群組管理服務帳號支援多部電腦使用相同的安全性原則  
  
-   中相關識別碼 (RID) 發行及監視好性成熟 Active Directory 網域中的改進  
  
最後，AD DS 獲利包含 Windows Server 2012，其他新功能：  
  
-   NIC 小組與資料中心橋接  
  
-   DNS 安全性和之後開機更快的廣告整合區域可用性  
  
-   HYPER-V 可靠性和延展性改良功能  
  
-   BitLocker 網路解除鎖定  
  
-   其他 Windows PowerShell 元件管理模組  
  
## <a name="technical-overview"></a>技術概觀  
  
### <a name="adprep-integration"></a>ADPREP 整合  
Active Directory 森林架構擴充功能，以及網域準備現在整合網域控制站設定程序。 如果您將新的網域控制站升級現有的樹系插入，程序偵測到的升級狀態並架構擴充功能，以及網域準備工作階段將會自動。 安裝的第一個 Windows Server 2012 網域控制站使用者必須仍然企業管理和架構管理員或提供有效的替代認證。  
  
Adprep.exe 會保留在不同的樹系和網域準備 DVD。 隨附 Windows Server 2012 版本是工具的 Windows Server 2008 x64 和 Windows Server 2008 R2 回溯相容性。 Adprep.exe 也支援遠端 forestprep 及準備網域，就像 ADDSDeployment 根據網域控制站設定工具。  
  
如 Adprep 和先前的作業系統樹系準備有關，請查看[執行 Adprep (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd464018(WS.10).aspx)。  
  
### <a name="server-manager-ad-ds-integration"></a>伺服器管理員 AD DS 整合  
![簡化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_Dashboard.png)  
  
伺服器管理員做為中樞伺服器管理工作。 儀表板樣式外觀定期重新整理] 檢視已安裝的角色，以及遠端伺服器的群組。 伺服器管理員提供的集中的管理本機與遠端伺服器，而不需要主機存取。  
  
Active Directory Domain Services 是一個中樞角色。執行伺服器管理員網域控制站或遠端伺服器管理工具，在 Windows 8，您會看到重要最近問題網域控制站在您的樹系上。  
  
這些檢視包括：  
  
-   伺服器的可用性  
  
-   效能監視器警示高 CPU 和記憶體使用量  
  
-   AD DS 特定 Windows 服務的狀態  
  
-   最近 Directory 服務相關的警告與錯誤中的項目事件登入  
  
-   最佳做法分析網域控制站針對一組 Microsoft 建議規則  
  
### <a name="active-directory-administrative-center-recycle-bin"></a>系統管理員中心的 active Directory 資源回收筒  
![簡化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_ADAC.png)  
  
Windows Server 2008 R2 引進了 Active Directory 資源回收桶，而不需要從備份還原、重新 AD DS 服務，或重新開機一次網域控制站復原刪除 Active Directory 物件的。  
  
Windows Server 2012 美化 Active Directory 管理中心中新的圖形介面現有 Windows PowerShell 型還原功能。 這可讓系統管理員讓資源回收筒]，尋找或還原刪除在之子-森林，但不直接執行 Windows PowerShell cmdlet 所有網域環境中的物件。 Active Directory 管理中心和 Active Directory 資源回收桶仍然使用 Windows PowerShell 在保護蓋，使仍然寶貴先前的指令碼與程序。  
  
如需有關 Active Directory 資訊[資源回收筒]，查看 Active Directory 資源回收桶 Step-by-Step 指南 (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd392261(WS.10).aspx)。  
  
### <a name="active-directory-administrative-center-fine-grained-password-policy"></a>Active Directory 系統管理員中心精細密碼原則  
![簡化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_FGPP.png)  
  
Windows Server 2008 導入了 Fine-Grained 密碼原則，可讓系統管理員設定多個密碼及 account 鎖定原則每個網域。 這可讓網域彈性方案執行更多或較少限制根據使用者和群組的密碼規則。 它已有任何管理介面及使用 Ldp.exe 或 Adsiedit.msc 設定所需的系統管理員。 Windows Server 2008 R2 的 Windows PowerShell，授與系統管理員 FGPP 命令列介面引進 Active Directory 模組。  
  
Windows Server 2012 帶來圖形介面 Fine-Grained 密碼的原則。 Active Directory 管理中心為主要的這個新的對話方塊，讓簡化的 FGPP 管理所有的系統管理員。  
  
有關 Fine-Grained 密碼原則，請查看[AD DS Fine-Grained 密碼，以及 Account 鎖定原則 Step-by-Step 指南 (Windows Server 2008 R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx)。  
  
### <a name="active-directory-administrative-center-windows-powershell-history-viewer"></a>Active Directory 系統管理員中心 Windows PowerShell 歷史檢視器  
![簡化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_HistoryViewer.png)  
  
Windows Server 2008 R2 引進了 Active Directory 管理中心，取代較舊的 Active Directory 使用者和電腦嵌入式管理單元建立 Windows 2000 的。 Active Directory 管理中心建立新然後 Active Directory 模組管理圖形介面的 Windows PowerShell。  
  
Active Directory 模組包含數百 cmdlet 上，同時可能陡峭的系統管理員的身分學習。 Windows PowerShell 經驗整合到 Windows 管理的策略，因為 Active Directory 管理中心現在包含檢視器，可讓您查看 cmdlet 執行中的圖形介面。 您可以搜尋、複製、清除歷史，以及加上的筆記與簡單介面。 用意是系統管理員可以使用的圖形介面建立及修改物件，並再會面深入了解 Windows PowerShell 指令碼，並修改範例歷史檢視器中。  
  
### <a name="ad-replication-windows-powershell"></a>廣告複寫 Windows PowerShell  
![簡化的管理](media/AD-DS-Simplified-Administration/ADDS_PSNewADReplSite.png)  
  
Windows Server 2012 新增額外的 Active Directory 複寫 cmdlet Active Directory Windows PowerShell 模組。 這些允許設定新的或現有的網站、子網路、連接、網站的連結和橋樑。 它們也會傳回 Active Directory 複寫中繼資料、複寫狀態，佇列，和最新版本向量資訊。 複寫 cmdlet-加上部署及其他現有的 AD DS cmdlet-導入可讓您可以使用 Windows PowerShell 只樹系的管理。 這會建立新的系統管理員想要提供，以及圖形介面，然後減少作業系統的攻擊 surface 不管理 Windows Server 2012 和維護需求的機會。 伺服器部署到密碼網際網路通訊協定路由器 (SIPR) 和公司 Dmz 高安全性網路時，這是非常重要。  
  
如需有關 AD DS 網站拓撲複寫，請查看[Windows Server Technical 參考](https://technet.microsoft.com/library/cc739127(WS.10).aspx)。  
  
### <a name="rid-management-and-issuance-improvements"></a>RID 的管理和發行改良功能  
Windows 2000 Active Directory 引進了移除主機，使用者、群組和電腦，例如網域控制站，以建立安全性識別碼 (Sid) 的安全性信任者相關 id 哪些問題集區。  根據預設，這全球 RID 名額有限 2<sup>30</sup>（或 1073741823）網域中建立的總 Sid。 Sid 無法傳回集區或是重新發出。 隨著時間大型網域可能會開始 Rid，過低或事故可能會導致不必要 RID 耗盡和最終耗盡。  
  
Windows Server 2012 發現的針對和 Microsoft 客戶支援為 AD DS RID 發行和管理問題的一些成熟自從第一次 Active Directory 網域建立在 1999 年地址。 這些功能包括：  
  
-   事件登入寫入定期 RID 消耗警告  
  
-   當系統管理員的身分失效 RID 集區的事件登入  
  
-   清除 [封鎖大小會立即執行 RID 原則上最大端點  
  
-   現在執行並登入的全域 RID 空間不足時，讓系統管理員身分執行動作前的全球空間用盡人造 RID 天花板  
  
-   現在可以的全域 RID 空間增加一位元，加倍 2 大小<sup>31</sup> (2147483648 Sid)  
  
如需有關 Rid 移除主機，請檢查[如何安全性識別碼工作](https://technet.microsoft.com/library/cc778824(WS.10).aspx)。  
  
## <a name="new-ad-ds-deployment-architecture"></a>新 AD DS 部署架構  
  
### <a name="ad-ds-role-deployment-and-management-architecture"></a>AD DS 角色部署及管理架構  
伺服器管理員和 ADDSDeployment Windows PowerShell 依賴的功能時，將部署或管理 AD DS 角色下列核心組件：  
  
-   Microsoft.ADroles.Aspects.dll  
  
-   Microsoft.ADroles.Instrumentation.dll  
  
-   Microsoft.ADRoles.ServerManager.Common.dll  
  
-   Microsoft.ADRoles.UI。Common.dll  
  
-   Microsoft.DirectoryServices.Deployment.Types.dll  
  
-   Microsoft.DirectoryServices.ServerManager.dll  
  
-   Addsdeployment.psm1  
  
-   Addsdeployment.psd1  
  
兩者都使用 Windows PowerShell 和遠端叫用-命令安裝遠端角色與設定。  
  
![簡化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_DepDLLs.png)  
  
Windows Server 2012 也來不及退出 LSASS.EXE 的一部分：  
  
-   DS 角色伺服器服務 (DsRoleSvc)  
  
-   DSRoleSvc.dll（載入 DsRoleSvc 服務）  
  
這項服務必須並升級、降級，或是複製 virtual 網域控制站才能執行。 安裝 AD DS 角色將這項服務，並設定預設手動] 的 [開始] 畫面類型。 不要停用此服務。  
  
### <a name="adprep-and-prerequisite-checking-architecture"></a>ADPrep 和必要條件檢查架構  
Adprep 不再需要的架構主機上執行。 它可以是執行 Windows Server 2008 x64 的電腦從遠端執行或更新版本。  
  
> [!NOTE]  
> Adprep 使用 LDAP 匯入 Schxx.ldf 檔案，並不會自動重新連接，當架構主機遺失期間匯入。 匯入程序的一部分，架構主機設定中的特定模式，並自動重新已停用，因為如果 LDAP 重新連接之後遺失，重新建立的連接不會在特定模式。 如此一來，不會正確更新結構描述。  
  
必要條件檢查確保某些條件為 true。 這些條件安裝所需成功 AD DS。 如果無法為 true 一些需要的條件，他們可以解析之前繼續安裝。 它還可以偵測的樹系或網域尚未尚未備好，好讓自動 Adprep 部署程式碼執行。  
  
#### <a name="adprep-executables-dlls-ldfs-files"></a>ADPrep 可執行檔，Dll，LDFs 檔案  
  
-   ADprep.dll  
  
-   Ldifde.dll  
  
-   Csvde.dll  
  
-   Sch14.ldf Sch56.ldf  
  
-   Schupgrade.cat  
  
-   *dcpromo.csv  
  
前身為位於 ADprep.exe AD 準備程式碼被重構 adprep.dll 插入。 這可以讓 windows ADPrep.exe 和 ADDSDeployment Windows PowerShell 模組媒體櫃使用相同的工作，並具有相同的功能。 Adprep.exe 隨附的安裝媒體，但自動程序進行不會直接呼叫-系統管理員身分執行它以手動方式。 它只可在 Windows Server 2008 x64 或更新版本作業系統上執行。 Ldifde.exe 和 csvde.exe 為載入準備程序的 Dll 有重構也版本。 架構延伸模組仍然會使用像是簽章驗證 LDF 檔案，在舊版的作業系統。  
  
![簡化的管理](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_AdprepDLLs.png)  
  
> [!IMPORTANT]  
> 還有 32 位元的 Windows Server 2012 Adprep32.exe 工具。 您必須至少一個 Windows Server 2008 x64、Windows Server 2008 R2 或 Windows Server 2012 電腦準備的樹系和網域執行為網域控制站伺服器成員，或工作群組中。 在 Windows Server 2003 x64 Adprep.exe 無法執行。  
  
#### <a name="BKMK_PrereuisiteChecking"></a>必要條件檢查  
必要條件檢查 [系統管理 ADDSDeployment Windows PowerShell 驗證碼到建置適用於不同的模式，根據作業。 下表描述每個測試，使用時，如何解釋及驗證功能。 這些表格可能有的問題，其中驗證失敗和錯誤不足，在問題的疑難排解才有用。  
  
這些測試登入**對部署**工作分類下的操作事件登入通道**核心**、永遠 263 為**103**。  
  
##### <a name="prerequisite-windows-powershell"></a>必要條件 Windows PowerShell  
有 ADDSDeployment Windows PowerShell cmdlet 提供網域控制站部署 cmdlet。 它們有約相同引數與他們相關 cmdlet。  
  
-   Test-ADDSDomainControllerInstallation  
  
-   Test-ADDSDomainControllerUninstallation  
  
-   Test-ADDSDomainInstallation  
  
-   Test-ADDSForestInstallation  
  
-   Test-ADDSReadOnlyDomainControllerAccountCreation  
  
執行下列 cmdlet，通常; 不需要他們已經自動執行部署 cmdlet 使用預設。  
  
##### <a name="BKMK_ADDSInstallPrerequisiteTests"></a>必要條件測試  
  
||||  
|-|-|-|  
|測試名稱|通訊協定<br /><br />使用|解釋和筆記|  
|VerifyAdminTrusted<br /><br />ForDelegationProvider|LDAP|確認您擁有的 [讓電腦和使用者帳號受信任的委派」(SeEnableDelegationPrivilege) 上的現有的合作夥伴網域控制站的權限。 這需要存取您的建構的 tokenGroups 屬性。<br /><br />與 Windows Server 2003 網域控制站連絡時，無法使用。 您必須手動確認此升級之前的權限|  
|VerifyADPrep<br /><br />必要條件（樹系）|LDAP|探索和使用進行 rootDSE namingContexts 屬性和架構命名操作 fsmoRoleOwner 屬性主機的連絡人。 判斷哪一個準備作業（forestprep、準備網域或 rodcprep）安裝所需 AD DS。 驗證架構係會如預期般和是否需要進一步擴充功能。|  
|VerifyADPrep<br /><br />必要條件（網域和 RODC）|LDAP|探索和使用進行 rootDSE namingContexts 屬性與基礎結構容器 fsmoRoleOwner 屬性基礎結構主機的連絡人。 如果是 RODC 安裝這項測試探索網域命名主機和確定它已 online。|  
|CheckGroup<br /><br />成員資格|LDAP」，<br /><br />RPC 透過 SMB (LSARPC)|驗證使用者屬於網域系統管理員或企業管理員群組中，根據操作 (DA 新增或降級網域控制站 EA 新增或移除網域中)|  
|CheckForestPrep<br /><br />GroupMembership|LDAP」，<br /><br />RPC 透過 SMB (LSARPC)|驗證使用者是架構系統管理員」的成員企業系統管理員群組和有管理稽核並現有的網域控制站權限的安全性事件登 (SesScurityPrivilege)|  
|CheckDomainPrep<br /><br />GroupMembership|LDAP」，<br /><br />RPC 透過 SMB (LSARPC)|驗證使用者網域管理群組成員並已管理稽核現有的網域控制站權限的安全性事件登 (SesScurityPrivilege)|  
|CheckRODCPrep<br /><br />GroupMembership|LDAP」，<br /><br />RPC 透過 SMB (LSARPC)|驗證使用者的企業系統管理員群組成員並已管理稽核現有的網域控制站權限的安全性事件登 (SesScurityPrivilege)|  
|VerifyInitSync<br /><br />AfterReboot|LDAP|因為它來進行 rootDSE 屬性 becomeSchemaMaster 上設定假價值重新啟動架構主機已至少一次覆寫的驗證|  
|VerifySFUHotFix<br /><br />套用|LDAP|驗證現有的樹系架構不包含 UID 屬性 OID 1.2.840.113556.1.4.7000.187.102 的已知的問題 SFU2 擴充功能<br /><br />([https://support.microsoft.com/kb/821732](https://support.microsoft.com/kb/821732))|  
|VerifyExchange<br /><br />SchemaFixed|LDAP、WMI、DCOM RPC|驗證現有的樹系架構不仍然包含問題 Exchange 2000 擴充功能 ms-Exch-小幫手」-名稱 ms-Exch-LabeledURI，與 ms Exch-館識別碼 ([https://support.microsoft.com/kb/314649](https://support.microsoft.com/kb/314649))|  
|VerifyWin2KSchema<br /><br />一致性|LDAP|驗證現有的樹系架構已一致（不正確的第三方修改）屬性和類別核心。|  
|帶領|透過 RPC，DRSR<br /><br />LDAP」，<br /><br />DNS<br /><br />RPC 透過 SMB (SAMR)|驗證命令列語法傳遞至促銷代碼並測試升級。 驗證樹系或網域並不存在如果建立新。|  
|VerifyOutbound<br /><br />ReplicationEnabled|LDAP，DRSR 透過 SMB RPC 透過 SMB (LSARPC)|驗證現有的網域控制站指定為複寫合作夥伴已輸出複寫檢查選項的設定 NTDS 物件的屬性 NTDSDSA_OPT_DISABLE_OUTBOUND_REPL (0x00000004) 的支援|  
|VerifyMachineAdmin<br /><br />密碼|透過 RPC，DRSR<br /><br />LDAP」，<br /><br />DNS<br /><br />RPC 透過 SMB (SAMR)|驗證設定 DSRM 符合網域複雜需求的安全模式下密碼。|  
|VerifySafeModePassword|*不適用*|驗證本機系統管理員密碼設定符合的電腦安全性原則複雜需求。|  
  


