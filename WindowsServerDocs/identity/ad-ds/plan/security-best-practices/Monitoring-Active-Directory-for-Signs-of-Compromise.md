---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: 監視 Active Directory 遭到危害的徵兆
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 40d0d06f8d6d25c2c1dbf4662d3296a996d22055
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882939"
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>監視 Active Directory 遭到危害的徵兆

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

*法律編號第五：永久性的警覺是安全性的代價。* - [10 不變法則安全性管理](https://technet.microsoft.com/library/cc722488.aspx)  
  
穩固的事件記錄檔，監視系統是任何安全的 Active Directory 設計的一個重要部分。 許多電腦安全性受侵犯可以及早在事件如果探索受害者制定適當的事件記錄檔監視和警示。 此結論，有長時間支援獨立的報表。 例如， [2009 Verizon 資料缺口報告](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf)狀態：  
  
「 事件監視和記錄檔分析明顯 ineffectiveness 仍然是稍微 enigma。 偵測的機會有;investigator 注意 66%的犧牲者有足夠的辨識項，可以在其記錄檔，以探索漏洞，它們已更努力一些，分析這類資源。 」  
  
缺乏此種監視作用中的事件記錄檔會有許多公司的安全性防禦計劃維持一致的弱點。 [2012 Verizon 資料缺口報告](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf)發現，即使 85%的漏洞，花了好幾個星期才能被發現，受害者 84%有破壞的辨識項在其事件記錄檔。  
  
## <a name="windows-audit-policy"></a>Windows 稽核原則

以下是 Microsoft 官方的企業支援部落格的連結。 這些部落格的內容會提供建議、 指引和建議相關的稽核將會協助您加強您的 Active Directory 基礎結構的安全性和稽核原則在設計時是很寶貴的資源。  
  
* [全域物件存取稽核是 Magic](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -描述控制機制，稱為 進階稽核原則設定已加入至 Windows 7 和 Windows Server 2008 R2，可讓您設定您想要輕鬆地稽核和不受何種類型的資料指令碼及 auditpol.exe 中。  
* [導入 Windows 2008 中的稽核變更](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx)-介紹 Windows Server 2008 中的稽核變更。  
* [非經常性存取稽核的技巧，在 Vista 和 2008年](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx)-說明有趣的稽核功能，Windows Vista 和 Windows Server 2008 可以用於疑難排解問題，或查看您的環境中的情況。  
* [執行 Windows Server 2008 和 Windows Vista 中的稽核一次達成](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx)-包含編譯的稽核功能和 Windows Server 2008 和 Windows Vista 中所包含的資訊。  
  
下列連結提供 Windows 稽核 Windows 8 和 Windows Server 2012 中的增強功能的相關資訊和 AD DS 的相關資訊，Windows Server 2008 中的稽核。  
  
* [What's New in 安全性稽核](https://technet.microsoft.com/library/hh849638.aspx)-提供新的安全性稽核 Windows 8 和 Windows Server 2012 功能的概觀。  
* [AD DS 稽核逐步指南](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx)-描述 Windows Server 2008 中新的 Active Directory 網域服務 (AD DS) 稽核功能。 它也提供程序來實作這項新功能。  
  
### <a name="windows-audit-categories"></a>Windows 稽核類別

前 Windows Vista 和 Windows Server 2008，Windows，必須只有九個事件記錄檔稽核原則類別目錄：  
  
* 帳戶登入事件  
* 帳戶管理  
* 目錄服務存取  
* 登入事件  
* 物件的存取  
* 原則變更  
* 特殊權限使用  
* 處理程序追蹤  
* 系統事件  
  
下列九種傳統的稽核類別構成的稽核原則。 每個稽核原則類別目錄可以啟用成功、 失敗或成功及失敗事件。 它們的描述會包含在下一節。  
  
#### <a name="audit-policy-category-descriptions"></a>稽核原則類別目錄的描述  
稽核原則類別目錄可讓下列事件記錄檔訊息類型。  
  
##### <a name="audit-account-logon-events"></a>稽核帳戶登入事件  
報告安全性主體 （例如使用者、 電腦或服務帳戶） 登入或登出從一部電腦可以驗證的帳戶會使用另一部電腦的每個執行的個體。 在網域控制站驗證的網域安全性主體帳戶時，會產生帳戶登入事件。 在本機電腦上的本機使用者的驗證會產生記錄的本機安全性記錄中的登入事件。 帳戶登出不記錄任何事件。  
  
此類別會產生大量 「 雜訊 」 因為 Windows 不斷發生登入的帳戶，並從本機和遠端電腦在一般的企業課程期間。 儘管如此，任何安全性計畫應包含成功和失敗的這個稽核類別目錄。  
  
##### <a name="audit-account-management"></a>稽核帳戶管理  
此稽核設定會決定是否要追蹤使用者和群組的管理。 例如，使用者和群組應該追蹤時建立、 變更或刪除; 使用者或電腦帳戶、 安全性群組或通訊群組當重新命名、 停用，或啟用，則使用者或電腦帳戶或者，使用者或電腦的密碼變更時。 使用者或群組加入或移除其他群組，就可以產生事件。  
  
##### <a name="audit-directory-service-access"></a>稽核目錄服務存取  

此原則設定會決定是否要稽核安全性主體存取 Active Directory 物件具有它自己指定的系統存取控制清單 (SACL)。 一般情況下，此類別僅應該在網域控制站上啟用。 啟用時，此設定會產生大量的 「 雜訊 」。  
  
##### <a name="audit-logon-events"></a>稽核登入事件  
本機安全性主體驗證本機電腦上時，會產生登入事件。 登入事件記錄會發生在本機電腦的網域登入。 不會產生帳戶登出事件。 啟用時，登入事件，將會產生大量的 「 雜訊 」，但它們應該預設會啟用任何安全性稽核計畫中。  
  
##### <a name="audit-object-access"></a>稽核物件存取  
物件的存取可以產生事件時啟用稽核功能的後續定義的物件存取 （適用於範例、 Opened、 讀取、 重新命名、 已刪除或已關閉）。 啟用主要的稽核類別之後，系統管理員必須個別定義的物件將會稽核已啟用。 稽核已啟用，所以啟用此類別通常會開始產生事件之前，系統管理員已定義任何隨附許多 Windows 系統物件。  
  
此類別非常 「 吵雜的"，且會產生每個物件存取的五到十個事件。 它很難讓系統管理員的新物件稽核取得有用的資訊。 此外，它才應該啟用時需要。  
  
##### <a name="auditing-policy-change"></a>稽核原則變更  
此原則設定可決定是否要稽核使用者權利指派原則、 Windows 防火牆原則、 信任原則或變更的稽核原則變更的每一個事件。 此類別應該在所有電腦上啟用。 它會產生極少的雜訊。  
  
##### <a name="audit-privilege-use"></a>稽核特殊權限使用  

有數十種使用者權限和 Windows （例如，登入為批次工作和做為作業系統的一部分） 中的權限。 此原則設定可決定是否要稽核安全性主體的每個執行個體，藉以使用者權限或權限。 啟用此類別會導致大量的 「 雜訊 」，但它能幫助您追蹤使用提高的權限的安全性主體帳戶。  
  
##### <a name="audit-process-tracking"></a>稽核程序追蹤  
此原則設定可決定是否要稽核追蹤資訊，例如程式啟動、 處理序結束，控制代碼重複和間接物件存取事件的詳細程序。 它可用於追蹤的惡意使用者和他們所用的程式。  
  
啟用稽核程序追蹤產生大量事件，因此通常是設定為**無稽核**。 不過，這項設定可以提供極大的好處，期間從啟動的處理序的詳細記錄檔事件回應和它們所啟動的時間。 網域控制站和其他單一角色基礎結構伺服器，此類別可以安全地開啟所有的時間。 單一角色的伺服器不會產生太多的程序追蹤其職責的正常過程期間的流量。 因此，它們可以啟用以擷取未經授權的事件，如果它們發生。  
  
##### <a name="system-events-audit"></a>系統事件稽核  

系統事件是幾乎泛型全面類別，註冊所影響的電腦、 其系統安全性或安全性記錄檔的各種事件。 它包含電腦關機和重新啟動、 電源故障，系統時間的變更、 驗證封裝的初始化、 稽核記錄檔空地所構成，模擬問題，以及一堆其他一般的事件的事件。 一般情況下，啟用此稽核類別目錄會產生大量的 「 雜訊 」，但它會產生足夠非常有用的事件，很難曾經建議您不啟用它。  
  
#### <a name="advanced-audit-policies"></a>進階的稽核原則

從 Windows Vista 和 Windows Server 2008 開始，Microsoft 更好的事件記錄檔類別選取項目可建立每個主要的稽核類別下的子類別目錄的方式。 子類別目錄可讓稽核比否則無法使用的主要類別更為精細。 使用子類別，您可以啟用部分特定的主要類別，並略過產生的事件就不會使用。 每個稽核原則子類別可以針對成功、失敗或成功和失敗事件啟用。  
  
若要列出所有可用稽核子類別目錄，檢閱群組原則物件中的 [進階稽核原則] 容器，或在執行 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008，Windows 8 的電腦上的命令提示字元輸入下列命令Windows 7 或 Windows Vista:  
  
`auditpol /list /subcategory:\*`
  
若要取得目前設定的稽核子類別目錄清單執行 Windows Server 2012、 Windows Server 2008 R2 或 Windows 2008 的電腦上，輸入下列命令：  
  
`auditpol /get /category:\*`
  
下列螢幕擷取畫面顯示 auditpol.exe 列出目前的稽核原則的範例。  
  
![監視 AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> 群組原則不會一律精確地報告狀態的所有已啟用的稽核原則，而 auditpol.exe 則。 請參閱[取得 Windows 7 和 2008 R2 中的有效稽核原則](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx)如需詳細資訊。  
  
每個主要的類別目錄具有多個子類別。 以下是一份類別、 其子類別目錄和它們的函式的描述。  
  
### <a name="auditing-subcategories-descriptions"></a>稽核子類別目錄說明  
稽核原則子類別會啟用下列事件記錄檔訊息類型：  
  
#### <a name="account-logon"></a>帳戶登入  
  
##### <a name="credential-validation"></a>認證驗證  
這個子類別會報告提交以供使用者帳戶登入要求的認證驗證測試的結果。 有權管理的認證在電腦上發生這些事件。 對於網域帳戶，網域控制站是授權的而本機帳戶，請在本機電腦有權管理。  
  
在網域環境中，大部分的帳戶登入事件會記錄在安全性記錄檔的網域控制站的網域帳戶的授權。 不過，這些事件可以發生在組織中其他電腦上，當本機帳戶用來登入。  
  
##### <a name="kerberos-service-ticket-operations"></a>Kerberos 服務票證作業  
這個子類別會報告有權管理的網域帳戶的網域控制站上的 Kerberos 票證要求程序所產生的事件。  
  
##### <a name="kerberos-authentication-service"></a>Kerberos 驗證服務  
此子分類報告 Kerberos 驗證服務所產生的事件。 有權管理的認證在電腦上發生這些事件。  
  
##### <a name="other-account-logon-events"></a>其他帳戶登入事件  
這個子類別會報告所提交之使用者帳戶登入要求的認證，不與認證驗證或 Kerberos 票證的回應中發生的事件。 有權管理的認證在電腦上發生這些事件。 對於網域帳戶，網域控制站是授權的而本機帳戶，請在本機電腦有權管理。  
  
在網域環境中，大部分的帳戶登入事件會記錄在安全性記錄檔的網域控制站的網域帳戶的授權。 不過，這些事件可以發生在組織中其他電腦上，當本機帳戶用來登入。 範例包括：  
  
* 遠端桌面服務工作階段中斷連線  
* 新的遠端桌面服務工作階段  
* 鎖定和解除鎖定工作站  
* 叫用螢幕保護裝置  
* 關閉螢幕保護裝置  
* Kerberos 偵測重新執行的攻擊中，兩次收到相同的資訊與 Kerberos 要求時  
* 存取權授與使用者或電腦帳戶的無線網路  
* 存取有線的 802.1 x 網路授與使用者或電腦帳戶  
  
#### <a name="account-management"></a>帳戶管理  
  
##### <a name="user-account-management"></a>使用者帳戶管理  
這個子類別會報告每個使用者帳戶管理，例如當建立、 變更或刪除; 使用者帳戶的事件重新命名、 停用，或啟用，則使用者帳戶或者，設定或變更密碼。 如果已啟用此稽核原則設定，系統管理員可以追蹤的事件，來偵測惡意、 意外，並已獲授權的使用者帳戶的建立。  
  
##### <a name="computer-account-management"></a>電腦帳戶管理  
這個子類別會報告電腦帳戶管理，例如當電腦帳戶是建立、 變更、 刪除、 重新命名、 停用，或啟用的每個事件。  
  
##### <a name="security-group-management"></a>安全性群組管理  
此子分類報告安全性的群組管理，例如當建立、 變更或刪除安全性群組，或當加入或移除的安全性群組成員的每一個事件。 如果已啟用此稽核原則設定，系統管理員可以追蹤的事件，來偵測惡意、 意外，並已獲授權建立的安全性群組帳戶。  
  
##### <a name="distribution-group-management"></a>通訊群組管理  
此子分類報告通訊群組管理，例如當建立、 變更或刪除通訊群組，或當加入或移除通訊群組成員的每一個事件。 如果已啟用此稽核原則設定，系統管理員可以追蹤的事件，來偵測惡意、 意外，並已獲授權建立的群組帳戶。  
  
##### <a name="application-group-management"></a>應用程式群組管理  
此子分類會報告每個事件的應用程式群組管理的電腦上，例如當建立、 變更或刪除應用程式群組，或當加入或移除應用程式群組成員。 如果已啟用此稽核原則設定，系統管理員可以追蹤的事件，來偵測惡意、 意外，並已獲授權的應用程式群組帳戶的建立。  
  
##### <a name="other-account-management-events"></a>其他帳戶管理事件  
這個子類別會報告其他帳戶管理事件。  
  
#### <a name="detailed-process-tracking"></a>詳細的程序追蹤  
  
##### <a name="process-creation"></a>建立處理程序  
建立處理序和使用者或建立它的程式的名稱，就會報告此子分類。  
  
##### <a name="process-termination"></a>處理序終止  
處理序終止時，就會報告此子分類。  
  
##### <a name="dpapi-activity"></a>DPAPI 活動  
加密或解密呼叫 「 資料保護應用程式開發介面 (DPAPI)，就會報告此子分類。 DPAPI 用來保護祕密的資訊，例如預存密碼和金鑰資訊。  
  
##### <a name="rpc-events"></a>RPC 事件  
這個子類別目錄報表遠端程序呼叫 (RPC) 連線事件。  
  
#### <a name="directory-service-access"></a>目錄服務存取  
  
##### <a name="directory-service-access"></a>目錄服務存取  
當存取 AD DS 物件時，就會回報此子分類。 使用設定 Sacl 的物件才會導致稽核事件時，能夠產生，而且只與 SACL 項目相符的方式存取。 這些事件是類似於在舊版的 Windows Server 目錄服務存取事件。 此子分類只適用於網域控制站。  
  
##### <a name="directory-service-changes"></a>目錄服務變更  
此子分類報告變更 AD DS 中的物件。 所報告的變更類型的建立、 修改、 移動和取消刪除的物件執行的作業。 目錄服務變更稽核，適用時，表示已變更的物件已變更之屬性的最大值舊和新值。 只使用 Sacl 的物件會造成要產生的而且只在符合其 SACL 項目方式存取時的稽核事件。 某些物件和屬性則不會因為結構描述中的物件類別上的設定會產生稽核事件。 此子分類只適用於網域控制站。  
  
##### <a name="directory-service-replication"></a>目錄服務複寫  
當兩個網域控制站之間的複寫會開始及結束時，就會回報此子分類。  
  
##### <a name="detailed-directory-service-replication"></a>詳細的目錄服務複寫  
這個子類別會報告網域控制站之間複寫的資訊的詳細的資訊。 這些事件可能很高磁碟區中。  
  
#### <a name="logonlogoff"></a>登入/登出  
  
##### <a name="logon"></a>登入  
當使用者嘗試登入系統時，就會回報此子分類。 存取電腦上發生這些事件。 互動式登入，產生這些事件就會發生在已登入的電腦上。 如果網路登入來存取共用，則會在裝載存取的資源的電腦上產生這些事件。 如果此設定設定為**沒有稽核**，很難或無法判斷哪些使用者有存取，或嘗試存取組織的電腦。  
  
##### <a name="network-policy-server"></a>網路原則伺服器  
此子分類報告 RADIUS (IAS) 與網路存取保護 (NAP) 的使用者存取要求所產生的事件。 這些要求可**Grant**，**拒絕**，**捨棄**，**隔離**，**鎖定**，與**解除鎖定**。 稽核此設定會導致中或高的磁碟區，將 NPS 和 IAS 伺服器上的記錄。  
  
##### <a name="ipsec-main-mode"></a>IPsec 主要模式  
此子分類主要模式交涉期間報告網際網路金鑰交換 (IKE) 通訊協定和已驗證網際網路通訊協定 (AuthIP) 的結果。  
  
##### <a name="ipsec-extended-mode"></a>IPsec 延伸模式  
此子分類延伸模式交涉期間報告 AuthIP 的結果。  
  
##### <a name="other-logonlogoff-events"></a>其他登入/登出事件  
這個子類別會報告其他登入和登出相關的事件，例如遠端桌面服務工作階段中斷連線，並重新連接時，使用 RunAs，執行程序，以不同的帳戶鎖定和解除鎖定工作站。  
  
##### <a name="logoff"></a>登出  
當使用者登出系統時，就會回報此子分類。 存取電腦上發生這些事件。 互動式登入，產生這些事件就會發生在已登入的電腦上。 如果網路登入來存取共用，則會在裝載存取的資源的電腦上產生這些事件。 如果此設定設定為**沒有稽核**，很難或無法判斷哪些使用者有存取，或嘗試存取組織的電腦。  
  
##### <a name="account-lockout"></a>帳戶鎖定  
當使用者的帳戶已遭鎖定，因為登入失敗嘗試次數過多時，就會回報此子分類。  
  
##### <a name="ipsec-quick-mode"></a>IPsec 快速模式  
這個子類別快速模式交涉期間報告 IKE 通訊協定和 AuthIP 的結果。  
  
##### <a name="special-logon"></a>特殊的登入  
使用特殊的登入 時，就會回報此子分類。 特殊的登入是具有系統管理員對等的權限，而且可用來提升為較高等級的程序的登入。  
  
#### <a name="policy-change"></a>原則變更  
  
##### <a name="audit-policy-change"></a>稽核原則變更  
此子分類報告中包括 SACL 變更的稽核原則的變更。  
  
##### <a name="authentication-policy-change"></a>驗證原則變更  
此子分類會回報在驗證原則中的變更。  
  
##### <a name="authorization-policy-change"></a>授權原則變更  
此子分類報告中包括權限 (DACL) 變更的授權原則的變更。  
  
##### <a name="mpssvc-rule-level-policy-change"></a>MPSSVC 規則層級原則變更  
此子分類報告 Microsoft 保護服務 (MPSSVC.exe) 所使用的原則規則中的變更。 此服務會使用 Windows 防火牆。  
  
##### <a name="filtering-platform-policy-change"></a>篩選平台原則變更  
此子分類可報告的新增和移除的物件從 WFP，包括啟動篩選條件。 這些事件可能很高磁碟區中。  
  
##### <a name="other-policy-change-events"></a>其他原則變更事件  
這個子類別會報告其他類型的安全性原則變更，例如信賴平台模組 (TPM) 或密碼編譯提供者的設定。  
  
#### <a name="privilege-use"></a>特殊權限使用  
  
##### <a name="sensitive-privilege-use"></a>機密特殊權限使用  
當使用者帳戶時，回報此子分類，或服務會使用機密特殊權限。 機密特殊權限包括下列的使用者權限： 做為作業系統的一部分、 將備份檔案和目錄、 建立權杖的物件、 偵錯程式、 啟用為受信任可以委派，則會產生安全性稽核的電腦和使用者帳戶在驗證後模擬用戶端、 載入和卸載裝置驅動程式、 管理稽核及安全性記錄檔、 修改韌體環境值，取代處理序層級 token、 還原檔案和目錄，和取得檔案或其他物件的擁有權。 此子分類的稽核將會建立大量的事件。  
  
##### <a name="nonsensitive-privilege-use"></a>Nonsensitive 特殊權限使用  
當使用者帳戶時，回報此子分類，或服務會使用 nonsensitive 的權限。 Nonsensitive 的權限包括下列的使用者權限： 存取受信任的呼叫端的認證管理員、 從網路存取這台電腦、 新增工作站到網域，調整處理序的記憶體配額，允許本機登入、 允許透過遠端登入桌面服務，略過周遊檢查，變更系統時間、 建立分頁檔、 建立全域物件、 建立永久共用的物件、 建立符號連結、 拒絕存取此電腦的網路、 拒絕以批次工作登入、 拒絕服務，以登入拒絕本機登入、 拒絕透過遠端桌面服務登入、 從遠端系統強制關機、 增加處理程序工作組、 提升排程優先權，在記憶體中鎖定分頁、 以批次工作登入、 以服務方式登、 修改物件標籤、 執行磁碟區維護工作，設定檔的單一處理序，可以分析系統效能、 電腦從銜接站移除、 關閉系統，和同步處理目錄服務的資料。 此子分類的稽核將會建立非常大量的事件。  
  
##### <a name="other-privilege-use-events"></a>其他特殊權限使用事件  
目前未使用此安全性原則設定。  
  
#### <a name="object-access"></a>物件的存取  
  
##### <a name="file-system"></a>檔案系統  
存取檔案系統物件時，就會回報此子分類。 只有檔案系統物件的 Sacl 會導致稽核事件產生的而且只在存取方式比對其 SACL 項目時。 單獨使用時，此原則設定不會造成任何事件的稽核。 它會判斷是否要稽核的事件，存取已指定的系統存取控制清單 (SACL)，為檔案系統物件之使用者的有效地啟用 若要進行稽核。  
  
如果稽核物件存取設定已設定為**成功**，稽核項目會產生每次使用者成功存取指定的 SACL 的物件。 如果此原則設定會設定為**失敗**，稽核項目會產生每個使用者在嘗試存取的物件與指定的 SACL 中失敗的時間。  
  
##### <a name="registry"></a>登錄  
存取登錄的物件時，就會回報此子分類。 只使用 Sacl 的物件登錄會造成要產生，而且只在符合其 SACL 項目方式存取時的稽核事件。 單獨使用時，此原則設定不會造成任何事件的稽核。  
  
##### <a name="kernel-object"></a>核心物件  
當核心物件，例如處理程序和 mutex 存取時，就會回報此子分類。 只使用 Sacl 的核心物件會造成要產生，而且只在符合其 SACL 項目方式存取時的稽核事件。 一般核心物件都只會指定 Sacl，如果已啟用 AuditBaseObjects 或 AuditBaseDirectories 的稽核選項。  
  
##### <a name="sam"></a>SAM  
存取本機安全性帳戶管理員 (SAM) 驗證資料庫物件時，就會回報此子分類。  
  
##### <a name="certification-services"></a>憑證服務  
憑證服務作業時，就會回報此子分類。  
  
##### <a name="application-generated"></a>產生應用程式  
當應用程式嘗試使用 Windows 稽核應用程式開發介面 (Api) 來產生稽核事件時，就會回報此子分類。  
  
##### <a name="handle-manipulation"></a>控制代碼操作  
物件的控制代碼是開啟或關閉時，就會報告此子分類。 只使用 Sacl 的物件會造成這些事件產生，並且只有在嘗試控制代碼作業符合 SACL 項目。 控制代碼操作事件才會產生其中對應物件的存取子類別目錄已啟用 （例如檔案系統或登錄） 的物件類型。  
  
##### <a name="file-share"></a>檔案共用  
存取檔案共用時，就會報告此子分類。 單獨使用時，此原則設定不會造成任何事件的稽核。 它會判斷是否要稽核存取已指定的系統存取控制清單 (SACL)，為檔案共用物件的使用者事件有效地啟用 若要進行稽核。  
  
##### <a name="filtering-platform-packet-drop"></a>篩選平台封包丟棄  
藉由 Windows 篩選平台 (WFP) 會捨棄封包時，就會回報此子分類。 這些事件可能很高磁碟區中。  
  
##### <a name="filtering-platform-connection"></a>篩選平台連線  
允許或封鎖的 WFP 連線時，就會回報此子分類。 這些事件可以是高磁碟區中。  
  
##### <a name="other-object-access-events"></a>其他物件存取事件  
這個子類別會報告其他物件存取權相關事件，例如工作排程器工作和 COM + 物件。  
  
#### <a name="system"></a>系統  
  
##### <a name="security-state-change"></a>安全性狀態變更  
此子分類報告中的系統，例如安全性子系統何時啟動和停止的安全性狀態的變更。  
  
##### <a name="security-system-extension"></a>安全性系統延伸模組  
此子分類，以安全性子系統報告延伸模組程式碼，例如驗證套件的載入。  
  
##### <a name="system-integrity"></a>系統完整性  
這個子類別會報告的安全性子系統完整性違規。  
  
IPsec 驅動程式  
  
此子分類的網際網路通訊協定安全性 (IPsec) 驅動程式的活動報告。  
  
##### <a name="other-system-events"></a>其他系統事件  
其他系統事件會報告此子分類。  
  
如需有關子類別目錄描述的詳細資訊，請參閱[Microsoft Security Compliance Manager 工具](https://technet.microsoft.com/library/cc677002.aspx)。  
  
每個組織應該檢閱上述涵蓋的分類和子類別，並啟用其最適合其環境的項目。 一律應在生產環境中部署之前測試稽核原則的變更。  
  
## <a name="configuring-windows-audit-policy"></a>設定 Windows 稽核原則

可以使用群組原則、 auditpol.exe、 Api 或登錄編輯來設定 Windows 稽核原則。 建議的方法，以設定稽核原則，對大部分公司而言是群組原則或 auditpol.exe。 設定系統稽核原則，需要系統管理員層級帳戶權限或適當的委派權限。  
  
> [!NOTE]  
> **管理稽核及安全性記錄**權限必須提供給安全性主體 （系統管理員讓它依預設） 表示允許修改物件存取稽核的個別資源，例如檔案、 使用中的選項目錄物件和登錄機碼。  
  
### <a name="setting-windows-audit-policy-by-using-group-policy"></a>使用群組原則設定 Windows 稽核原則

若要設定稽核原則使用群組原則，設定適當的稽核類別位於**電腦設定 \windows 設定 \ 原則 \ 稽核原則**(請參閱下列螢幕擷取畫面範例從本機群組原則編輯器 (gpedit.msc)）。 每個稽核原則類別目錄可以啟用**成功**，**失敗**，或**成功**和失敗事件。  
  
![監視 AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
使用 Active Directory 或本機群組原則，可以設定進階的稽核原則。 若要設定進階稽核原則，設定適當的子類別目錄位於**電腦設定 \windows 設定 \ 安全性設定 \ 進階稽核原則**（請參閱下列螢幕擷取畫面如需範例，從本機群組原則編輯器 (gpedit.msc)）。 每個稽核原則子類別可以啟用**成功**，**失敗**，或**成功**並**失敗**事件。  
  
![監視 AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
### <a name="setting-windows-audit-policy-using-auditpolexe"></a>使用 Auditpol.exe 設定 Windows 稽核原則

Windows Server 2008 和 Windows Vista 中引進了 Auditpol.exe （適用於設定 Windows 稽核原則）。 一開始，只有 auditpol.exe 無法用來設定進階稽核原則，但是可以在 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008、 Windows 8 和 Windows 7 中使用群組原則。  
  
Auditpol.exe 是命令列公用程式。 語法如下所示：  
  
`auditpol /set /<Category|Subcategory>:<audit category> /<success|failure:> /<enable|disable>`
  
Auditpol.exe 語法範例：  
  
`auditpol /set /subcategory:"user account management" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"logon" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"IPSEC Main Mode" /failure:enable`
  
> [!NOTE]  
> Auditpol.exe 本機設定進階稽核原則。 如果與 Active Directory 或本機群組原則衝突的本機原則，群組原則設定通常會風行於 auditpol.exe 的設定。 當多個群組或本機原則衝突存在時，只能有一個原則為準 （也就，取代）。 稽核原則會將不會合併。  
  
#### <a name="scripting-auditpol"></a>指令碼 Auditpol

Microsoft 提供[的範例指令碼](https://support.microsoft.com/kb/921469)的系統管理員想要將進階稽核原則設定而不是以手動方式輸入 auditpol.exe 中的每個命令使用指令碼。  
  
**請注意**群組原則並不一定可以正確地報告狀態的所有已啟用的稽核原則，而 auditpol.exe 則。 請參閱[取得 Windows 7 和 Windows 2008 R2 中的有效稽核原則](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx)如需詳細資訊。  
  
#### <a name="other-auditpol-commands"></a>其他 Auditpol 命令

可以使用 Auditpol.exe，來儲存和還原本機稽核原則，並檢視其他稽核相關的命令。 以下是另**auditpol**命令。  
  
`auditpol /clear` -用來清除及重設本機稽核原則  
  
`auditpol /backup /file:<filename>` -用來備份目前的本機稽核原則，為二進位檔案  
  
`auditpol /restore /file:<filename>` -用來匯入先前儲存的稽核原則檔案，本機稽核原則  
  
`auditpol /<get/set> /option:<CrashOnAuditFail> /<enable/disable>` -如果啟用此稽核原則設定時，它會導致系統立即停止 (停止：C0000244 {稽核失敗}） 如果安全性稽核不會記錄訊息因為任何原因。 一般而言，事件，將安全性稽核記錄檔已滿，而且指定的安全性記錄的保留方法時，記錄就會失敗**不覆寫事件**或是**依日期的覆寫事件**。 通常它才會啟用所需要的安全性記錄檔記錄的較高保證的環境。 如果啟用，系統管理員必須仔細觀察安全性記錄檔大小，和旋轉所需的記錄檔。 它也可以設定與群組原則修改安全性選項**稽核：當無法記錄安全性稽核系統立即關閉**(預設 = 已停用)。  
  
`auditpol /<get/set> /option:<AuditBaseObjects> /<enable/disable>` -此稽核原則設定可決定是否要稽核通用系統物件的存取。 如果啟用此原則時，它會導致系統物件，例如 mutex、 事件、 信號及 DOS 裝置得以建立而預設系統存取控制清單 (SACL)。 大部分的系統管理員，請考慮稽核通用系統物件是太 「 吵雜，」，他們將只啟用，才能夠為可疑的惡意駭客。 只有具名的物件會獲得 SACL。 如果也已啟用稽核物件存取稽核原則 （或核心物件的稽核子類別），這些系統物件存取稽核。 當這個安全性設定，變更不會生效之前重新啟動 Windows。 此原則也可以設定與群組原則修改系統物件的安全性選項稽核存取 (預設 = 已停用)。  
  
`auditpol /<get/set> /option:<AuditBaseDirectories> /<enable/disable>` -此稽核原則設定會指定具名的核心物件 （例如 mutex 和信號） 會在建立時指定 Sacl。 AuditBaseDirectories AuditBaseObjects 影響不能包含其他物件的物件時，會影響容器物件。  
  
`auditpol /<get/set> /option:<FullPrivilegeAuditing> /<enable/disable>` -此稽核原則設定指定用戶端產生的事件，當一或多個這些權限指派給使用者的安全性權杖：AssignPrimaryTokenPrivilege、 AuditPrivilege、 BackupPrivilege、 CreateTokenPrivilege、 DebugPrivilege、 EnableDelegationPrivilege、 ImpersonatePrivilege、 LoadDriverPrivilege、 RestorePrivilege、 SecurityPrivilege、 SystemEnvironmentPrivilege，TakeOwnershipPrivilege 和 TcbPrivilege。 如果未啟用此選項 (預設 = 已停用)，不會記錄 BackupPrivilege 和 RestorePrivilege 權限。 啟用此選項，可以在備份作業期間進行安全性記錄檔極為吵雜 （有時上百個第二個事件）。 此原則也可以設定與群組原則修改安全性選項**稽核：稽核的備份和還原權限使用**。  
  
> [!NOTE]  
> 這裡提供一些資訊取自 Microsoft[稽核選項類型](https://msdn.microsoft.com/library/dd973862(prot.20).aspx)和 Microsoft SCM 工具。  
  
## <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>強制執行傳統的稽核 」 或 「 進階稽核

在 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008、 Windows 8、 Windows 7 和 Windows Vista，管理員可以選擇啟用九種傳統的類別，或將子類別目錄。 這是二進位的選擇，必須變更為每個 Windows 系統中。 可以啟用主要類別或是 subcategoriesit 不能同時。  
  
若要防止舊版的傳統類別目錄原則覆寫稽核原則子類別，您必須啟用**強制執行稽核原則子類別設定 (Windows Vista 或更新版本) 來覆寫稽核原則類別設定**原則設定位於**電腦設定 \windows 設定 \ 本機原則 \ 安全性選項**。  
  
我們建議您啟用及設定而不是 9 個主要類別子類別目錄。 這需要，好讓您覆寫的稽核類別目錄的子類別） 啟用群組原則設定，以及設定不同的子類別目錄支援稽核原則。  
  
可以使用數種方法，包括群組原則的命令列程式、 auditpol.exe 設定稽核子類別。  
  
## <a name="next-steps"></a>後續步驟
  
* [進階的安全性稽核 Windows 7 和 Windows Server 2008 R2](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
* [稽核和 Windows Server 2008 中的合規性](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
* [如何使用群組原則設定詳細的安全性在 Windows Server 2008 網域中、 在 Windows Server 2003 網域中，或在 Windows 2000 網域的 Windows Vista 和 Windows Server 2008 電腦的稽核設定](https://support.microsoft.com/kb/921469)  
  
* [進階安全性稽核原則逐步指南](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  
