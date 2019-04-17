---
ms.assetid: e4c31187-f15f-410b-bb79-8d63e2f2b421
title: "升級到 Windows Server 2012 R2 和 Windows Server 2012 的網域控制站"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e317c5a939d417bac844c4080223d7b5e0eec149
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012"></a>升級到 Windows Server 2012 R2 和 Windows Server 2012 的網域控制站

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題提供 Active Directory Domain Services Windows Server 2012 R2 和 Windows Server 2012 中的背景資訊與解釋升級網域控制站的 Windows Server 2008 或 Windows Server 2008 R2 的程序。  
  
-   [網域控制站升級步驟](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_UpgradeWorkflow)  
  
-   [Windows Server 2012 中的新功能？](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_WhatsNewEight)  
  
-   [在 Windows Server 2012 R2 AD DS 中的新功能？](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_NewWS2012R2)  
  
-   [Windows Server 2012 中 AD DS 中的新功能？](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_WhatsNewAD)  
  
-   [AD DS 伺服器角色安裝變更](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_InstallationChanges)  
  
-   [過時的功能和與 Windows Server 2012 中 AD DS 行為變更](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_DeprecatedFeatures)  
  
-   [系統需求](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_SysReqs)  
  
-   [支援的就地升級路徑](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_UpgradePaths)  
  
-   [層級的功能和需求](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_FunctionalLevels)  
  
-   [Windows 作業系統其他伺服器角色與 AD DS 交互操作](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_ServerRoles)  
  
-   [操作主機角色](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_OpsMasters)  
  
-   [執行 Windows Server 2012 化網域控制站](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_Virtual)  
  
-   [管理 Windows Server 2012 伺服器](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_Admin)  
  
-   [應用程式的相容性](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat)  
  
-   [已知的問題](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_KnownIssues)  
  
## <a name="BKMK_UpgradeWorkflow"></a>網域控制站升級步驟  
升級網域的建議的方式是升級執行較新版本的 Windows Server 並降級視較舊的網域控制站的網域控制站。 升級現有的網域控制站的作業系統最好的方法。 這份清單涵蓋一般到您的網域控制站執行 Windows Server 有較新版本的升級之前，請依照下列步驟：  
  
1.  確認目標伺服器符合[系統需求](https://technet.microsoft.com/library/dn303418.aspx)。  
  
2.  確認[的應用程式的相容性](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat)。  
  
3.  檢查安全性設定。 如需詳細資訊，請查看[到 AD DS，Windows Server 2012 中相關的 Deprecated 功能和變更行為](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_DeprecatedFeatures)和[安全的 Windows Server 2008 和 Windows Server 2008 R2 的預設設定](https://technet.microsoft.com/library/upgrade-domain-controllers-to-windows-server-2008-r2(WS.10).aspx#BKMK_SecureDefault)。  
  
4.  檢查您想要執行安裝電腦的目標伺服器連接。  
  
5.  檢查有可用的必要作業主機的角色：  
  
    -   若要安裝 Windows Server 2012 上執行的現有的網域和樹系的第一個 DC，執行安裝所在的電腦需要連接為了執行 adprep /forestprep 主機和為了執行 adprep /domainprep 基礎結構主機。  
  
    -   若要安裝的第一個 DC 樹系架構會已經延伸的網域中，您只需要連接基礎結構主機。  
  
    -   若要安裝或在現有的樹系移除網域，您需要網域命名主機連接。  
  
    -   任何網域控制站安裝也需要連接 RID 主機。  
  
    -   如果您第一個唯讀網域控制站安裝現有的樹系，您會需要為每個應用程式 directory 磁碟分割，也就是非網域命名操作或 NDNC 基礎結構主機連接。  
  
6.  請務必提供執行 AD DS 安裝必要的認證。  
  
    |安裝動作|認證需求|  
    |-----------------------|---------------------------|  
    |安裝新的樹系|本機目標伺服器上的系統管理員|  
    |在現有的樹系安裝新的網域|企業系統管理員|  
    |安裝其他俠現有網域中|網域系統管理員 」|  
    |執行 adprep /forestprep|架構系統管理員企業系統管理員，網域系統管理員|  
    |執行 adprep /domainprep|網域系統管理員 」|  
    |執行 adprep /domainprep /gpprep|網域系統管理員 」|  
    |執行 adprep /rodcprep|企業系統管理員|  
  
    您可以委派 AD DS 的權限。 如需詳細資訊，請查看[安裝管理工作](https://technet.microsoft.com/library/cc773327(WS.10).aspx)。  
  
在以下連結中可以找到宣傳新的與 Windows Server 2012 複本網域控制站使用 Windows PowerShell cmdlet 和伺服器管理員步驟來執行 「 步驟的指示執行：  
  
-   [安裝 Active Directory Domain Services (層級 100)](https://technet.microsoft.com/library/hh472162.aspx)  
  
-   [安裝新 Windows Server 2012 Active Directory 森林 (層級 200)](https://technet.microsoft.com/library/jj574166.aspx)  
  
-   [安裝複本 Windows Server 2012 網域控制站在現有的網域 (層級 200)](https://technet.microsoft.com/library/jj574134.aspx)  
  
-   [安裝新的 Windows Server 2012 Active Directory 子女或樹網域 (層級 200)](https://technet.microsoft.com/library/jj574105.aspx)  
  
-   [安裝 Windows Server 2012 Active Directory Read-Only 網域控制站 (RODC) (層級 200)](https://technet.microsoft.com/library/jj574152.aspx)  
  
-   [設定好 Windows Server 2012 網域控制站 (EN-US) 逐步指南](https://social.technet.microsoft.com/wiki/contents/articles/12370.step-by-step-guide-for-setting-up-windows-server-2012-domain-controller-en-us.aspx)  
  
## <a name="BKMK_WhatsNewEight"></a>Windows Server 2012 中的新功能？  
伺服器角色所列出的新功能和技術區域的如下表所示。 如需詳細白皮書、 視訊示範，以及簡報關於 Windows Server 2012 中的其他功能，請查看[Server 和雲端平台](https://www.microsoft.com/server-cloud/default.aspx)。  
  
||||  
|-|-|-|  
|[Active Directory 憑證 Services (AD CS)](https://technet.microsoft.com/library/hh831373.aspx)|[Active Directory 權限 Management Services (AD RMS)](https://technet.microsoft.com/library/hh831554.aspx)|[BitLocker 磁碟機加密](https://technet.microsoft.com/library/hh831412.aspx)|  
|[BranchCache](https://technet.microsoft.com/library/jj127252.aspx)|[動態主機設定通訊協定 」 (DHCP)](https://technet.microsoft.com/library/jj200226.aspx)|[網域名稱系統」(DNS)](https://technet.microsoft.com/library/jj200224.aspx)|  
|[容錯](https://technet.microsoft.com/library/hh831414.aspx)|[檔案伺服器資源管理員](https://technet.microsoft.com/library/hh831746.aspx)|[群組原則](https://technet.microsoft.com/library/jj574108.aspx)|  
|[Hyper-v](https://technet.microsoft.com/library/hh831410.aspx)|[(IPAM) 的 IP 位址管理](https://technet.microsoft.com/library/jj200214.aspx)|[F:kerberos 驗證](https://technet.microsoft.com/library/hh831747.aspx)|  
|[管理帳號服務](https://technet.microsoft.com/library/hh831451.aspx)|[網路功能](https://technet.microsoft.com/library/jj200215.aspx)|[遠端桌面服務](https://technet.microsoft.com/library/hh831527.aspx)|  
|[安全性稽核](https://technet.microsoft.com/library/hh849638.aspx)|[伺服器管理員](https://blogs.technet.com/b/servermanager/archive/2012/06/27/server-manager-power-of-many-simplicity-of-one.aspx)|[智慧卡](https://technet.microsoft.com/library/hh849637.aspx)|  
|[TLS SSL (Schannel SSP)](https://technet.microsoft.com/library/hh831771.aspx)|[Windows 部署服務](https://technet.microsoft.com/library/hh974416.aspx)|[Windows PowerShell 3.0](https://technet.microsoft.com/library/hh857339)|  
  
### <a name="automatic-maintenance-and-changes-to-restart-behavior-after-updates-are-applied-by-windows-update"></a>自動維護和變更 Windows Update 來套用更新之後，請重新開機行為  
之前版本的 Windows 8，Windows Update 來管理它自己內部排程來檢查有更新，以及下載及安裝它們。 它所需的 Windows 更新代理程式正在永遠執行在背景中耗用記憶體和其他的系統資源。  
  
Windows 8 和 Windows Server 2012 引進新功能，稱為[自動維護](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx)。 每可用來管理它自己排程並執行邏輯自動維護將彙總許多不同的功能。 這個彙總允許所有元件使用少系統資源、 一致運作，請尊重新的[連接待命](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx)狀態的新裝置類型，並使用較少可移植裝置上的電池。  
  
Windows Update 會自動維護 Windows 8 和 Windows Server 2012 中的一部分，因為已不再生效內部排程它自己設定的日期和時間，若要安裝的更新。 若要協助確保一致性與可預測重新企業的所有裝置與電腦的行為，包括以及執行 Windows 8 和 Windows Server 2012，請查看 Microsoft 知識庫文章[2885694](https://support.microsoft.com/kb/2885694) (看到累積 2013 年 10 月的彙總或[2883201](https://support.microsoft.com/kb/2883201))，然後原則設定 WSUS 部落格文章中所述[讓更多可預測的 Windows Update 體驗適用於 Windows 8 和 Windows Server 2012 (KB 2885694)](http://blogs.technet.com/b/wsus/archive/2013/10/08/enabling-a-more-predictable-windows-update-experience-for-windows-8-and-windows-server-2012-kb-2885694.aspx)。  
  

## <a name="BKMK_NewWS2012R2"></a>在 Windows Server 2012 R2 AD DS 中的新功能？  
下表摘要的更多詳細資訊，都能連結 AD DS，在 Windows Server 2012 R2 的新功能。 更多需某些功能，包括其需求，請查看[在 Windows Server 2012 R2 的 Active Directory 中的新功能](https://technet.microsoft.com/library/dn268294.aspx)。  
  
|功能|描述|  
|-----------|---------------|  
|[加入的工作地點](https://technet.microsoft.com/library/dn280945.aspx)|可讓資訊背景工作加入存取公司資源和服務的公司使用個人裝置。|  
|[Web 應用程式 Proxy](https://technet.microsoft.com/library/dn280942.aspx)|提供 web 應用程式使用新的遠端存取的角色服務的存取。|  
|[Active Directory 同盟服務](https://technet.microsoft.com/library/hh831502.aspx)|AD FS 已經簡化的部署與改良功能，讓使用者可以存取資源的個人裝置，並協助管理存取控制 IT 部門。|  
|[SPN 和 UPN 唯一性](https://technet.microsoft.com/library/dn535779.aspx)|執行 Windows Server 2012 R2 網域控制站封鎖建立主體名稱 (Spn) 重複服務及使用者主體名稱 (Upn)。|  
|[Winlogon 自動重新登入 (ARSO)](https://technet.microsoft.com/library/dn535772.aspx)|可讓鎖定畫面應用程式會重新啟動和 Windows 8.1 裝置上提供。|  
|[TPM 金鑰證明](https://technet.microsoft.com/library/dn581921.aspx)|可讓 Ca 密碼編譯證明在發行憑證的申請者私密金鑰確實由信賴平台模組 」 (TPM) 受保護的憑證。|  
|[認證保護與管理](https://technet.microsoft.com/library/dn408190.aspx)|新 credential 保護和網域驗證控制項，以減少認證竊取。|  
|[取代了檔案複寫服務 (FRS)](https://technet.microsoft.com/library/dn535775.aspx)|Windows Server 2003 網域功能層級也會取代 FRS 使用複寫 SYSVOL 層級正常運作，因為。 表示您在執行 Windows Server 2012 R2 的伺服器上建立新的網域時，網域功能等級必須 Windows Server 2008，或較新版本。 您仍然可以加入現有的 Windows Server 2003 網域功能等級; 網域執行 Windows Server 2012 R2 網域控制站您只是無法建立新的網域該層級。|  
|[新的網域及森林功能等級](../active-directory-functional-levels.md)|有新功能的層級的 Windows Server 2012 R2。 新的功能都可在 Windows Server 2012 R2 DFL。|  
|[LDAP 查詢最佳化的變更](https://technet.microsoft.com/library/dn535775.aspx)|LDAP 搜尋效率和 LDAP 搜尋查詢複雜時間效能改進。|  
|[1644 事件改良功能](https://technet.microsoft.com/library/dn535775.aspx)|事件，協助您疑難排解 ID 1644 已加入 LDAP 搜尋結果統計資料。|  
|[Active Directory 複寫輸送量改進](https://technet.microsoft.com/library/dn535775.aspx)|調整至約 600 Mbps 40Mbps 從最大 AD 複寫輸送量|  
  
## <a name="BKMK_WhatsNewAD"></a>Windows Server 2012 中 AD DS 中的新功能？  
下表摘要的更多詳細資訊，都能連結 AD ds，在 Windows Server 2012 中的新功能。 更多需某些功能，包括其需求，請查看[在 Active Directory Domain Services (AD DS) 中的新功能](https://technet.microsoft.com/library/hh831477.aspx)。  
  
|功能|描述|  
|-----------|---------------|  
|Active Directory 型啟動 (廣告 BA) 查看[磁碟區啟動概觀](https://technet.microsoft.com/library/hh831612.aspx)|簡化的設定散發和管理磁碟區軟體授權的工作。|  
|[Active Directory 同盟服務 (AD FS)](https://technet.microsoft.com/library/hh831502.aspx)|新增角色安裝透過伺服器管理員中，簡化信任-設定、 自動信任的管理、 SAML-通訊協定支援，及更多。|  
|Active Directory 遺失的頁面清除事件|使用 jet 錯誤-1119 NTDS ISAM 事件 530 是偵測到 Active Directory 資料庫遺失的頁面清除事件登入。|  
|[Active Directory 資源回收筒使用者介面](https://technet.microsoft.com/library/hh831702.aspx#ad_recycle_bin_mgmt)|Active Directory 管理中心 (ADAC) 新增資源回收筒] 功能在 Windows Server 2008 R2 原始導入了 GUI 管理。|  
|[Active Directory 複寫和拓撲 Windows PowerShell cmdlet](https://technet.microsoft.com/library/hh831757.aspx)|支援的建立及管理 Active Directory 網站網站連結、 連接物件，以及更多使用 Windows PowerShell。|  
|[動態存取控制](https://technet.microsoft.com/library/hh831717.aspx)|新宣告為基礎的授權平台美化舊版存取控制模型。|  
|[微調密碼原則使用者介面](https://technet.microsoft.com/library/hh831702.aspx#fine_grained_pswd_policy_mgmt)|ADAC 新增 GUI 建立、 編輯及指派 Pso 原始加入 Windows Server 2008 的支援。|  
|[群組管理服務帳號 (gMSA)](https://technet.microsoft.com/library/hh831782.aspx)|安全性主體新型稱為 gMSA。 在多部主機上執行之服務可以在同一個 gMSA account 執行。|  
|[DirectAccess 離線網域加入](https://technet.microsoft.com/library/jj574150.aspx)|延伸離線加入網域，包括 DirectAccess 必要條件。|  
|[快速部署透過 virtual 網域控制站 DC 複製](https://technet.microsoft.com/library/hh831734.aspx#virtualized_dc_cloning)|可以複製使用 Windows PowerShell cmdlet 現有 virtual 網域控制站快速部署模擬的 Dc。|  
|[移除集區的變更](https://technet.microsoft.com/library/jj574229.aspx)|新增監視新的事件和配額以對抗過消耗全球 RID 集區。 選擇增加一倍全球 RID 集區大小如果在用完原始集區。|  
|安全時間服務|美化 W32tm 的安全性，藉由移除可從網路、 移除 MD5 hash 功能需要驗證與 Windows 8 的時間用戶端伺服器|  
|[USN 回復模擬網域控制站的保護](https://technet.microsoft.com/library/hh831734.aspx#safe_virt_dc)|不小心將模擬網域控制站快照備份還原不會再造成 USN 復原。|  
|[Windows PowerShell 歷史檢視器](https://technet.microsoft.com/library/hh831702.aspx#windows_powershell_history_viewer)|讓系統管理員，若要檢視使用 ADAC 執行的 Windows PowerShell 命令。|  
  
### <a name="BKMK_"></a>自動維護和變更 Windows Update 來套用更新之後，請重新開機行為  
之前版本的 Windows 8，Windows Update 來管理它自己內部排程來檢查有更新，以及下載及安裝它們。 它所需的 Windows 更新代理程式正在永遠執行在背景中耗用記憶體和其他的系統資源。  
  
Windows 8 和 Windows Server 2012 引進新功能，稱為[自動維護](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx)。 每可用來管理它自己排程並執行邏輯自動維護將彙總許多不同的功能。 這個彙總允許所有元件使用少系統資源、 一致運作，請尊重新的[連接待命](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx)狀態的新裝置類型，並使用較少可移植裝置上的電池。  
  
Windows Update 會自動維護 Windows 8 和 Windows Server 2012 中的一部分，因為已不再生效內部排程它自己設定的日期和時間，若要安裝的更新。 若要可協助確保您的企業，以及執行 Windows 8 和 Windows Server 2012，包括中的所有的裝置和電腦的一致性與可預測重新開機行為，您可以設定群組原則設定下列：  
  
-   **電腦設定 |原則 |系統管理範本 |Windows 元件 |Windows Update |設定自動更新**  
  
-   **電腦設定 |原則 |系統管理範本 |Windows 元件 |Windows Update |不自動重新登入的使用者使用**  
  
-   **電腦設定 |原則 |系統管理範本 |Windows 元件 |維護排程器 |維護隨機延遲**  
  
下表列出如何進行這些設定，以提供您想要重新開機問題的一些事情。  
  
|||  
|-|-|  
|**案例**|**建議的組態**|  
|**WSUS 管理**<br /><br />-星期每一次更新安裝<br />-下午 11 開機星期五|自動安裝、 防止自動重新開機至您想要的時間來設定電腦<br /><br />**原則**： 設定自動更新] （功能）<br /><br />設定自動更新： 4-自動下載和排程安裝<br /><br />**原則**： 不自動重新登入的使用者 （停用）<br /><br />**WSUS 期限**： 下午 11 星期五設定|  
|**WSUS 管理**<br /><br />-偏位安裝跨不同的時間日期|設定不同群組的電腦應一起更新的目標群組<br /><br />使用上述步驟之前案例<br /><br />設定不同的期限不同的目標群組|  
|**不 WSUS 管理-不支援期限**<br /><br />-偏位安裝在不同時間|**原則**： 設定自動更新] （功能）<br /><br />設定自動更新： 4-自動下載和排程安裝<br /><br />**登錄鍵：**讓 Microsoft 知識庫文章討論登錄[2835627](https://support.microsoft.com/kb/2835627)<br /><br />**原則：**自動維護隨機延遲 （功能）<br /><br />設定**定期維護隨機延遲**來提供下列行為 6 個小時的隨機延遲 PT6H:<br /><br />的設定的維護時間加上隨機延遲會安裝更新<br /><br />-重新開機的每一部電腦不會發生完全 3 天之後<br /><br />或者，設定為每一組電腦不同維護時間|  
  
如需有關原因 Windows 工程小組實作這些變更，查看[在 Windows Update 自動更新後的重新開機最小化](http://blogs.msdn.com/b/b8/archive/2011/11/14/minimizing-restarts-after-automatic-updating-in-windows-update.aspx)。  
  
## <a name="BKMK_InstallationChanges"></a>AD DS 伺服器角色安裝變更  
在 Windows Server 2008 R2 到 Windows Server 2003，在 x86 或 X64 版本的之前，請先執行 Active Directory 安裝精靈、 Dcpromo.exe，以及 Dcpromo.exe Adprep.exe 命令列工具必須自動安裝或從媒體安裝選用的變化。  
  
從 Windows Server 2012，命令列安裝是使用 Windows PowerShell 模組 ADDSDeployment 來執行。 Gui 促銷，在伺服器管理員中使用全新 AD DS 設定精靈會執行。 若要簡化的安裝程序，ADPREP 已經整合到 AD DS 安裝，並且會視自動執行。 Windows PowerShell 型 AD DS 設定精靈會自動目標 Dc 位置新增，然後遠端相關網域控制站執行所需的 ADPREP 命令網域中的架構與基礎結構主要角色。  
  
開始安裝之前，必要條件檢查 AD DS 安裝精靈中的找出潛在的錯誤。 若要排除關注事項的部分完成升級可以修正錯誤條件。 精靈會也匯出包含所有選項圖形安裝期間所指定的 Windows PowerShell 指令碼。  
  
數位簽章 AD DS 安裝變更簡化俠角色安裝程序，並減少管理錯誤的機率，尤其是當您要部署多網域控制站在全球地區和網域。  
如需詳細資訊 GUI 及 Windows PowerShell 型安裝，包括命令列語法與逐步精靈中的指示，請查看[安裝 Active Directory Domain Services](https://technet.microsoft.com/library/hh472162.aspx)。 對於想要控制引入架構變更獨立安裝 Windows Server 2012 網域控制站在現有的樹系的 Active Directory 森林中的系統管理員，Adprep.exe 命令仍然可以執行在已提升權限的命令提示字元。  
  
## <a name="BKMK_DeprecatedFeatures"></a>過時的功能和與 Windows Server 2012 中 AD DS 行為變更  
有一些到 AD DS 有關的變更：  
  
-   **取代 Adprep32.exe 了**  
  
    只有一個 Adprep.exe 版本，並可以視需要執行 Windows Server 2008 64 位元的伺服器上執行或更新版本。 它可以在遠端電腦上，執行，而且如果的目標的作業裝載主角或 Windows Server 2003 32 位元作業系統上必須遠端執行。  
  
-   **取代 Dcpromo.exe 了**  
  
    Windows Server 2012 中它只，仍然可以執行回應檔案或命令列參數，讓組織時間轉換到新的 Windows PowerShell 安裝選項現有自動化雖然已被取代帶領。  
  
-   **LMHash 帳號已停用**  
  
    在 Windows Server 2008、 Windows Server 2008 R2 和 Windows Server 2012 上的範本可讓 NoLMHash 原則已停用安全性範本 Windows 2000 和 Windows Server 2003 網域控制站中的安全性安全的預設值。 使用 KB 文件中的步驟必要時，LMHash 相關戶端 NoLMHash 原則停用[946405](https://support.microsoft.com/kb/946405)。  
  
開始使用 Windows Server 2008、 網域控制站也有下列安全預設設定，相較於執行 Windows Server 2003 或 Windows 2000 的網域控制站。  
  
|||||  
|-|-|-|-|  
|加密類型或原則|Windows Server 2008 預設|Windows Server 2012 和 Windows Server 2008 R2 預設|意見|  
|AllowNT4Crypto|停用|停用|第三方伺服器訊息區 (SMB) 戶端可能會不相容的網域控制站在安全的預設設定。 在所有案例中，這些設定可以允許交互操作，但僅限執行安全性於輕鬆置於。 如需詳細資訊，請查看[文章 942564](https://go.microsoft.com/fwlink/?LinkId=164558)中「Microsoft 知識庫 (https://go.microsoft.com/fwlink/?LinkId=164558)。|  
|DES|支援|停用|[文章 977321](https://go.microsoft.com/fwlink/?LinkId=177717)在「Microsoft 知識庫 (https://go.microsoft.com/fwlink/?LinkId=177717)|  
|延伸 CBT 日保護的整合式驗證|不適用|支援|查看[Microsoft 安全性建議 (937811)](https://go.microsoft.com/fwlink/?LinkId=164559) (https://go.microsoft.com/fwlink/?LinkId=164559) 及[文章 976918](https://go.microsoft.com/fwlink/?LinkId=178251)中「Microsoft 知識庫 (https://go.microsoft.com/fwlink/?LinkId=178251)。<br /><br />檢視並安裝中的[文章 977073](https://go.microsoft.com/fwlink/?LinkId=186394) (https://go.microsoft.com/fwlink/?LinkId=186394) 中所需 Microsoft 知識庫。|  
|LMv2|支援|停用|[文章 976918](https://go.microsoft.com/fwlink/?LinkId=178251)在「Microsoft 知識庫 (https://go.microsoft.com/fwlink/?LinkId=178251)|  
  
## <a name="BKMK_SysReqs"></a>系統需求  
下表列出的 Windows Server 2012 的最低系統需求。 系統需求的相關詳細資訊和預先安裝的資訊，請查看[安裝 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。 有安裝新的 Active Directory 樹系不額外的系統需求，但您應該會新增以改善效能網域控制站、 LDAP client 要求和 Active Directory 功能的應用程式的快取的 Active Directory 資料庫到記憶體不足。 如果您升級現有的網域控制站或新增新的網域控制站現有的樹系，檢視下一節，以確保伺服器符合磁碟空間需求。  
  
|||  
|-|-|  
|處理器|1.4 Ghz 64 位元處理器|  
|RAM|512 MB|  
|可用磁碟空間需求|32 GB|  
|螢幕解析度|800 x 600 或更高版本|  
|其他|DVD 光碟機、 鍵盤、 網際網路存取權|  
  
### <a name="BKMK_DiskSpaceDCWin8"></a>升級網域控制站的磁碟空間需求  
本章節涵蓋僅適用於升級網域控制站的 Windows Server 2008 或 Windows Server 2008 R2 的磁碟空間需求。 如升級到較舊版本的 Windows Server 的網域控制站的磁碟空間需求的相關詳細資訊，請查看[磁碟空間需求升級到 Windows Server 2008 的](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008)或[磁碟空間需求升級到 Windows Server 2008 R2 的](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008R2)。  
  
調整大小裝載 Active Directory 資料庫並登入檔案，以配合自訂和導向的應用程式架構延伸、 應用程式和系統管理員車載機起始索引加物件的屬性，您會新增至 directory 部署的使用時間的網域控制站 （通常是到 8 5 年） 上的空間的磁碟。 立即縮放部署的時間，通常是很好的投資相較於部署後展開存放磁碟區所需的更多觸控成本。 如需詳細資訊，請查看[的 Active Directory Domain Services 容量計劃](https://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx)。  
  
在 [網域控制站想要升級，請確定主控 Active Directory 的磁碟機資料庫 (NTDS。DIT) 具有代表至少 20%NTDS 的可用磁碟空間。在您開始作業系統升級之前的 DIT 檔案。 如果磁碟區的磁碟空間不足，升級將會失敗並升級的相容性報告傳回錯誤，指出可用磁碟空間不足：  
  
若是如此，您可以嘗試重新擷取額外的空間的 Active Directory 資料庫離線磁碟重組並再試一次升級。 如需詳細資訊，請查看[以壓縮 Directory 資料庫檔案 （Offline 重組）](https://technet.microsoft.com/library/cc794920(v=WS.10).aspx)。  
  
### <a name="available-skus"></a>使用 Sku  
有 4 版本的 Windows Server： 基本知識、 Essentials、 Standard 和 Datacenter。   
支援 AD DS 角色兩個版本的 Standard 和 Datacenter。  
  
先前的版本，在 Windows Server 版本得到在他們的伺服器角色、 處理器計數與大量記憶體支援的支援。 Standard 和 Datacenter 版本的 Windows Server 支援所有的功能和硬體基礎，但在他們的模擬權利-而有所不同標準版允許兩個 virtual 執行個體，Datacenter edition 允許無限制 virtual 執行個體。  
  
### <a name="windows-client-and-windows-server-operating-systems-that-are-supported-to-join-windows-server-domains"></a>Windows client 與 Windows Server 支援加入網域 Windows 伺服器作業系統  
下列 Windows client 與 Windows Server 作業系統為支援執行 Windows Server 2012 」 的網域控制站的網域成員電腦或更新版本：  
  
-   Client 作業系統： Windows 8.1、 Windows 8、 Windows 7、 Windows Vista 
  
    執行 Windows 8.1 或 Windows 8 的電腦都也能加入網域的網域控制站該執行的舊版 Windows Server、 Windows Server 2003 包括或更新版本。 在這種情形下不過，某些 Windows 8 功能可能需要額外的設定或可能無法使用。 如需有關這些功能來管理 Windows 8 戶端舊版網域中的其他建議，請查看[在 Windows Server 2003 網域中的執行 Windows 8 成員電腦](https://social.technet.microsoft.com/wiki/contents/articles/17361.running-windows-8-member-computers-in-windows-server-2003-domains.aspx)。  
  
-   伺服器作業系統： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008、 Windows Server 2003 R2、 Windows Server 2003  
  
## <a name="BKMK_UpgradePaths"></a>支援的就地升級路徑  
執行 64 位元版本的 Windows Server 2008 或 Windows Server 2008 R2 網域控制站可以升級到 Windows Server 2012。 您無法升級執行 Windows Server 2003 或 Windows Server 2008 32 位元版本的網域控制站。 若要將它們安裝執行網域中的較新版的 Windows Server 網域控制站並移除網域控制站的 Windows Server 2003。  
  
|如果您正在執行這些版本|您可以這些版本升級|  
|-------------------------------------|-------------------------------------|  
|SP2 與 Windows Server 2008 Standard<br /><br />或<br /><br />SP2 與 Windows Server 2008 Enterprise|Windows Server 2012 標準<br /><br />或<br /><br />Windows Server 2012 資料中心|  
|SP2 與 Windows Server 2008 Datacenter|Windows Server 2012 資料中心|  
|Windows Server 2008 的 Web|Windows Server 2012 標準|  
|Windows Server 2008 R2 標準 sp1<br /><br />或<br /><br />Windows Server 2008 R2 企業 sp1|Windows Server 2012 標準<br /><br />或<br /><br />Windows Server 2012 資料中心|  
|Windows Server 2008 R2 Datacenter sp1|Windows Server 2012 資料中心|  
|Windows Server 2008 R2 的 Web|Windows Server 2012 標準|  
  
如需支援的升級路徑，請查看[評估版本和升級選項適用於 Windows Server 2012](https://go.microsoft.com/fwlink/?LinkId=260917)。 請注意，您無法將轉換網域控制站所執行的 Windows Server 2012 評估版直接到零售版。 而執行零售版的伺服器上安裝其他網域控制站 AD DS 移除網域控制站的試用版上執行。  
  
已知問題，因為您無法升級至 Server Core 安裝的 Windows Server 2012 執行了 Server Core 所安裝的 Windows Server 2008 R2 網域控制站。 升級將會在升級程序實心黑色畫面上停止回應。 這類 Dc 重新開機一次公開回復到先前的作業系統版本 boot.ini 檔案的選項。 其他重新開機觸發自動回復到舊版的作業系統。 之前方案，建議您安裝新的網域控制站執行而不是就地升級執行 Windows Server 2008 R2 的 Server Core 安裝現有網域控制站的 Windows Server 2012 Server Core 安裝。 如需詳細資訊，查看知識庫文章[2734222](https://support.microsoft.com/kb/2734222)。  
  
## <a name="BKMK_FunctionalLevels"></a>層級的功能和需求  
 Windows Server 2012 需要 Windows Server 2003 森林功能層級。 是的您可以加入現有的 Active Directory 樹系執行 Windows Server 2012 」 的網域控制站之前的樹系功能層級必須 Windows Server 2003 或更高版本。 這表示執行 Windows Server 2008 R2、 Windows Server 2008 或 Windows Server 2003 的網域控制站相同的樹系，可以運作，但不是執行 Windows 2000 Server 的網域控制站支援且將會封鎖安裝執行 Windows Server 2012 」 的網域控制站。 如果樹系包含執行 Windows Server 2003 網域控制站或更新版本正常運作的樹系但層級仍是 Windows 2000，也會封鎖安裝。  
  
Windows Server 2012 網域控制站新增到您的樹系前必須移除 Windows 2000 的網域控制站。 若是如此，請考慮將下列工作流程：  
  
1.  安裝網域控制站執行 Windows Server 2003 或更新版本。 這些網域控制站可以在 Windows Server 的試用版部署。 這個步驟也需要[執行 adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx)針對該作業系統版本成必要條件。  
  
2.  Windows 2000 的網域控制站中移除。 尤其是、 適當降級或強制移除 Windows Server 2000 網域控制站網域使用 Active Directory 使用者及移除所有已移除的網域控制站的網域控制站帳號電腦。  
  
3.  提高或更高到 Windows Server 2003 森林功能等級。  
  
4.  安裝執行 Windows Server 2012 」 的網域控制站。  
  
5.  移除網域控制站執行較舊的 Windows Server 版本。  
  
新的 Windows Server 2012 網域功能等級可讓一個新功能： **\ [KDC 支援宣告、 複合驗證以及 Kerberos 保護 \** \ [KDC 系統管理範本原則有兩種設定 (**永遠提供宣告**並**失敗護身的驗證要求**) 需要的 Windows Server 2012 網域功能層級。  
  
Windows Server 2012 森林功能層級不提供任何新的功能，但確保任何新的網域建立森林中將會自動操作網域層級 Windows Server 2012 正常運作。 Windows Server 2012 網域功能等級不提供宣告、 複合驗證以及 Kerberos 保護 \ \ [KDC 支援以外的其他新功能。 但它可以確保網域中的任何網域控制站執行 Windows Server 2012。 如需有關其他功能，可正常運作的不同層級，請查看[Active Directory Domain Services 了解 (AD DS) 功能的層級](../active-directory-functional-levels.md)。  
  
森林功能層級設定為某個值之後，您無法復原或降低森林功能等級，使用下列例外： 您升級到 Windows Server 2012 的樹系功能層級之後，您可以降低以 Windows Server 2008 R2。 如果 Active Directory 資源回收桶尚未，您也可以降低回到功能層級的 Windows Server 2012 的 Windows Server 2008 R2 或 Windows Server 2008，或 Windows Server 2008 R2 的 Windows Server 2008 的樹系。 如果的樹系功能層級設定為 Windows Server 2008 R2，它無法復原，例如，Windows Server 2003。  
  
網域功能層級設定為某個值之後，您無法復原或降低網域功能等級，使用下列例外： 循環網域功能等級的選項時您提高網域功能等級 Windows Server 2008 R2 或 Windows Server 2012，如果 Windows Server 2008 或較低的樹系功能的等級，您有回到 Windows Server 2008 或 Windows Server 2008 R2。 您可以降低只從 Windows Server 2008 R2 或 Windows Server 2008 的 Windows Server 2012 或 Windows Server 2008 R2 到 Windows Server 2008 網域功能等級。 如果網域功能層級設定為 Windows Server 2008 R2，它無法復原，例如，以 Windows Server 2003。  
  
如需較低的功能層級的功能，請查看[Active Directory Domain Services 了解 (AD DS) 功能的層級](../active-directory-functional-levels.md)。  
  
功能層級以外執行 Windows Server 2012」的網域控制站提供並不適用於執行較舊版本的 Windows Server 的網域控制站的額外功能。 例如，執行 Windows Server 2012」的網域控制站可用於 virtual 網域控制站複製，而無法執行較舊版本的 Windows Server 的網域控制站。 但 virtual 網域控制站複製與 Windows Server 2012 中的 virtual 網域控制站保護不需要任何功能層級需求。  
  
> [!NOTE]  
> Microsoft Exchange Server 2013 需要森林功能層級的 Windows server 2003 或更高版本。  
  
## <a name="BKMK_ServerRoles"></a>Windows 作業系統其他伺服器角色與 AD DS 交互操作  
在下列 Windows 作業系統 AD DS 不支援：  
  
-   Windows 單多點 Server  
  
-   Windows Server 2012 程式集  
  
AD DS 無法也會執行下列伺服器角色或角色服務的伺服器上安裝：  
  
-   HYPER-V Server  
  
-   遠端桌面連接代理人  
  
## <a name="BKMK_OpsMasters"></a>操作主機角色  
一些新功能在 Windows Server 2012 中的影響作業主機的角色：  
  
-   支援複製 virtual 網域控制站的 Windows Server 2012 必須執行肯定。 有其他複製網域控制站的必要條件。 如需詳細資訊，請查看[Active Directory Domain Services (AD DS) 模擬](https://technet.microsoft.com/library/hh831734.aspx)。  
  
-   肯定執行 Windows Server 2012 時，會建立新的安全性原則。  
  
-   移除主機有新 RID 發行及監視功能。 改進包括好事件登入，更適當限制，以及一個位元能力-緊急位在增加整體 RID 集區的配置。 如需詳細資訊，請查看[管理移除發行](../../ad-ds/manage/Managing-RID-Issuance.md)。  
  
> [!NOTE]  
> 雖然不是操作主機角色 AD DS 安裝在另一項變更是所有網域控制站執行 Windows Server 2012 預設會安裝 DNS 伺服器角色與通用。  
  
## <a name="BKMK_Virtual"></a>化網域控制站  
AD DS 開始在 Windows Server 2012 中的改進支援的網域控制站安全模擬與複製網域控制站的能力。 依序複製網域控制站可快速在新的網域和其他優點其他網域控制站部署。 如需詳細資訊，請查看[Active Directory Domain Services 和 #40; 簡介AD DS 和 #41;模擬與 #40;層級 100 和 #41;](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).  
  
## <a name="BKMK_Admin"></a>管理 Windows Server 2012 伺服器  
使用[遠端伺服器管理工具適用於 Windows 8](https://www.microsoft.com/download/details.aspx?id=28972)以管理網域控制站與其他執行 Windows Server 2012 的伺服器。 您可以在執行 Windows 8 的電腦上執行 Windows Server 2012 遠端伺服器管理工具。  
  
## <a name="BKMK_AppCompat"></a>應用程式的相容性  
下表包含一般的 Active Directory 整合 Microsoft 應用程式。 下表包含何種版本的應用程式可以在安裝 Windows Server 與 Windows Server 2012 網域控制站導入是否會影響應用程式的相容性。  
  
|Product|筆記|  
|-----------|---------|  
|[2007 Microsoft 組態管理員](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|組態管理員 2007 sp2 （包括組態管理員 2007 R2 和組態管理員 2007 R3）：<br /><br />Windows 8 專業版<br />-Windows 8 企業版<br />Windows Server 2012 標準<br />Windows Server 2012 Datacenter**請注意：**這些將為戶端完全支援，但已新增使用 Configuration Manager 2007 作業系統部署功能來部署這些作業系統為支援不計劃。 此外，不需要網站伺服器或網站系統將會支援在所有 SKU 的 Windows Server 2012 」。|  
|[Microsoft SharePoint 2007](https://support.microsoft.com/kb/2728964)|Microsoft Office SharePoint 伺服器 2007年不支援在 Windows Server 2012 上進行安裝。|  
|[Microsoft SharePoint 2010](https://support.microsoft.com/kb/2724471)|SharePoint 2010 Service Pack 2，才能安裝及操作 <br />Windows Server 2012 的伺服器上 SharePoint 2010<br /><br />安裝與操作 SharePoint 2010 基礎 Windows Server 2012 的伺服器上所需 SharePoint 2010 基本知識 Service Pack 2<br /><br />在 Windows Server 2012 上失敗，SharePoint Server 2010 （不含 service pack) 的安裝程序<br /><br />SharePoint Server 2010 必要條件安裝程式 (PrerequisiteInstaller.exe) 失敗，並顯示錯誤 「 此程式都擁有相容性問題 」。 按一下 [未取得協助執行程式 」，會顯示錯誤 「 確認如果可以安裝 SharePoint 和 #124;SharePoint Server 2010 （不含 service pack) 無法安裝 Windows Server 2012。 」|  
|[Microsoft SharePoint 2013](https://technet.microsoft.com/library/cc262485(v=office.15).aspx)|資料庫中發電廠伺服器的最低需求<br /><br />64 位元版本的 Windows Server 2008 R2 Service Pack 1 (SP1) 標準、 企業版或 Datacenter 或 64 位元版本的 Windows Server 2012 標準或 Datacenter<br /><br />建資料庫單一伺服器的最低需求：<br /><br />64 位元版本的 Windows Server 2008 R2 Service Pack 1 (SP1) 標準、 企業版或 Datacenter 或 64 位元版本的 Windows Server 2012 標準或 Datacenter<br /><br />伺服器前端網頁和應用程式伺服器的最低需求：<br /><br />64 位元版本的 Windows Server 2008 R2 Service Pack 1 (SP1) 標準、 企業版或 Datacenter 或 64 位元版本的 Windows Server 2012 標準或資料中心。|  
|[Microsoft System Center Configuration Manager 2012](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|系統中心 2012年組態管理員 Service Pack 1:<br /><br />Microsoft 會在下列作業系統加入 Service Pack 1 發行我們 client 支援矩陣：<br /><br />Windows 8 專業版<br />-Windows 8 企業版<br />Windows Server 2012 標準<br />Windows Server 2012 資料中心<br /><br />可以包括網站伺服器、 簡訊提供者和管理點-所有網站伺服器角色都部署到伺服器作業系統版本：<br /><br />Windows Server 2012 標準<br />Windows Server 2012 資料中心|  
|[Microsoft Lync Server 2013](https://technet.microsoft.com/library/gg412883.aspx)|Windows Server 2008 R2 或 Windows Server 2012，需要 Lync Server 2013。 無法在 Server Core 安裝執行它。 它可以執行[virtual 伺服器]](https://technet.microsoft.com/library/gg399035.aspx)。|  
|[Lync Server 2010](https://support.microsoft.com/kb/2777359)|Lync Server 2010 可以安裝新 （不升級） 安裝 Windows Server 2012 上，如果[Lync Server 2012 年 10 月累積更新](https://support.microsoft.com/?kbid=2493736)安裝。 不支援的 Lync Server 2010 的現有的安裝升級到 Windows Server 2012 的作業系統。 Windows Server 2012 上也不支援 Microsoft Lync Server 2010 群組聊天伺服器。|  
|[System Center 2012 Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|System Center 2012 Endpoint Protection Service Pack 1 將更新以包含在下列作業系統 client 支援矩陣<br /><br />Windows 8 專業版<br />-Windows 8 企業版<br />Windows Server 2012 標準<br />Windows Server 2012 資料中心|  
|[System Center 2012 Forefront Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|更新彙總套件 1 FEP 2010 將更新以包含在下列作業系統 client 支援矩陣：<br /><br />Windows 8 專業版<br />-Windows 8 企業版<br />Windows Server 2012 標準<br />Windows Server 2012 資料中心|  
|Forefront 威脅管理閘道 (TMG)|TMG 支援在 Windows Server 2008 和 Windows Server 2008 R2 上執行。 如需詳細資訊，請查看[系統需求 Forefront TMG](https://technet.microsoft.com/library/dd896981.aspx)。|  
|Windows Server Update Services|此版本的 WSUS 已經支援 Windows 8 電腦或 Windows Server 2012 為基礎的電腦做為戶端。|  
|Windows Server Update Services 3.0|更新知識庫文章[2734608](https://support.microsoft.com/kb/2734608)可讓伺服器正在執行 Windows Server Update Services (WSUS) 3.0 SP2 提供的電腦執行 Windows 8 或 Windows Server 2012 的更新：**請注意：** WSUS 3.0 sp2 的獨立 WSUS 3.0 SP2 環境或 System Center Configuration Manager 2007 Service Pack 2 的環境針對需要[2734608](https://support.microsoft.com/kb/2734608)正確管理 Windows 8 電腦或 Windows Server 2012 電腦為戶端。|  
|[換貨 2013](https://technet.microsoft.com/library/bb691354.aspx)|Windows Server 2012 標準和 Datacenter 支援下列的角色： 架構主機、 通用伺服器、 網域控制站信箱和 client 存取伺服器角色<br /><br />森林功能層級： Windows Server 2003 或更高版本<br /><br />來源： 換貨 2013年系統需求|  
|換貨 2010|[來源： 換貨 2010 Service Pack 3](https://blogs.technet.com/b/exchange/archive/2012/09/25/announcing-exchange-2010-service-pack-3.aspx)<br /><br />換貨 2010 含 Service Pack 3 可以安裝 Windows Server 2012 成員伺服器上。<br /><br />[換貨 2010年系統需求](https://technet.microsoft.com/library/aa996719(EXCHG.141).aspx)列出最新支援的架構主要，，全球 catalog 和網域控制站與 Windows Server 2008 R2。<br /><br />森林功能層級： Windows Server 2003 或更高版本|  
|SQL Server 2012|來源： KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Windows Server 2012 上 SQL Server 2012 RTM 支援。|  
|SQL Server 2008 R2|來源： KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />若要在 Windows Server 2012 上安裝需要 SQL Server 2008 R2 含 Service Pack 1 或更新版本。|  
|SQL Server 2008|來源： KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />若要在 Windows Server 2012 上安裝需要 SQL Server 2008 含 Service Pack 3 或更新版本。|  
|SQL Server 2005|來源： KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />不支援 Windows Server 2012 上安裝。|  
  
## <a name="BKMK_KnownIssues"></a>已知的問題  
下表列出到 AD DS 安裝相關的已知的問題。  
  
||||  
|-|-|-|  
|KB 文章數字和標題|少數的技術區域|問題描述日|  
|[2830145](https://support.microsoft.com/kb/2830145)： 無法在 Windows 7 或 Windows Server 2008 R2 電腦網域環境中的對應 SID 1-18 1 及 SID-1-18-2|AD DS 管理應用程式相容|應用程式的地圖 SID 1-18 1 和 SID-1-18-2、 Windows Server 2012 中的新功能，這可能會失敗，因為 Windows 7 或 Windows Server 2008 R2 的電腦上無法解析 Sid。 若要修正這個問題的相關，hotfix 網域中的 Windows 7 與 Windows Server 2008 R2 的電腦上。|  
|[2737129](https://support.microsoft.com/kb/2737129)： 準備群組原則不會執行時，會自動準備 Windows Server 2012 的現有的網域|AD DS 安裝|Adprep /domainprep /gpprep 無法自動執行安裝執行 Windows Server 2012 網域中的第一個 DC 的一部分。 如果該從未執行先前網域中，它必須執行以手動方式。|  
|[2737416](https://support.microsoft.com/kb/2737416): Windows PowerShell 根據網域控制站部署重複警告|AD DS 安裝|可以必要條件在驗證期間會顯示警告，並再重新出現在安裝期間。|  
|[2737424](https://support.microsoft.com/kb/2737424): 「 指定的網域名稱的格式不正確的 「 錯誤，當您嘗試移除網域控制站 Active Directory Domain Services|AD DS 安裝|如果您要移除預先建立的 RODC 帳號仍然存在的網域中的最後一個 DC 會出現這個錯誤。 這會影響 Windows Server 2012、 Windows Server 2008 R2 和 Windows Server 2008。|  
|[2737463](https://support.microsoft.com/kb/2737463)： 網域控制站不會開始，就會發生 c00002e2 錯誤，或會顯示 [選擇選項]|AD DS 安裝|DC 不會開始因為系統管理員使用 Dism.exe、 Pkgmgr.exe 或 Ocsetup.exe 以移除對-DomainController 角色。|  
|[2737516](https://support.microsoft.com/kb/2737516)： 在 Windows Server 2012 伺服器管理員中的 IFM 驗證限制|AD DS 安裝|IFM 驗證 KB 文件中所述，可以讓限制。|  
|[2737535](https://support.microsoft.com/kb/2737535)： 安裝-AddsDomainController cmdlet 傳回參數設定 RODC 錯誤|AD DS 安裝|當您嘗試伺服器附加至 RODC 帳號，如果您已經會填入引數指定預先建立 RODC 帳號，您可以收到錯誤。|  
|[2737560](https://support.microsoft.com/kb/2737560): 「 無法執行換貨架構衝突檢查 」 錯誤，以及必要條件檢查失敗|AD DS 安裝|當您設定 Windows Server 2012 DC 第一次現有網域中，因為網域控制站的遺失 SeServiceLogonRight 網路的服務，或封鎖 WMI 或 DCOM 通訊協定因為必要條件檢查將會失敗。|  
|[2737797](https://support.microsoft.com/kb/2737797): AddsDeployment 模組-引數則會顯示不正確的 DNS 結果|AD DS 安裝|-參數顯示 DNS 伺服器將不會安裝，但它將會。|  
|[2737807](https://support.microsoft.com/kb/2737807): 網域控制站選項] 頁面上不適下一步]|AD DS 安裝|[下一步] 按鈕已停用網域控制站選項] 頁面上，因為目標俠的 IP 位址未對應現有子網路或網站，或是無法輸入並確認正確 DSRM 密碼。|  
|[2737935](https://support.microsoft.com/kb/2737935): active Directory 安裝停止在 [建立 NTDS 設定物件 」 階段|AD DS 安裝|安裝無回應，因為本機系統管理員密碼和網域系統管理員密碼，或是網路問題讓重要複寫無法完成。|  
|[2738060](https://support.microsoft.com/kb/2738060): 「 存取 」 時，您的子女網域從遠端使用建立安裝-AddsDomain 錯誤訊息|AD DS 安裝|當您執行安裝-ADDSDomain 與叫用命令 cmdlet DNSDelegationCredential 有錯誤的密碼，您會收到的錯誤。|  
|[2738697](https://support.microsoft.com/kb/2738697): 「 伺服器不操作 「 網域控制站設定錯誤當您使用伺服器管理員中設定伺服器|AD DS 安裝|當您嘗試群組的電腦上安裝 AD DS，因為已停用 NTLM 驗證時，您會收到這個錯誤。|  
|[2738746](https://support.microsoft.com/kb/2738746)： 您收到錯誤拒絕在您登入本機系統管理員核對之後存取|AD DS 安裝|當您使用本機系統管理員帳號，而非建登入，然後建立新的網域 account 是未加入網域系統管理員 」 群組。|  
|[2743345](https://support.microsoft.com/kb/2743345): 「 系統找不到指定的檔案] Adprep /gpprep 錯誤或工具當機的問題|AD DS 安裝|您收到這個錯誤當您執行 adprep /gpprep 因為基礎結構主機實作分開命名空間|  
|[2743367](https://support.microsoft.com/kb/2743367): Adprep 「 不是有效 Win32 應用程式 」 在 Windows Server 2003 64 位元版本的錯誤|AD DS 安裝|因為 Windows Server 2012 Adprep 無法執行 Windows Server 2003，您會收到這個錯誤。|  
|[2753560](https://support.microsoft.com/kb/2753560): 3.2 ADMT 和 PES 3.1 Windows Server 2012 上的安裝錯誤|ADMT|ADMT 3.2 無法安裝 Windows Server 2012 上所設計。|  
|[2750857](https://support.microsoft.com/kb/2750857): DFS 複寫診斷報告無法正確顯示在 Internet Explorer 10|DFS 複寫|DFS 複寫診斷報告無法正確顯示，因為 Internet Explorer 10 中的變更。|  
|[2741537](https://support.microsoft.com/kb/2741537)： 遠端群組原則更新使用者都能看見|群組原則|這是因為的層級的每一位使用者登入執行排定的工作。 「 Windows 工作排程器要求在本案例中的互動式提示。|  
|[2741591](https://support.microsoft.com/kb/2741591): ADM 檔案無法在 SYSVOL 中有 GPMC 基礎結構狀態] 選項|群組原則|因為 GPMC 基礎結構狀態不符合自訂篩選規則 GP 複寫可以報告 「 複寫進行中的]。|  
|[2737880](https://support.microsoft.com/kb/2737880): 「 無法開始服務 」 時發生錯誤 AD DS 設定|Virtual 俠複製|發生這個錯誤時安裝或移除 AD DS，複製，因為已停用 DS 角色伺服器服務。|  
|[2742836](https://support.microsoft.com/kb/2742836)： 當您使用複製的功能 VDC 兩個 DHCP 租用建立的每個網域控制站|Virtual 俠複製|這是因為複製的網域控制站收到租用之前，請先複製並再試一次時複製已完成。|  
|[2742844](https://support.microsoft.com/kb/2742844)： 網域控制站伺服器失敗，且複製重新開機以 Windows Server 2012 中 DSRM|Virtual 俠複製|複製的俠開始在 DSRM 因為複製各種不同的原因 KB 文章中列出的任何失敗。|  
|[2742874](https://support.microsoft.com/kb/2742874)： 網域控制站複製並不會重新建立的所有服務主體名稱|Virtual 俠複製|某些三個部分 Spn 會不重新建立上複製俠因為重新命名程序的限制。|  
|[2742908](https://support.microsoft.com/kb/2742908): 「 不登入伺服器可 」 之後複製網域控制站的錯誤|Virtual 俠複製|當您嘗試登入失敗，因為複製複製模擬的 DC 後 DC DSRM 在開始時，您會收到這個錯誤。 登入。 \administrator 疑難排解複製失敗。|  
|[2742916](https://support.microsoft.com/kb/2742916)： 網域控制站複製失敗，錯誤 8610 dcpromo.log 中|Virtual 俠複製|因為網域磁碟分割，可能是由於的角色轉移輸入的複寫不執行肯定，請複製失敗。|  
|[2742927](https://support.microsoft.com/kb/2742927): [索引超出範圍 」 新-AdDcCloneConfig 錯誤|Virtual 俠複製|新增-ADDCCloneConfigFile cmdlet 執行時複製 virtual Dc，可能是因為 cmdlet 已提升權限的命令提示字元中執行，或是您存取權杖不包含系統管理員群組之後，您收到的錯誤。|  
|[2742959](https://support.microsoft.com/kb/2742959)： 網域控制站複製失敗，錯誤 8437︰ 「 不正確的參數指定此複寫操作 」|Virtual 俠複製|複製失敗，因為指定無效複製名稱或重複 NetBIOS 名稱。|  
|[2742970](https://support.microsoft.com/kb/2742970): DC Cloning 失敗，並不 DSRM，重複的來源和複製電腦|Virtual 俠複製|複製 virtual 俠開機中 Directory 服務修復模式 (DSRM)，因為不正確的位置中建立 DCCloneConfig.xml 檔案，或是複製之前來源 DC 重新開機，做為來源俠使用重複的名稱。|  
|[2743278](https://support.microsoft.com/kb/2743278)： 網域控制站複製 0x80041005 錯誤|Virtual 俠複製|複製的俠開機至 DSRM 因為指定只有一個 WINS 伺服器。 如果指定任何 WINS 伺服器，則必須指定慣用和其他贏得伺服器。|  
|[2745013](https://support.microsoft.com/kb/2745013): 「 伺服器無法操作 「 錯誤訊息如果您執行 Windows Server 2012 中的新-AdDcCloneConfigFile|Virtual 俠複製|因為伺服器無法連絡通用伺服器執行新-ADDCCloneConfigFile cmdlet 之後，您會收到這個錯誤。|  
|[2747974](https://support.microsoft.com/kb/2747974)： 網域控制站複製事件 2224年提供正確的指導方針|Virtual 俠複製|事件 ID 2224 正確狀態受管理的服務帳號，必須移除之前，請先複製。 獨立 MSAs 必須移除，但是群組 MSAs 不會封鎖複製。|  
|[2748266](https://support.microsoft.com/kb/2748266)： 之後在您升級到 Windows 8，您無法解除鎖定加密 BitLocker 磁碟機|BitLocker|當您嘗試已從 Windows 7 升級的電腦上的磁碟機解除鎖定時，您會收到 「 找不到應用程式 」 錯誤。|  
  
## <a name="see-also"></a>也了  
[Windows Server 2012 評估資源](https://technet.microsoft.com/evalcenter/hh708766.aspx)  
[Windows Server 2012 評估指南](https://download.microsoft.com/download/5/B/2/5B254183-FA53-4317-B577-7561058CEF42/WS%202012%20Evaluation%20Guide.pdf)  
[安裝和部署 Windows Server 2012](https://technet.microsoft.com/library/hh831620.aspx)  
  


