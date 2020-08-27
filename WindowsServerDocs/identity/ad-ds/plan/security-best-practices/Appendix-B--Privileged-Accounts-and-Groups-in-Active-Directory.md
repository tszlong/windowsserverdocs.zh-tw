---
ms.assetid: 79b9c912-ea3e-4679-ab41-893e096c4d09
title: 附錄 B-Active Directory 中的特殊許可權帳戶和群組
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 22bcea1426502af83fdeeecb0005324de2d54e64
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941568"
---
# <a name="appendix-b-privileged-accounts-and-groups-in-active-directory"></a>附錄 B︰Active Directory 中具特殊權限的帳戶和群組

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-b-privileged-accounts-and-groups-in-active-directory"></a>附錄 B︰Active Directory 中具特殊權限的帳戶和群組
Active Directory 中的「特殊許可權」帳戶和群組，就是授與強大許可權、許可權和許可權的使用者，讓他們能夠在 Active Directory 和已加入網域的系統上執行幾乎任何動作。 本附錄一開始先討論權限、許可權和許可權，接著是 Active Directory 中「最高許可權」帳戶和群組的相關資訊，也就是最強大的帳戶和群組。

除了許可權之外，也會提供 Active Directory 中內建和預設帳戶和群組的相關資訊。 雖然以個別的附錄提供保護最高許可權帳戶和群組的特定設定建議，但本附錄提供的背景資訊可協助您識別應該專注于保護的使用者和群組。 您應這麼做，因為攻擊者可以利用它們來入侵，甚至終結您的 Active Directory 安裝。

### <a name="rights-privileges-and-permissions-in-active-directory"></a>Active Directory 中的許可權、許可權和許可權
許可權、許可權和許可權之間的差異可能會令人困惑和矛盾，即使是在 Microsoft 的檔中也一樣。 本節將說明本檔中使用這些功能的部分特性。 這些描述不應被視為其他 Microsoft 檔的授權，因為它可能會以不同的方式使用這些條款。

#### <a name="rights-and-privileges"></a>權利和許可權
權利和許可權實際上是授與安全性主體（例如使用者、服務、電腦或群組）的相同全系統功能。 在 IT 專業人員通常使用的介面中，這些通常稱為「權利」或「使用者權限」，通常是由群組原則物件所指派。 下列螢幕擷取畫面顯示一些最常見的使用者權限，可以指派給安全性主體 (它代表 Windows Server 2012 網域) 中的預設網域控制站 GPO。 這些許可權的其中一部分適用于 Active Directory，例如 [ **啟用電腦和使用者帳戶為可信任委派** ] 使用者權限，而其他許可權則適用于 Windows 作業系統，例如 **變更系統時間**。

![特殊許可權帳戶和群組](media/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory/SAD_8.gif)

在群組原則物件編輯器之類的介面中，這些可指派的所有功能都被視為廣泛的使用者權限。 但實際上，某些使用者權限會以程式設計的方式稱為許可權，而有些則是以程式設計的方式來稱為許可權。 表 B-1：使用者權限和許可權提供一些最常見的可指派使用者權限及其程式設計常數。 雖然群組原則和其他介面會將這些全部視為使用者權限，但有些會以程式設計的方式識別為許可權，而有些則是定義為許可權。

如需下表所列的每個使用者權限的詳細資訊，請使用表格中的連結，或參閱 Microsoft TechNet 網站上的 [威脅與對策指南：](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh125917(v=ws.10)) Windows Server 2008 R2 的 [威脅和弱點緩和](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755181(v=ws.10)) 指南中的使用者權利。 如需適用于 Windows Server 2008 的詳細資訊，請參閱 Microsoft TechNet 網站上的[威脅和弱點緩和](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755181(v=ws.10))檔中的[使用者權利](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10))。 撰寫本檔時，尚未發佈 Windows Server 2012 的對應檔。

> [!NOTE]
> 基於本檔的目的，除非另有指定，否則會使用「權利」和「使用者權利」等條款來識別權利和許可權。

##### <a name="table-b-1-user-rights-and-privileges"></a>表 B-1：使用者權限和許可權

|**群組原則中的使用者權限**|**常數的名稱**|
|--|--|
|[存取認證管理員做為信任的呼叫者](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_2)|SeTrustedCredManAccessPrivilege|
|[從網路存取這台電腦](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_1)|SeNetworkLogonRight|
|[作為作業系統的一部分](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_3)|SeTcbPrivilege|
|[將工作站新增至網域](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_4)|SeMachineAccountPrivilege|
|[調整處理序的記憶體配額](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_5)|SeIncreaseQuotaPrivilege|
|[允許本機登入](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_6)|SeInteractiveLogonRight|
|[允許透過終端機服務登入](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_7)|SeRemoteInteractiveLogonRight|
|[備份檔案及目錄](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_8)|SeBackupPrivilege|
|[略過周遊檢查](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_9)|SeChangeNotifyPrivilege|
|[變更系統時間](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_10)|SeSystemtimePrivilege|
|[變更時區](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_11)|SeTimeZonePrivilege|
|[建立分頁檔](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_12)|SeCreatePagefilePrivilege|
|[建立權杖物件](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_13)|SeCreateTokenPrivilege|
|[建立通用物件](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_14)|SeCreateGlobalPrivilege|
|[建立永久共用物件](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_15)|SeCreatePermanentPrivilege|
|[建立符號連結](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_16)|SeCreateSymbolicLinkPrivilege|
|[偵錯程式](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_17)|SeDebugPrivilege|
|[拒絕從網路存取這部電腦](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_18)|SeDenyNetworkLogonRight|
|[拒絕以批次工作登入](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_18a)|SeDenyBatchLogonRight|
|[拒絕以服務方式登入](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_19)|SeDenyServiceLogonRight|
|[拒絕本機登入](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_20)|SeDenyInteractiveLogonRight|
|[拒絕透過終端機服務登入](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_21)|SeDenyRemoteInteractiveLogonRight|
|[讓電腦及使用者帳戶受信賴，以進行委派](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_22)|SeEnableDelegationPrivilege|
|[強制從遠端系統關機](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_23)|SeRemoteShutdownPrivilege|
|[產生安全性審核](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_24)|SeAuditPrivilege|
|[在驗證後模擬用戶端](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_25)|SeImpersonatePrivilege|
|[增加處理程序工作組](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_26)|SeIncreaseWorkingSetPrivilege|
|[增加排程優先順序](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_27)|SeIncreaseBasePriorityPrivilege|
|[載入及解除載入裝置驅動程式](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_28)|SeLoadDriverPrivilege|
|[在記憶體中鎖定頁面](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_29)|SeLockMemoryPrivilege|
|[以批次工作登入](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_30)|登入 sebatchlogonright|
|[登入為服務](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_31)|SeServiceLogonRight|
|[管理稽核和安全性記錄檔](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_32)|SeSecurityPrivilege|
|[修改物件標籤](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_33)|SeRelabelPrivilege|
|[修改韌體環境值](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_34)|SeSystemEnvironmentPrivilege|
|[執行磁碟區維護工作](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_35)|SeManageVolumePrivilege|
|[監視單一處理程序](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_36)|SeProfileSingleProcessPrivilege|
|[監視系統效能](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_37)|SeSystemProfilePrivilege|
|[從銜接站移除電腦](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_38)|SeUndockPrivilege|
|[取代處理程序等級權杖](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_39)|SeAssignPrimaryTokenPrivilege|
|[還原檔案及目錄](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_40)|SeRestorePrivilege|
|[關閉系統](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_41)|SeShutdownPrivilege|
|[同步處理目錄服務資料](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_42)|SeSyncAgentPrivilege|
|[取得檔案或其他物件的擁有權](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_43)|SeTakeOwnershipPrivilege|

#### <a name="permissions"></a>權限
許可權是套用至安全物件（例如檔案系統、登錄、服務和 Active Directory 物件）的存取控制。 每個安全物件都有相關聯的存取控制清單 (ACL) ，其中包含授與或拒絕安全性主體 (使用者、服務、電腦或群組的存取控制)  (專案，) 可以在物件上執行各種作業的能力。 例如，Active Directory 中許多物件的 Acl 包含 Ace，可讓經過驗證的使用者讀取物件的一般資訊，但不授與讀取機密資訊或變更物件的能力。
除了每個網域的內建來賓帳戶之外，登入並由 Active Directory 樹系或受信任樹系中的網域控制站所驗證的每個安全性主體，都已驗證的使用者安全識別碼 (SID) 預設會新增至其存取權杖。 因此，無論使用者、服務或電腦帳戶是否嘗試讀取網域中使用者物件的一般屬性，讀取作業都會成功。

如果安全性主體嘗試存取未定義任何 Ace 且包含主體存取權杖中之 SID 的物件，則主體無法存取該物件。 此外，如果物件 ACL 中的 ACE 包含符合使用者存取權杖之 SID 的拒絕專案，則 "deny" ACE 通常會覆寫衝突的「允許」 ACE。 如需 Windows 中存取控制的詳細資訊，請參閱 MSDN 網站上的 [存取控制](/windows/win32/secauthz/access-control) 。

在本檔中，許可權是指在安全物件上被授與或拒絕安全性主體的功能。 每當使用者權限和許可權之間發生衝突時，使用者權限通常會優先使用。 例如，如果 Active Directory 中的物件已使用拒絕系統管理員對物件的所有讀取和寫入存取權的 ACL 進行設定，則為該網域的 Administrators 群組成員的使用者將無法查看有關此物件的大量資訊。 不過，由於系統管理員群組被授與使用者權限「取得檔案或其他物件的擁有權」，因此使用者可以直接取得有問題的物件擁有權，然後重寫物件的 ACL，以授與系統管理員對該物件的完全控制權。

基於這個理由，本檔建議您避免使用強大的帳戶和群組進行日常管理，而不是嘗試限制帳戶和群組的功能。 您無法使用這些認證來存取強大認證的已判定使用者，以取得任何安全性實體資源的存取權。

### <a name="built-in-privileged-accounts-and-groups"></a>內建的特殊許可權帳戶和群組
Active Directory 的目的是為了促進系統管理委派，以及指派權利和許可權的最低許可權原則。 依預設，在 Active Directory 網域中具有帳戶的「一般」使用者可以讀取儲存在目錄中的大部分內容，但只能變更目錄中一組非常有限的資料。 需要額外許可權的使用者可以被授與目錄內建的各種特殊許可權群組的成員資格，讓他們可以執行與其角色相關的特定工作，但無法執行與其職責無關的工作。

在 Active Directory 中，有三個內建組組成目錄中的最高許可權群組： Enterprise Admins (EA) 群組、Domain Admins (DA) 群組，以及內建的系統管理員 (BA) 群組。

第四個群組（架構管理員 (SA) 群組）具有的許可權（如果濫用）可能會損毀或終結整個 Active Directory 樹系，但此群組的功能比 EA、DA 和 BA 群組更受限制。

除了這四個群組以外，Active Directory 中還有一些額外的內建和預設帳戶和群組，其中每一個都是授與許可權，讓您能夠執行特定的系統管理工作。 雖然本附錄不提供 Active Directory 中每個內建或預設群組的詳盡討論，但它確實提供了您最可能在安裝中看到的群組和帳戶的表格。

例如，如果您將 Microsoft Exchange Server 安裝到 Active Directory 樹系中，則可以在網域的內建和使用者容器中建立其他帳戶和群組。 本附錄只會說明根據原生角色和功能，在 Active Directory 的內建和使用者容器中建立的群組和帳戶。 不包括由企業軟體安裝所建立的帳戶和群組。

#### <a name="enterprise-admins"></a>企業系統管理員
Enterprise Admins (EA) 群組位於樹系根域中，依預設，它是樹系中每個網域內內建系統管理員群組的成員。 樹系根域中的內建系統管理員帳戶是 EA 群組的唯一預設成員。 EAs 被授與許可權，可讓他們影響整個樹系的變更。 這些變更會影響樹系中的所有網域，例如新增或移除網域、建立樹系信任，或提高樹系功能等級。 在適當設計及實施的委派模型中，只有在第一次建立樹系或進行特定全樹系的變更（例如建立輸出樹系信任）時，才需要 EA 成員資格。

EA 群組依預設位於樹系根域的 Users 容器中，而且是通用安全性群組，除非樹系根域是以 Windows 2000 伺服器混合模式執行，在此情況下，群組是全域安全性群組。 雖然有些許可權是直接授與 EA 群組，但因為它是樹系中每個網域的 Administrators 群組成員，所以此群組的許多許可權實際上都是由 EA 群組所繼承。 企業系統管理員在工作站或成員伺服器上沒有預設許可權。

#### <a name="domain-admins"></a>網域管理員
樹系中的每個網域都有自己的 Domain Admins (DA) 群組，也就是該網域的內建系統管理員 (BA) 群組的成員，以及加入網域的每一部電腦上的本機 Administrators 群組成員。 網域的 DA 群組唯一預設成員是該網域的內建系統管理員帳戶。

DAs 在其網域內全都功能強大，而 EAs 具有全樹系的許可權。 在適當設計及實施的委派模型中，只有在「中斷玻璃」案例中才需要 DA 成員資格，也就是需要在網域中的每部電腦上具有高階許可權的帳戶，或必須進行某些網域廣泛變更時。 雖然原生 Active Directory 委派機制允許委派的程度只是在緊急情況下可以使用 DA 帳戶，但是建立有效的委派模型可能相當耗時，而且許多組織都使用協力廠商應用程式來加速程式。

DA 群組是位於網域的 Users 容器中的全域安全性群組。 樹系中的每個網域都有一個 DA 群組，而 DA 群組的唯一預設成員是網域的內建系統管理員帳戶。 因為網域的 DA 群組會嵌套在網域的 BA 群組和每個加入網域的系統的本機系統管理員群組中，所以 DAs 不僅具有明確授與 Domain Admins 的許可權，也會繼承所有授與網域的系統管理員群組和所有已加入網域之系統上的本機系統管理員群組的許可權。

#### <a name="administrators"></a>系統管理員
內建的系統管理員 (BA) 群組是網域內建容器中的網域本機群組，其中 DAs 和 EAs 會進行嵌套，而這是授與目錄和網域控制站上許多直接許可權的群組。 但是，網域的 Administrators 群組沒有成員伺服器或工作站上的任何許可權。 已加入網域之電腦的本機系統管理員群組成員資格是授與本機許可權的位置;和討論的群組，根據預設，只有 DAs 是所有加入網域之電腦的本機系統管理員群組的成員。

Administrators 群組是網域內建容器中的網域本機群組。 依預設，每個網域的 BA 群組都包含本機網域的內建 Administrator 帳戶、本機網域的 DA 群組，以及樹系根域的 EA 群組。 在 Active Directory 和網域控制站上的許多使用者權限，特別授與 Administrators 群組，而不是 EAs 或 DAs。 網域的 BA 群組會被授與大部分目錄物件的「完全控制」許可權，而且可以取得目錄物件的擁有權。 雖然 EA 和 DA 群組被授與樹系和網域中特定特定物件的許可權，但是群組的大部分功能實際上都是從其在 BA 群組的成員資格中「繼承」。

> [!NOTE]
> 雖然這些是這些特殊許可權群組的預設設定，但這三個群組中的任一個成員都可以操作目錄，以得到任何其他群組的成員資格。 在某些情況下，雖然很容易達成，但是在其他的情況下，比較困難，但是從潛在的許可權觀點來看，所有三個群組都應該視為有效

#### <a name="schema-admins"></a>Schema Admins
架構管理員 (SA) 群組是樹系根域中的萬用群組，而且只有該網域的內建系統管理員帳戶做為預設成員，類似于 EA 群組。 雖然 SA 群組中的成員資格可能會讓攻擊者危害 Active Directory 架構，也就是整個 Active Directory 樹系的架構，所以 SAs 擁有的預設許可權和許可權超過架構。

您應該仔細管理和監視 SA 群組中的成員資格，但在某些情況下，此群組的許可權比稍早所述的三個最高許可權群組低，因為其許可權範圍很窄;也就是，除了架構以外，SAs 沒有任何系統管理許可權。

#### <a name="additional-built-in-and-default-groups-in-active-directory"></a>Active Directory 中的其他內建和預設群組
為了協助在目錄中委派管理，Active Directory 會隨附已授與特定權利和許可權的各種內建和預設群組。 下表簡要說明這些群組。

下表列出 Active Directory 中的內建和預設群組。 預設會有兩組群組：不過，根據預設，內建組會在 Active Directory 中的內建容器中)  (預設群組，而預設群組則預設 (位於) 的 Users 容器中 Active Directory。 內建容器中的群組都是網域本機群組，而 [使用者] 容器中的群組則是 [網域本機]、[全域] 和 [通用] 群組的混合，除了三個個別使用者帳戶以外 (系統管理員、來賓和 Krbtgt) 。

除了本附錄稍早所述的最高許可權群組之外，某些內建和預設的帳戶和群組也會被授與較高的許可權，而且應該只在安全的系統管理主機上受到保護和使用。 您可以在資料表 B-1 中的陰影資料列中找到這些群組和帳戶： Active Directory 中的內建和預設群組和帳戶。 由於其中某些群組和帳戶被授與的權利和許可權可能會被誤用，以危及 Active Directory 或網域控制站，因此會獲得額外的保護，如 [附錄 C： Active Directory 中的受保護帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)所述。

##### <a name="table-b-1-built-in-and-default-accounts-and-groups-in-active-directory"></a>表 B-1： Active Directory 中的內建和預設帳戶和群組

|**帳戶或群組**|**預設容器、群組範圍和類型**|**描述和預設使用者權利**|
|--|--|--|
|Windows Server 2012 中的存取控制協助操作員 (Active Directory) |內建容器<p>網域本機安全性群組|此群組的成員可以從遠端查詢此電腦上資源的授權屬性和許可權。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Account Operators|內建容器<p>網域本機安全性群組|成員可以管理網域使用者和群組帳戶。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|系統管理員帳戶|使用者容器<p>不是群組|用於管理網域的內建帳戶。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>調整處理程序的記憶體配額<p>允許本機登入<p>允許透過遠端桌面服務登入<p>備份檔案及目錄<p>略過周遊檢查<p>變更系統時間<p>變更時區<p>建立分頁檔<p>建立通用物件<p>建立符號連結<p>偵錯程式<p>讓電腦及使用者帳戶受信賴，以進行委派<p>強制從遠端系統關機<p>在驗證後模擬用戶端<p>增加處理程序工作組<p>增加排程優先順序<p>載入及解除載入裝置驅動程式<p>以批次工作登入<p>管理稽核和安全性記錄檔<p>修改韌體環境值<p>執行磁碟區維護工作<p>監視單一處理程序<p>監視系統效能<p>從銜接站移除電腦<p>還原檔案及目錄<p>關閉系統<p>取得檔案或其他物件的擁有權|
|系統管理員群組|內建容器<p>網域本機安全性群組|系統管理員具有網域的完整且不受限制的存取權。<p>**直接使用者權限：**<p>從網路存取這台電腦<p>調整處理程序的記憶體配額<p>允許本機登入<p>允許透過遠端桌面服務登入<p>備份檔案及目錄<p>略過周遊檢查<p>變更系統時間<p>變更時區<p>建立分頁檔<p>建立通用物件<p>建立符號連結<p>偵錯程式<p>讓電腦及使用者帳戶受信賴，以進行委派<p>強制從遠端系統關機<p>在驗證後模擬用戶端<p>增加排程優先順序<p>載入及解除載入裝置驅動程式<p>以批次工作登入<p>管理稽核和安全性記錄檔<p>修改韌體環境值<p>執行磁碟區維護工作<p>監視單一處理程序<p>監視系統效能<p>從銜接站移除電腦<p>還原檔案及目錄<p>關閉系統<p>取得檔案或其他物件的擁有權<p>繼承的使用者權限：<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|允許的 RODC 密碼複寫群組|使用者容器<p>網域本機安全性群組|此群組中的成員可以將其密碼複寫到網域中的所有唯讀網域控制站。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Backup Operators|內建容器<p>網域本機安全性群組|備份操作員可以針對備份或還原檔案的唯一目的覆寫安全性限制。<p>**直接使用者權限：**<p>允許本機登入<p>備份檔案及目錄<p>以批次工作登入<p>還原檔案及目錄<p>關閉系統<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Cert Publishers|使用者容器<p>網域本機安全性群組|允許此群組的成員將憑證發佈至目錄。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|憑證服務 DCOM 存取|內建容器<p>網域本機安全性群組|如果憑證服務是安裝在網域控制站 (不建議) ，此群組會授與網域使用者和網域電腦的 DCOM 註冊存取權。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|在 Windows Server 2012AD DS 中 Cloneable 網域控制站 (AD DS) |使用者容器<p>全域安全性群組|可以複製此群組的成員，也就是網域控制站。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Cryptographic Operators|內建容器<p>網域本機安全性群組|授權成員執行密碼編譯作業。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|偵錯工具使用者|這並不是預設值，也不是內建組，但在 AD DS 時，會造成進一步的調查。|如果有偵錯工具使用者群組，表示偵錯工具已安裝在系統上，不論是透過 Visual Studio、SQL、Office 或其他需要和支援偵錯工具的應用程式。 此群組允許對電腦進行遠端偵錯存取。 當這個群組存在於網域層級時，即表示已在網域控制站上安裝包含偵錯工具的偵錯工具或應用程式。|
|拒絕的 RODC 密碼複寫群組|使用者容器<p>網域本機安全性群組|此群組中的成員無法將其密碼複寫到網域中的任何唯讀網域控制站。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|DHCP 系統管理員|使用者容器<p>網域本機安全性群組|此群組的成員具有 DHCP 伺服器服務的系統管理存取權。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|DHCP 使用者|使用者容器<p>網域本機安全性群組|此群組的成員具有 DHCP 伺服器服務的僅限 view 存取權。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Distributed COM Users|內建容器<p>網域本機安全性群組|此群組的成員可以在這部電腦上啟動、啟用和使用分散式 COM 物件。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|DnsAdmins|使用者容器<p>網域本機安全性群組|此群組的成員具有 DNS 伺服器服務的系統管理存取權。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|DnsUpdateProxy|使用者容器<p>全域安全性群組|此群組的成員是允許代表用戶端執行動態更新的 DNS 用戶端，而這些用戶端本身無法執行動態更新。 此群組的成員通常是 DHCP 伺服器。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|網域管理員|使用者容器<p>全域安全性群組|網域的指定系統管理員;Domain Admins 是每個加入網域之電腦的本機系統管理員群組的成員，而且除了網域的 Administrators 群組以外，也會收到授與本機系統管理員群組的許可權。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>調整處理程序的記憶體配額<p>允許本機登入<p>允許透過遠端桌面服務登入<p>備份檔案及目錄<p>略過周遊檢查<p>變更系統時間<p>變更時區<p>建立分頁檔<p>建立通用物件<p>建立符號連結<p>偵錯程式<p>讓電腦及使用者帳戶受信賴，以進行委派<p>強制從遠端系統關機<p>在驗證後模擬用戶端<p>增加處理程序工作組<p>增加排程優先順序<p>載入及解除載入裝置驅動程式<p>以批次工作登入<p>管理稽核和安全性記錄檔<p>修改韌體環境值<p>執行磁碟區維護工作<p>監視單一處理程序<p>監視系統效能<p>從銜接站移除電腦<p>還原檔案及目錄<p>關閉系統<p>取得檔案或其他物件的擁有權|
|網域電腦|使用者容器<p>全域安全性群組|所有已加入網域的工作站和伺服器預設為此群組的成員。<p>**預設的直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|網域控制站|使用者容器<p>全域安全性群組|網域中的所有網域控制站。 注意：網域控制站不是網域電腦群組的成員。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|網域來賓|使用者容器<p>全域安全性群組|網域中的所有來賓<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|網域使用者|使用者容器<p>全域安全性群組|網域中的所有使用者<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Enterprise Admins (只存在於樹系根域) |使用者容器<p>通用安全性群組|Enterprise Admins 具有變更整個樹系設定的許可權;Enterprise Admins 是每個網域之系統管理員群組的成員，而且會收到授與該群組的許可權。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>調整處理程序的記憶體配額<p>允許本機登入<p>允許透過遠端桌面服務登入<p>備份檔案及目錄<p>略過周遊檢查<p>變更系統時間<p>變更時區<p>建立分頁檔<p>建立通用物件<p>建立符號連結<p>偵錯程式<p>讓電腦及使用者帳戶受信賴，以進行委派<p>強制從遠端系統關機<p>在驗證後模擬用戶端<p>增加處理程序工作組<p>增加排程優先順序<p>載入及解除載入裝置驅動程式<p>以批次工作登入<p>管理稽核和安全性記錄檔<p>修改韌體環境值<p>執行磁碟區維護工作<p>監視單一處理程序<p>監視系統效能<p>從銜接站移除電腦<p>還原檔案及目錄<p>關閉系統<p>取得檔案或其他物件的擁有權|
|企業唯讀網域控制站|使用者容器<p>通用安全性群組|此群組包含樹系中所有唯讀網域控制站的帳戶。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Event Log Readers|內建容器<p>網域本機安全性群組|中這個群組的成員可以讀取網域控制站上的事件記錄檔。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Group Policy Creator Owners|使用者容器<p>全域安全性群組|此群組的成員可以建立和修改網域中群組原則的物件。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|來賓|使用者容器<p>不是群組|這是 AD DS 網域中唯一未將已驗證的使用者 SID 新增至其存取權杖的帳戶。 因此，此帳戶將無法存取設定為對已驗證的使用者群組授與存取權的任何資源。 這對網域來賓和來賓群組的成員而言不是正確的，不過，這些群組的成員會將已驗證的使用者 SID 新增至其存取權杖。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>略過周遊檢查<p>增加處理程序工作組|
|Guests|內建容器<p>網域本機安全性群組|根據預設，來賓與使用者群組的成員具有相同的存取權，但 Guest 帳戶除外，如先前所述，會進一步限制。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Hyper-v 系統管理員 (Windows Server 2012) |內建容器<p>網域本機安全性群組|此群組的成員具有 Hyper-v 所有功能的完整且不受限制的存取權。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|IIS_IUSRS|內建容器<p>網域本機安全性群組|Internet Information Services 所使用的內建組。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|傳入樹系信任產生器 (只存在於樹系根域) |內建容器<p>網域本機安全性群組|此群組的成員可以建立這個樹系的連入單向信任。  (建立輸出樹系信任是保留給 Enterprise Admins。 ) <p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Krbtgt|使用者容器<p>不是群組|Krbtgt 帳戶是網域中 Kerberos 金鑰發佈中心的服務帳戶。 此帳戶可存取儲存在 Active Directory 中的所有帳號憑證。 此帳戶預設為停用，且永遠不會啟用<p>**使用者權限：** N/A|
|Network Configuration Operators|內建容器<p>網域本機安全性群組|此群組的成員會被授與許可權，讓他們能夠管理網路功能的設定。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|效能記錄使用者|內建容器<p>網域本機安全性群組|此群組的成員可以排程效能計數器的記錄、啟用追蹤提供者，以及在本機和遠端存取電腦收集事件追蹤。<p>**直接使用者權限：**<p>以批次工作登入<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|效能監視器使用者|內建容器<p>網域本機安全性群組|此群組的成員可以在本機和遠端存取效能計數器資料。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Pre-Windows 2000 Compatible Access|內建容器<p>網域本機安全性群組|這個群組是為了與 Windows 2000 伺服器之前的作業系統相容，而且可讓成員讀取網域中的使用者和群組資訊。<p>**直接使用者權限：**<p>從網路存取這台電腦<p>略過周遊檢查<p>**繼承的使用者權限：**<p>將工作站新增至網域<p>增加處理程序工作組|
|Print Operators|內建容器<p>網域本機安全性群組|此群組的成員可以管理網域印表機。<p>**直接使用者權限：**<p>允許本機登入<p>載入及解除載入裝置驅動程式<p>關閉系統<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|RAS 與 資訊存取伺服器|使用者容器<p>網域本機安全性群組|此群組中的伺服器可以讀取網域中使用者帳戶的遠端存取屬性。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|RDS 端點伺服器 (Windows Server 2012) |內建容器<p>網域本機安全性群組|此群組中的伺服器會執行使用者 RemoteApp 程式和個人虛擬桌面執行所在的虛擬機器和主機會話。 您必須在執行 RD 連線代理人的伺服器上填入此群組。 部署中使用的 RD 工作階段主機伺服器和 RD 虛擬主機伺服器必須在此群組中。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|RDS 管理伺服器 (Windows Server 2012) |內建容器<p>網域本機安全性群組|此群組中的伺服器可以在執行遠端桌面服務的伺服器上執行例行的系統管理動作。 此群組必須填入遠端桌面服務部署中的所有伺服器上。 執行 RDS 中央管理服務的伺服器必須包含在此群組中。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|RDS 遠端存取服務器 (Windows Server 2012) |內建容器<p>網域本機安全性群組|此群組中的伺服器可讓 RemoteApp 程式的使用者和個人虛擬桌面存取這些資源。 在面向網際網路的部署中，這些伺服器通常會部署在邊緣網路中。 您必須在執行 RD 連線代理人的伺服器上填入此群組。 部署中使用的 RD 閘道伺服器和 RD Web 存取伺服器必須在此群組中。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Read-only Domain Controllers|使用者容器<p>全域安全性群組|此群組包含網域中的所有唯讀網域控制站。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Remote Desktop Users|內建容器<p>網域本機安全性群組|此群組的成員會被授與使用 RDP 從遠端登入的許可權。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|遠端系統管理使用者 (Windows Server 2012) |內建容器<p>網域本機安全性群組|此群組的成員可以透過管理通訊協定存取 WMI 資源 (例如透過 Windows 遠端管理服務) 的 WS-MANAGEMENT。 這僅適用于授與使用者存取權的 WMI 命名空間。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Replicator|內建容器<p>網域本機安全性群組|支援網域中的舊版檔案複寫。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|架構管理員 (只存在於樹系根域) |使用者容器<p>通用安全性群組|架構管理員是唯一可以修改 Active Directory 架構的使用者，而且只有在架構是可寫入的情況下才可進行修改。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Server Operators|內建容器<p>網域本機安全性群組|此群組的成員可以管理網域伺服器。<p>**直接使用者權限：**<p>允許本機登入<p>備份檔案及目錄<p>變更系統時間<p>變更時區<p>強制從遠端系統關機<p>還原檔案及目錄<p>關閉系統<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|終端機伺服器授權伺服器|內建容器<p>網域本機安全性群組|此群組的成員可以更新 Active Directory 中的使用者帳戶，並提供授權發行的相關資訊，以追蹤和報告 TS 每個使用者的 CAL 使用量<p>**預設的直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|使用者|內建容器<p>網域本機安全性群組|使用者擁有的許可權可讓他們在 Active Directory 中讀取許多物件和屬性，但無法變更大部分的物件和屬性。 使用者無法進行意外或刻意進行全系統的變更，並且可以執行大部分的應用程式。<p>**直接使用者權限：**<p>增加處理程序工作組<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查|
|Windows Authorization Access 群組|內建容器<p>網域本機安全性群組|這個群組的成員可以存取使用者物件上的計算 tokenGroupsGlobalAndUniversal 屬性<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|WinRMRemoteWMIUsers_ (Windows Server 2012) |使用者容器<p>網域本機安全性群組|此群組的成員可以透過管理通訊協定存取 WMI 資源 (例如透過 Windows 遠端管理服務) 的 WS-MANAGEMENT。 這僅適用于授與使用者存取權的 WMI 命名空間。<p>**直接使用者權限：** 沒有<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
