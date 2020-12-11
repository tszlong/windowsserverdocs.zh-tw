---
description: 深入瞭解：監視入侵徵兆的 Active Directory
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: 監視 Active Directory 遭到危害的徵兆
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 30aa1010d1222b61ed21d2a3921d0166f24f7a79
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97042476"
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>監視 Active Directory 遭到危害的徵兆

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

*定律號碼5：永久性警覺性來得是安全性的價格。* - [安全性管理的10個不可變法則](/previous-versions/cc722488(v=technet.10))

穩固的事件日誌監視系統是任何安全 Active Directory 設計的重要部分。 如果受害者制定適當的事件記錄檔監視和警示，可能會提早發現許多電腦安全性性危害。 此結論是獨立報表的長期支援。 例如， [2009 Verizon 資料缺口報告](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf) 指出：

「事件監視和記錄分析的明顯 ineffectiveness 會持續成為 enigma。 偵測的機會有：調查人員注意到66% 的受害者在其記錄中有足夠的可用辨識項，以探索缺口，讓他們在分析這類資源時更用心。」

這缺乏監視使用中的事件記錄，在許多公司的安全性防禦計畫中仍維持一致的弱點。 [2012 Verizon 資料缺口報告](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf)發現，即使85% 的違規時間花了數周的時間才會被察覺，但84% 的受害者在事件記錄檔中有缺口。

## <a name="windows-audit-policy"></a>Windows 稽核原則

以下是 Microsoft 官方企業支援 blog 的連結。 這些 blog 的內容提供有關進行審核的建議、指引和建議，可協助您強化 Active Directory 基礎結構的安全性，並在設計稽核原則時是很重要的資源。

* [全域物件存取審核是神奇](/archive/blogs/askds/global-object-access-auditing-is-magic) 之處，說明了一種稱為 Advanced Audit Policy Configuration 的控制機制，它已新增至 windows 7 和 windows Server 2008 R2，可讓您設定想要輕鬆進行審核的資料類型，而不是操控腳本和 auditpol.exe。
* [Windows 2008 中的審核變更簡介](/archive/blogs/askds/introducing-auditing-changes-in-windows-2008) -介紹 windows Server 2008 中所做的審核變更。
* [Vista 和2008中的酷炫審核技巧](/archive/blogs/askds/cool-auditing-tricks-in-vista-and-2008) -說明 Windows Vista 和 windows Server 2008 的有趣審核功能，可用於疑難排解問題或查看環境中發生的狀況。
* [Windows server 2008 和 Windows vista 中的一項-停用一次進行審核](/archive/blogs/askds/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista) -包含 windows server 2008 和 windows vista 所包含的審核功能和資訊的編譯。

下列連結提供有關 Windows 8 和 Windows Server 2012 中 Windows 審核功能的改進資訊，以及 Windows Server 2008 中 AD DS 審核的相關資訊。

* [安全性審核的新](/previous-versions/orphan-topics/ws.11/hh849638(v=ws.11)) 功能-概述 Windows 8 和 Windows Server 2012 中的新安全性審核功能。
* [AD DS 《逐步解說》逐步指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731607(v=ws.10)) -說明 Windows Server 2008 中的新 Active Directory Domain Services (AD DS) 審核功能。 它也提供執行這項新功能的程式。

### <a name="windows-audit-categories"></a>Windows Audit 類別

在 Windows Vista 和 Windows Server 2008 之前，Windows 只有九個事件記錄檔稽核原則類別：

* 帳戶登入事件
* 帳戶管理
* 目錄服務存取
* 登入事件
* 物件存取
* 原則變更
* 許可權使用
* 進程追蹤
* 系統事件

這九個傳統審核類別組成稽核原則。 每個稽核原則類別都可以啟用成功、失敗或成功和失敗事件。 下一節包含其說明。

#### <a name="audit-policy-category-descriptions"></a>稽核原則類別描述
稽核原則類別目錄可啟用下列事件記錄檔訊息類型。

##### <a name="audit-account-logon-events"></a>審核帳戶登入事件
報告安全性主體的每個實例 (例如，登入或登出一部電腦的使用者、電腦或服務帳戶) ，而另一部電腦是用來驗證帳戶。 當網域控制站上的網域安全性主體帳戶經過驗證時，就會產生帳戶登入事件。 本機電腦上的本機使用者驗證會產生登入本機安全性記錄檔的登入事件。 不會記錄任何帳戶登出事件。

此類別會產生許多「雜訊」，因為 Windows 會在正常運作的情況下，讓帳戶在本機和遠端電腦上一直登入或登出。 但是，任何安全性方案都應該包含此審核類別的成功和失敗。

##### <a name="audit-account-management"></a>審核帳戶管理
此審核設定會決定是否要追蹤使用者和群組的管理。 例如，當使用者或電腦帳戶、安全性群組或通訊群組建立、變更或刪除時，應追蹤使用者和群組;當使用者或電腦帳戶重新命名、停用或啟用時，或當使用者或電腦密碼變更時。 可以針對新增至其他群組或從其他群組移除的使用者或群組產生事件。

##### <a name="audit-directory-service-access"></a>稽核目錄服務存取

此原則設定會決定是否要針對擁有自己指定之系統存取控制清單 (SACL) 的 Active Directory 物件，來審核安全性主體存取權。 一般而言，此類別應該只在網域控制站上啟用。 啟用時，此設定會產生許多「雜訊」。

##### <a name="audit-logon-events"></a>Audit Logon 事件
當本機電腦上的本機安全性主體經過驗證時，就會產生登入事件。 Logon 事件會記錄在本機電腦上發生的網域登入。 不會產生帳戶登出事件。 啟用時，登入事件會產生許多「雜訊」，但應該在任何安全性審核計畫中預設啟用。

##### <a name="audit-object-access"></a>Audit 物件存取
存取已啟用審核的後續定義物件時，物件存取可以產生事件 (例如，開啟、讀取、重新命名、刪除或關閉) 。 啟用主要審核分類之後，系統管理員必須個別定義哪些物件將啟用審核。 許多 Windows 系統物件都已啟用審核功能，因此啟用此類別通常會在系統管理員定義任何事件之前開始產生事件。

這個類別是「有雜訊的」類別，會針對每個物件存取產生五到十個事件。 系統管理員可能很難取得物件審核，以取得有用的資訊。 它應該只在需要時才啟用。

##### <a name="auditing-policy-change"></a>稽核原則變更
此原則設定可決定是否要對使用者權限指派原則、Windows 防火牆原則、信任原則或稽核原則的變更，進行每個變更的變更。 此類別目錄應該在所有電腦上啟用。 它會產生極少的雜訊。

##### <a name="audit-privilege-use"></a>Audit 許可權使用

Windows (有數十種使用者權利和許可權，例如，以批次工作登入，並作為作業系統) 的一部分。 此原則設定會藉由行使使用者權限或許可權，來決定是否要審核安全性主體的每個實例。 啟用此類別會導致許多「雜訊」，但在使用較高的許可權來追蹤安全性主體帳戶時可能會很有説明。

##### <a name="audit-process-tracking"></a>Audit 進程追蹤
此原則設定會決定是否要針對事件（例如程式啟用、進程結束、控制碼重複和間接物件存取），來審核詳細的進程追蹤資訊。 它很適合用來追蹤惡意使用者及其使用的程式。

啟用 Audit Process 追蹤會產生大量的事件，因此通常會設定為 [ **無**]。 不過，這項設定可在事件回應期間，從所啟動的處理常式詳細記錄和啟動的時間，提供絕佳的效益。 若為網域控制站和其他單一角色基礎結構伺服器，則隨時都可以安全地開啟這個類別。 在正常的工作過程中，單一角色服務器不會產生太多處理常式追蹤流量。 因此，您可以啟用它們來捕捉未經授權的事件（如果發生的話）。

##### <a name="system-events-audit"></a>系統事件審核

系統事件幾乎是一般的「全部攔截」類別，可註冊影響電腦、其系統安全性或安全性記錄檔的各種事件。 它包含電腦關機和重新開機、電源故障、系統時間變更、驗證套件初始化、audit log clearings、模擬問題，以及其他一般事件的主機的事件。 一般情況下，啟用此審核類別會產生許多「雜訊」，但它會產生足夠的非常實用事件，因此很難建議不要啟用。

#### <a name="advanced-audit-policies"></a>Advanced Audit 原則

從 Windows Vista 和 Windows Server 2008 開始，Microsoft 藉由在每個主要審核類別下建立子類別，來改善事件記錄類別的選取方式。 子類別可讓您比使用主要類別更細微地進行審核。 藉由使用子類別，您只能啟用特定主要類別的部分，並略過產生您不使用的事件。 每個稽核原則子類別可以針對成功、失敗或成功和失敗事件啟用。

若要列出所有可用的審核子類別，請檢查群組原則物件中的 Advanced Audit Policy 容器，或在任何執行 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008、Windows 8、Windows 7 或 Windows Vista 的電腦上，于命令提示字元中輸入下列命令：

`auditpol /list /subcategory:*`

若要在執行 Windows Server 2012、Windows Server 2008 R2 或 Windows 2008 的電腦上取得目前已設定之審核子類別的清單，請輸入下列內容：

`auditpol /get /category:*`

下列螢幕擷取畫面顯示 auditpol.exe 列出目前稽核原則的範例。

![監視 AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)

> [!NOTE]
> 群組原則不一定會正確地報告所有啟用之稽核原則的狀態，而是 auditpol.exe。 如需詳細資訊，請參閱 [取得 Windows 7 和 2008 R2 中的有效稽核原則](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731607(v=ws.10)) 。

每個主要類別都有多個子類別。 以下是類別清單、其子類別和其函式的描述。

### <a name="auditing-subcategories-descriptions"></a>審核子類別描述
稽核原則子類別可啟用下列事件記錄檔訊息類型：

#### <a name="account-logon"></a>帳戶登入

##### <a name="credential-validation"></a>認證驗證
此子類別報告針對使用者帳戶登入要求所提交之認證的驗證測試結果。 這些事件會出現在授權認證的電腦上。 網域帳戶的網域控制站為授權，而針對本機帳戶，則本機電腦為授權。

在網域環境中，大部分的帳戶登入事件都會記錄在網域帳戶權威之網域控制站的安全性記錄中。 不過，當使用本機帳戶登入時，可能會在組織中的其他電腦上發生這些事件。

##### <a name="kerberos-service-ticket-operations"></a>Kerberos 服務票證作業
此子類別報告網域帳戶的授權網域控制站上，Kerberos 票證要求進程所產生的事件。

##### <a name="kerberos-authentication-service"></a>Kerberos 驗證服務
此子類別報告 Kerberos 驗證服務所產生的事件。 這些事件會出現在授權認證的電腦上。

##### <a name="other-account-logon-events"></a>其他帳戶登入事件
此子類別報告針對使用者帳戶登入要求（與認證驗證或 Kerberos 票證無關）所提交的認證所發生的事件。 這些事件會出現在授權認證的電腦上。 網域帳戶的網域控制站為授權，而針對本機帳戶，則本機電腦為授權。

在網域環境中，大部分的帳戶登入事件都會記錄在網域帳戶權威之網域控制站的安全性記錄中。 不過，當使用本機帳戶登入時，可能會在組織中的其他電腦上發生這些事件。 範例可以包含下列各項：

* 遠端桌面服務會話中斷連線
* 新的遠端桌面服務會話
* 鎖定和解除鎖定工作站
* 叫用螢幕保護裝置
* 關閉螢幕保護裝置程式
* 偵測 Kerberos 重新執行攻擊，其中會收到具有相同資訊的 Kerberos 要求兩次
* 存取授與使用者或電腦帳戶的無線網路
* 存取授與使用者或電腦帳戶的有線 802.1 x 網路

#### <a name="account-management"></a>帳戶管理

##### <a name="user-account-management"></a>使用者帳戶管理
此子類別報告使用者帳戶管理的每個事件，例如建立、變更或刪除使用者帳戶時;使用者帳戶已重新命名、停用或啟用;或密碼已設定或變更。 如果啟用此稽核原則設定，系統管理員可以追蹤事件，以偵測使用者帳戶的惡意、意外和授權建立。

##### <a name="computer-account-management"></a>電腦帳戶管理
此子類別報告電腦帳戶管理的每個事件，例如建立、變更、刪除、重新命名、停用或啟用電腦帳戶時。

##### <a name="security-group-management"></a>安全性群組管理
此子類別會報告安全性群組管理的每個事件，例如建立、變更或刪除安全性群組時，或是在安全性群組中新增或移除成員時。 如果啟用此稽核原則設定，系統管理員可以追蹤事件，以偵測安全性群組帳戶的惡意、意外和授權建立。

##### <a name="distribution-group-management"></a>通訊群組管理
此子類別報告通訊群組管理的每個事件，例如建立、變更或刪除通訊群組時，或在通訊群組中新增或移除成員時。 如果啟用此稽核原則設定，系統管理員可以追蹤事件，以偵測群組帳戶的惡意、意外和授權建立。

##### <a name="application-group-management"></a>應用程式群組管理
此子類別會回報電腦上應用程式群組管理的每個事件，例如當應用程式群組建立、變更或刪除時，或是在應用程式群組中新增或移除成員時。 如果啟用此稽核原則設定，系統管理員可以追蹤事件，以偵測應用程式群組帳戶的惡意、意外和授權建立。

##### <a name="other-account-management-events"></a>其他帳戶管理事件
此子類別報告其他帳戶管理事件。

#### <a name="detailed-process-tracking"></a>詳細的進程追蹤

##### <a name="process-creation"></a>處理序建立
此子類別報告進程的建立，以及建立它的使用者或程式的名稱。

##### <a name="process-termination"></a>進程終止
此子類別報告進程終止的時間。

##### <a name="dpapi-activity"></a>DPAPI 活動
此子類別目錄會報告加密或解密 (DPAPI) 資料保護應用程式開發介面的呼叫。 DPAPI 是用來保護秘密資訊，例如儲存的密碼和金鑰資訊。

##### <a name="rpc-events"></a>RPC 事件
此子類別報告遠端程序呼叫 (RPC) 連接事件。

#### <a name="directory-service-access"></a>目錄服務存取

##### <a name="directory-service-access"></a>目錄服務存取
此子類別目錄會報告何時存取 AD DS 物件。 只有已設定 Sacl 的物件會導致產生 audit 事件，而且只有在以符合 SACL 專案的方式存取時才會產生。 這些事件類似于舊版 Windows Server 中的目錄服務存取事件。 此子類別僅適用于網域控制站。

##### <a name="directory-service-changes"></a>目錄服務變更
此子類別報告 AD DS 中的物件變更。 所報告的變更類型是在物件上執行的建立、修改、移動和取消刪除作業。 目錄服務變更審核，適當的情況下，會指出已變更之物件變更屬性的舊值和新值。 只有具有 Sacl 的物件會導致產生 audit 事件，而且只有在以符合其 SACL 專案的方式存取時才會產生。 某些物件和屬性不會導致因為架構中的物件類別設定而產生 audit 事件。 此子類別僅適用于網域控制站。

##### <a name="directory-service-replication"></a>目錄服務複寫
此子類別報告兩個網域控制站之間的複寫開始與結束的時間。

##### <a name="detailed-directory-service-replication"></a>詳細目錄服務複寫
此子類別報告有關網域控制站之間複寫之資訊的詳細資訊。 這些事件在磁片區中可能很高。

#### <a name="logonlogoff"></a>登入/登出

##### <a name="logon"></a>登入
此子類別報告使用者嘗試登入系統的時間。 這些事件會發生在存取的電腦上。 若為互動式登入，則會在登入的電腦上產生這些事件。 如果發生網路登入來存取共用，這些事件會在裝載已存取資源的電腦上產生。 如果這項設定設定為 [ **無**]，則很難或無法判斷哪些使用者已存取或嘗試存取組織電腦。

##### <a name="network-policy-server"></a>網路原則伺服器
此子類別會報告 RADIUS (IAS) 和網路存取保護 (NAP) 使用者存取要求所產生的事件。 這些要求可以是 **授** 與、 **拒絕**、 **捨棄**、 **隔離**、 **鎖定** 和 **解除鎖定**。 若要進行此設定，將會導致 NPS 和 資訊存取伺服器上的記錄量適中或大量。

##### <a name="ipsec-main-mode"></a>IPsec 主要模式
此子類別報告在主要模式協商期間網際網路金鑰交換 (IKE) 通訊協定和驗證的網際網路通訊協定 (AuthIP) 的結果。

##### <a name="ipsec-extended-mode"></a>IPsec 延伸模式
此子類別報告在擴充模式協商期間的 AuthIP 結果。

##### <a name="other-logonlogoff-events"></a>其他登入/登出事件
此子類別報告其他登入和登出相關事件，例如遠端桌面服務會話中斷連線和重新連線、使用 RunAs 在不同的帳戶下執行進程，以及鎖定和解除鎖定工作站。

##### <a name="logoff"></a>Logoff
此子類別報告使用者登出系統的時間。 這些事件會發生在存取的電腦上。 若為互動式登入，則會在登入的電腦上產生這些事件。 如果發生網路登入來存取共用，這些事件會在裝載已存取資源的電腦上產生。 如果這項設定設定為 [ **無**]，則很難或無法判斷哪些使用者已存取或嘗試存取組織電腦。

##### <a name="account-lockout"></a>帳戶鎖定
此子類別報告使用者的帳戶因為登入嘗試失敗太多次而遭到鎖定。

##### <a name="ipsec-quick-mode"></a>IPsec 快速模式
此子類別報告在快速模式協商期間，IKE 通訊協定和 AuthIP 的結果。

##### <a name="special-logon"></a>特殊登入
當使用特殊登入時，此子類別會進行報告。 特殊登入是具有系統管理員對等許可權的登入，可用來將進程提升至較高的層級。

#### <a name="policy-change"></a>原則變更

##### <a name="audit-policy-change"></a>稽核原則變更
此子類別報告稽核原則中的變更，包括 SACL 變更。

##### <a name="authentication-policy-change"></a>驗證原則變更
此子類別報告驗證原則中的變更。

##### <a name="authorization-policy-change"></a>授權原則變更
此子類別報告授權原則的變更，包括許可權 (DACL) 變更。

##### <a name="mpssvc-rule-level-policy-change"></a>MPSSVC Rule-Level 原則變更
此子類別報告 Microsoft 保護服務 ( # A0) 所使用原則規則中的變更。 Windows 防火牆會使用此服務。

##### <a name="filtering-platform-policy-change"></a>篩選平臺原則變更
此子類別報告從 WFP 新增和移除物件，包括啟動篩選。 這些事件在磁片區中可能很高。

##### <a name="other-policy-change-events"></a>其他原則變更事件
此子類別報告其他類型的安全性原則變更，例如 (TPM) 或密碼編譯提供者的「信賴平臺模組」設定。

#### <a name="privilege-use"></a>許可權使用

##### <a name="sensitive-privilege-use"></a>敏感性許可權使用
當使用者帳戶或服務使用敏感性許可權時，此子類別會進行報告。 敏感性許可權包含下列使用者權限：作為作業系統的一部分、備份檔案和目錄、建立權杖物件、偵錯工具、讓電腦和使用者帳戶可受信任以進行委派、產生安全性審核、在驗證後模擬用戶端、載入和卸載設備磁碟機、管理審核和安全性記錄、修改固件環境值、取代進程層級的權杖、還原檔案和目錄，以及取得檔案或其他物件的擁有權。 此子類別目錄的審核將會建立大量的事件。

##### <a name="nonsensitive-privilege-use"></a>Nonsensitive 許可權使用
當使用者帳戶或服務使用 nonsensitive 許可權時，此子類別會進行報告。 Nonsensitive 許可權包括下列使用者權限：以受信任的呼叫者存取認證管理員、從網路存取此電腦、將工作站新增至網域、調整進程的記憶體配額、允許本機登入、允許登入遠端桌面服務、略過遍歷檢查、變更系統時間、建立分頁檔、建立全域物件、建立永久共用物件、建立符號連結、[拒絕從網路存取這部電腦]、[拒絕以批次工作登入]、[拒絕以服務方式登入]、[拒絕以服務方式登入]、[拒絕本機登入]、[拒絕登入]、[拒絕本機登入]、[遠端桌面服務拒絕從遠端系統登入]、[鎖定記憶體中的分頁]、[以服務方式登入、執行磁片區維護工作、分析單一進程、配置檔案系統效能、從銜接站移除電腦、關閉系統，以及同步處理目錄服務資料。 此子類別目錄的審核將會產生非常大量的事件。

##### <a name="other-privilege-use-events"></a>其他許可權使用事件
目前未使用此安全性原則設定。

#### <a name="object-access"></a>物件存取

##### <a name="file-system"></a>檔案系統
此子類別目錄會報告檔案系統物件的存取時機。 只有具有 Sacl 的檔案系統物件會導致產生 audit 事件，而且只有在以符合其 SACL 專案的方式存取時才會產生。 此原則設定本身將不會造成任何事件的審核。 它會決定是否要審核存取檔案系統物件的使用者，該物件具有指定的系統存取控制清單 (的 SACL) ，可有效地啟用進行中的審核。

如果 audit 物件存取設定已設定為 [ **成功**]，每次使用者以指定的 SACL 成功存取物件時，就會產生一個 audit 專案。 如果此原則設定設定為 [ **失敗**]，每次使用者嘗試存取具有指定 SACL 的物件時，就會產生 audit 專案。

##### <a name="registry"></a>登錄
此子類別目錄會報告何時存取登錄物件。 只有具有 Sacl 的登錄物件會導致產生 audit 事件，而且只有在以符合其 SACL 專案的方式存取時才會產生。 此原則設定本身將不會造成任何事件的審核。

##### <a name="kernel-object"></a>核心物件
當存取核心物件（例如處理常式和 mutex）時，這個子類別會進行報告。 只有具有 Sacl 的核心物件會導致產生 audit 事件，而且只有在以符合其 SACL 專案的方式存取它們時才會產生。 只有在啟用 AuditBaseObjects 或 AuditBaseDirectories 審核選項的情況下，核心物件才會獲得 Sacl。

##### <a name="sam"></a>SAM
當存取本機安全性帳戶管理員 (SAM) 驗證資料庫物件時，會報告此子類別。

##### <a name="certification-services"></a>認證服務
此子類別報告執行憑證服務作業的時間。

##### <a name="application-generated"></a>產生的應用程式
當應用程式嘗試使用 Windows audit 應用程式開發介面 (Api) 來產生 audit 事件時，此子類別目錄會進行報告。

##### <a name="handle-manipulation"></a>控制碼操作
當物件的控制碼為開啟或關閉時，此子類別會進行報告。 只有具有 Sacl 的物件會導致產生這些事件，而且只有在嘗試的控制碼作業符合 SACL 專案時才會產生。 系統只會針對 (已啟用對應物件存取子類別的物件類型（例如，檔案系統或登錄) ）產生控制碼操作事件。

##### <a name="file-share"></a>檔案共用
此子類別目錄會報告何時存取檔案共用。 此原則設定本身將不會造成任何事件的審核。 它會決定是否要審核存取檔案共用物件的使用者，該物件具有指定的系統存取控制清單 (SACL) ，可有效地啟用進行中的審核。

##### <a name="filtering-platform-packet-drop"></a>篩選平臺封包捨棄
當 Windows 篩選平台 (WFP) 捨棄封包時，此子類別會進行報告。 這些事件在磁片區中可能很高。

##### <a name="filtering-platform-connection"></a>篩選平臺連接
當 WFP 允許或封鎖連接時，此子類別會回報。 這些事件的音量可能很高。

##### <a name="other-object-access-events"></a>其他物件存取事件
此子類別報告其他物件存取相關事件，例如工作排程器作業和 COM + 物件。

#### <a name="system"></a>系統

##### <a name="security-state-change"></a>安全性狀態變更
此子類別報告系統安全性狀態的變更，例如當安全性子系統啟動和停止時。

##### <a name="security-system-extension"></a>安全性系統延伸模組
此子類別報告擴充程式碼的載入，例如安全性子系統的驗證套件。

##### <a name="system-integrity"></a>系統完整性
此子類別報告安全性子系統的完整性違規。

IPsec 驅動程式

此子類別報告網際網路通訊協定安全性 (IPsec) 驅動程式的活動。

##### <a name="other-system-events"></a>其他系統事件
此子類別報告其他系統事件。

如需子類別描述的詳細資訊，請參閱 [Microsoft 安全性合規性管理員工具](/previous-versions/tn-archive/cc677002(v=technet.10))。

每個組織都應該檢查先前涵蓋的類別和子類別，並啟用最適合其環境的類別。 在生產環境中部署之前，應該一律先測試稽核原則的變更。

## <a name="configuring-windows-audit-policy"></a>設定 Windows 稽核原則

您可以使用群組原則、auditpol.exe、Api 或登錄編輯來設定 Windows audit policy。 針對大多數公司設定稽核原則的建議方法是群組原則或 auditpol.exe。 設定系統的稽核原則需要系統管理員層級的帳戶許可權或適當的委派許可權。

> [!NOTE]
> [ **管理審核及安全性記錄** ] 許可權必須提供給安全性主體， (系統管理員預設) 允許修改個別資源（例如檔案、Active Directory 物件和登錄機碼）的物件存取審核選項。

### <a name="setting-windows-audit-policy-by-using-group-policy"></a>使用群組原則設定 Windows 稽核原則

若要使用 [群組原則] 設定稽核原則，請設定位於 [ **電腦設定 \ 系統設定 \ 系統設定 \ 系統設定 \ 系統設定 \ 本機本機審核原則** ] 底下的適當審核分類 (參閱下列螢幕擷取畫面，以取得本機群組原則編輯器 (gpedit.msc) # A3 每個稽核原則類別都可以啟用 **成功**、 **失敗** 或 **成功** 和失敗事件。

![監視 AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)

您可以使用 Active Directory 或本機群組原則來設定 Advanced Audit Policy。 若要設定 [Advanced Audit Policy]，請設定位於 [ **電腦設定 \windows 設定 \ \Windows 進階審核 (策略** ] 底下的適當子類別。如需本機群組原則編輯器 (gpedit.msc) # A3 的範例，請參閱下列螢幕擷取畫面。 每個稽核原則子類別都可以啟用 **成功**、 **失敗** 或 **成功** 和 **失敗** 事件。

![監視 AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)

### <a name="setting-windows-audit-policy-using-auditpolexe"></a>使用 Auditpol.exe 設定 Windows 稽核原則

Windows Server 2008 和 Windows Vista 中引進了設定 Windows 稽核原則) 的 Auditpol.exe (。 一開始，只有 auditpol.exe 可以用來設定 Advanced 稽核原則，但群組原則可以在 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008、Windows 8 和 Windows 7 中使用。

Auditpol.exe 是命令列公用程式。 語法如下：

`auditpol /set /<Category|Subcategory>:<audit category> /<success|failure:> /<enable|disable>`

Auditpol.exe 語法範例：

`auditpol /set /subcategory:"user account management" /success:enable /failure:enable`

`auditpol /set /subcategory:"logon" /success:enable /failure:enable`

`auditpol /set /subcategory:"IPSEC Main Mode" /failure:enable`

> [!NOTE]
> Auditpol.exe 設定本機的 Advanced Audit Policy。 如果本機原則與 Active Directory 或本機群組原則發生衝突，群組原則設定通常會超越 auditpol.exe 設定。 當有多個群組或本機原則發生衝突時，只有一個原則會被 (也就是取代) 。 稽核原則不會合並。

#### <a name="scripting-auditpol"></a>腳本處理 Auditpol

Microsoft 提供的 [範例腳本](https://support.microsoft.com/kb/921469) 適用于想要使用腳本設定「Advanced Audit Policy」的系統管理員，而不是在每個 auditpol.exe 命令中手動輸入。

**注意** 群組原則不一定會正確地報告所有啟用之稽核原則的狀態，而是 auditpol.exe。 如需詳細資訊，請參閱 [取得 windows 7 和 windows 2008 R2 中的有效稽核原則](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731607(v=ws.10)) 。

#### <a name="other-auditpol-commands"></a>其他 Auditpol 命令

Auditpol.exe 可以用來儲存和還原本機稽核原則，以及查看其他與審核相關的命令。 以下是其他的 **auditpol** 命令。

`auditpol /clear` -用來清除和重設本機稽核原則

`auditpol /backup /file:<filename>` -用來將目前的本機稽核原則備份到二進位檔案

`auditpol /restore /file:<filename>` -用來將先前儲存的稽核原則檔案匯入本機稽核原則

`auditpol /<get/set> /option:<CrashOnAuditFail> /<enable/disable>` -如果已啟用此稽核原則設定，則會導致系統立即停止 (但停止： C0000244 {Audit Failed} message) 如果安全性審核因任何原因而無法記錄。 一般來說，當安全性審核記錄已滿，且為安全性記錄檔指定的保留方法不會 **覆寫事件** 或 **依天數覆寫** 事件時，將無法記錄事件。 一般來說，它只會由需要更高保證安全性記錄檔記錄的環境來啟用。 如果啟用，系統管理員必須仔細監看安全性記錄檔大小，並視需要輪替記錄。 您也可以藉由修改安全性選項 **Audit：「如果無法記錄安全性審核** (預設值 = 停用) ，立即關閉系統，以群組原則設定它。

`auditpol /<get/set> /option:<AuditBaseObjects> /<enable/disable>` -此稽核原則設定會決定是否要審核全域系統物件的存取權。 如果啟用此原則，則會使用預設的系統存取控制清單建立系統物件（例如 mutex、事件、信號和 DOS 裝置）， (SACL) 。 大部分的系統管理員都會考慮將全域系統物件視為「雜訊」，而只有在懷疑惡意入侵時才會啟用。 只有命名物件會獲得 SACL。 如果 [audit 物件存取稽核原則] (或 [核心物件審核] 子類別目錄) 也會啟用，則會審核這些系統物件的存取權。 設定此安全性設定時，除非您重新開機 Windows，否則變更將不會生效。 您也可以藉由修改 [將全域系統物件的存取權 (預設 = 停用的) ] 的安全性選項，以群組原則設定此原則。

`auditpol /<get/set> /option:<AuditBaseDirectories> /<enable/disable>` -此稽核原則設定會指定在建立的情況下，將指定的核心物件 (（例如 mutex 和信號）) 為 Sacl。 當 AuditBaseObjects 影響的物件不能包含其他物件時，AuditBaseDirectories 會影響容器物件。

`auditpol /<get/set> /option:<FullPrivilegeAuditing> /<enable/disable>` -此稽核原則設定會指定當您將一或多個許可權指派給使用者安全性權杖時，用戶端是否會產生事件： AssignPrimaryTokenPrivilege、AuditPrivilege、BackupPrivilege、CreateTokenPrivilege、DebugPrivilege、EnableDelegationPrivilege、ImpersonatePrivilege、LoadDriverPrivilege、RestorePrivilege、SecurityPrivilege、SystemEnvironmentPrivilege、TakeOwnershipPrivilege 和 TcbPrivilege。 如果未啟用此選項 (預設 = 停用的) ，則不會記錄 BackupPrivilege 和 RestorePrivilege 許可權。 啟用此選項可能會讓安全性記錄非常雜訊 (有時在備份作業期間，會有數百個事件是第二個) 。 您也可以藉由修改安全性選項 **audit： Audit Backup 和 Restore 許可權的使用，** 以群組原則設定此原則。

> [!NOTE]
> 此處提供的部分資訊取自 Microsoft [Audit 選項類型](/openspecs/windows_protocols/ms-gpac/262a2bed-93d4-4c04-abec-cf06e9ec72fd) 和 microsoft SCM 工具。

## <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>強制傳統的審核或 Advanced 審核

在 Windows Server 2012、Windows Server 2008 R2、Windows Server 2008、Windows 8、Windows 7 和 Windows Vista 中，系統管理員可以選擇啟用九種傳統類別或使用子類別。 它是必須在每個 Windows 系統中進行的二進位選擇。 您可以啟用主要類別或 subcategoriesit 不能同時為兩者。

若要防止舊版傳統類別目錄原則覆寫稽核原則子類別，您必須啟用 [ **強制稽核原則子類別設定] (Windows Vista 或更新版本，) 覆寫** 位於 [ **電腦設定 \** 使用者設定 \ 使用者設定 \ 使用者設定 \ 使用者設定 \ 系統設定 \ 使用者原則 \ 使用者

建議您啟用和設定子類別，而不是設定九個主要類別。 這需要啟用群組原則設定 (允許子類別覆寫審核類別) 以及設定支援稽核原則的不同子類別。

您可以使用數種方法（包括群組原則和命令列程式 auditpol.exe）來設定審核子類別。

## <a name="next-steps"></a>後續步驟

* [Windows Server 2008 中的審核和合規性](/previous-versions/technet-magazine/cc194392(v=msdn.10))

* [如何使用群組原則來設定 windows Server 2008 網域、Windows Server 2003 網域或 Windows 2000 網域中 Windows Vista 和 Windows Server 2008 電腦的詳細安全性審核設定](https://support.microsoft.com/kb/921469)

* [進階安全性稽核原則逐步指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd408940(v=ws.10))
