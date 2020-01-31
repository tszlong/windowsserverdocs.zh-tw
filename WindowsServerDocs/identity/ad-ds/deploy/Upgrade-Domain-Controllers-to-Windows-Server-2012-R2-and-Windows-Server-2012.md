---
ms.assetid: e4c31187-f15f-410b-bb79-8d63e2f2b421
title: 將網域控制站升級為 Windows Server 2012 R2 與 Windows Server 2012
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f8e5164ee1b5729d30536ae61df7cf3579e57fe6
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822721"
---
# <a name="upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012"></a>將網域控制站升級為 Windows Server 2012 R2 與 Windows Server 2012

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題提供有關 Windows Server 2012 R2 和 Windows Server 2012 中 Active Directory Domain Services 的背景資訊，並說明從 Windows Server 2008 或 Windows Server 2008 R2 升級網域控制站的程式。  
  
## <a name="BKMK_UpgradeWorkflow"></a>網域控制站升級步驟  
升級網域的建議方式是視需要升級執行較新版 Windows Server 的網域控制站，以及降級舊版網域控制站。 該方法是升級現有網域控制站之作業系統的慣用方法。 這份清單涵蓋執行較新版本之 Windows Server 的網域控制站升級之前要遵循的一般步驟：  
  
1. 確認目標伺服器符合 [系統需求](https://technet.microsoft.com/library/dn303418.aspx)。  
2. 確認 [Application compatibility](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat)。  
3. 確認安全性設定。 如需詳細資訊，請參閱 與 Windows Server 2012 中 AD DS 相關的過時功能與行為變更 和 Secure default settings in Windows Server 2008 和 Windows Server 2008 R2。  
4. 檢查要執行安裝的電腦與目標伺服器的連線。  
5. 檢查必要操作主機角色的可用性：  

   - 若要在現有網域和樹系中安裝執行 Windows Server 2012 的第一個 DC，執行安裝的電腦必須連線到架構主機才能執行 adprep/forestprep 和基礎結構主機，以便執行 adprep/domainprep。  
   - 若要在已經延伸樹系架構的網域中安裝第一個 DC，則只需要連線到基礎結構主機即可。  
   - 若要在現有樹系中安裝或移除網域，您需要連線到網域命名主機。  
   - 任何網域控制站安裝也需要連線到 RID 主機。  
   - 若要在現有樹系中安裝第一個唯讀網域控制站，您需要連接到每一個應用程式目錄分割的基礎結構主機，也稱為非網域命名內容或 NDNC。  

6. 務必提供必要的認證，才能執行 AD DS 安裝。  

   |安裝動作|認證要求|  
   |-----------------------|---------------------------|  
   |安裝新樹系|目標伺服器上的本機 Administrator|  
   |在現有樹系中安裝新網域|Enterprise Admins|  
   |在現有網域中安裝其他 DC|Domain Admins|  
   |執行 adprep /forestprep|Schema Admins、Enterprise Admins 和 Domain Admins|  
   |執行 adprep /domainprep|Domain Admins|  
   |執行 adprep /domainprep /gpprep|Domain Admins|  
   |執行 adprep /rodcprep|Enterprise Admins|  

   您可以委派 AD DS 的安裝權限。 如需詳細資訊，請參閱 [安裝管理工作](https://technet.microsoft.com/library/cc773327(WS.10).aspx)。  

以下連結提供使用 Windows PowerShell Cmdlet 和伺服器管理員升級新增和複本 Windows Server 2012 網域控制站的逐步指示：  

- [安裝 Active Directory Domain Services （技術等級100）](https://technet.microsoft.com/library/hh472162.aspx)  
- [安裝新的 Windows Server 2012 Active Directory 樹系（技術等級200）](https://technet.microsoft.com/library/jj574166.aspx)  
- [在現有網域中安裝複本 Windows Server 2012 網域控制站（技術等級200）](https://technet.microsoft.com/library/jj574134.aspx)  
- [安裝新的 Windows Server 2012 Active Directory 子域或樹狀目錄網域（等級200）](https://technet.microsoft.com/library/jj574105.aspx)  
- [安裝 Windows Server 2012 Active Directory 唯讀網域控制站（RODC）（技術等級200）](https://technet.microsoft.com/library/jj574152.aspx)  
- [設定 Windows Server 2012 網域控制站的逐步指南（en-us）](https://social.technet.microsoft.com/wiki/contents/articles/12370.step-by-step-guide-for-setting-up-windows-server-2012-domain-controller-en-us.aspx)  

## <a name="windows-update-considerations"></a>Windows Update 考慮

在 Windows 8 版本之前，Windows Update 會管理其內部排程以檢查更新，並下載及安裝它們。 這需要始終在背景執行 Windows Update 代理程式，因而會耗用記憶體和其他系統資源。  
  
Windows 8 與 Windows Server 2012 引進一項名為 [自動維護](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx)的新功能。 自動維護合併了許多不同的功能，這些功能是分別用來管理它自己的排程及執行邏輯。 這項合併可以讓所有這些元件使用最少的系統資源、穩定運作、尊重新裝置類型的新 [連線待命](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) 狀態，並使用較少的可攜式裝置電池電力。  
  
因為 Windows Update 是 Windows 8 和 Windows Server 2012 中自動維護的一部分，其本身設定日期和時間來安裝更新的內部排程已不再有效。 為協助確保您企業中的所有裝置與電腦 (包括執行 Windows 8 與 Windows Server 2012 的所有裝置與電腦) 有一致且可預期的重新啟動行為，請參閱 Microsoft 知識庫文章 [2885694](https://support.microsoft.com/kb/2885694) (或參閱 2013 年 10 月的累積彙總套件 [2883201](https://support.microsoft.com/kb/2883201))，然後設定下列 WSUS 部落格文章中描述的原則設定： [為 Windows 8 與 Windows Server 2012 啟用更好預測的 Windows Update 體驗 (KB 2885694)](https://blogs.technet.com/b/wsus/archive/2013/10/08/enabling-a-more-predictable-windows-update-experience-for-windows-8-and-windows-server-2012-kb-2885694.aspx)。  

## <a name="BKMK_NewWS2012R2"></a>Windows Server 2012 R2 中 AD DS 的新功能

下表摘要說明 Windows Server 2012 R2 中的 AD DS 新功能，並附上含有詳細資訊的連結。 如需某些功能更詳細的說明，包含這些功能的需求，請參閱 [Windows Server 2012 R2 中 Active Directory 的新功能](https://technet.microsoft.com/library/dn268294.aspx)。  

|功能|說明|  
|-----------|---------------|  
|[Workplace Join](https://technet.microsoft.com/library/dn280945.aspx)|可讓資訊工作者將其個人裝置與公司聯結，以存取公司資源和服務。|  
|[Web 應用程式 Proxy](https://technet.microsoft.com/library/dn280942.aspx)|運用新的遠端存取角色服務提供 Web 應用程式的存取。|  
|[Active Directory 同盟服務](https://technet.microsoft.com/library/hh831502.aspx)|AD FS 簡化了部署與改良功能，讓使用者可以從個人裝置存取資源，並協助 IT 部門管理存取控制。|  
|[SPN 和 UPN 的唯一性](https://technet.microsoft.com/library/dn535779.aspx)|執行 Windows Server 2012 R2 的網域控制站會阻止建立重複的服務主體名稱 (SPN) 與使用者主體名稱 (UPN)。|  
|[Winlogon 自動重新啟動登入 (ARSO)](https://technet.microsoft.com/library/dn535772.aspx)|讓鎖定螢幕應用程式可在 Windows 8.1 裝置上重新啟動並使用。|  
|[TPM 金鑰證明](https://technet.microsoft.com/library/dn581921.aspx)|讓 CA 以密碼編譯方式證明發行的憑證中，憑證要求者私密金鑰實際上受信賴平台模組 (TPM) 的保護。|  
|[認證保護和管理](https://technet.microsoft.com/library/dn408190.aspx)|新的認證保護與網域驗證控制可減少認證被盜的機會。|  
|[淘汰檔案複寫服務（FRS）](https://technet.microsoft.com/library/dn535775.aspx)|Windows Server 2003 網域功能等級也已被取代，因為在該功能等級中，FRS 是用來複寫 SYSVOL。 那表示當您在執行 Windows Server 2012 R2 的伺服器上建立新網域時，網域功能等級需為 Windows Server 2008 或更新版本。 您仍然可以將執行 Windows Server 2012 R2 的網域控制站新增至具有 Windows Server 2003 網域功能等級的現有網域;您就無法在該層級建立新的網域。|  
|[新的網域和樹系功能等級](../active-directory-functional-levels.md)|Windows Server 2012 R2 有新的功能等級。 Windows Server 2012 R2 DFL 有新功能。|  
|[LDAP 查詢最佳化工具變更](https://technet.microsoft.com/library/dn535775.aspx)|LDAP 對於複雜查詢的搜尋效率與搜尋時間的效能改善。|  
|[1644 事件改進](https://technet.microsoft.com/library/dn535775.aspx)|LDAP 搜尋結果統計資料已加入事件識別碼 1644 以協助疑難排解。|  
|[Active Directory 複寫輸送量改進](https://technet.microsoft.com/library/dn535775.aspx)|將最大 AD 複寫輸送量由 40Mbps 調整為約 600 Mbps|  

## <a name="BKMK_WhatsNewAD"></a>Windows Server 2012 中 AD DS 的新功能

下表摘要說明 Windows Server 2012 中的 AD DS 新功能，並附上含有詳細資訊的連結。 如需某些功能的詳細說明，包括其需求，請參閱[Active Directory Domain Services （AD DS）中的新](https://technet.microsoft.com/library/hh831477.aspx)功能。  
  
|功能|說明|  
|-----------|---------------|  
|Active Directory 型啟用 (AD BA)；請參閱 [大量啟用概觀](https://technet.microsoft.com/library/hh831612.aspx)|簡化大量軟體授權發佈和管理的設定工作。|  
|[Active Directory 同盟服務（AD FS）](https://technet.microsoft.com/library/hh831502.aspx)|透過伺服器管理員新增角色安裝、簡化信任設定、自動化信任管理、SAML 通訊協定支援等等。|  
|Active Directory 遺失頁面排清事件|NTDS ISAM 事件 530 與 Jet 錯誤 -1119 的記錄是偵測 Active Directory 資料庫的遺失頁面排清事件。|  
|[Active Directory 回收站使用者介面](https://technet.microsoft.com/library/hh831702.aspx#ad_recycle_bin_mgmt)|Active Directory 管理中心 (ADAC) 為原本由 Windows Server 2008 R2 引入的資源回收筒功能新增了 GUI 管理。|  
|[Active Directory 複寫和拓撲 Windows PowerShell Cmdlet](https://technet.microsoft.com/library/hh831757.aspx)|支援使用 Windows PowerShell 建立和管理 Active Directory 站台、站台連結、連線物件等等。|  
|[動態存取控制](https://technet.microsoft.com/library/hh831717.aspx)|增強傳統存取控制模型的新宣告型授權平台。|  
|[更細緻的密碼原則使用者介面](https://technet.microsoft.com/library/hh831702.aspx#fine_grained_pswd_policy_mgmt)|ADAC 為原本在 Windows Server 2008 加入的 PSO 建立、編輯和指派新增了 GUI 支援。|  
|[群組受管理的服務帳戶（gMSA）](https://technet.microsoft.com/library/hh831782.aspx)|稱為 gMSA 的新安全性主體類型 在多個主機上執行的服務可以在同一個 gMSA 帳戶下執行。|  
|[DirectAccess 離線網域加入](https://technet.microsoft.com/library/jj574150.aspx)|透過包含 DirectAccess 先決條件以擴充離線網域加入功能。|  
|[透過虛擬網域控制站（DC）複製快速部署](https://technet.microsoft.com/library/hh831734.aspx#virtualized_dc_cloning)|虛擬 DC 可以使用 Windows PowerShell Cmdlet 複製現有的虛擬網域控制站，進行快速部署。|  
|[RID 集區變更](https://technet.microsoft.com/library/jj574229.aspx)|新增監視事件與配額，以確保全域 RID 集區不會過度消耗。 如果原始集區已用盡，可選擇性的加倍全域 RID 集區的大小。|  
|保護時間服務的安全|移除有線密碼、移除 MD5 雜湊函數，以及要求伺服器與 Windows 8 時間用戶端進行驗證，加強 W32tm 的安全性。|  
|[虛擬 Dc 的 USN 復原保護](https://technet.microsoft.com/library/hh831734.aspx#safe_virt_dc)|意外地還原虛擬 DC 的快照備份時，不會再造成 USN 復原。|  
|[Windows PowerShell 歷程記錄檢視器](https://technet.microsoft.com/library/hh831702.aspx#windows_powershell_history_viewer)|使用 ADAC 時，系統管理員可以檢視執行的 Windows PowerShell 命令。|  
  
### <a name="BKMK_"></a>套用更新後，自動維護和重新開機行為的變更 Windows Update

在 Windows 8 版本之前，Windows Update 會管理其內部排程以檢查更新，並下載及安裝它們。 這需要始終在背景執行 Windows Update 代理程式，因而會耗用記憶體和其他系統資源。  
  
Windows 8 與 Windows Server 2012 引進一項名為 [自動維護](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx)的新功能。 自動維護合併了許多不同的功能，這些功能是分別用來管理它自己的排程及執行邏輯。 這項合併可以讓所有這些元件使用最少的系統資源、穩定運作、尊重新裝置類型的新 [連線待命](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) 狀態，並使用較少的可攜式裝置電池電力。  
  
因為 Windows Update 是 Windows 8 和 Windows Server 2012 中自動維護的一部分，其本身設定日期和時間來安裝更新的內部排程已不再有效。 為協助確保您企業中的所有裝置與電腦 (包括執行 Windows 8 與 Windows Server 2012 的所有裝置與電腦) 有一致且可預期的重新啟動行為，您可以設定下列群組原則設定：  

- **電腦設定 |原則 |系統管理範本 |Windows 元件 |Windows Update |設定自動更新**  
- **電腦設定 |原則 |系統管理範本 |Windows 元件 |Windows Update |已登入的使用者不會自動重新開機**  
- **電腦設定 |原則 |系統管理範本 |Windows 元件 |維護排程器 |維護隨機延遲**  

下表列出如何設定這些設定以提供所需的重新啟動行為的範例。  

|||  
|-|-|  
|**案例**|**建議的設定**|  
|**受 WSUS 管理**<br /><br />-每週安裝一次更新<br />-在晚上11點時重新開機星期五|設定機器自動安裝，防止在需要的時間之前重新開機<br /><br />**原則**：設定自動更新 (啟用)<br /><br />設定自動更新： 4-自動下載和排程安裝<br /><br />**原則**：不會自動重新開機已登入的使用者（已停用）<br /><br />**WSUS 期限**：設定為星期五晚上 11 點|  
|**受 WSUS 管理**<br /><br />-在不同的小時/天內錯開安裝|為應該一起更新的不同電腦群組設定目標群組<br /><br />為先前的案例使用上述步驟<br /><br />為不同的目標群組設定不同期限|  
|**不受 WSUS 管理-不支援期限**<br /><br />-在不同時間錯開安裝|**原則**：設定自動更新 (啟用)<br /><br />設定自動更新： 4-自動下載和排程安裝<br /><br />**登錄機碼：** 如需啟用登錄機碼的資訊，請參閱 Microsoft 知識庫文章 [2835627](https://support.microsoft.com/kb/2835627)<br /><br />**原則：** 自動維護隨機延遲 (啟用)<br /><br />將 [定期維護隨機延遲] 設為 PT6H 以設定 6 小時的隨機延遲，可提供下列行為：<br /><br />-更新將會在設定的維護時間加上隨機延遲後進行安裝。<br /><br />-每台機器的重新開機只會在3天后進行<br /><br />或者，為每個電腦群組設定不同的維護時間|  

如需 Windows 工程小組為何實作這些變更的詳細資訊，請參閱 [最小化在 Windows Update 執行自動更新後的重新啟動次數](https://blogs.msdn.com/b/b8/archive/2011/11/14/minimizing-restarts-after-automatic-updating-in-windows-update.aspx)。  

## <a name="BKMK_InstallationChanges"></a>AD DS 伺服器角色安裝變更

從 Windows Server 2003 到 Windows Server 2008 R2，您都是在執行 Active Directory 安裝精靈 Dcpromo.exe 之前執行 x86 或 X64 版本的 Adprep.exe 命令列工具，而且 Dcpromo.exe 擁有選擇性變數可從媒體安裝或進行自動安裝。  
  
自 Windows Server 2012 起，命令列安裝是使用 Windows PowerShell 的 ADDSDeployment 模組執行。 GUI 升級是在伺服器管理員中使用全新的 AD DS 設定精靈執行。 為簡化安裝程序，ADPREP 已經整合在 AD DS 安裝之中，視需要自動執行。 以 Windows PowerShell 為基礎的 AD DS Configuration Wizard 會自動以新增 Dc 的網域中的架構和結構主機角色為目標，然後在相關的網域控制站上遠端執行必要的 ADPREP 命令。  
  
AD DS 安裝精靈的先決條件檢查會在安裝開始前識別可能的錯誤。 您可以更正錯誤狀況，以排除升級不完全的問題。 此精靈也可匯出包含圖形化安裝期間指定之所有選項的 Windows PowerShell 指令碼。  
  
綜合以上所述，AD DS 安裝變更簡化了 DC 角色安裝程序，而且降低管理錯誤的發生，特別是當您跨通用區域和網域部署多個網域控制站時。  
更多 GUI 和 Windows PowerShell 安裝的詳細資訊，包含命令列語法和逐步精靈指示，請參閱 [安裝 Active Directory 網域服務](https://technet.microsoft.com/library/hh472162.aspx)。 系統管理員如果希望控制在 Active Directory 樹系 (與現有樹系中的 Windows Server 2012 DC 安裝無關) 採用的架構變更，還是可以在提升權限的命令提示字元執行 Adprep.exe 命令。  

## <a name="BKMK_DeprecatedFeatures"></a>Windows Server 2012 中與 AD DS 相關的已淘汰功能和行為變更

以下是與 AD DS 有關的變更：  

- **取代 Adprep32.exe**
   - Adprep.exe 只有一個版本，可以視需要在執行 Windows Server 2008 或更新版本的 64 位元伺服器執行。 它可以在遠端執行，而且如果目標作業主機角色裝載在 32 位元作業系統或 Windows Server 2003 上，就必須要在遠端執行。  
- **Dcpromo.exe 的取代**
   - 雖然在 Windows Server 2012 中，Dcpromo 已被取代，但仍可使用回應檔案或命令列參數來執行，讓組織有時間將現有的自動化轉換成新的 Windows PowerShell 安裝選項。  
- **使用者帳戶上的 Lm 已停用**
  - Windows Server 2008、Windows Server 2008 R2 和 Windows Server 2012 安全性範本的安全預設會啟用 NoLMHash 原則，此原則在 Windows 2000 和 Windows Server 2003 網域控制站的安全性範本為停用。 可使用知識庫文章 [946405](https://support.microsoft.com/kb/946405)中的步驟，視需要停用與 LMHash 相依之用戶端的 NoLMHash 原則。  

從 Windows Server 2008 開始，相較于執行 Windows Server 2003 或 Windows 2000 的網域控制站，網域控制站也具有下列安全預設設定。

|||||  
|-|-|-|-|  
|加密類型或原則|Windows Server 2008 預設值|Windows Server 2012 和 Windows Server 2008 R2 預設值|留言|  
|AllowNT4Crypto|停用|停用|協力廠商伺服器訊息區 (SMB) 用戶端可能與網域控制站上的安全預設設定不相容。 在所有情況下，這些設定可放寬以允許交互操作性，但同時也會產生安全風險。 如需詳細資訊，請參閱 Microsoft 知識庫中的[文章 942564](https://go.microsoft.com/fwlink/?LinkId=164558) （ https://go.microsoft.com/fwlink/?LinkId=164558) 。|  
|DES|Enabled|停用|Microsoft 知識庫中的[文章 977321](https://go.microsoft.com/fwlink/?LinkId=177717) （ https://go.microsoft.com/fwlink/?LinkId=177717)|  
|CBT/整合式驗證的擴充保護|無|Enabled|請參閱 microsoft 資訊[安全摘要報告（937811）](https://go.microsoft.com/fwlink/?LinkId=164559) （ https://go.microsoft.com/fwlink/?LinkId=164559) 和 microsoft 知識庫中的[文章 976918](https://go.microsoft.com/fwlink/?LinkId=178251) （ https://go.microsoft.com/fwlink/?LinkId=178251) 。<br /><br />如有需要，請在 Microsoft 知識庫[文章 977073](https://go.microsoft.com/fwlink/?LinkId=186394) （ https://go.microsoft.com/fwlink/?LinkId=186394) 中查看並安裝此修補程式。|  
|LMv2|Enabled|停用|Microsoft 知識庫中的[文章 976918](https://go.microsoft.com/fwlink/?LinkId=178251) （ https://go.microsoft.com/fwlink/?LinkId=178251)|  

## <a name="BKMK_SysReqs"></a>作業系統需求

下表列出 Windows Server 2012 的最低系統需求。 如需系統需求及預先安裝資訊的詳細資訊，請參閱 [安裝 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。 安裝新 Active Directory 樹系沒有額外的系統需求，但是您應該增加足夠的記憶體用於快取 Active Directory 資料庫的內容，以便提升網域控制站、LDAP 用戶端要求和 Active Directory 應用程式的效能。 如果您要升級現有網域控制站或將新的網域控制站新增到現有的樹系，請參閱下一節以確保伺服器符合磁碟空間需求。  

|||  
|-|-|  
|處理者|1.4 Ghz 64 位元處理器|  
|RAM|512 MB|  
|可用磁碟空間需求|32 GB|  
|螢幕解析度|800 x 600 或更高|  
|其他|DVD 光碟機、鍵盤、網際網路存取|  

### <a name="BKMK_DiskSpaceDCWin8"></a>升級網域控制站的磁碟空間需求

本節只涵蓋從 Windows Server 2008 或 Windows Server 2008 R2 升級網域控制站的磁碟空間需求。 如需將網域控制站升級到舊版 Windows Server 之磁碟空間需求的詳細資訊，請參閱 [升級到 Windows Server 2008 的磁碟空間需求](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008) 或 [升級到 Windows Server 2008 R2 的磁碟空間需求](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008R2)。  
  
估計裝載 Active Directory 資料庫和記錄檔之磁碟的大小，這個大小必須能夠容納自訂和應用程式驅動的架構延伸、應用程式和由系統管理員起始的索引，還需要網域控制站部署存留期 (通常為 5 到 8 年) 新增到目錄之物件和屬性所需的空間。 與部署之後擴充磁碟存放裝置所需的更多成本相較之下，在部署階段決定正確的大小是一項非常有價值的投資。 如需詳細資訊，請參閱 [Active Directory 網域服務容量規劃](https://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx)。  
  
在計劃升級的網域控制站上，確定裝載了 Active Directory 資料庫 (NTDS.DIT) 的磁碟機擁有至少相當於 20% NTDS.DIT 檔案的可用磁碟空間，然後才開始作業系統升級。 如果磁碟區上沒有足夠的可用磁碟空間，升級會失敗，而且升級相容性報告會傳回錯誤，指示可用磁碟空間不足：  
  
在這個情況下，您可以嘗試離線磁碟重組 Active Directory 資料庫以重新抓取額外的空間，然後重試升級。 如需詳細資訊，請參閱 [壓縮目錄資料庫檔案 (離線磁碟重組)](https://technet.microsoft.com/library/cc794920(v=WS.10).aspx)。  
  
### <a name="available-skus"></a>可用的 SKU

有 4 個版本的 Windows Server：Foundation、Essentials、Standard 及 Datacenter。
Standard 和 Datacenter 這兩個版本可支援 AD DS 角色。  
  
在之前的版本中，Windows Server 版本的差異在於伺服器角色的支援、處理器計數以及大型記憶體支援。 Standard 和 Datacenter 版本的 Windows Server 支援所有功能和基礎硬體，但不同于其虛擬化許可權-Standard edition 允許兩個虛擬實例，而資料中心允許無限制的虛擬實例公告.  
  
### <a name="windows-client-and-windows-server-operating-systems-that-are-supported-to-join-windows-server-domains"></a>可加入 Windows Server 網域的 Windows 用戶端和 Windows Server 作業系統

網域成員電腦與執行 Windows Server 2012 或更新版本的網域控制站支援以下 Windows 用戶端和 Windows Server 作業系統：  
  
- 用戶端作業系統： Windows 8.1、Windows 8、Windows 7、Windows Vista
   - 執行 Windows 8.1 或 Windows 8 的電腦也可加入網域控制站執行舊版 Windows Server (包括 Windows Server 2003 或更新版本) 的網域中。 不過，在此情況下，某些 Windows 8 功能可能需要額外的設定或可能無法使用。 如需這些功能和管理舊版網域中 Windows 8 用戶端的其他建議的詳細資訊，請參閱 [在 Windows Server 2003 網域中執行 Windows 8 成員電腦](https://social.technet.microsoft.com/wiki/contents/articles/17361.running-windows-8-member-computers-in-windows-server-2003-domains.aspx)。  
- 伺服器作業系統：Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 R2 及 Windows Server 2003  

## <a name="BKMK_UpgradePaths"></a>支援的就地升級路徑

執行64位版本的 Windows Server 2008 或 Windows Server 2008 R2 的網域控制站可以升級至 Windows Server 2012。 您不能升級執行 Windows Server 2003 或 32 位元版本 Windows Server 2008 的網域控制站。 若要取代它們，請在網域中安裝執行更新版 Windows Server 的網域控制站，然後移除執行 Windows Server 2003 的網域控制站。  

|如果您執行這些版本|您可以升級到這些版本|  
|-------------------------------------|-------------------------------------|  
|Windows Server 2008 Standard (含 SP2)<br /><br />或者<br /><br />Windows Server 2008 Enterprise (含 SP2)|Windows Server 2012 Standard<br /><br />或者<br /><br />Windows Server 2012 Datacenter|  
|Windows Server 2008 Datacenter (含 SP2)|Windows Server 2012 Datacenter|  
|Windows Web Server 2008|Windows Server 2012 Standard|  
|Windows Server 2008 R2 Standard (含 SP1)<br /><br />或者<br /><br />Windows Server 2008 R2 Enterprise (含 SP1)|Windows Server 2012 Standard<br /><br />或者<br /><br />Windows Server 2012 Datacenter|  
|Windows Server 2008 R2 Datacenter (含 SP1)|Windows Server 2012 Datacenter|  
|Windows Web Server 2008 R2|Windows Server 2012 Standard|  
  
如需支援的升級路徑詳細資訊，請參閱 [適用於 Windows Server 2012 的評估版與升級選項](https://go.microsoft.com/fwlink/?LinkId=260917)。 請注意，您不能將執行評估版 Windows Server 2012 的網域控制站直接轉換為零售版。 您必須在執行零售版的伺服器上安裝另一個網域控制站，然後從在評估版執行的網域控制站移除 AD DS。  
  
由於已知問題，您無法將執行 Windows Server 2008 R2 之 Server Core 安裝的網域控制站升級至 Windows Server 2012 的 Server Core 安裝。 在升級程序後期，升級會當機並呈現全黑的螢幕。 重新啟動這類 DC 會在 boot.ini 檔案中看到一個選項，回復到之前的作業系統版本。 再次重新開機會觸發自動回復到之前的作業系統版本。 在有解決方案可用之前，建議您安裝新的網域控制站，執行 Windows Server 2012 的 Server Core 安裝，而不是就地升級執行 Windows Server 之 Server Core 安裝的現有網域控制站。2008 R2。 如需詳細資訊，請參閱知識庫文章 [2734222](https://support.microsoft.com/kb/2734222)。  

## <a name="BKMK_FunctionalLevels"></a>功能等級功能和需求

Windows Server 2012 需要 Windows Server 2003 樹系功能等級。 也就是說，在您可以將執行 Windows Server 2012 的網域控制站新增至現有的 Active Directory 樹系之前，樹系功能等級必須是 Windows Server 2003 或更高的版本。 這表示執行 Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 的網域控制站可以在相同的樹系中運作，但不支援執行 Windows 2000 Server 的網域控制站，而且會封鎖執行 Windows Server 2012 之網域控制站的安裝。 如果樹系包含執行 Windows Server 2003 或更新版本的網域控制站，但樹系功能等級還是 Windows 2000，也會封鎖安裝。  
  
Windows 2000 網域控制站必須先行移除，才能將 Windows Server 2012 網域控制站新增到您的樹系。 在這種情況下，請考慮下列工作流程：  

1. 安裝執行 Windows Server 2003 或更新版本的網域控制站。 可以在評估版的 Windows Server 上部署這些網域控制站。 這個步驟還有一個先決條件，就是需要針對該作業系統版本 [執行 adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx) 。  
2. 移除 Windows 2000 網域控制站。 具體來說，就是以正常方式降級或以強制方式從網域和使用的 Active Directory 使用者和電腦移除 Windows Server 2000 網域控制站，藉此移除所有已移除之網域控制站的網域控制站帳戶。  
3. 將樹系功能等級提升至 Windows Server 2003 或更高。  
4. 安裝執行 Windows Serer 2012 的網域控制站。  
5. 移除執行舊版 Windows Server 的網域控制站。  

新的 Windows Server 2012 網域功能等級可提供一項新功能：**宣告、複合驗證和 Kerberos**防護 kdc 系統管理範本原則的 kdc 支援有兩個設定（**一律提供宣告**和**失敗未受防護驗證要求**），其需要 Windows Server 2012 網域功能等級。  
  
Windows Server 2012 樹系功能等級不提供任何新功能，但可確保樹系中建立的任何新網域都能自動在 Windows Server 2012 網域功能等級上運作。 Windows Server 2012 網域功能等級不提供宣告、複合驗證和 Kerberos 防護的 KDC 支援以外的其他新功能。 但它可以確保網域中的任何網域控制站都執行 Windows Server 2012。 如需不同功能等級可用之其他功能的詳細資訊，請參閱 [了解 Active Directory 網域服務 (AD DS) 功能等級](../active-directory-functional-levels.md)。  
  
將樹系功能等級設定為某個值之後，就無法回復或降低樹系功能等級，下列情況除外：將樹系功能等級提高至 Windows Server 2012 之後，您可以將它降低為 Windows Server 2008 R2。 如果 Active Directory 回收站尚未啟用，您也可以將樹系功能等級從 Windows Server 2012 降低到 Windows Server 2008 R2 或 Windows Server 2008，或從 Windows Server 2008 R2 降回 Windows Server 2008。 例如，如果樹系功能等級設定為 Windows Server 2008 R2，就無法回復到 Windows Server 2003。  
  
將網域功能等級設定為某個值之後，您就無法回復或降低網域功能等級，但有下列例外狀況：當您將網域功能等級提高至 Windows Server 2008 R2 或 Windows Server 2012 時，以及樹系函數nal 層級是 Windows Server 2008 或更低版本，您可以選擇將網域功能等級回復至 Windows Server 2008 或 Windows Server 2008 R2。 您只能將網域功能等級從 Windows Server 2012 降低到 Windows Server 2008 R2 或 Windows Server 2008，或從 Windows Server 2008 R2 降到 Windows Server 2008。 例如，如果網域功能等級設定為 Windows Server 2008 R2，就無法回復至 Windows Server 2003。  
  
如需較低功能等級上可用之功能的詳細資訊，請參閱 [了解 Active Directory 網域服務 (AD DS) 功能等級](../active-directory-functional-levels.md)。  
  
除了功能等級之外，執行 Windows Server 2012 的網域控制站還提供執行舊版 Windows Server 之網域控制站上無法使用的其他功能。 例如，執行 Windows Server 2012 的網域控制站可以用於虛擬網域控制站複製，而執行舊版 Windows Server 的網域控制站則不能。 但是 Windows Server 2012 中的虛擬網域控制站複製和虛擬網域控制站保護沒有任何功能等級的需求。  

> [!NOTE]  
> Microsoft Exchange Server 2013 需要 Windows Server 2003 或更高的樹系功能等級。  

## <a name="BKMK_ServerRoles"></a>AD DS 與其他伺服器角色和 Windows 作業系統的互通性

下列 Windows 作業系統不支援 AD DS：  
  
- Windows MultiPoint Server  
- Windows Server 2012 Essentials  
  
AD DS 無法安裝在同時執行下列伺服器角色或角色服務的伺服器上：  
  
- Hyper-V 伺服器  
- 遠端桌面連線代理人  
  
## <a name="BKMK_OpsMasters"></a>操作主機角色

Windows Server 2012 中的一些新功能會影響操作主機角色：  

- PDC 模擬器必須執行 Windows Server 2012，才能支援複製虛擬網域控制站。 複製 DC 需要額外的先決條件。 如需詳細資訊，請參閱 [Active Directory 網域服務 (AD DS) 虛擬化](https://technet.microsoft.com/library/hh831734.aspx)。  
- 當 PDC 模擬器執行 Windows Server 2012 時，會建立新的安全性主體。  
- RID 主機具有新的 RID 發行和監視功能。 這些改進包括更好的事件記錄、更適當的限制，以及 (在緊急情況下) 將整體 RID 集區配置提高一些的能力。 如需詳細資訊，請參閱 [Managing RID Issuance](../../ad-ds/manage/Managing-RID-Issuance.md)。  

> [!NOTE]  
> 雖然它們不是操作主機角色，但 AD DS 安裝的另一項變更是，預設會在執行 Windows Server 2012 的所有網域控制站上安裝 DNS 伺服器角色和通用類別目錄。  

## <a name="BKMK_Virtual"></a>虛擬化網域控制站

從 Windows Server 2012 開始的 AD DS 增強功能可讓網域控制站的虛擬化更安全，以及複製網域控制站的能力。 複製網域控制站的能力接著能夠在新的網域中快速部署其他網域控制站並提供其他好處。 如需詳細資訊，請參閱[Active Directory Domain Services &#40;AD DS&#41;虛擬&#40;化層&#41;級100的簡介](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md)。  

## <a name="BKMK_Admin"></a>Windows Server 2012 伺服器的管理

使用[windows 8 的遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=28972)來管理網域控制站和執行 windows Server 2012 的其他伺服器。 您可以在執行 Windows 8 的電腦上執行 Windows Server 2012 遠端伺服器管理工具。  

## <a name="BKMK_AppCompat"></a>應用程式相容性

下表涵蓋常見的整合 Active Directory Microsoft 應用程式。 表格內容包含應用程式可以安裝在哪些版本的 Windows Server 上，以及採用 Windows Server 2012 DC 是否會對應用程式相容性產生影響。  

|產品|備註|  
|-----------|---------|  
|[Microsoft SharePoint 2010](https://support.microsoft.com/kb/2724471)|需要 SharePoint 2010 Service Pack 2，才能在 <br />Windows Server 2012 伺服器上 安裝和操作 SharePoint 2010<br /><br />在 Windows Server 2012 伺服器上安裝和操作 SharePoint 2010 Foundation 需要有 SharePoint 2010 Foundation Service Pack 2<br /><br />SharePoint Server 2010 (不含 Service Pack) 安裝程序在 Windows Server 2012 上會失敗<br /><br />SharePoint Server 2010 必要條件安裝程式（Prerequisiteinstaller.exe）失敗，並出現「此程式有相容性問題」錯誤。 按一下 [執行程式但不取得協助]，會顯示錯誤「正在驗證 SharePoint 是否可以&#124;安裝 sharepoint server 2010 （不含 service pack），無法安裝在 Windows Server 2012 上」。|  
|[Microsoft SharePoint 2013](https://technet.microsoft.com/library/cc262485(v=office.15).aspx)|伺服器陣列中的資料庫伺服器最低需求：<br /><br />64 位元版本的 Windows Server 2008 R2 Service Pack 1 (SP1) Standard、Enterprise 或 Datacenter，或是 64 位元版本的 Windows Server 2012 Standard 或 Datacenter<br /><br />含內建資料庫的單一伺服器最低需求：<br /><br />64 位元版本的 Windows Server 2008 R2 Service Pack 1 (SP1) Standard、Enterprise 或 Datacenter，或是 64 位元版本的 Windows Server 2012 Standard 或 Datacenter<br /><br />伺服器陣列中的前端網頁伺服器和應用程式伺服器最低需求：<br /><br />64 位元版本的 Windows Server 2008 R2 Service Pack 1 (SP1) Standard、Enterprise 或 Datacenter，或是 64 位元版本的 Windows Server 2012 Standard 或 Datacenter。|  
|[Configuration Manager 2012](https://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|Configuration Manager 2012 Service Pack 1：<br /><br />Microsoft 會在發行 Service Pack 1 時，在我們的用戶端支援基礎新增下列作業系統：<br /><br />-Windows 8 專業版<br />-Windows 8 企業版<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter<br /><br />所有站台伺服器角色 (包含站台伺服器、SMS 提供者和管理點) 都可以部署在含下列作業系統版本的伺服器：<br /><br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|  
|[Microsoft 端點 Configuration Manager （最新分支）](https://docs.microsoft.com/configmgr/core/plan-design/configs/supported-configurations)|[Configuration Manager 網站系統伺服器支援的作業系統](https://docs.microsoft.com/configmgr/core/plan-design/configs/supported-operating-systems-for-site-system-servers)。|  
|[Microsoft Lync Server 2013](https://technet.microsoft.com/library/gg412883.aspx)|Lync Server 2013 需要搭配 Windows Server 2008 R2 或 Windows Server 2012。 它不能在 Server Core 安裝上執行， 可以在 [虛擬伺服器](https://technet.microsoft.com/library/gg399035.aspx)上執行。|  
|[Lync Server 2010](https://support.microsoft.com/kb/2777359)|如果安裝了 [Lync Server 的 2012 年 10 月累計更新](https://support.microsoft.com/?kbid=2493736) ，Lync Server 2010 便無法安裝在新的 (而非升級的) Windows Server 2012 安裝。 不支援將現有 Lync Server 2010 安裝的作業系統升級到 Windows Server 2012。 Windows Server 2012 也不支援 Microsoft Lync Server 2010 群組聊天伺服器。|  
|[System Center 2012 Endpoint Protection](https://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|System Center 2012 Endpoint Protection Service Pack 1 會更新用戶端支援基礎，以包含下列作業系統：<br /><br />-Windows 8 專業版<br />-Windows 8 企業版<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|  
|[System Center 2012 Forefront Endpoint Protection](https://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|FEP 2010 含更新彙總套件 1 會更新用戶端支援基礎，以包含下列作業系統：<br /><br />-Windows 8 專業版<br />-Windows 8 企業版<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|  
|Forefront Threat Management Gateway (TMG)|僅支援 TMG 在 Windows Server 2008 和 Windows Server 2008 R2 上執行。 如需詳細資訊，請參閱 [Forefront TMG 的系統需求](https://technet.microsoft.com/library/dd896981.aspx)。|  
|Windows Server Update Services|這個版本的 WSUS 已經可以支援 Windows 8 電腦或 Windows Server 2012 電腦做為用戶端。|  
|Windows Server Update Services 3.0|更新知識庫文章[2734608](https://support.microsoft.com/kb/2734608)讓執行 WINDOWS SERVER UPDATE SERVICES （WSUS） 3.0 SP2 的伺服器能夠更新執行 Windows 8 或 Windows Server 2012：**注意：** 具有獨立 wsus 3.0 SP2 環境或 Configuration Manager 2007 Service PACK 2 環境搭配 WSUS 3.0 SP2 的客戶需要[2734608](https://support.microsoft.com/kb/2734608)以適當的方式管理以 windows 8 為基礎的電腦或以 windows Server 2012 為基礎的電腦做為用戶端。|  
|[Exchange 2013](https://technet.microsoft.com/library/bb691354.aspx)|Windows Server 2012 Standard 和 Datacenter 支援下列角色：架構主機、通用類別目錄伺服器、網域控制站、信箱和用戶端存取伺服器角色<br /><br />樹系功能等級：Windows Server 2003 或更新版本<br /><br />來源：Exchange 2013 系統需求|  
|Exchange 2010|[來源： Exchange 2010 Service Pack 3](https://blogs.technet.com/b/exchange/archive/2012/09/25/announcing-exchange-2010-service-pack-3.aspx)<br /><br />Exchange 2010 (含 Service Pack 3) 可以安裝在 Windows Server 2012 成員伺服器上。<br /><br />[Exchange 2010 系統需求](https://technet.microsoft.com/library/aa996719(EXCHG.141).aspx) 會列出最新支援的架構主機、通用類別目錄和網域控制站，如同 Windows Server 2008 R2。<br /><br />樹系功能等級：Windows Server 2003 或更新版本|  
|SQL Server 2012|來源：KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Windows Server 2012 支援 SQL Server 2012 RTM。|  
|SQL Server 2008 R2|來源：KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />需要 SQL Server 2008 R2 (含 Service Pack 1) 或更新版本才能安裝在 Windows Server 2012。|  
|SQL Server 2008|來源：KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />需要 SQL Server 2008 (含 Service Pack 3) 或更新版本才能安裝在 Windows Server 2012。|  
|SQL Server 2005|來源：KB [2681562](https://support.microsoft.com/kb/2681562)<br /><br />不支援安裝在 Windows Server 2012。|  

## <a name="BKMK_KnownIssues"></a>已知問題

下表列出與 AD DS 安裝相關的已知問題。  

||||  
|-|-|-|  
|知識庫文章編號和標題|受影響的技術領域|問題/描述|  
|[2830145](https://support.microsoft.com/kb/2830145)：SID S-1-18-1 與 SID S-1-18-2 無法對應在網域環境中的 Windows 7 或 Windows Server 2008 R2 電腦上|AD DS 管理/應用程式相容|對應 SID S-1-18-1 與 SID S-1-18-2 的應用程式 (Windows Server 2012 中的新功能) 可能會失敗，因為 SID 無法在 Windows 7 或 Windows Server 2008 R2 電腦上解析。 如果要解決這個問題，請在網域的 Windows 7 與 Windows Server 2008 R2 電腦上安裝 Hotfix。|  
|[2737129](https://support.microsoft.com/kb/2737129)：當您自動準備 Windows Server 2012 的現有網域時不會執行群組原則準備|AD DS 安裝|安裝第一個在網域中執行 Windows Server 2012 的 DC 時，不會自動執行 Adprep /domainprep /gpprep。 如果之前從未在網域執行過，必須手動執行。|  
|[2737416](https://support.microsoft.com/kb/2737416)：Windows PowerShell 網域控制站部署重複出現警告|AD DS 安裝|警告可能在先決條件驗證期間出現，在安裝時又重複出現。|  
|[2737424](https://support.microsoft.com/kb/2737424)：當您嘗試從網域控制站移除 Active Directory 網域服務時發生「指定的網域名稱格式不正確」錯誤|AD DS 安裝|如果您移除網域中的最後一個 DC，但該網域中預先建立的 RODC 帳戶仍然存在，就會出現此錯誤。 這會影響 Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008。|  
|[2737463](https://support.microsoft.com/kb/2737463)：網域控制站未啟動、發生 c00002e2 錯誤或顯示「選擇選項」|AD DS 安裝|因為系統管理員使用 Dism.exe、Pkgmgr.exe 或 Ocsetup.exe 移除 DirectoryServices-DomainController 角色，所以 DC 沒有啟動。|  
|[2737516](https://support.microsoft.com/kb/2737516)：Windows Server 2012 伺服器管理員的 IFM 驗證限制|AD DS 安裝|IFM 驗證有所限制，如同知識庫文章中的說明。|  
|[2737535](https://support.microsoft.com/kb/2737535)：Install-AddsDomainController Cmdlet 傳回 RODC 的參數設定錯誤|AD DS 安裝|如果您指定的引數已經填入預先建立的 RODC 帳戶，當您嘗試將伺服器連結到 RODC 帳戶時會收到錯誤。|  
|[2737560](https://support.microsoft.com/kb/2737560)：「無法執行 Exchange 架構衝突檢查」錯誤和先決條件檢查失敗|AD DS 安裝|當您在現有的網域中設定第一個 Windows Server 2012 DC 時，因為 DC 遺失網路服務的 SeServiceLogonRight 或因為 WMI 或 DCOM 通訊協定被封鎖，先決條件檢查會失敗。|  
|[2737797](https://support.microsoft.com/kb/2737797)：AddsDeployment 模組搭配 -Whatif 引數顯示不正確的 DNS 結果|AD DS 安裝|-WhatIf 參數會顯示 DNS 伺服器將不會安裝，但會是。|  
|[2737807](https://support.microsoft.com/kb/2737807)：[網域控制站選項] 頁面的 [下一步] 按鈕無法使用|AD DS 安裝|因為目標 DC 的 IP 位址沒有對應到現有的子網路或站台，或是因為並未正確輸入和確認 DSRM 密碼，所以會停用 [網域控制站選項] 頁面的 [下一步] 按鈕。|  
|[2737935](https://support.microsoft.com/kb/2737935)：Active Directory 安裝在「建立 NTDS 設定物件」階段停止|AD DS 安裝|因為本機系統管理員密碼與網域系統管理員密碼相同，或因為網路問題而無法完成關鍵性複寫，因此安裝停滯。|  
|[2738060](https://support.microsoft.com/kb/2738060)：使用 Install-AddsDomain 遠端建立子網域時出現「拒絕存取」錯誤訊息|AD DS 安裝|如果 DNSDelegationCredential 密碼錯誤，當您使用 Invoke-Command Cmdlet 執行 Install-ADDSDomain 時，會收到這個錯誤。|  
|[2738697](https://support.microsoft.com/kb/2738697)：使用伺服器管理員設定伺服器時出現「無法操作伺服器」網域控制站設定錯誤|AD DS 安裝|當您嘗試在工作群組電腦安裝 AD DS 時，因為 NTLM 驗證停用，所以會收到這個錯誤。|  
|[2738746](https://support.microsoft.com/kb/2738746)：登入本機系統管理員網域帳戶之後收到拒絕存取錯誤|AD DS 安裝|當您使用本機系統管理員帳戶而不是內建系統管理員帳戶登入，然後建立新的網域，帳戶不會新增到 Domain Admins 群組。|  
|[2743345](https://support.microsoft.com/kb/2743345)：「系統找不到指定的檔案」Adprep /gpprep 錯誤，或工具損毀|AD DS 安裝|當您執行 adprep /gpprep 時，因為基礎結構主機實作一個不相鄰的命名空間，所以會收到這個錯誤。|  
|[2743367](https://support.microsoft.com/kb/2743367)：64 位元 Windows Server 2003 發生 Adprep「不是有效的 Win32 應用程式」錯誤|AD DS 安裝|因為 Windows Server 2012 Adprep 不能在 Windows Server 2003 上執行，所以會收到這個錯誤。|  
|[2753560](https://support.microsoft.com/kb/2753560)：Windows Server 2012 發生 ADMT 3.2 和 PES 3.1 安裝錯誤|ADMT|Windows Server 2012 的設計不能安裝 ADMT 3.2。|  
|[2750857](https://support.microsoft.com/kb/2750857)：Internet Explorer 10 無法正確顯示 DFS 複寫診斷報告|DFS 複寫|因為 Internet Explorer 10 有所變更，所以無法正確顯示 DFS 複寫診斷報告。|  
|[2741537](https://support.microsoft.com/kb/2741537)：使用者可以看見遠端群組原則更新|群組原則|這是因為排程工作會在每個登入的使用者內容執行。 Windows 工作排程器的設計在這種情況下需要互動式提示。|  
|[2741591](https://support.microsoft.com/kb/2741591)：GPMC 基礎結構狀態選項的 SYSVOL 中沒有 ADM 檔案|群組原則|GP 複寫可以報告「複寫進行中」，因為 GPMC 基礎結構狀態不會遵循自訂篩選規則。|  
|[2737880](https://support.microsoft.com/kb/2737880)：AD DS 設定期間發生「無法啟動服務」錯誤|虛擬 DC 複製|因為 DS 角色伺服器服務停用，所以安裝、移除 AD DS 或複製時會收到這個錯誤。|  
|[2742836](https://support.microsoft.com/kb/2742836)：使用 VDC 複製功能時會為每個網域控制站建立兩個 DHCP 租用|虛擬 DC 複製|因為複製的網域控制站會在複製之前和複製完成時各收到一個租用，所以會發生這種狀況。|  
|[2742844](https://support.microsoft.com/kb/2742844)：在 Windows Server 2012，網域控制站複製失敗且伺服器以 DSRM 重新啟動|虛擬 DC 複製|因為知識庫文章中所列的各種理由，複製的 DC 會因複製失敗而以 DSRM 啟動。|  
|[2742874](https://support.microsoft.com/kb/2742874)：網域控制站複製不會重新建立所有服務主體名稱|虛擬 DC 複製|因為網域重新命名程序的限制，所以複製的 DC 上不會重新建立某些協力廠商 SPN。|  
|[2742908](https://support.microsoft.com/kb/2742908)：複製網域控制站後發生「無可用的登入伺服器」錯誤|虛擬 DC 複製|因為複製虛擬 DC 失敗且 DC 以 DSRM 啟動，所以當您在複製之後嘗試登入時，會收到這個錯誤。 以 .\administrator 的身分登入可排除這個複製失敗。|  
|[2742916](https://support.microsoft.com/kb/2742916)：網域控制站複製失敗，dcpromo.log 中的錯誤為 8610|虛擬 DC 複製|PDC 模擬器未執行網域分割的輸入複寫，很可能是因為角色已移轉，所以複製失敗。|  
|[2742927](https://support.microsoft.com/kb/2742927)：「索引超出範圍」New-AdDcCloneConfig 錯誤|虛擬 DC 複製|複製虛擬 DC 時執行 New-ADDCCloneConfigFile Cmdlet 之後，因為不是從提升權限的命令提示字元執行 Cmdlet，或因為您的存取權杖不含 Administrators 群組，所以會收到這個錯誤。|  
|[2742959](https://support.microsoft.com/kb/2742959)：網域控制站複製失敗，錯誤為 8437：「指定了不正確的參數給這項複寫操作」|虛擬 DC 複製|因為指定無效的複製名稱或重複的 NetBIOS 名稱，所以複製失敗。|  
|[2742970](https://support.microsoft.com/kb/2742970)：DC 複製失敗，沒有 DSRM、重複來源和複製電腦|虛擬 DC 複製|因為沒有在正確的位置建立 DCCloneConfig.xml 檔案，或因為來源 DC 已經在複製之前重新啟動，所以複製的虛擬 DC 使用重複名稱做為來源 DC，以目錄服務修復模式 (DSRM) 啟動。|  
|[2743278](https://support.microsoft.com/kb/2743278)：網域控制站複製錯誤 0x80041005|虛擬 DC 複製|因為只指定一個 WINS 伺服器，所以複製的 DC 開機到 DSRM。 如果指定了 WINS 伺服器，也必須同時指定慣用和其他 WINS 伺服器。|  
|[2745013](https://support.microsoft.com/kb/2745013)：在 Windows Server 2012 執行 New-AdDcCloneConfigFile 會出現「無法操作伺服器」錯誤訊息|虛擬 DC 複製|因為伺服器無法連線到通用類別目錄伺服器，所以執行 New-ADDCCloneConfigFile Cmdlet 之後會收到這個錯誤。|  
|[2747974](https://support.microsoft.com/kb/2747974)：網域控制站複製事件 2224 提供不正確的指導|虛擬 DC 複製|事件識別碼 2224 不正確的說明必須在複製之前移除受管理的服務帳戶。 獨立 MSA 必須移除，但群組 MSA 不會封鎖複製。|  
|[2748266](https://support.microsoft.com/kb/2748266)：升級至 Windows 8 之後無法解除鎖定 BitLocker 加密的磁碟機|BitLocker|當您嘗試在已從 Windows 7 升級的電腦上解除鎖定磁片磁碟機時，會收到「找不到應用程式」錯誤。|  

## <a name="see-also"></a>請參閱

[Windows Server 2012 評估資源](https://technet.microsoft.com/evalcenter/hh708766.aspx)  
[Windows Server 2012 評估指南](https://download.microsoft.com/download/5/B/2/5B254183-FA53-4317-B577-7561058CEF42/WS%202012%20Evaluation%20Guide.pdf)  
[安裝和部署 Windows Server 2012](https://technet.microsoft.com/library/hh831620.aspx)  
