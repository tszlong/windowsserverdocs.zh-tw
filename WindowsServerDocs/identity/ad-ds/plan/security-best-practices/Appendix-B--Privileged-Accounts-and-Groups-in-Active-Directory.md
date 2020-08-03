---
ms.assetid: 79b9c912-ea3e-4679-ab41-893e096c4d09
title: 附錄 B-Active Directory 中的特殊許可權帳戶和群組
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 9b9f2700ff4c7ef265603301ccebc95f37b8861c
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2020
ms.locfileid: "87520147"
---
# <a name="appendix-b-privileged-accounts-and-groups-in-active-directory"></a>附錄 B︰Active Directory 中具特殊權限的帳戶和群組

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-b-privileged-accounts-and-groups-in-active-directory"></a>附錄 B︰Active Directory 中具特殊權限的帳戶和群組
Active Directory 中的「特殊許可權」帳戶和群組是授與功能強大的權利、許可權和許可權，讓他們能夠在 Active Directory 和加入網域的系統上執行幾乎任何動作。 本附錄一開始會討論權限、許可權和許可權，然後是 Active Directory 中「最高許可權」帳戶和群組的相關資訊，也就是最強大的帳戶和群組。

除了其權利之外，也會提供 Active Directory 內建和預設帳戶和群組的相關資訊。 雖然保護最高許可權帳戶和群組的特定設定建議是以個別附錄的方式提供，但本附錄提供的背景資訊可協助您識別應專注于保護的使用者和群組。 您應該這麼做，因為攻擊者可以利用它們來入侵，甚至摧毀您的 Active Directory 安裝。

### <a name="rights-privileges-and-permissions-in-active-directory"></a>Active Directory 中的許可權、許可權和許可權
權利、許可權和許可權之間的差異可能會令人困惑，甚至是 Microsoft 的檔中的衝突。 本節說明每個內容在本檔中使用時的某些特性。 這些描述不應視為其他 Microsoft 檔的權威，因為它可能會以不同的方式使用這些詞彙。

#### <a name="rights-and-privileges"></a>權利和許可權
許可權和許可權實際上是授與安全性主體（例如使用者、服務、電腦或群組）的相同全系統功能。 在一般由 IT 專業人員使用的介面中，這些通常稱為「許可權」或「使用者權限」，而且通常是由群組原則物件所指派。 下列螢幕擷取畫面顯示一些最常見的使用者權限，可以指派給安全性主體（它代表 Windows Server 2012 網域中的預設網域控制站 GPO）。 這些許可權中有部分適用于 Active Directory，例如「**讓電腦和使用者帳戶受信任委派**」使用者權限，而其他許可權則適用于 Windows 作業系統，例如**變更系統時間**。

![特殊許可權帳戶和群組](media/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory/SAD_8.gif)

在群組原則物件編輯器之類的介面中，這些可指派的功能都廣泛地被視為使用者權限。 不過事實上，有些使用者權限是以程式設計方式稱為「許可權」，而有些則是以程式設計方式稱為「許可權」。 表 B-1：使用者權限和許可權提供一些最常見的可指派使用者權限及其程式設計常數。 雖然群組原則和其他介面會將這些全都視為使用者權限，但有些是以程式設計方式識別為許可權，而有些則是定義為許可權。

如需下表所列每個使用者權利的詳細資訊，請使用表格中的連結，或參閱 Microsoft TechNet 網站上的[威脅與對策指南：](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/hh125917(v=ws.10)) Windows Server 2008 R2 的[威脅和弱點緩和](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755181(v=ws.10))指南中的使用者權限。 如需適用于 Windows Server 2008 的資訊，請參閱 Microsoft TechNet 網站上的[威脅和弱點緩和措施](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755181(v=ws.10))檔中的[使用者權限](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10))。 在撰寫本檔時，尚未發行 Windows Server 2012 的對應檔。

> [!NOTE]
> 基於本檔的目的，除非另有指定，否則會使用「權利」和「使用者權限」等詞彙來識別許可權和許可權。

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
|[產生安全性審查](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_24)|SeAuditPrivilege|
|[在驗證後模擬用戶端](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_25)|SeImpersonatePrivilege|
|[增加處理程序工作組](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_26)|工作組 seincreaseworkingsetprivilege|
|[增加排程優先順序](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_27)|SeIncreaseBasePriorityPrivilege|
|[載入及解除載入裝置驅動程式](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_28)|SeLoadDriverPrivilege|
|[在記憶體中鎖定頁面](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd349804(v=ws.10)#BKMK_29)|分頁 selockmemoryprivilege|
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
許可權是套用到安全物件（例如檔案系統、登錄、服務和 Active Directory 物件）的存取控制。 每個安全物件都有相關聯的存取控制清單（ACL），其中包含授與或拒絕安全性主體（使用者、服務、電腦或群組）的存取控制專案（Ace），可以在物件上執行各種作業。 例如，Active Directory 中許多物件的 Acl 包含 Ace，可讓已驗證的使用者讀取物件的一般資訊，但不授與他們讀取機密資訊或變更物件的能力。
除了每個網域的內建來賓帳戶以外，登入並由 Active Directory 樹系或受信任樹系中的網域控制站驗證的每個安全性主體，預設都會將已驗證的使用者安全識別碼（SID）新增至其存取權杖。 因此，無論使用者、服務或電腦帳戶是否嘗試讀取網域中使用者物件的一般屬性，讀取操作都會成功。

如果安全性主體嘗試存取的物件未定義任何 Ace，而且包含主體的存取權杖中存在的 SID，則主體將無法存取該物件。 此外，如果物件的 ACL 中的 ACE 包含符合使用者存取權杖之 SID 的拒絕專案，則 "deny" ACE 通常會覆寫衝突的 "allow" ACE。 如需 Windows 存取控制的詳細資訊，請參閱 MSDN 網站上的[存取控制](/windows/win32/secauthz/access-control)。

在本檔中，許可權是指對安全物件上的安全性主體授與或拒絕的功能。 每當使用者權限與許可權之間發生衝突時，使用者權限通常會優先使用。 例如，如果 Active Directory 中的物件已設定 ACL，而拒絕系統管理員對物件的所有讀取和寫入權限，則身為網域系統管理員群組成員的使用者將無法查看物件的許多相關資訊。 不過，因為系統管理員群組被授與使用者「取得檔案或其他物件的擁有權」許可權，所以使用者可以只取得有問題之物件的擁有權，然後重寫物件的 ACL，以授與系統管理員對物件的完全控制權。

基於這個理由，這份檔鼓勵您避免使用功能強大的帳戶和群組來進行日常管理，而不是嘗試限制帳戶和群組的功能。 您不能有效地停止具有強大認證存取權的已判定使用者，使其無法使用這些認證來取得任何安全性實體資源的存取權。

### <a name="built-in-privileged-accounts-and-groups"></a>內建的特殊許可權帳戶和群組
Active Directory 旨在促進系統管理委派，以及指派權利和許可權的最低許可權原則。 根據預設，在 Active Directory 網域中具有帳戶的「一般」使用者，可以讀取儲存在目錄中的大部分內容，但只能變更目錄中非常有限的資料集。 需要額外許可權的使用者可以被授與目錄內建的各種特殊許可權群組的成員資格，使其可以執行與其角色相關的特定工作，但無法執行與其責任無關的工作。

在 Active Directory 中，有三個內建組組成目錄中的最高許可權群組： Enterprise Admins （EA）群組、Domain Admins （DA）群組，以及內建的 Administrators （BA）群組。

第四個群組（架構系統管理員（SA）群組）具有許可權，如果被濫用，可能會損毀或破壞整個 Active Directory 樹系，但此群組的功能比 EA、DA 和 BA 群組更受限制。

除了這四個群組之外，Active Directory 還有一些額外的內建和預設帳戶和群組，每個都被授與許可權，以允許執行特定的系統管理工作。 雖然本附錄並未提供 Active Directory 中每個內建或預設群組的完整討論，但它確實提供了您在安裝中最有可能看到的群組和帳戶的表格。

例如，如果您將 Microsoft Exchange Server 安裝到 Active Directory 樹系，則可以在您網域中的內建和使用者容器中建立其他帳戶和群組。 本附錄僅說明根據原生角色和功能，在 Active Directory 內建和使用者容器中建立的群組和帳戶。 不包含由企業軟體安裝所建立的帳戶和群組。

#### <a name="enterprise-admins"></a>企業系統管理員
Enterprise Admins （EA）群組位於樹系根域中，根據預設，它是樹系中每個網域內內建系統管理員群組的成員。 樹系根域中的內建 Administrator 帳戶是 EA 群組的唯一預設成員。 EAs 會被授與許可權，使其能夠影響整個樹系的變更。 這些變更會影響樹系中的所有網域，例如新增或移除網域、建立樹系信任，或提高樹系功能等級。 在適當設計和實行的委派模型中，只有在第一次建立樹系或進行特定樹系的變更（例如建立輸出樹系信任）時，才需要 EA 成員資格。

根據預設，EA 群組位於樹系根域的 [使用者] 容器中，而它是通用安全性群組，除非樹系根域是在 Windows 2000 Server 混合模式下執行，在此情況下，群組是全域安全性群組。 雖然部分許可權會直接授與 EA 群組，但 EA 群組實際上會繼承其中許多許可權，因為它是樹系中每個網域中 Administrators 群組的成員。 企業系統管理員在工作站或成員伺服器上沒有預設許可權。

#### <a name="domain-admins"></a>網域管理員
樹系中的每個網域都有自己的 Domain Admins （DA）群組，這是該網域內建系統管理員（BA）群組的成員，以及每一部加入網域的電腦上本機 Administrators 群組的成員。 網域的 DA 群組唯一的預設成員是該網域的內建系統管理員帳戶。

DAs 在其網域內具有強大的功能，而 EAs 則具有全樹系許可權。 在適當設計並實行的委派模型中，只有在「中斷玻璃」的情況下才需要 DA 成員資格，這種情況是需要在網域中的每部電腦上具有高層級許可權的帳戶，或是必須進行特定網域變更時。 雖然原生 Active Directory 委派機制允許委派的範圍，只有在緊急情況下才能夠使用 DA 帳戶，而建立有效的委派模型可能相當耗時，而許多組織會使用協力廠商應用程式來加速此流程。

「DA」群組是位於網域的 [使用者] 容器中的全域安全性群組。 樹系中的每個網域都有一個 DA 群組，而只有一個 DA 群組的預設成員是網域的內建系統管理員帳戶。 因為定義域的 DA 群組是嵌套在網域的 BA 群組中，而每個加入網域的系統本機 Administrators 群組中，所以 DAs 不僅擁有專門授與 Domain Admins 的許可權，還會繼承所有已加入網域之系統管理員群組和本機 Administrators 群組的擁有權利和許可權。

#### <a name="administrators"></a>Administrators
內建的系統管理員（BA）群組是網域內建容器中的網域本機群組，其中會將 DAs 和 EAs 嵌套在其中，而此群組會被授與目錄中和網域控制站上許多直接的許可權。 不過，網域的 Administrators 群組不具有成員伺服器或工作站上的任何許可權。 已加入網域之電腦的本機系統管理員群組成員資格是授與本機許可權的位置;和所討論的群組，只有 DAs 是所有已加入網域之電腦的本機系統管理員群組的成員。

Administrators 群組是網域內建容器中的一個網網域本機群組。 根據預設，每個網域的 BA 群組都會包含本機網域的內建系統管理員帳戶、本機網域的 DA 群組，以及樹系根域的 EA 群組。 Active Directory 和網域控制站上的許多使用者權限專門授與系統管理員群組，而不是 EAs 或 DAs。 定義域的 BA 群組會被授與大部分目錄物件的完全控制許可權，而且可以取得目錄物件的擁有權。 雖然 EA 和 DA 群組會被授與樹系和網域中特定物件特定的許可權，但群組的大部分功能實際上是從其 BA 群組的成員資格中「繼承」。

> [!NOTE]
> 雖然這些是這些特殊許可權群組的預設設定，但其中任一個群組的成員都可以操控該目錄，以取得任何其他群組中的成員資格。 在某些情況下，很容易就能達成，而在其他方面則比較困難，但是從潛在許可權的觀點來看，這三個群組都應該視為有效的對等專案。

#### <a name="schema-admins"></a>Schema Admins
架構系統管理員（SA）群組是樹系根域中的萬用群組，而且只有該網域的內建 Administrator 帳戶做為預設成員，與 EA 群組類似。 雖然 SA 群組中的成員資格可以讓攻擊者危害 Active Directory 架構，這是整個 Active Directory 樹系的架構，但 SAs 在架構之外有一些預設許可權和許可權。

您應該仔細管理及監視 SA 群組中的成員資格，但在某些方面，此群組的許可權比先前所述的三個最高許可權群組「低許可權」，因為其許可權的範圍非常窄;也就是說，SAs 在架構以外的任何地方都沒有系統管理許可權。

#### <a name="additional-built-in-and-default-groups-in-active-directory"></a>Active Directory 中的其他內建和預設群組
為了協助委派目錄中的管理，Active Directory 隨附各種已授與特定權利和許可權的內建和預設群組。 下表將簡短說明這些群組。

下表列出 Active Directory 中的內建和預設群組。 預設會有這兩組群組：不過，內建組會在 Active Directory 的內建容器中找到（根據預設），而預設群組則位於 Active Directory 的 [使用者] 容器中。 內建容器中的群組是所有的網域本機群組，而 [使用者] 容器中的群組則是除了三個個別使用者帳戶（系統管理員、來賓和 Krbtgt）以外，還會混用網域本機、全域和萬用群組。

除了本附錄稍早所述的最高許可權群組以外，某些內建和預設帳戶和群組會被授與較高的許可權，而且也應該受到保護並僅用於安全的系統管理主機。 這些群組和帳戶可在資料表 B-1 的陰影資料列中找到： Active Directory 中的內建和預設群組和帳戶。 因為這些群組和帳戶中有部分是被授與的權利和許可權，可以誤用以洩露 Active Directory 或網域控制站，所以會以 Active Directory 中的[附錄 C：受保護的帳戶和群組中](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)所述的方式，為其進行額外的保護。

##### <a name="table-b-1-built-in-and-default-accounts-and-groups-in-active-directory"></a>表 B-1： Active Directory 中的內建和預設帳戶和群組

|**帳戶或群組**|**預設容器、群組範圍和類型**|**描述和預設使用者權力**|
|--|--|--|
|存取控制協助操作員（Windows Server 2012 中的 Active Directory）|內建容器<p>網域-本機安全性群組|此群組的成員可以從遠端查詢此電腦上資源的授權屬性和許可權。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Account Operators|內建容器<p>網域-本機安全性群組|成員可以管理網域使用者和群組帳戶。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|系統管理員帳戶|使用者容器<p>不是群組|用來管理網域的內建帳戶。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>調整處理程序的記憶體配額<p>允許本機登入<p>允許透過遠端桌面服務登入<p>備份檔案及目錄<p>略過周遊檢查<p>變更系統時間<p>變更時區<p>建立分頁檔<p>建立通用物件<p>建立符號連結<p>偵錯程式<p>讓電腦及使用者帳戶受信賴，以進行委派<p>強制從遠端系統關機<p>在驗證後模擬用戶端<p>增加處理程序工作組<p>增加排程優先順序<p>載入及解除載入裝置驅動程式<p>以批次工作登入<p>管理稽核和安全性記錄檔<p>修改韌體環境值<p>執行磁碟區維護工作<p>監視單一處理程序<p>監視系統效能<p>從銜接站移除電腦<p>還原檔案及目錄<p>關閉系統<p>取得檔案或其他物件的擁有權|
|系統管理員群組|內建容器<p>網域-本機安全性群組|系統管理員對網域具有完整且不受限制的存取權。<p>**直接使用者權限：**<p>從網路存取這台電腦<p>調整處理程序的記憶體配額<p>允許本機登入<p>允許透過遠端桌面服務登入<p>備份檔案及目錄<p>略過周遊檢查<p>變更系統時間<p>變更時區<p>建立分頁檔<p>建立通用物件<p>建立符號連結<p>偵錯程式<p>讓電腦及使用者帳戶受信賴，以進行委派<p>強制從遠端系統關機<p>在驗證後模擬用戶端<p>增加排程優先順序<p>載入及解除載入裝置驅動程式<p>以批次工作登入<p>管理稽核和安全性記錄檔<p>修改韌體環境值<p>執行磁碟區維護工作<p>監視單一處理程序<p>監視系統效能<p>從銜接站移除電腦<p>還原檔案及目錄<p>關閉系統<p>取得檔案或其他物件的擁有權<p>繼承的使用者權限：<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|允許的 RODC 密碼複寫群組|使用者容器<p>網域-本機安全性群組|此群組中的成員可以將其密碼複寫到網域中的所有唯讀網域控制站。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Backup Operators|內建容器<p>網域-本機安全性群組|備份操作員可以針對備份或還原檔案的唯一目的，覆寫安全性限制。<p>**直接使用者權限：**<p>允許本機登入<p>備份檔案及目錄<p>以批次工作登入<p>還原檔案及目錄<p>關閉系統<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Cert Publishers|使用者容器<p>網域-本機安全性群組|允許此群組的成員將憑證發佈至目錄。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|憑證服務 DCOM 存取|內建容器<p>網域-本機安全性群組|如果憑證服務是安裝在網域控制站上（不建議），則此群組會授與網域使用者和網域電腦的 DCOM 註冊存取權。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Cloneable 網域控制站（Windows Server 2012AD DS 中的 AD DS）|使用者容器<p>全域安全性群組|此群組的成員可能會進行複製。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Cryptographic Operators|內建容器<p>網域-本機安全性群組|成員已獲授權可執行密碼編譯作業。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|偵錯工具使用者|這既不是預設值，也不是內建群組，但如果存在於 AD DS 中，則是進一步調查的原因。|有了偵錯工具使用者群組，表示已經在某個時間點安裝偵錯工具，不論是透過 Visual Studio、SQL、Office 或其他需要和支援偵錯工具的應用程式。 此群組允許對電腦進行遠端偵錯的存取。 當此群組存在於網域層級時，表示已在網域控制站上安裝包含偵錯工具的偵錯工具或應用程式。|
|拒絕的 RODC 密碼複寫群組|使用者容器<p>網域-本機安全性群組|此群組中的成員不能將其密碼複寫到網域中的任何唯讀網域控制站。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|DHCP 系統管理員|使用者容器<p>網域-本機安全性群組|此群組的成員具有 DHCP 伺服器服務的系統管理存取權。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|DHCP 使用者|使用者容器<p>網域-本機安全性群組|此群組的成員具有 DHCP 伺服器服務的僅限查看存取權。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Distributed COM Users|內建容器<p>網域-本機安全性群組|此群組的成員可在這部電腦上啟動、啟用及使用分散式 COM 物件。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|DnsAdmins|使用者容器<p>網域-本機安全性群組|此群組的成員具有 DNS 伺服器服務的系統管理存取權。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|DnsUpdateProxy|使用者容器<p>全域安全性群組|此群組的成員是允許執行動態更新的 DNS 用戶端，這些用戶端無法自行執行動態更新。 此群組的成員通常是 DHCP 伺服器。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|網域管理員|使用者容器<p>全域安全性群組|網域的指定系統管理員;網域系統管理員是每個加入網域之電腦的本機系統管理員群組的成員，除了網域的 Administrators 群組之外，還會接收授與本機 Administrators 群組的許可權。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>調整處理程序的記憶體配額<p>允許本機登入<p>允許透過遠端桌面服務登入<p>備份檔案及目錄<p>略過周遊檢查<p>變更系統時間<p>變更時區<p>建立分頁檔<p>建立通用物件<p>建立符號連結<p>偵錯程式<p>讓電腦及使用者帳戶受信賴，以進行委派<p>強制從遠端系統關機<p>在驗證後模擬用戶端<p>增加處理程序工作組<p>增加排程優先順序<p>載入及解除載入裝置驅動程式<p>以批次工作登入<p>管理稽核和安全性記錄檔<p>修改韌體環境值<p>執行磁碟區維護工作<p>監視單一處理程序<p>監視系統效能<p>從銜接站移除電腦<p>還原檔案及目錄<p>關閉系統<p>取得檔案或其他物件的擁有權|
|網域電腦|使用者容器<p>全域安全性群組|加入網域的所有工作站和伺服器都是此群組的預設成員。<p>**預設直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|網域控制站|使用者容器<p>全域安全性群組|網域中的所有網域控制站。 注意：網域控制站不是網域電腦群組的成員。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|網域來賓|使用者容器<p>全域安全性群組|網域中的所有來賓<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|網域使用者|使用者容器<p>全域安全性群組|網域中的所有使用者<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Enterprise Admins （僅存在於樹系根域中）|使用者容器<p>通用安全性群組|企業系統管理員具有變更全樹系設定的許可權;Enterprise Admins 是每個網域之系統管理員群組的成員，並且會接收授與該群組的許可權和許可權。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>調整處理程序的記憶體配額<p>允許本機登入<p>允許透過遠端桌面服務登入<p>備份檔案及目錄<p>略過周遊檢查<p>變更系統時間<p>變更時區<p>建立分頁檔<p>建立通用物件<p>建立符號連結<p>偵錯程式<p>讓電腦及使用者帳戶受信賴，以進行委派<p>強制從遠端系統關機<p>在驗證後模擬用戶端<p>增加處理程序工作組<p>增加排程優先順序<p>載入及解除載入裝置驅動程式<p>以批次工作登入<p>管理稽核和安全性記錄檔<p>修改韌體環境值<p>執行磁碟區維護工作<p>監視單一處理程序<p>監視系統效能<p>從銜接站移除電腦<p>還原檔案及目錄<p>關閉系統<p>取得檔案或其他物件的擁有權|
|企業唯讀網域控制站|使用者容器<p>通用安全性群組|此群組包含樹系中所有唯讀網域控制站的帳戶。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Event Log Readers|內建容器<p>網域-本機安全性群組|中此群組的成員可以讀取網域控制站上的事件記錄檔。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Group Policy Creator Owners|使用者容器<p>全域安全性群組|此群組的成員可以建立和修改網域中群組原則物件。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|來賓|使用者容器<p>不是群組|這是在未將已驗證的使用者 SID 新增至其存取權杖的 AD DS 網域中唯一的帳戶。 因此，設定為授與已驗證使用者群組存取權的任何資源，將無法供此帳戶存取。 網域來賓和來賓群組的成員不會有此行為，不過，這些群組的成員會將已驗證的使用者 SID 新增至其存取權杖。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>略過周遊檢查<p>增加處理程序工作組|
|Guests|內建容器<p>網域-本機安全性群組|根據預設，來賓具有與 Users 群組成員相同的存取權，但 Guest 帳戶除外，這會進一步受到限制，如先前所述。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Hyper-v 系統管理員（Windows Server 2012）|內建容器<p>網域-本機安全性群組|此群組的成員具有 Hyper-v 所有功能的完整且不受限制的存取權。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|IIS_IUSRS|內建容器<p>網域-本機安全性群組|Internet Information Services 使用的內建組。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|連入樹系信任產生器（只存在於樹系根域中）|內建容器<p>網域-本機安全性群組|此群組的成員可以建立對此樹系的連入單向信任。 （建立輸出樹系信任是保留給企業系統管理員。）<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Krbtgt|使用者容器<p>不是群組|Krbtgt 帳戶是網域中 Kerberos 金鑰發佈中心的服務帳戶。 此帳戶可存取儲存在 Active Directory 中的所有帳號憑證。 此帳戶預設為停用，且永遠不會啟用<p>**使用者權限：** N/A|
|Network Configuration Operators|內建容器<p>網域-本機安全性群組|此群組的成員會被授與許可權，讓他們能夠管理網路功能的設定。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|效能記錄使用者|內建容器<p>網域-本機安全性群組|此群組的成員可以排程效能計數器的記錄、啟用追蹤提供者，以及在本機收集事件追蹤，以及透過遠端存取電腦。<p>**直接使用者權限：**<p>以批次工作登入<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|效能監視器使用者|內建容器<p>網域-本機安全性群組|此群組的成員可以在本機和遠端存取效能計數器資料。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Pre-Windows 2000 Compatible Access|內建容器<p>網域-本機安全性群組|此群組存在於 Windows 2000 Server 之前的作業系統回溯相容性，並可讓成員讀取網域中的使用者和群組資訊。<p>**直接使用者權限：**<p>從網路存取這台電腦<p>略過周遊檢查<p>**繼承的使用者權限：**<p>將工作站新增至網域<p>增加處理程序工作組|
|Print Operators|內建容器<p>網域-本機安全性群組|此群組的成員可以管理網域印表機。<p>**直接使用者權限：**<p>允許本機登入<p>載入及解除載入裝置驅動程式<p>關閉系統<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|RAS 與 資訊存取伺服器|使用者容器<p>網域-本機安全性群組|此群組中的伺服器可以讀取網域中使用者帳戶的遠端存取屬性。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|RDS 端點伺服器（Windows Server 2012）|內建容器<p>網域-本機安全性群組|此群組中的伺服器會執行使用者 RemoteApp 程式和個人虛擬桌面電腦上執行的虛擬機器和主機會話。 此群組必須填入執行 RD 連線代理人的伺服器上。 部署中使用的 RD 工作階段主機伺服器和 RD 虛擬主機伺服器必須在此群組中。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|RDS 管理伺服器（Windows Server 2012）|內建容器<p>網域-本機安全性群組|此群組中的伺服器可以在執行遠端桌面服務的伺服器上執行例行的系統管理動作。 此群組必須填入遠端桌面服務部署中的所有伺服器上。 執行 RDS 中央管理服務的伺服器必須包含在此群組中。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|RDS 遠端存取服務器（Windows Server 2012）|內建容器<p>網域-本機安全性群組|此群組中的伺服器可讓 RemoteApp 程式的使用者和個人虛擬桌面存取這些資源。 在網際網路面向的部署中，這些伺服器通常會部署在邊緣網路中。 此群組必須填入執行 RD 連線代理人的伺服器上。 部署中使用的 RD 閘道伺服器和 RD Web 存取伺服器必須在此群組中。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Read-only Domain Controllers|使用者容器<p>全域安全性群組|此群組包含網域中所有的唯讀網域控制站。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Remote Desktop Users|內建容器<p>網域-本機安全性群組|此群組的成員會被授與使用 RDP 從遠端登入的許可權。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|遠端系統管理使用者（Windows Server 2012）|內建容器<p>網域-本機安全性群組|此群組的成員可以透過管理通訊協定（例如透過 Windows 遠端管理服務的 WS-MANAGEMENT）存取 WMI 資源。 這只適用于授與使用者存取權的 WMI 命名空間。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Replicator|內建容器<p>網域-本機安全性群組|支援網域中的舊版檔案複寫。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|架構管理員（只存在於樹系根域中）|使用者容器<p>通用安全性群組|架構管理員是唯一可以對 Active Directory 架構進行修改的使用者，而且只有在架構已啟用寫入的情況下才會進行。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|Server Operators|內建容器<p>網域-本機安全性群組|此群組的成員可以管理網域伺服器。<p>**直接使用者權限：**<p>允許本機登入<p>備份檔案及目錄<p>變更系統時間<p>變更時區<p>強制從遠端系統關機<p>還原檔案及目錄<p>關閉系統<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|終端機伺服器授權伺服器|內建容器<p>網域-本機安全性群組|此群組的成員可以使用授權發行的相關資訊來更新 Active Directory 中的使用者帳戶，以便追蹤和報告 TS 每個使用者 CAL 使用量<p>**預設直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|使用者|內建容器<p>網域-本機安全性群組|使用者擁有允許他們在 Active Directory 中讀取許多物件和屬性的許可權，雖然它們無法變更。 使用者無法進行意外或刻意的全系統變更，而且可以執行大部分的應用程式。<p>**直接使用者權限：**<p>增加處理程序工作組<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查|
|Windows Authorization Access 群組|內建容器<p>網域-本機安全性群組|此群組的成員可以存取使用者物件上的計算 tokenGroupsGlobalAndUniversal 屬性<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
|WinRMRemoteWMIUsers_ （Windows Server 2012）|使用者容器<p>網域-本機安全性群組|此群組的成員可以透過管理通訊協定（例如透過 Windows 遠端管理服務的 WS-MANAGEMENT）存取 WMI 資源。 這只適用于授與使用者存取權的 WMI 命名空間。<p>**直接使用者權限：** 無<p>**繼承的使用者權限：**<p>從網路存取這台電腦<p>將工作站新增至網域<p>略過周遊檢查<p>增加處理程序工作組|
