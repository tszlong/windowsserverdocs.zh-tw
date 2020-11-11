---
ms.assetid: e4c31187-f15f-410b-bb79-8d63e2f2b421
title: 將網域控制站升級為 Windows Server 2012 R2 與 Windows Server 2012
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.openlocfilehash: ebbbefebc420d83f8f74466698729c26395bdbec
ms.sourcegitcommit: b39ea3b83280f00e5bb100df0dc8beaf1fb55be2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/11/2020
ms.locfileid: "94520501"
---
# <a name="upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012"></a>將網域控制站升級為 Windows Server 2012 R2 與 Windows Server 2012

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題提供有關 Windows Server 2012 R2 和 Windows Server 2012 中 Active Directory Domain Services 的背景資訊，以及說明從 Windows Server 2008 或 Windows Server 2008 R2 升級網域控制站的程式。

## <a name="domain-controller-upgrade-steps"></a><a name="BKMK_UpgradeWorkflow"></a>網域控制站升級步驟

升級網域的建議方式是視需要升級執行較新版 Windows Server 的網域控制站，以及降級舊版網域控制站。 該方法是升級現有網域控制站之作業系統的慣用方法。 這份清單涵蓋您升級執行較新版本 Windows Server 的網域控制站之前，要遵循的一般步驟：

1. 確認目標伺服器符合 [系統需求](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303418(v=ws.11))。
2. 確認[應用程式相容性](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat)。
3. 確認安全性設定。 如需詳細資訊，請參閱 [Windows server 2012 中 AD DS 的已淘汰功能和行為變更](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_DeprecatedFeatures) ，以及 [windows Server 2008 和 Windows server 2008 R2 中的安全預設設定](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee522994(v=ws.10)#BKMK_SecureDefault)。
4. 檢查要執行安裝的電腦與目標伺服器的連線。
5. 檢查必要操作主機角色的可用性：

   - 若要在現有網域和樹系中安裝第一個執行 Windows Server 2012 的 DC，執行安裝的電腦必須連線到架構主機才能執行 adprep/forestprep 和基礎結構主機，以便執行 adprep/domainprep。
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

   您可以委派 AD DS 的安裝權限。 如需詳細資訊，請參閱 [安裝管理工作](/previous-versions/windows/it-pro/windows-server-2003/cc773327(v=ws.10))。

以下連結提供使用 Windows PowerShell Cmdlet 和伺服器管理員升級新增和複本 Windows Server 2012 網域控制站的逐步指示：

- [安裝 Active Directory 網域服務 (層級 100)](./install-active-directory-domain-services--level-100-.md)
- [安裝新的 Windows Server 2012 Active Directory 樹系 (等級 200)](./install-a-new-windows-server-2012-active-directory-forest--level-200-.md)
- [在現有網域中安裝複本 Windows Server 2012 網域控制站 (等級 200)](./install-a-replica-windows-server-2012-domain-controller-in-an-existing-domain--level-200-.md)
- [安裝新的 Windows Server 2012 Active Directory 子網域或樹狀目錄網域 (等級 200)](./install-a-new-windows-server-2012-active-directory-child-or-tree-domain--level-200-.md)
- [安裝 Windows Server 2012 Active Directory 唯讀網域控制站 (RODC) (等級 200)](./rodc/install-a-windows-server-2012-active-directory-read-only-domain-controller--rodc---level-200-.md)
- [有關網域控制站的 Windows Server 2012 論壇](/answers/topics/windows-server-2012.html)

## <a name="windows-update-considerations"></a>Windows Update 考慮

在 Windows 8 版本之前，Windows Update 會管理其內部排程以檢查更新，並下載及安裝它們。 這需要始終在背景執行 Windows Update 代理程式，因而會耗用記憶體和其他系統資源。

Windows 8 與 Windows Server 2012 引進一項名為 [自動維護](/windows/win32/w8cookbook/automatic-maintenance)的新功能。 自動維護合併了許多不同的功能，這些功能是分別用來管理它自己的排程及執行邏輯。 這項合併可以讓所有這些元件使用最少的系統資源、穩定運作、尊重新裝置類型的新 [連線待命](/windows/win32/w8cookbook/automatic-maintenance) 狀態，並使用較少的可攜式裝置電池電力。

因為 Windows Update 是 Windows 8 和 Windows Server 2012 中自動維護的一部分，其本身設定日期和時間來安裝更新的內部排程已不再有效。 為協助確保您企業中的所有裝置與電腦 (包括執行 Windows 8 與 Windows Server 2012 的所有裝置與電腦) 有一致且可預期的重新啟動行為，請參閱 Microsoft 知識庫文章 [2885694](https://support.microsoft.com/kb/2885694) (或參閱 2013 年 10 月的累積彙總套件 [2883201](https://support.microsoft.com/kb/2883201))，然後設定下列 WSUS 部落格文章中描述的原則設定： [為 Windows 8 與 Windows Server 2012 啟用更好預測的 Windows Update 體驗 (KB 2885694)](/archive/blogs/wsus/enabling-a-more-predictable-windows-update-experience-for-windows-8-and-windows-server-2012-kb-2885694)。

## <a name="whats-new-in-ad-ds-in-windows-server-2012-r2"></a><a name="BKMK_NewWS2012R2"></a>Windows Server 2012 R2 中 AD DS 有哪些新功能？

下表摘要說明 Windows Server 2012 R2 中的 AD DS 新功能，並附上含有詳細資訊的連結。 如需某些功能更詳細的說明，包含這些功能的需求，請參閱 [Windows Server 2012 R2 中 Active Directory 的新功能](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn268294(v=ws.11))。

|功能|描述|
|-----------|---------------|
|[加入工作場所](../../ad-fs/operations/join-to-workplace-from-any-device-for-sso-and-seamless-second-factor-authentication-across-company-applications.md)|可讓資訊工作者將其個人裝置與公司聯結，以存取公司資源和服務。|
|[Web 應用程式 Proxy](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280942(v=ws.11))|運用新的遠端存取角色服務提供 Web 應用程式的存取。|
|[Active Directory 同盟服務](../../active-directory-federation-services.md) \(英文\)|AD FS 簡化了部署與改良功能，讓使用者可以從個人裝置存取資源，並協助 IT 部門管理存取控制。|
|[SPN 和 UPN 的唯一性](../manage/component-updates/spn-and-upn-uniqueness.md)|執行 Windows Server 2012 R2 的網域控制站會阻止建立重複的服務主體名稱 (SPN) 與使用者主體名稱 (UPN)。|
|[Winlogon 自動重新啟動登入 (ARSO)](../manage/component-updates/winlogon-automatic-restart-sign-on--arso-.md)|讓鎖定螢幕應用程式可在 Windows 8.1 裝置上重新啟動並使用。|
|[TPM 金鑰證明](../manage/component-updates/tpm-key-attestation.md)|讓 CA 以密碼編譯方式證明發行的憑證中，憑證要求者私密金鑰實際上受信賴平台模組 (TPM) 的保護。|
|[認證保護和管理](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn408190(v=ws.11))|新的認證保護與網域驗證控制可減少認證被盜的機會。|
|[檔案複寫服務 (FRS) 過時](../manage/component-updates/directory-services-component-updates.md)|Windows Server 2003 網域功能等級也已被取代，因為在該功能等級中，FRS 是用來複寫 SYSVOL。 那表示當您在執行 Windows Server 2012 R2 的伺服器上建立新網域時，網域功能等級需為 Windows Server 2008 或更新版本。 您仍然可以將執行 Windows Server 2012 R2 的網域控制站新增到具有 Windows Server 2003 網域功能等級的現有網域;您只是無法在該層級建立新的網域。|
|[新網域與樹系功能等級](../active-directory-functional-levels.md)|Windows Server 2012 R2 有新的功能等級。 Windows Server 2012 R2 DFL 有新功能。|
|[LDAP 查詢最佳化工具變更](../manage/component-updates/directory-services-component-updates.md)|LDAP 對於複雜查詢的搜尋效率與搜尋時間的效能改善。|
|[1644 事件改進](../manage/component-updates/directory-services-component-updates.md)|LDAP 搜尋結果統計資料已加入事件識別碼 1644 以協助疑難排解。|
|[Active Directory 複寫輸送量改進](../manage/component-updates/directory-services-component-updates.md)|將最大 AD 複寫輸送量由 40Mbps 調整為約 600 Mbps|

## <a name="whats-new-in-ad-ds-in-windows-server-2012"></a><a name="BKMK_WhatsNewAD"></a>Windows Server 2012 中 AD DS 有哪些新功能？

下表摘要說明 Windows Server 2012 中的 AD DS 新功能，並附上含有詳細資訊的連結。 如需某些功能的詳細說明，包括它們的需求，請參閱 [Active Directory Domain Services (AD DS) 的新 ](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831477(v=ws.11))功能。

|功能|描述|
|-----------|---------------|
|Active Directory 型啟用 (AD BA)；請參閱 [大量啟用概觀](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831612(v=ws.11))|簡化大量軟體授權發佈和管理的設定工作。|
|[Active Directory Federation Services (AD FS)](../../active-directory-federation-services.md)|透過伺服器管理員新增角色安裝、簡化信任設定、自動化信任管理、SAML 通訊協定支援等等。|
|Active Directory 遺失頁面排清事件|NTDS ISAM 事件 530 與 Jet 錯誤 -1119 的記錄是偵測 Active Directory 資料庫的遺失頁面排清事件。|
|[Active Directory 資源回收筒使用者介面](../get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-.md#ad_recycle_bin_mgmt)|Active Directory 管理中心 (ADAC) 為原本由 Windows Server 2008 R2 引入的資源回收筒功能新增了 GUI 管理。|
|[Active Directory 複寫和拓撲 Windows PowerShell Cmdlet](../manage/powershell/introduction-to-active-directory-replication-and-topology-management-using-windows-powershell--level-100-.md)|支援使用 Windows PowerShell 建立和管理 Active Directory 站台、站台連結、連線物件等等。|
|[動態存取控制](../../solution-guides/dynamic-access-control--scenario-overview.md)|增強傳統存取控制模型的新宣告型授權平台。|
|[更細緻的密碼原則使用者介面](../get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-.md#fine_grained_pswd_policy_mgmt)|ADAC 為原本在 Windows Server 2008 加入的 PSO 建立、編輯和指派新增了 GUI 支援。|
|[群組受管理的服務帳戶 (gMSA)](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831782(v=ws.11))|稱為 gMSA 的新安全性主體類型 在多個主機上執行的服務可以在同一個 gMSA 帳戶下執行。|
|[DirectAccess 離線網域加入](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574150(v=ws.11))|透過包含 DirectAccess 先決條件以擴充離線網域加入功能。|
|[透過虛擬網域控制站 (DC) 複製進行快速部署](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574150(v=ws.11)#virtualized_dc_cloning)|虛擬 DC 可以使用 Windows PowerShell Cmdlet 複製現有的虛擬網域控制站，進行快速部署。|
|[RID 集區變更](../manage/managing-rid-issuance.md)|新增監視事件與配額，以確保全域 RID 集區不會過度消耗。 如果原始集區已用盡，可選擇性的加倍全域 RID 集區的大小。|
|保護時間服務的安全|移除有線密碼、移除 MD5 雜湊函數，以及要求伺服器與 Windows 8 時間用戶端進行驗證，加強 W32tm 的安全性。|
|[虛擬 DC 的 USN 復原保護](../manage/managing-rid-issuance.md)|意外地還原虛擬 DC 的快照備份時，不會再造成 USN 復原。|
|[Windows PowerShell 歷程記錄檢視器](../get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-.md#windows_powershell_history_viewer)|使用 ADAC 時，系統管理員可以檢視執行的 Windows PowerShell 命令。|

### <a name="automatic-maintenance-and-changes-to-restart-behavior-after-updates-are-applied-by-windows-update"></a><a name="BKMK_"></a>自動維護與 Windows Update 套用更新後重新啟動行為的變更

在 Windows 8 版本之前，Windows Update 會管理其內部排程以檢查更新，並下載及安裝它們。 這需要始終在背景執行 Windows Update 代理程式，因而會耗用記憶體和其他系統資源。

Windows 8 與 Windows Server 2012 引進一項名為 [自動維護](/windows/win32/w8cookbook/automatic-maintenance)的新功能。 自動維護合併了許多不同的功能，這些功能是分別用來管理它自己的排程及執行邏輯。 這項合併可以讓所有這些元件使用最少的系統資源、穩定運作、尊重新裝置類型的新 [連線待命](/windows/win32/w8cookbook/automatic-maintenance) 狀態，並使用較少的可攜式裝置電池電力。

因為 Windows Update 是 Windows 8 和 Windows Server 2012 中自動維護的一部分，其本身設定日期和時間來安裝更新的內部排程已不再有效。 為協助確保您企業中的所有裝置與電腦 (包括執行 Windows 8 與 Windows Server 2012 的所有裝置與電腦) 有一致且可預期的重新啟動行為，您可以設定下列群組原則設定：

- **電腦設定||系統管理範本|Windows 元件|Windows Update|設定自動更新**
- **電腦設定|原則|系統管理範本|Windows 元件|Windows Update|不隨著使用者登入而自動重新啟動**
- **電腦設定|原則|系統管理範本|Windows 元件|維護排程器|維護隨機延遲**

下表列出如何設定這些設定以提供所需的重新啟動行為的範例。

|||
|-|-|
|**案例**|**建議的 configuration (s)**|
|**WSUS 管理的**<p>-每週安裝更新一次<br />-晚上11點重新開機星期五|設定機器自動安裝，防止在需要的時間之前重新開機<p>**原則** ：設定自動更新 (啟用)<p>設定自動更新： 4-自動下載並排程安裝<p>**原則** ： (停用登入的使用者不會自動重新開機) <p>**WSUS 期限** ：設定為星期五晚上 11 點|
|**WSUS 管理的**<p>-在不同的小時/天錯開安裝|為應該一起更新的不同電腦群組設定目標群組<p>為先前的案例使用上述步驟<p>為不同的目標群組設定不同期限|
|**不受 WSUS 管理-不支援期限**<p>-錯開不同時間的安裝|**原則** ：設定自動更新 (啟用)<p>設定自動更新： 4-自動下載並排程安裝<p>**登錄機碼：** 如需啟用登錄機碼的資訊，請參閱 Microsoft 知識庫文章 [2835627](https://support.microsoft.com/kb/2835627)<p>**原則：** 自動維護隨機延遲 (啟用)<p>將 [定期維護隨機延遲] 設為 PT6H 以設定 6 小時的隨機延遲，可提供下列行為：<p>-更新將會安裝在設定的維護時間加上隨機延遲<p>-每台電腦的重新開機將會在3天后進行<p>或者，為每個電腦群組設定不同的維護時間|

如需 Windows 工程團隊如何實行這些變更的詳細資訊，請參閱 [如何降低系統提示重新開機電腦的機會](https://docs.microsoft.com/troubleshoot/windows-server/deployment/why-prompted-restart-computer#how-to-reduce-your-chances-of-being-prompted-to-restart-your-computer)。

## <a name="ad-ds-server-role-installation-changes"></a><a name="BKMK_InstallationChanges"></a>AD DS 伺服器角色安裝變更

從 Windows Server 2003 到 Windows Server 2008 R2，您都是在執行 Active Directory 安裝精靈 Dcpromo.exe 之前執行 x86 或 X64 版本的 Adprep.exe 命令列工具，而且 Dcpromo.exe 擁有選擇性變數可從媒體安裝或進行自動安裝。

自 Windows Server 2012 起，命令列安裝是使用 Windows PowerShell 的 ADDSDeployment 模組執行。 GUI 升級是在伺服器管理員中使用全新的 AD DS 設定精靈執行。 為簡化安裝程序，ADPREP 已經整合在 AD DS 安裝之中，視需要自動執行。 以 Windows PowerShell 為基礎的 AD DS 設定向導會自動以要新增 Dc 的網域中的架構和基礎結構主機角色為目標，然後從遠端在相關的網域控制站上執行所需的 ADPREP 命令。

AD DS 安裝精靈的先決條件檢查會在安裝開始前識別可能的錯誤。 您可以更正錯誤狀況，以排除升級不完全的問題。 此精靈也可匯出包含圖形化安裝期間指定之所有選項的 Windows PowerShell 指令碼。

綜合以上所述，AD DS 安裝變更簡化了 DC 角色安裝程序，而且降低管理錯誤的發生，特別是當您跨通用區域和網域部署多個網域控制站時。
更多 GUI 和 Windows PowerShell 安裝的詳細資訊，包含命令列語法和逐步精靈指示，請參閱 [安裝 Active Directory 網域服務](./install-active-directory-domain-services--level-100-.md)。 系統管理員如果希望控制在 Active Directory 樹系 (與現有樹系中的 Windows Server 2012 DC 安裝無關) 採用的架構變更，還是可以在提升權限的命令提示字元執行 Adprep.exe 命令。

## <a name="deprecated-features-and-behavior-changes-related-to-ad-ds-in-windows-server-2012"></a><a name="BKMK_DeprecatedFeatures"></a>與 Windows Server 2012 中 AD DS 相關的過時功能與行為變更

以下是與 AD DS 有關的變更：

- **Adprep32.exe 過時**
   - Adprep.exe 只有一個版本，可以視需要在執行 Windows Server 2008 或更新版本的 64 位元伺服器執行。 它可以在遠端執行，而且如果目標作業主機角色裝載在 32 位元作業系統或 Windows Server 2003 上，就必須要在遠端執行。
- **Dcpromo.exe 過時**
   - 雖然在 Windows Server 2012 中，Dcpromo 仍會被取代，但仍然可以使用回應檔案或命令列參數來執行，讓組織有時間將現有的自動化轉換到新的 Windows PowerShell 安裝選項。
- **使用者帳戶停用 LMHash**
  - Windows Server 2008、Windows Server 2008 R2 和 Windows Server 2012 安全性範本的安全預設會啟用 NoLMHash 原則，此原則在 Windows 2000 和 Windows Server 2003 網域控制站的安全性範本為停用。 您可以視需要停用 Lm 相依用戶端的 NoLMHash 原則，方法是使用頁面中的 [如何防止 Windows 將您密碼的 LAN manager 雜湊儲存在 Active Directory 和本機 SAM 資料庫中](https://docs.microsoft.com/troubleshoot/windows-server/windows-security/prevent-windows-store-lm-hash-password)所述的步驟。

從 Windows Server 2008 開始，相較于執行 Windows Server 2003 或 Windows 2000 的網域控制站，網域控制站也有下列安全的預設設定：

| 加密類型或原則 | Windows Server 2008 預設值 | Windows Server 2012 和 Windows Server 2008 R2 預設值 | 註解 |
|--|--|--|--|
| AllowNT4Crypto | 已停用 | 已停用 | 協力廠商伺服器訊息區 (SMB) 用戶端可能與網域控制站上的安全預設設定不相容。 在所有情況下，這些設定可放寬以允許交互操作性，但同時也會產生安全風險。 如需詳細資訊，請參閱 Microsoft 知識庫中的 [文章 942564](https://go.microsoft.com/fwlink/?LinkId=164558) (https://go.microsoft.com/fwlink/?LinkId=164558) 。 |
| DES | 啟用 | 已停用 | Microsoft 知識庫中的[文章 977321](https://go.microsoft.com/fwlink/?LinkId=177717) (https://go.microsoft.com/fwlink/?LinkId=177717) |
| CBT/整合式驗證的擴充保護 | N/A | 啟用 | 請參閱 microsoft 知識庫 (中的 [Microsoft 資訊安全諮詢 (937811) ](https://go.microsoft.com/fwlink/?LinkId=164559) (https://go.microsoft.com/fwlink/?LinkId=164559) 和 [文章 976918](https://go.microsoft.com/fwlink/?LinkId=178251) https://go.microsoft.com/fwlink/?LinkId=178251) 。<p>如有必要，請在 Microsoft 知識庫的 [文章 977073](https://go.microsoft.com/fwlink/?LinkId=186394) (中，檢查並安裝此修正程式 https://go.microsoft.com/fwlink/?LinkId=186394) 。 |
| LMv2 | 啟用 | 已停用 | Microsoft 知識庫中的[文章 976918](https://go.microsoft.com/fwlink/?LinkId=178251) (https://go.microsoft.com/fwlink/?LinkId=178251) |

## <a name="operating-system-requirements"></a><a name="BKMK_SysReqs"></a>作業系統需求

下表列出 Windows Server 2012 的最低系統需求。 如需系統需求及預先安裝資訊的詳細資訊，請參閱 [安裝 Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134246(v=ws.11))。 安裝新 Active Directory 樹系沒有額外的系統需求，但是您應該增加足夠的記憶體用於快取 Active Directory 資料庫的內容，以便提升網域控制站、LDAP 用戶端要求和 Active Directory 應用程式的效能。 如果您要升級現有網域控制站或將新的網域控制站新增到現有的樹系，請參閱下一節以確保伺服器符合磁碟空間需求。

| 需求 | 值 |
| ---------- | ----- |
| 處理器 | 1.4 Ghz 64 位元處理器 |
| RAM | 512 MB |
| 可用磁碟空間需求 | 32 GB |
| 螢幕解析度 | 800 x 600 或更高 |
| 其他 | DVD 光碟機、鍵盤、網際網路存取 |

### <a name="disk-space-requirements-for-upgrading-domain-controllers"></a><a name="BKMK_DiskSpaceDCWin8"></a>升級網域控制站的磁碟空間需求

本節僅涵蓋從 Windows Server 2008 或 Windows Server 2008 R2 升級網域控制站的磁碟空間需求。 如需將網域控制站升級到舊版 Windows Server 之磁碟空間需求的詳細資訊，請參閱 [升級到 Windows Server 2008 的磁碟空間需求](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754463(v=ws.10)#BKMK_2008) 或 [升級到 Windows Server 2008 R2 的磁碟空間需求](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754463(v=ws.10)#BKMK_2008R2)。

估計裝載 Active Directory 資料庫和記錄檔之磁碟的大小，這個大小必須能夠容納自訂和應用程式驅動的架構延伸、應用程式和由系統管理員起始的索引，還需要網域控制站部署存留期 (通常為 5 到 8 年) 新增到目錄之物件和屬性所需的空間。 與部署之後擴充磁碟存放裝置所需的更多成本相較之下，在部署階段決定正確的大小是一項非常有價值的投資。 如需詳細資訊，請參閱 [Active Directory 網域服務容量規劃](../../../administration/performance-tuning/role/active-directory-server/capacity-planning-for-active-directory-domain-services.md)。

在計劃升級的網域控制站上，確定裝載了 Active Directory 資料庫 (NTDS.DIT) 的磁碟機擁有至少相當於 20% NTDS.DIT 檔案的可用磁碟空間，然後才開始作業系統升級。 如果磁碟區上沒有足夠的可用磁碟空間，升級會失敗，而且升級相容性報告會傳回錯誤，指示可用磁碟空間不足：

在這個情況下，您可以嘗試離線磁碟重組 Active Directory 資料庫以重新抓取額外的空間，然後重試升級。 如需詳細資訊，請參閱 [壓縮目錄資料庫檔案 (離線磁碟重組)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc794920(v=ws.10))。

### <a name="available-skus"></a>可用的 SKU

有 4 個版本的 Windows Server：Foundation、Essentials、Standard 及 Datacenter。
Standard 和 Datacenter 這兩個版本可支援 AD DS 角色。

在之前的版本中，Windows Server 版本的差異在於伺服器角色的支援、處理器計數以及大型記憶體支援。 Standard 和 Datacenter edition 的 Windows Server 支援所有功能和基礎硬體，但其虛擬化許可權不同-Standard edition 允許兩個虛擬實例，且 Datacenter edition 允許無限制的虛擬實例。

### <a name="windows-client-and-windows-server-operating-systems-that-are-supported-to-join-windows-server-domains"></a>可加入 Windows Server 網域的 Windows 用戶端和 Windows Server 作業系統

網域成員電腦與執行 Windows Server 2012 或更新版本的網域控制站支援以下 Windows 用戶端和 Windows Server 作業系統：

- 伺服器作業系統：Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008、Windows Server 2003 R2 及 Windows Server 2003

## <a name="supported-in-place-upgrade-paths"></a><a name="BKMK_UpgradePaths"></a>支援的就地升級路徑

執行64位版 Windows Server 2008 或 Windows Server 2008 R2 的網域控制站可以升級至 Windows Server 2012。 您不能升級執行 Windows Server 2003 或 32 位元版本 Windows Server 2008 的網域控制站。 若要取代它們，請在網域中安裝執行更新版 Windows Server 的網域控制站，然後移除執行 Windows Server 2003 的網域控制站。

| 如果您執行這些版本 | 您可以升級到這些版本 |
|--|--|
| Windows Server 2008 Standard (含 SP2)<p>或<p>Windows Server 2008 Enterprise (含 SP2) | Windows Server 2012 Standard<p>或<p>Windows Server 2012 Datacenter |
| Windows Server 2008 Datacenter (含 SP2) | Windows Server 2012 Datacenter |
| Windows Web Server 2008 | Windows Server 2012 Standard |
| Windows Server 2008 R2 Standard (含 SP1)<p>或<p>Windows Server 2008 R2 Enterprise (含 SP1) | Windows Server 2012 Standard<p>或<p>Windows Server 2012 Datacenter |
| Windows Server 2008 R2 Datacenter (含 SP1) | Windows Server 2012 Datacenter |
| Windows Web Server 2008 R2 | Windows Server 2012 Standard |

如需支援的升級路徑詳細資訊，請參閱 [適用於 Windows Server 2012 的評估版與升級選項](https://go.microsoft.com/fwlink/?LinkId=260917)。 請注意，您不能將執行評估版 Windows Server 2012 的網域控制站直接轉換為零售版。 您必須在執行零售版的伺服器上安裝另一個網域控制站，然後從在評估版執行的網域控制站移除 AD DS。

由於已知的問題，您無法將執行 Windows Server 2008 R2 之 Server Core 安裝的網域控制站升級至 Windows Server 2012 的 Server Core 安裝。 在升級程序後期，升級會當機並呈現全黑的螢幕。 重新啟動這類 DC 會在 boot.ini 檔案中看到一個選項，回復到之前的作業系統版本。 再次重新開機會觸發自動回復到之前的作業系統版本。 在有解決方案可用之前，建議您安裝新的網域控制站，執行 Windows Server 2012 的 Server Core 安裝，而不是就地升級執行 Windows Server 2008 R2 之 Server Core 安裝的現有網域控制站。 如需詳細資訊，請參閱知識庫文章 [2734222](https://support.microsoft.com/kb/2734222)。

## <a name="functional-level-features-and-requirements"></a><a name="BKMK_FunctionalLevels"></a>功能等級功能和需求

Windows Server 2012 需要 Windows Server 2003 樹系功能等級。 也就是說，在您可以將執行 Windows Server 2012 的網域控制站新增至現有的 Active Directory 樹系之前，樹系功能等級必須是 Windows Server 2003 或更高的版本。 這表示執行 Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 的網域控制站可以在相同的樹系中運作，但不支援執行 Windows 2000 Server 的網域控制站，而且會封鎖執行 Windows Server 2012 之網域控制站的安裝。 如果樹系包含執行 Windows Server 2003 或更新版本的網域控制站，但樹系功能等級還是 Windows 2000，也會封鎖安裝。

Windows 2000 網域控制站必須先行移除，才能將 Windows Server 2012 網域控制站新增到您的樹系。 在這種情況下，請考慮下列工作流程：

1. 安裝執行 Windows Server 2003 或更新版本的網域控制站。 可以在評估版的 Windows Server 上部署這些網域控制站。 這個步驟還有一個先決條件，就是需要針對該作業系統版本 [執行 adprep.exe](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)) 。
2. 移除 Windows 2000 網域控制站。 具體來說，就是以正常方式降級或以強制方式從網域和使用的 Active Directory 使用者和電腦移除 Windows Server 2000 網域控制站，藉此移除所有已移除之網域控制站的網域控制站帳戶。
3. 將樹系功能等級提升至 Windows Server 2003 或更高。
4. 安裝執行 Windows Serer 2012 的網域控制站。
5. 移除執行舊版 Windows Server 的網域控制站。

新的 Windows Server 2012 網域功能等級啟用了一項新功能： **宣告、複合驗證和 Kerberos** 防護 kdc 系統管理範本原則的 KDC 支援有兩項設定 ( **一律提供宣告** ，以及 **未受防護驗證要求失敗** ，) 需要 Windows Server 2012 網域功能等級。

Windows Server 2012 樹系功能等級不提供任何新功能，但可確保樹系中建立的任何新網域都會自動在 Windows Server 2012 網域功能等級運作。 Windows Server 2012 網域功能等級不提供宣告、複合驗證和 Kerberos 防護的 KDC 支援以外的其他新功能。 但它可以確保網域中的任何網域控制站都執行 Windows Server 2012。 如需不同功能等級可用之其他功能的詳細資訊，請參閱 [了解 Active Directory 網域服務 (AD DS) 功能等級](../active-directory-functional-levels.md)。

將樹系功能等級設定為某個值之後，您就無法回復或降低樹系功能等級，但有下列例外：當您將樹系功能等級提高至 Windows Server 2012 之後，您可以將它降低為 Windows Server 2008 R2。 如果未啟用 Active Directory 資源回收筒，您也可以將樹系功能等級從 Windows Server 2012 降低為 Windows Server 2008 R2 或 Windows Server 2008，或從 Windows Server 2008 R2 降低為 Windows Server 2008。 如果樹系功能等級設定為 Windows Server 2008 R2，則無法復原到 Windows Server 2003。

將網域功能等級設定為某個值之後，您無法回復或降低網域功能等級，但有下列例外狀況：當您將網域功能等級提高至 Windows Server 2008 R2 或 Windows Server 2012 時，如果樹系功能等級為 Windows Server 2008 或更低，您可以選擇將網域功能等級回復至 Windows Server 2008 或 Windows Server 2008 R2。 您只能將網域功能等級從 Windows Server 2012 降低為 Windows Server 2008 R2 或 windows server 2008，或從 Windows Server 2008 R2 降低至 Windows Server 2008。 如果網域功能等級設定為 Windows Server 2008 R2，則無法復原到 Windows Server 2003。

如需較低功能等級上可用之功能的詳細資訊，請參閱 [了解 Active Directory 網域服務 (AD DS) 功能等級](../active-directory-functional-levels.md)。

除了功能等級之外，執行 Windows Server 2012 的網域控制站提供執行舊版 Windows Server 之網域控制站上無法使用的其他功能。 例如，執行 Windows Server 2012 的網域控制站可以用於虛擬網域控制站複製，而執行舊版 Windows Server 的網域控制站則無法使用。 但是 Windows Server 2012 中的虛擬網域控制站複製和虛擬網域控制站保護沒有任何功能等級需求。

> [!NOTE]
> Microsoft Exchange Server 2013 需要 Windows Server 2003 或更高的樹系功能等級。

## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a><a name="BKMK_ServerRoles"></a>AD DS 與其他伺服器角色和 Windows 作業系統的互通性

下列 Windows 作業系統不支援 AD DS：

- Windows MultiPoint Server
- Windows Server 2012 Essentials

AD DS 無法安裝在同時執行下列伺服器角色或角色服務的伺服器上：

- Hyper-V 伺服器
- 遠端桌面連線代理人

## <a name="operations-master-roles"></a><a name="BKMK_OpsMasters"></a>操作主機角色

Windows Server 2012 中的一些新功能會影響操作主機角色：

- PDC 模擬器必須正在執行 Windows Server 2012，才能支援複製虛擬網域控制站。 複製 DC 需要額外的先決條件。 如需詳細資訊，請參閱 [Active Directory 網域服務 (AD DS) 虛擬化](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10))。
- 當 PDC 模擬器執行 Windows Server 2012 時，就會建立新的安全性主體。
- RID 主機具有新的 RID 發行和監視功能。 這些改進包括更好的事件記錄、更適當的限制，以及 (在緊急情況下) 將整體 RID 集區配置提高一些的能力。 如需詳細資訊，請參閱[管理 RID 發行](../../ad-ds/manage/Managing-RID-Issuance.md)。

> [!NOTE]
> 雖然它們不是操作主機角色，但 AD DS 安裝的另一項變更是，預設會在執行 Windows Server 2012 的所有網域控制站上安裝 DNS 伺服器角色及通用類別目錄。

## <a name="virtualizing-domain-controllers"></a><a name="BKMK_Virtual"></a>虛擬化網域控制站

從 Windows Server 2012 開始的 AD DS 增強功能，可讓您更安全地虛擬化網域控制站和複製網域控制站的能力。 複製網域控制站的能力接著能夠在新的網域中快速部署其他網域控制站並提供其他好處。 如需詳細資訊，請參閱 [Active Directory Domain Services &#40;AD DS&#41; 虛擬化 &#40;層級 100&#41;的簡介 ](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md)。

## <a name="administration-of-windows-server-2012-servers"></a><a name="BKMK_Admin"></a>Windows Server 2012 伺服器的管理

使用 [Windows 8 的遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=28972) 管理執行 Windows Server 2012 的網域控制站及其他伺服器。 您可以在執行 Windows 8 的電腦上執行 Windows Server 2012 遠端伺服器管理工具。

## <a name="application-compatibility"></a><a name="BKMK_AppCompat"></a>應用程式相容性

下表涵蓋常見的整合 Active Directory Microsoft 應用程式。 表格內容包含應用程式可以安裝在哪些版本的 Windows Server 上，以及採用 Windows Server 2012 DC 是否會對應用程式相容性產生影響。

|產品|備註|
|-----------|---------|
|[Microsoft SharePoint 2010](https://support.microsoft.com/kb/2724471)|需要 SharePoint 2010 Service Pack 2，才能在  <br />Windows Server 2012 伺服器上 安裝和操作 SharePoint 2010<p>在 Windows Server 2012 伺服器上安裝和操作 SharePoint 2010 Foundation 需要有 SharePoint 2010 Foundation Service Pack 2<p>SharePoint Server 2010 (不含 Service Pack) 安裝程序在 Windows Server 2012 上會失敗<p>SharePoint Server 2010 必要條件安裝程式 ( # A0) 失敗，並出現錯誤「此程式有相容性問題」。 按一下 [執行程式而不取得說明]，會顯示錯誤「正在驗證是否可以在不含 service pack &#124; SharePoint Server 2010 (中安裝 SharePoint) 無法在 Windows Server 2012 上安裝。」|
|[Microsoft SharePoint 2013](/SharePoint/install/hardware-and-software-requirements-0)|伺服器陣列中的資料庫伺服器最低需求：<p>64 位元版本的 Windows Server 2008 R2 Service Pack 1 (SP1) Standard、Enterprise 或 Datacenter，或是 64 位元版本的 Windows Server 2012 Standard 或 Datacenter<p>含內建資料庫的單一伺服器最低需求：<p>64 位元版本的 Windows Server 2008 R2 Service Pack 1 (SP1) Standard、Enterprise 或 Datacenter，或是 64 位元版本的 Windows Server 2012 Standard 或 Datacenter<p>伺服器陣列中的前端網頁伺服器和應用程式伺服器最低需求：<p>64 位元版本的 Windows Server 2008 R2 Service Pack 1 (SP1) Standard、Enterprise 或 Datacenter，或是 64 位元版本的 Windows Server 2012 Standard 或 Datacenter。|
|[Configuration Manager 2012](/SharePoint/install/hardware-and-software-requirements-0)|Configuration Manager 2012 Service Pack 1：<p>Microsoft 會在發行 Service Pack 1 時，在我們的用戶端支援基礎新增下列作業系統：<p>-Windows 8 Pro<br />-Windows 8 企業版<br />-Windows Server 2012 標準版<br />-Windows Server 2012 Datacenter<p>所有站台伺服器角色 (包含站台伺服器、SMS 提供者和管理點) 都可以部署在含下列作業系統版本的伺服器：<p>-Windows Server 2012 標準版<br />-Windows Server 2012 Datacenter|
|[Microsoft Endpoint Configuration Manager (最新分支) ](/configmgr/core/plan-design/configs/supported-configurations)|[Configuration Manager 網站系統伺服器支援的作業系統](/configmgr/core/plan-design/configs/supported-operating-systems-for-site-system-servers)。|
|[Microsoft Lync Server 2013](/lyncserver/lync-server-2013-server-and-tools-operating-system-support)|Lync Server 2013 需要搭配 Windows Server 2008 R2 或 Windows Server 2012。 它不能在 Server Core 安裝上執行， 可以在 [虛擬伺服器](/lyncserver/lync-server-2013-running-lync-server-on-virtual-servers)上執行。|
|[Lync Server 2010](https://support.microsoft.com/kb/2777359)|如果安裝了 [Lync Server 的 2012 年 10 月累計更新](https://support.microsoft.com/?kbid=2493736) ，Lync Server 2010 便無法安裝在新的 (而非升級的) Windows Server 2012 安裝。 不支援將現有 Lync Server 2010 安裝的作業系統升級到 Windows Server 2012。 Windows Server 2012 也不支援 Microsoft Lync Server 2010 群組聊天伺服器。|
|[System Center 2012 Endpoint Protection](/SharePoint/install/hardware-and-software-requirements-0)|System Center 2012 Endpoint Protection Service Pack 1 會更新用戶端支援基礎，以包含下列作業系統：<p>-Windows 8 Pro<br />-Windows 8 企業版<br />-Windows Server 2012 標準版<br />-Windows Server 2012 Datacenter|
|[System Center 2012 Forefront Endpoint Protection](/SharePoint/install/hardware-and-software-requirements-0)|FEP 2010 含更新彙總套件 1 會更新用戶端支援基礎，以包含下列作業系統：<p>-Windows 8 Pro<br />-Windows 8 企業版<br />-Windows Server 2012 標準版<br />-Windows Server 2012 Datacenter|
|Forefront Threat Management Gateway (TMG)|僅支援 TMG 在 Windows Server 2008 和 Windows Server 2008 R2 上執行。 如需詳細資訊，請參閱 [Forefront TMG 的系統需求](/previous-versions/tn-archive/dd896981(v=technet.10))。|
|Windows Server Update Services|這個版本的 WSUS 已經可以支援 Windows 8 電腦或 Windows Server 2012 電腦做為用戶端。|
|Windows Server Update Services 3.0|Update KB 文章 [2734608](https://support.microsoft.com/kb/2734608) 可讓執行 WINDOWS SERVER UPDATE SERVICES (WSUS) 3.0 SP2 的伺服器為執行 Windows 8 或 Windows Server 2012 的電腦提供更新： **注意：** 具有獨立 wsus 3.0 SP2 環境或 Configuration Manager 2007 Service PACK 2 環境搭配 wsus 3.0 SP2 的客戶需要 [2734608](https://support.microsoft.com/kb/2734608) 才能適當地管理 Windows 8 型電腦或 windows server 2012 電腦做為用戶端。|
|[Exchange 2013](/Exchange/plan-and-deploy/prerequisites?view=exchserver-2019)|Windows Server 2012 Standard 和 Datacenter 支援下列角色：架構主機、通用類別目錄伺服器、網域控制站、信箱和用戶端存取伺服器角色<p>樹系功能等級：Windows Server 2003 或更新版本<p>來源：Exchange 2013 系統需求|
|Exchange 2010|[來源：Exchange 2010 Service Pack 3](https://techcommunity.microsoft.com/t5/exchange-team-blog/bg-p/Exchange)<p>Exchange 2010 (含 Service Pack 3) 可以安裝在 Windows Server 2012 成員伺服器上。<p>[Exchange 2010 系統需求](/previous-versions/office/exchange-server-2010/aa996719(v=exchg.141)) 會列出最新支援的架構主機、通用類別目錄和網域控制站，如同 Windows Server 2008 R2。<p>樹系功能等級：Windows Server 2003 或更新版本|
|SQL Server 2012|來源：KB [2681562](https://support.microsoft.com/kb/2681562)<p>Windows Server 2012 支援 SQL Server 2012 RTM。|
|SQL Server 2008 R2|來源：KB [2681562](https://support.microsoft.com/kb/2681562)<p>需要 SQL Server 2008 R2 (含 Service Pack 1) 或更新版本才能安裝在 Windows Server 2012。|
|SQL Server 2008|來源：KB [2681562](https://support.microsoft.com/kb/2681562)<p>需要 SQL Server 2008 (含 Service Pack 3) 或更新版本才能安裝在 Windows Server 2012。|
|SQL Server 2005|來源：KB [2681562](https://support.microsoft.com/kb/2681562)<p>不支援安裝在 Windows Server 2012。|

## <a name="known-issues"></a><a name="BKMK_KnownIssues"></a>已知問題

下表列出與 AD DS 安裝相關的已知問題：

| 知識庫文章編號和標題 | 受影響的技術領域 | 問題/描述 |
|--|--|--|
| [2830145](https://support.microsoft.com/kb/2830145)：SID S-1-18-1 與 SID S-1-18-2 無法對應在網域環境中的 Windows 7 或 Windows Server 2008 R2 電腦上 | AD DS 管理/應用程式相容 | 對應 SID S-1-18-1 與 SID S-1-18-2 的應用程式 (Windows Server 2012 中的新功能) 可能會失敗，因為 SID 無法在 Windows 7 或 Windows Server 2008 R2 電腦上解析。 如果要解決這個問題，請在網域的 Windows 7 與 Windows Server 2008 R2 電腦上安裝 Hotfix。 |
| [2737129](https://support.microsoft.com/kb/2737129)：當您自動準備 Windows Server 2012 的現有網域時不會執行群組原則準備 | AD DS 安裝 | 安裝第一個在網域中執行 Windows Server 2012 的 DC 時，不會自動執行 Adprep /domainprep /gpprep。 如果之前從未在網域執行過，必須手動執行。 |
| [2737416](https://support.microsoft.com/kb/2737416)：Windows PowerShell 網域控制站部署重複出現警告 | AD DS 安裝 | 警告可能在先決條件驗證期間出現，在安裝時又重複出現。 |
| [2737424](https://support.microsoft.com/kb/2737424)：當您嘗試從網域控制站移除 Active Directory 網域服務時發生「指定的網域名稱格式不正確」錯誤 | AD DS 安裝 | 如果您移除網域中的最後一個 DC，但該網域中預先建立的 RODC 帳戶仍然存在，就會出現此錯誤。 這會影響 Windows Server 2012、Windows Server 2008 R2 和 Windows Server 2008。 |
| [2737463](https://support.microsoft.com/kb/2737463)：網域控制站未啟動、發生 c00002e2 錯誤或顯示「選擇選項」 | AD DS 安裝 | 因為系統管理員使用 Dism.exe、Pkgmgr.exe 或 Ocsetup.exe 移除 DirectoryServices-DomainController 角色，所以 DC 沒有啟動。 |
| [2737516](https://support.microsoft.com/kb/2737516)：Windows Server 2012 伺服器管理員的 IFM 驗證限制 | AD DS 安裝 | IFM 驗證有所限制，如同知識庫文章中的說明。 |
| [2737535](https://support.microsoft.com/kb/2737535)：Install-AddsDomainController Cmdlet 傳回 RODC 的參數設定錯誤 | AD DS 安裝 | 如果您指定的引數已經填入預先建立的 RODC 帳戶，當您嘗試將伺服器連結到 RODC 帳戶時會收到錯誤。 |
| [2737560](https://support.microsoft.com/kb/2737560)：「無法執行 Exchange 架構衝突檢查」錯誤和先決條件檢查失敗 | AD DS 安裝 | 當您在現有的網域中設定第一個 Windows Server 2012 DC 時，因為 DC 遺失網路服務的 SeServiceLogonRight 或因為 WMI 或 DCOM 通訊協定被封鎖，先決條件檢查會失敗。 |
| [2737797](https://support.microsoft.com/kb/2737797)：AddsDeployment 模組搭配 -Whatif 引數顯示不正確的 DNS 結果 | AD DS 安裝 | -WhatIf 參數會顯示將不會安裝 DNS 伺服器，但它將會是。 |
| [2737807](https://support.microsoft.com/kb/2737807)：[網域控制站選項] 頁面的 [下一步] 按鈕無法使用 | AD DS 安裝 | 因為目標 DC 的 IP 位址沒有對應到現有的子網路或站台，或是因為並未正確輸入和確認 DSRM 密碼，所以會停用 [網域控制站選項] 頁面的 [下一步] 按鈕。 |
| [2737935](https://support.microsoft.com/kb/2737935)：Active Directory 安裝在「建立 NTDS 設定物件」階段停止 | AD DS 安裝 | 因為本機系統管理員密碼與網域系統管理員密碼相同，或因為網路問題而無法完成關鍵性複寫，因此安裝停滯。 |
| [2738060](https://support.microsoft.com/kb/2738060)：使用 Install-AddsDomain 遠端建立子網域時出現「拒絕存取」錯誤訊息 | AD DS 安裝 | 如果 DNSDelegationCredential 密碼錯誤，當您使用 Invoke-Command Cmdlet 執行 Install-ADDSDomain 時，會收到這個錯誤。 |
| [2738697](https://support.microsoft.com/kb/2738697)：使用伺服器管理員設定伺服器時出現「無法操作伺服器」網域控制站設定錯誤 | AD DS 安裝 | 當您嘗試在工作群組電腦安裝 AD DS 時，因為 NTLM 驗證停用，所以會收到這個錯誤。 |
| [2738746](https://support.microsoft.com/kb/2738746)：登入本機系統管理員網域帳戶之後收到拒絕存取錯誤 | AD DS 安裝 | 當您使用本機系統管理員帳戶而不是內建系統管理員帳戶登入，然後建立新的網域，帳戶不會新增到 Domain Admins 群組。 |
| [2743345](https://support.microsoft.com/kb/2743345)：「系統找不到指定的檔案」Adprep /gpprep 錯誤，或工具損毀 | AD DS 安裝 | 當您執行 adprep /gpprep 時，因為基礎結構主機實作一個不相鄰的命名空間，所以會收到這個錯誤。 |
| [2743367](https://support.microsoft.com/kb/2743367)：64 位元 Windows Server 2003 發生 Adprep「不是有效的 Win32 應用程式」錯誤 | AD DS 安裝 | 因為 Windows Server 2012 Adprep 不能在 Windows Server 2003 上執行，所以會收到這個錯誤。 |
| [2753560](https://support.microsoft.com/kb/2753560)：Windows Server 2012 發生 ADMT 3.2 和 PES 3.1 安裝錯誤 | ADMT | Windows Server 2012 的設計不能安裝 ADMT 3.2。 |
| [2750857](https://support.microsoft.com/kb/2750857)：Internet Explorer 10 無法正確顯示 DFS 複寫診斷報告 | DFS 複寫 | 因為 Internet Explorer 10 有所變更，所以無法正確顯示 DFS 複寫診斷報告。 |
| [2741537](https://support.microsoft.com/kb/2741537)：使用者可以看見遠端群組原則更新 | 群組原則 | 這是因為排程工作會在每個登入的使用者內容執行。 Windows 工作排程器的設計在這種情況下需要互動式提示。 |
| [2741591](https://support.microsoft.com/kb/2741591)：GPMC 基礎結構狀態選項的 SYSVOL 中沒有 ADM 檔案 | 群組原則 | GP 複寫可以報告「複寫進行中」，因為 GPMC 基礎結構狀態不符合自訂篩選規則。 |
| [2737880](https://support.microsoft.com/kb/2737880)：AD DS 設定期間發生「無法啟動服務」錯誤 | 虛擬 DC 複製 | 因為 DS 角色伺服器服務停用，所以安裝、移除 AD DS 或複製時會收到這個錯誤。 |
| [2742836](https://support.microsoft.com/kb/2742836)：使用 VDC 複製功能時會為每個網域控制站建立兩個 DHCP 租用 | 虛擬 DC 複製 | 因為複製的網域控制站會在複製之前和複製完成時各收到一個租用，所以會發生這種狀況。 |
| [2742844](https://support.microsoft.com/kb/2742844)：在 Windows Server 2012，網域控制站複製失敗且伺服器以 DSRM 重新啟動 | 虛擬 DC 複製 | 因為知識庫文章中所列的各種理由，複製的 DC 會因複製失敗而以 DSRM 啟動。 |
| [2742874](https://support.microsoft.com/kb/2742874)：網域控制站複製不會重新建立所有服務主體名稱 | 虛擬 DC 複製 | 因為網域重新命名程序的限制，所以複製的 DC 上不會重新建立某些協力廠商 SPN。 |
| [2742908](https://support.microsoft.com/kb/2742908)：複製網域控制站後發生「無可用的登入伺服器」錯誤 | 虛擬 DC 複製 | 因為複製虛擬 DC 失敗且 DC 以 DSRM 啟動，所以當您在複製之後嘗試登入時，會收到這個錯誤。 以 .\administrator 的身分登入可排除這個複製失敗。 |
| [2742916](https://support.microsoft.com/kb/2742916)：網域控制站複製失敗，dcpromo.log 中的錯誤為 8610 | 虛擬 DC 複製 | PDC 模擬器未執行網域分割的輸入複寫，很可能是因為角色已移轉，所以複製失敗。 |
| [2742927](https://support.microsoft.com/kb/2742927)：「索引超出範圍」New-AdDcCloneConfig 錯誤 | 虛擬 DC 複製 | 複製虛擬 DC 時執行 New-ADDCCloneConfigFile Cmdlet 之後，因為不是從提升權限的命令提示字元執行 Cmdlet，或因為您的存取權杖不含 Administrators 群組，所以會收到這個錯誤。 |
| [2742959](https://support.microsoft.com/kb/2742959)：網域控制站複製失敗，錯誤為 8437：「指定了不正確的參數給這項複寫操作」 | 虛擬 DC 複製 | 因為指定無效的複製名稱或重複的 NetBIOS 名稱，所以複製失敗。 |
| [2742970](https://support.microsoft.com/kb/2742970)：DC 複製失敗，沒有 DSRM、重複來源和複製電腦 | 虛擬 DC 複製 | 因為沒有在正確的位置建立 DCCloneConfig.xml 檔案，或因為來源 DC 已經在複製之前重新啟動，所以複製的虛擬 DC 使用重複名稱做為來源 DC，以目錄服務修復模式 (DSRM) 啟動。 |
| [2743278](https://support.microsoft.com/kb/2743278)：網域控制站複製錯誤 0x80041005 | 虛擬 DC 複製 | 因為只指定一個 WINS 伺服器，所以複製的 DC 開機到 DSRM。 如果指定了 WINS 伺服器，也必須同時指定慣用和其他 WINS 伺服器。 |
| [2745013](https://support.microsoft.com/kb/2745013)：在 Windows Server 2012 執行 New-AdDcCloneConfigFile 會出現「無法操作伺服器」錯誤訊息 | 虛擬 DC 複製 | 因為伺服器無法連線到通用類別目錄伺服器，所以執行 New-ADDCCloneConfigFile Cmdlet 之後會收到這個錯誤。 |
| [2747974](https://support.microsoft.com/kb/2747974)：網域控制站複製事件 2224 提供不正確的指導 | 虛擬 DC 複製 | 事件識別碼 2224 不正確的說明必須在複製之前移除受管理的服務帳戶。 獨立 MSA 必須移除，但群組 MSA 不會封鎖複製。 |
| [2748266](https://support.microsoft.com/kb/2748266)：升級至 Windows 8 之後無法解除鎖定 BitLocker 加密的磁碟機 | BitLocker | 當您嘗試在已從 Windows 7 升級的電腦上解除鎖定磁片磁碟機時，收到「找不到應用程式」錯誤。 |

## <a name="see-also"></a>另請參閱

[Windows Server 2012 評估資源](https://www.microsoft.com/en-us/evalcenter/) 
[Windows Server 2012 評估指南](https://download.microsoft.com/download/5/B/2/5B254183-FA53-4317-B577-7561058CEF42/WS%202012%20Evaluation%20Guide.pdf) 
[安裝和部署 Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831620(v=ws.11))
