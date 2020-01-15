---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: 監視 Active Directory 遭到危害的徵兆
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e51b7ea151db1ca5d53a8cacef3b042e345175de
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949638"
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>監視 Active Directory 遭到危害的徵兆

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

*第五條法則：永久性警覺性來得是安全性的代價。* - [安全性管理的10個不可變法則](https://technet.microsoft.com/library/cc722488.aspx)  
  
穩固的事件記錄檔監視系統是任何安全 Active Directory 設計的重要部分。 如果受害者制訂適當的事件記錄檔監視和警示，可能會提早在事件中發現許多電腦安全性性危害。 這是一段完整支援的獨立報表。 例如， [2009 Verizon 資料缺口報告](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf)指出：  
  
「事件監視和記錄分析的明顯 ineffectiveness，會持續成為 enigma。 偵測的機會是：調查者已注意到66% 的受害者在其記錄內有足夠的辨識項可用來探索缺口，讓他們更用心分析這類資源。」  
  
這種缺乏監視作用中事件記錄檔的漏洞，在許多公司的安全性防護計畫中仍然是一致的弱點。 [2012 Verizon 的資料缺口報告](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf)發現，即使有85% 的缺口需要注意幾個星期，但84% 的受害者在其事件記錄檔中有缺口的證據。  
  
## <a name="windows-audit-policy"></a>Windows 稽核原則

以下是 Microsoft 官方企業支援 blog 的連結。 這些 blog 的內容提供有關審核的建議、指引和建議，可協助您增強 Active Directory 基礎結構的安全性，而且在設計稽核原則時是一項重要的資源。  
  
* [全域物件存取的審核功能非常神奇](https://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx)-說明一種稱為「先進的稽核原則設定」的控制機制，已新增至 windows 7 和 Windows Server 2008 R2，讓您能夠輕鬆地設定您想要的資料類型，而不是操控腳本和 auditpol .exe。  
* [Windows 2008 中的審核變更簡介](https://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx)-引進 windows Server 2008 中的審核功能變更。  
* [Vista 和2008中](https://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx)的非經常性的審核技巧-說明 Windows Vista 和 windows Server 2008 的有趣審核功能，可用於疑難排解問題或查看環境中發生的狀況。  
* [Windows server 2008 和 Windows vista 中的一次性審核](https://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx)-包含 windows server 2008 和 windows vista 中包含的審核功能和資訊的編譯。  
  
下列連結提供 windows 8 和 Windows Server 2012 中 Windows 審核功能改進的相關資訊，以及 Windows Server 2008 中 AD DS 審核的相關資訊。  
  
* [安全性審查的新](https://technet.microsoft.com/library/hh849638.aspx)功能-概述 windows 8 和 windows Server 2012 中的新安全性審查功能。  
* [AD DS 審核逐步指南](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx)-說明 Windows Server 2008 中的新 Active Directory Domain Services （AD DS）審核功能。 它也會提供執行這項新功能的程式。  
  
### <a name="windows-audit-categories"></a>Windows Audit 分類

在 Windows Vista 和 Windows Server 2008 之前，Windows 只有9個事件記錄檔稽核原則類別：  
  
* 帳戶登入事件  
* 帳戶管理  
* 目錄服務存取  
* 登入事件  
* 物件存取  
* 原則變更  
* 許可權使用  
* 進程追蹤  
* 系統事件  
  
這九個傳統的審計類別組成一個稽核原則。 可以針對成功、失敗或成功和失敗事件啟用每個稽核原則類別目錄。 其說明包含在下一節中。  
  
#### <a name="audit-policy-category-descriptions"></a>稽核原則類別描述  
稽核原則類別目錄會啟用下列事件記錄檔訊息類型。  
  
##### <a name="audit-account-logon-events"></a>審核帳戶登入事件  
報告登入或登出一部電腦的每個安全性主體實例（例如，使用者、電腦或服務帳戶），其中另一部電腦用來驗證帳戶。 當網域安全性主體帳戶在網域控制站上進行驗證時，就會產生帳戶登入事件。 本機電腦上本機使用者的驗證會產生登入本機安全性記錄檔的 logon 事件。 不會記錄任何帳戶登出事件。  
  
這個類別會產生許多「雜訊」，因為 Windows 在正常的商務過程中，經常會有登入和登出本機和遠端電腦的帳戶。 儘管如此，任何安全性計畫都應該包含此 audit 類別目錄的成功和失敗。  
  
##### <a name="audit-account-management"></a>審核帳戶管理  
此審核設定會決定是否要追蹤使用者和群組的管理。 例如，當使用者或電腦帳戶、安全性群組或通訊群組建立、變更或刪除時，應該追蹤使用者和群組;當使用者或電腦帳戶重新命名、停用或啟用時;或者，當使用者或電腦的密碼變更時。 可以針對新增至其他群組或從中移除的使用者或群組產生事件。  
  
##### <a name="audit-directory-service-access"></a>稽核目錄服務存取  

此原則設定可決定是否要針對具有自己指定系統存取控制清單（SACL）的 Active Directory 物件，來審核安全性主體存取權。 一般來說，此類別目錄應該只在網域控制站上啟用。 啟用時，此設定會產生許多「雜訊」。  
  
##### <a name="audit-logon-events"></a>審核登入事件  
本機電腦上的本機安全性主體經過驗證時，就會產生登入事件。 Logon 事件會記錄在本機電腦上發生的網域登入。 不會產生帳戶登出事件。 啟用時，登入事件會產生許多「雜訊」，但在任何安全性審核計畫中應該預設為啟用。  
  
##### <a name="audit-object-access"></a>Audit 物件存取  
存取已啟用審核功能的後續定義物件（例如，開啟、讀取、重新命名、刪除或關閉）時，物件存取可以產生事件。 啟用主要審核類別之後，系統管理員必須個別定義哪些物件將啟用審核。 許多 Windows 系統物件都已啟用審核，因此在系統管理員定義任何之前，啟用此類別通常會開始產生事件。  
  
此類別非常「雜訊」，而且會針對每個物件存取產生五到十個事件。 系統管理員對於物件的審核功能可能很容易取得有用的資訊。 它應該只在需要時才啟用。  
  
##### <a name="auditing-policy-change"></a>稽核原則變更  
此原則設定可決定是否要針對使用者權限指派原則、Windows 防火牆原則、信任原則或稽核原則的變更，來審核每個變更的發生。 此類別目錄應在所有電腦上啟用。 它會產生非常少的雜訊。  
  
##### <a name="audit-privilege-use"></a>Audit 許可權使用  

Windows 中有數十種使用者權利和許可權（例如，以批次工作登入，並作為作業系統的一部分）。 此原則設定會決定是否要藉由使用使用者權限或許可權，來審查安全性主體的每個實例。 啟用此類別會產生許多「雜訊」，但它有助於使用較高的許可權來追蹤安全性主體帳戶。  
  
##### <a name="audit-process-tracking"></a>審核進程追蹤  
此原則設定可決定是否要針對事件（例如程式啟動、進程結束、控制碼重複和間接物件存取），審查詳細的進程追蹤資訊。 這適用于追蹤惡意使用者和其所使用的程式。  
  
啟用 Audit Process 追蹤會產生大量的事件，因此通常會設定為 [**無**]。 不過，這項設定可在事件回應期間，從所啟動程式的詳細記錄和啟動的時間，提供絕佳的優勢。 對於網域控制站和其他單一角色基礎結構伺服器，此類別隨時都可以安全地開啟。 單一角色服務器在其職責的正常過程中，不會產生太多處理常式追蹤流量。 因此，它們可以啟用以在發生未經授權的事件時加以捕捉。  
  
##### <a name="system-events-audit"></a>系統事件 Audit  

系統事件幾乎是一般的全部 catch 類別，註冊會影響電腦、系統安全性或安全性記錄檔的各種事件。 其中包括電腦關機和重新開機的事件、電源失敗、系統時間變更、驗證套件初始化、audit log clearings、模擬問題，以及其他一般事件的主機。 一般來說，啟用此 audit 類別會產生許多「雜訊」，但它會產生足夠的實用事件，而不建議將它啟用。  
  
#### <a name="advanced-audit-policies"></a>Advanced Audit 原則

從 Windows Vista 和 Windows Server 2008 開始，Microsoft 藉由在每個主要審核類別下建立子類別目錄，來改善事件記錄檔類別選擇的方式。 子類別目錄可讓您比使用主要類別更細微的方式進行審核。 藉由使用子類別，您可以只啟用特定主要類別的某些部分，並略過產生無法使用的事件。 每個稽核原則子類別可以針對成功、失敗或成功和失敗事件啟用。  
  
若要列出所有可用的審核子類別，請在群組原則物件中檢查 Advanced Audit Policy 容器，或在執行 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008、Windows 8 的任何電腦上，于命令提示字元中輸入下列命令：Windows 7 或 Windows Vista：  
  
`auditpol /list /subcategory:*`
  
若要在執行 Windows Server 2012、Windows Server 2008 R2 或 Windows 2008 的電腦上取得目前設定的審核子類別清單，請輸入下列內容：  
  
`auditpol /get /category:*`
  
下列螢幕擷取畫面顯示 auditpol 的範例，其中列出目前的稽核原則。  
  
![監視 AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> 群組原則不一定會正確地報告所有已啟用之稽核原則的狀態，而 auditpol 則會執行。 如需詳細資訊，請參閱[在 Windows 7 和 2008 R2 中取得有效的稽核原則](https://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx)。  
  
每個主要類別都有多個子類別。 以下是類別目錄、其子類別和其功能的描述清單。  
  
### <a name="auditing-subcategories-descriptions"></a>審核子類別描述  
稽核原則子類別會啟用下列事件記錄檔訊息類型：  
  
#### <a name="account-logon"></a>帳戶登入  
  
##### <a name="credential-validation"></a>認證驗證  
此子類別會報告針對使用者帳戶登入要求所提交認證的驗證測試結果。 這些事件發生在具有認證授權的電腦上。 對於網域帳戶，網域控制站是授權的，而對於本機帳戶，本機電腦是授權的。  
  
在網域環境中，大部分的帳戶登入事件都會記錄在網域控制站的安全性記錄檔中，該網域是網域帳戶的授權。 不過，當使用本機帳戶登入時，這些事件可能會發生在組織中的其他電腦上。  
  
##### <a name="kerberos-service-ticket-operations"></a>Kerberos 服務票證作業  
此子類別會報告網域帳戶的授權網域控制站上，Kerberos 票證要求程式所產生的事件。  
  
##### <a name="kerberos-authentication-service"></a>Kerberos 驗證服務  
此子類別會報告 Kerberos 驗證服務所產生的事件。 這些事件發生在具有認證授權的電腦上。  
  
##### <a name="other-account-logon-events"></a>其他帳戶登入事件  
此子類別會回報針對使用者帳戶登入要求（與認證驗證或 Kerberos 票證無關）所提交認證所發生的事件。 這些事件發生在具有認證授權的電腦上。 對於網域帳戶，網域控制站是授權的，而對於本機帳戶，本機電腦是授權的。  
  
在網域環境中，大部分的帳戶登入事件都會記錄在網域控制站的安全性記錄檔中，該網域是網域帳戶的授權。 不過，當使用本機帳戶登入時，這些事件可能會發生在組織中的其他電腦上。 範例可包括下列各項：  
  
* 遠端桌面服務會話中斷連線  
* 新的遠端桌面服務會話  
* 鎖定和解除鎖定工作站  
* 叫用螢幕保護裝置  
* 關閉螢幕保護裝置  
* 偵測 Kerberos 重新執行攻擊，其中包含相同資訊的 Kerberos 要求會收到兩次  
* 存取授與使用者或電腦帳戶的無線網路  
* 存取授與使用者或電腦帳戶的有線 802.1 x 網路  
  
#### <a name="account-management"></a>帳戶管理  
  
##### <a name="user-account-management"></a>使用者帳戶管理  
這個子類別會報告每個使用者帳戶管理的事件，例如建立、變更或刪除使用者帳戶的時間;已重新命名、停用或啟用使用者帳戶;或密碼已設定或變更。 如果已啟用此稽核原則設定，系統管理員可以追蹤事件，以偵測使用者帳戶的惡意、意外和授權建立。  
  
##### <a name="computer-account-management"></a>電腦帳戶管理  
此子類別會報告電腦帳戶管理的每個事件，例如建立、變更、刪除、重新命名、停用或啟用電腦帳戶的時間。  
  
##### <a name="security-group-management"></a>安全性群組管理  
這個子類別會報告每個安全性群組管理的事件，例如建立、變更或刪除安全性群組時，或是在安全性群組中新增或移除成員時。 如果已啟用此稽核原則設定，系統管理員可以追蹤事件，以偵測安全性群組帳戶的惡意、意外和授權建立。  
  
##### <a name="distribution-group-management"></a>通訊群組管理  
此子類別會報告通訊群組管理的每個事件，例如，建立、變更或刪除通訊群組，或在通訊群組中新增或移除成員時。 如果已啟用此稽核原則設定，系統管理員可以追蹤事件，以偵測群組帳戶的惡意、意外和授權建立。  
  
##### <a name="application-group-management"></a>應用程式群組管理  
這個子類別會報告電腦上應用程式群組管理的每個事件，例如建立、變更或刪除應用程式群組的時間，或是在應用程式群組中新增或移除成員的時間。 如果已啟用此稽核原則設定，系統管理員可以追蹤事件，以偵測應用程式群組帳戶的惡意、意外和授權建立。  
  
##### <a name="other-account-management-events"></a>其他帳戶管理事件  
此子類別會報告其他帳戶管理事件。  
  
#### <a name="detailed-process-tracking"></a>詳細的進程追蹤  
  
##### <a name="process-creation"></a>處理序建立  
這個子類別會報告建立程式的方式，以及建立該進程的使用者或程式的名稱。  
  
##### <a name="process-termination"></a>進程終止  
此子類別會報告進程終止的時間。  
  
##### <a name="dpapi-activity"></a>DPAPI 活動  
此子類別會報告對資料保護應用程式開發介面（DPAPI）的加密或解密呼叫。 DPAPI 是用來保護秘密資訊，例如儲存的密碼和金鑰資訊。  
  
##### <a name="rpc-events"></a>RPC 事件  
此子類別會報告遠端程序呼叫（RPC）連接事件。  
  
#### <a name="directory-service-access"></a>目錄服務存取  
  
##### <a name="directory-service-access"></a>目錄服務存取  
此子類別會報告何時存取 AD DS 物件。 只有已設定 Sacl 的物件才會產生 audit 事件，而且只有在以符合 SACL 專案的方式存取時才會發生。 這些事件類似于舊版 Windows Server 中的目錄服務存取事件。 此子類別僅適用于網域控制站。  
  
##### <a name="directory-service-changes"></a>目錄服務變更  
此子類別會報告 AD DS 中物件的變更。 所報告的變更類型為在物件上執行的建立、修改、移動和取消刪除作業。 適當時，目錄服務變更審核會指出已變更之物件的已變更屬性的舊值和新值。 只有具有 Sacl 的物件才會產生 audit 事件，而且只有在以符合其 SACL 專案的方式存取時才會發生。 某些物件和屬性不會因為架構中物件類別的設定而導致產生 audit 事件。 此子類別僅適用于網域控制站。  
  
##### <a name="directory-service-replication"></a>目錄服務複寫  
此子類別會報告兩個網域控制站之間的複寫開始和結束的時間。  
  
##### <a name="detailed-directory-service-replication"></a>詳細目錄服務複寫  
此子類別會報告在網域控制站之間複寫之資訊的詳細資訊。 這些事件在大量中可能非常高。  
  
#### <a name="logonlogoff"></a>登入/登出  
  
##### <a name="logon"></a>Logon  
當使用者嘗試登入系統時，此子類別會報告。 這些事件會發生在存取的電腦上。 若為互動式登入，則會在登入的電腦上產生這些事件。 如果進行網路登入以存取共用，這些事件會在裝載所存取資源的電腦上產生。 如果此設定設為 [**無**]，則很難或無法判斷哪些使用者已存取或嘗試存取組織電腦。  
  
##### <a name="network-policy-server"></a>網路原則伺服器  
此子類別會報告 RADIUS （IAS）和網路存取保護（NAP）使用者存取要求所產生的事件。 這些要求可以是**Grant**、 **Deny**、**捨棄**、**隔離**、**鎖定**和**解除鎖定**。 此設定會導致 NPS 和 資訊存取伺服器上的記錄量適中或大量。  
  
##### <a name="ipsec-main-mode"></a>IPsec 主要模式  
此子類別會報告在主要模式協商期間，網際網路金鑰交換（IKE）通訊協定和已驗證的網際網路通訊協定（AuthIP）的結果。  
  
##### <a name="ipsec-extended-mode"></a>IPsec 延伸模式  
此子類別會在延伸模式協商期間報告 AuthIP 的結果。  
  
##### <a name="other-logonlogoff-events"></a>其他登入/登出事件  
此子類別會報告其他登入和登出相關的事件，例如遠端桌面服務會話中斷連線並重新連線、使用 RunAs 在不同帳戶下執行進程，以及鎖定和解除鎖定工作站。  
  
##### <a name="logoff"></a>登出  
此子類別會報告使用者何時登出系統。 這些事件會發生在存取的電腦上。 若為互動式登入，則會在登入的電腦上產生這些事件。 如果進行網路登入以存取共用，這些事件會在裝載所存取資源的電腦上產生。 如果此設定設為 [**無**]，則很難或無法判斷哪些使用者已存取或嘗試存取組織電腦。  
  
##### <a name="account-lockout"></a>帳戶鎖定  
此子類別會回報使用者的帳戶因太多次失敗的登入嘗試而遭到鎖定的情況。  
  
##### <a name="ipsec-quick-mode"></a>IPsec 快速模式  
此子類別會報告快速模式協商期間 IKE 通訊協定和 AuthIP 的結果。  
  
##### <a name="special-logon"></a>特殊登入  
此子類別會報告何時使用特殊登入。 特殊登入是具有系統管理員對等許可權的登入，可以用來將進程提升至較高的層級。  
  
#### <a name="policy-change"></a>原則變更  
  
##### <a name="audit-policy-change"></a>稽核原則變更  
此子類別會報告稽核原則中的變更，包括 SACL 變更。  
  
##### <a name="authentication-policy-change"></a>驗證原則變更  
此子類別會報告驗證原則中的變更。  
  
##### <a name="authorization-policy-change"></a>授權原則變更  
此子類別會報告授權原則中的變更，包括許可權（DACL）變更。  
  
##### <a name="mpssvc-rule-level-policy-change"></a>MPSSVC 規則層級原則變更  
此子類別會報告 Microsoft 保護服務（MPSSVC）所使用之原則規則中的變更。 Windows 防火牆會使用此服務。  
  
##### <a name="filtering-platform-policy-change"></a>篩選平臺原則變更  
此子類別會報告從 WFP 新增和移除物件，包括啟動篩選。 這些事件在大量中可能非常高。  
  
##### <a name="other-policy-change-events"></a>其他原則變更事件  
此子類別會報告其他類型的安全性原則變更，例如信賴平臺模組（TPM）或密碼編譯提供者的設定。  
  
#### <a name="privilege-use"></a>許可權使用  
  
##### <a name="sensitive-privilege-use"></a>敏感性許可權使用  
此子類別會報告使用者帳戶或服務何時使用敏感性許可權。 敏感性許可權包括下列使用者權限：作為作業系統的一部分、備份檔案和目錄、建立權杖物件、偵錯工具、讓電腦和使用者帳戶可供委派、產生安全性審核、在驗證之後模擬用戶端、載入和卸載設備磁碟機、管理審核和安全性記錄、修改固件環境值、取代進程層級權杖、還原檔案和目錄，以及取得檔案或其他物件的擁有權。 此子類別目錄的審核將會建立大量的事件。  
  
##### <a name="nonsensitive-privilege-use"></a>Nonsensitive 許可權使用  
此子類別會報告使用者帳戶或服務何時使用 nonsensitive 許可權。 Nonsensitive 許可權包括下列使用者權限：以信任的呼叫者身分存取認證管理員、從網路存取這部電腦、將工作站新增到網域、調整進程的記憶體配額、允許本機登入、允許透過遠端登入桌面服務，略過遍歷檢查，變更系統時間，建立分頁檔，建立全域物件，建立永久共用物件，建立符號連結，拒絕從網路存取這台電腦，拒絕以批次工作登入，拒絕以服務方式登入，拒絕本機登入、拒絕透過遠端桌面服務登入、強制從遠端系統關機、增加進程工作集、增加排程優先順序、鎖定記憶體中的分頁、以批次工作方式登入、修改物件標籤、執行磁片區維護工作、分析單一進程、分析系統效能、從銜接站移除電腦、關閉系統，以及同步處理目錄服務資料。 此子類別目錄的審核將會建立非常大量的事件。  
  
##### <a name="other-privilege-use-events"></a>其他許可權使用事件  
目前未使用此安全性原則設定。  
  
#### <a name="object-access"></a>物件存取  
  
##### <a name="file-system"></a>檔案系統  
此子類別會報告檔案系統物件的存取時機。 只有具有 Sacl 的檔案系統物件才會產生 audit 事件，而且只有在以符合其 SACL 專案的方式存取時才會發生。 此原則設定本身將不會造成任何事件的審核。 它會決定是否要針對存取具有指定系統存取控制清單（SACL）之檔案系統物件的使用者，以有效地啟用進行中的審核來進行事件。  
  
如果 audit 物件存取設定是設定為 [**成功**]，每次使用者成功存取具有指定 SACL 的物件時，就會產生 audit 專案。 如果此原則設定已設定為 [**失敗**]，則每次使用者嘗試存取具有指定 SACL 的物件失敗時，就會產生 audit 專案。  
  
##### <a name="registry"></a>登錄  
此子類別會報告登錄物件的存取時機。 只有具有 Sacl 的 registry 物件才會產生 audit 事件，而且只有在以符合其 SACL 專案的方式存取時才會發生。 此原則設定本身將不會造成任何事件的審核。  
  
##### <a name="kernel-object"></a>核心物件  
此子類別會報告核心物件（例如處理常式和 mutex）的存取時機。 只有具有 Sacl 的核心物件才會產生 audit 事件，而且只有在以符合其 SACL 專案的方式存取時才會發生。 一般來說，只有在啟用 AuditBaseObjects 或 AuditBaseDirectories 的審核選項時，才會提供 Sacl 的核心物件。  
  
##### <a name="sam"></a>SAM  
此子類別會報告本機安全性帳戶管理員（SAM）驗證資料庫物件的存取時機。  
  
##### <a name="certification-services"></a>認證服務  
此子類別會報告憑證服務作業的執行時間。  
  
##### <a name="application-generated"></a>應用程式已產生  
當應用程式嘗試使用 Windows 審核應用程式開發介面（Api）來產生 audit 事件時，此子類別會回報。  
  
##### <a name="handle-manipulation"></a>處理操作  
這個子類別會報告物件的控制碼何時開啟或關閉。 只有具有 Sacl 的物件才會產生這些事件，而且只有在嘗試的控制碼作業符合 SACL 專案時才會這麼做。 只有在已啟用對應物件存取子類別的物件類型（例如，檔案系統或登錄），才會產生處理操作事件。  
  
##### <a name="file-share"></a>檔案共用  
此子類別會報告檔案共用的存取時機。 此原則設定本身將不會造成任何事件的審核。 它會決定是否要針對存取具有指定系統存取控制清單（SACL）之檔案共用物件的使用者，以有效地啟用進行中的審核來進行事件。  
  
##### <a name="filtering-platform-packet-drop"></a>篩選平臺封包捨棄  
此子類別會報告 Windows 篩選平台（WFP）何時捨棄封包。 這些事件在大量中可能非常高。  
  
##### <a name="filtering-platform-connection"></a>篩選平臺連接  
此子類別會報告 WFP 允許或封鎖連接的時間。 這些事件的數量可能很高。  
  
##### <a name="other-object-access-events"></a>其他物件存取事件  
這個子類別會報告其他與物件存取相關的事件，例如工作排程器作業和 COM + 物件。  
  
#### <a name="system"></a>[系統]  
  
##### <a name="security-state-change"></a>安全性狀態變更  
此子類別會報告系統安全性狀態的變更，例如安全性子系統啟動和停止的時間。  
  
##### <a name="security-system-extension"></a>安全性系統延伸模組  
此子類別會報告延伸模組程式碼的載入，例如安全性子系統的驗證套件。  
  
##### <a name="system-integrity"></a>系統完整性  
此子類別會報告安全性子系統的完整性違規。  
  
IPsec 驅動程式  
  
此子類別會報告網際網路通訊協定安全性（IPsec）驅動程式的活動。  
  
##### <a name="other-system-events"></a>其他系統事件  
此子類別會報告其他系統事件。  
  
如需有關子類別目錄描述的詳細資訊，請參閱[Microsoft 安全性合規性管理員工具](https://technet.microsoft.com/library/cc677002.aspx)。  
  
每個組織都應該審查先前涵蓋的分類和子類別，並啟用最適合其環境的類別。 在生產環境中部署之前，應一律先測試稽核原則的變更。  
  
## <a name="configuring-windows-audit-policy"></a>正在設定 Windows 稽核原則

您可以使用群組原則、auditpol .exe、Api 或登錄編輯來設定 Windows 稽核原則。 為大部分公司設定稽核原則的建議方法是群組原則或 auditpol. exe。 設定系統的稽核原則時，需要有系統管理員層級的帳戶許可權或適當的委派許可權。  
  
> [!NOTE]  
> 「**管理」和「安全性記錄**」許可權必須提供給安全性主體（系統管理員預設會有），以允許修改個別資源（例如檔案、Active Directory 物件和登錄機碼）的物件存取審核選項。  
  
### <a name="setting-windows-audit-policy-by-using-group-policy"></a>使用群組原則設定 Windows 稽核原則

若要使用群組原則來設定稽核原則，請在 [電腦設定] [設定 \**本機本機審核原則**] 底下設定適當的 audit 類別（請參閱下列螢幕擷取畫面，以取得本機群組原則編輯器（gpedit.msc）中的範例）。 可以針對**成功**、**失敗**或**成功**和失敗事件啟用每個稽核原則類別目錄。  
  
![監視 AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
您可以使用 Active Directory 或本機群組原則來設定 Advanced Audit Policy。 若要設定「高級稽核原則」，請在 [電腦設定 \ 檢查子設定] **\Windows 安全性進階稽核原則**（請參閱下列螢幕擷取畫面，以取得來自本機群組原則編輯器（gpedit.msc）的範例）。 每個稽核原則子類別都可以啟用**成功**、**失敗**或**成功**和**失敗**事件。  
  
![監視 AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
### <a name="setting-windows-audit-policy-using-auditpolexe"></a>使用 Auditpol 設定 Windows 稽核原則

Auditpol （用於設定 Windows 稽核原則）是在 Windows Server 2008 和 Windows Vista 中引進。 一開始，只有 auditpol 可以用來設定 Advanced 稽核原則，但群組原則可以用於 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008、Windows 8 和 Windows 7。  
  
Auditpol 是命令列公用程式。 語法如下：  
  
`auditpol /set /<Category|Subcategory>:<audit category> /<success|failure:> /<enable|disable>`
  
Auditpol .exe 語法範例：  
  
`auditpol /set /subcategory:"user account management" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"logon" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"IPSEC Main Mode" /failure:enable`
  
> [!NOTE]  
> Auditpol 會在本機設定 Advanced 稽核原則。 如果本機原則與 Active Directory 或本機群組原則衝突，群組原則設定通常會優先于 auditpol. exe 設定。 當有多個群組或本機原則衝突存在時，只會有一個原則（也就是取代）。 稽核原則不會合並。  
  
#### <a name="scripting-auditpol"></a>腳本 Auditpol

Microsoft 會針對想要使用腳本設定「高級稽核原則」的系統管理員提供[範例腳本](https://support.microsoft.com/kb/921469)，而不是在每個 printbrm.exe 命令中手動輸入。  
  
**注意**群組原則不一定會正確地報告所有已啟用之稽核原則的狀態，而 auditpol 則會執行。 如需詳細資訊，請參閱[在 Windows 7 和 windows 2008 R2 中取得有效的稽核原則](https://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx)。  
  
#### <a name="other-auditpol-commands"></a>其他 Auditpol 命令

Auditpol 可以用來儲存和還原本機稽核原則，以及查看其他的審核相關命令。 以下是其他**auditpol**命令。  
  
`auditpol /clear`-用來清除和重設本機稽核原則  
  
`auditpol /backup /file:<filename>`-用來將目前的本機稽核原則備份至二進位檔案  
  
`auditpol /restore /file:<filename>`-用來將先前儲存的稽核原則檔匯入本機稽核原則  
  
`auditpol /<get/set> /option:<CrashOnAuditFail> /<enable/disable>`-如果已啟用此稽核原則設定，則如果因為任何原因無法記錄安全性 audit，它會導致系統立即停止（使用 STOP： C0000244 {Audit Failed} 訊息）。 一般而言，當安全性審核記錄已滿，而且針對安全性記錄檔指定的保留方法不會依天數**覆寫事件**或**覆寫事件**時，就無法記錄事件。 通常只會由需要更高保證安全性記錄檔正在記錄的環境來啟用。 啟用時，系統管理員必須仔細監看安全性記錄大小，並視需要輪替記錄。 也可以藉由修改安全性選項 [ **Audit：立即關閉系統] （如果無法記錄安全性審核**（預設值 = 已停用）來設定群組原則。  
  
`auditpol /<get/set> /option:<AuditBaseObjects> /<enable/disable>`-此稽核原則設定會決定是否要審核全域系統物件的存取權。 如果啟用此原則，則會使用預設的系統存取控制清單（SACL）來建立系統物件，例如 mutex、事件、信號和 DOS 裝置。 大部分的系統管理員會考慮將全域系統物件視為「雜訊」，而只有在懷疑惡意入侵時才會啟用。 只有命名物件會獲得 SACL。 如果也啟用「audit 物件存取稽核原則」（或「核心物件」 audit 子類別），則會對這些系統物件的存取進行審核。 進行此安全性設定時，除非您重新開機 Windows，否則變更不會生效。 這項原則也可以透過下列方式來設定群組原則藉由修改安全性選項 [審查全域系統物件的存取權（預設值 = 停用）]。  
  
`auditpol /<get/set> /option:<AuditBaseDirectories> /<enable/disable>`-此稽核原則設定指定在建立時，會為 Sacl 提供名為的核心物件（例如 mutex 和信號）。 當 AuditBaseObjects 影響不能包含其他物件的物件時，AuditBaseDirectories 會影響容器物件。  
  
`auditpol /<get/set> /option:<FullPrivilegeAuditing> /<enable/disable>`-此稽核原則設定指定當有一或多個許可權指派給使用者安全性權杖時，用戶端是否會產生事件： AssignPrimaryTokenPrivilege、AuditPrivilege、BackupPrivilege、CreateTokenPrivilege、DebugPrivilege、EnableDelegationPrivilege、ImpersonatePrivilege、LoadDriverPrivilege、RestorePrivilege、SecurityPrivilege、SystemEnvironmentPrivilege、TakeOwnershipPrivilege 和 TcbPrivilege。 如果未啟用此選項（預設值 = 停用），則不會記錄 BackupPrivilege 和 RestorePrivilege 許可權。 啟用此選項可能會在備份作業期間，讓安全性記錄產生極雜訊的事件（有時候會有數百個事件）。 此原則也可以藉由修改安全性選項**Audit： audit 使用備份和還原許可權**來設定群組原則。  
  
> [!NOTE]  
> 此處提供的部分資訊是取自 Microsoft [Audit 選項類型](https://msdn.microsoft.com/library/dd973862(prot.20).aspx)和 microsoft SCM 工具。  
  
## <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>強制執行傳統的審核或先進的審核

在 Windows Server 2012、Windows Server 2008 R2、Windows Server 2008、Windows 8、Windows 7 和 Windows Vista 中，系統管理員可以選擇啟用九種傳統類別，或使用子類別。 這是必須在每個 Windows 系統中進行的二進位選擇。 可以啟用主要類別，或 subcategoriesit 不能同時為兩者。  
  
若要防止傳統傳統類別目錄原則覆寫稽核原則子類別，您必須啟用 [**強制稽核原則子類別設定（Windows Vista 或更新版本）** ]，以覆寫位於 [電腦設定] [設定] \ [保護 \] \ 安全性**選項**下的稽核原則類別設定原則  
  
我們建議您啟用和設定子類別，而不是9個主要類別。 這需要啟用群組原則設定（以允許子類別覆寫審核類別），以及設定支援稽核原則的不同子類別。  
  
您可以使用數種方法來設定 [審核子類別]，包括群組原則和命令列程式（auditpol .exe）。  
  
## <a name="next-steps"></a>接下來的步驟
  
* [Windows 7 和 Windows Server 2008 R2 中的先進安全性審查](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
* [Windows Server 2008 中的審核與合規性](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
* [如何使用群組原則，為 windows Server 2008 網域、windows Server 2003 網域或 Windows 2000 網域中的 Windows Vista 和 Windows Server 2008 電腦設定詳細的安全性審核設定](https://support.microsoft.com/kb/921469)  
  
* [Advanced Security 稽核原則逐步指南](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  
