---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: "Active Directory 監視危害的符號"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 096f2fa58b9aae53a06bf26c2107eb4cee3aa5d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>Active Directory 監視危害的符號

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

*法律號碼五：Eternal 警覺是安全的價格。* - [10 變的法律的安全性管理](https://technet.microsoft.com/library/cc722488.aspx)  
  
監控系統實心事件登入是重要任何安全 Active Directory 設計的一部分。 許多電腦安全性折衷可能會發現優先事件如果受害者准許監視和警告適當的事件登入。 獨立報告長已經支援這個結束。 例如，[2009 Verizon 資料違約報告](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf)狀態：  
  
「明顯的事件監視和登入分析 ineffectiveness 繼續有些 enigma。 機會偵測有;現場 experience 受害者 66%有充分探索違約它們已在分析這類資源更仔細他們登中可用的辨識項。」  
  
許多公司安全性防護計劃缺乏監視使用事件登維持一致的弱點。 [2012 Verizon 資料違約報告](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf)中找到，即使破壞 85%拍攝幾個星期會注意到，受害者 84%有書面違約辨識他們事件登入。  
  
## <a name="windows-audit-policy"></a>Windows 稽核原則  
以下是 Microsoft 正式企業支援部落格的連結。 Content 的這些部落格提供建議、指導方針和建議的稽核會協助您美化 Active Directory 基礎結構的安全性，而且有價值的資源時設計稽核原則。  
  
-   [稽核全球物件存取是魔力](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx)-描述稱為 [進階稽核原則設定已加入到 Windows 7 和 Windows Server 2008 R2 可讓您設定您想要稽核輕鬆，並不操控指令碼和 auditpol.exe 何種類型的資料控制機制。  
  
-   [引進 Windows 2008 稽核變更](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx)-導入稽核 Windows Server 2008 進行的變更。  
  
-   [很棒稽核技巧，Vista 與 2008 年](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx)-解釋有趣稽核功能 Windows Vista 和 Windows Server 2008，可以用於問題的疑難排解，或查看您的環境中事情。  
  
-   [Windows Server 2008 和 Windows Vista 中稽核購買一次](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx)-包含編譯稽核功能與 Windows Server 2008 和 Windows Vista 中所包含的資訊。  
  
下列連結提供稽核在 Windows 8 和 Windows Server 2012、Windows 改良功能的相關資訊和資訊 AD DS 稽核 Windows Server 2008。  
  
-   [最新的安全性稽核](https://technet.microsoft.com/library/hh849638.aspx)-提供新的安全性稽核功能在 Windows 8 和 Windows Server 2012 的概觀。  
  
-   [AD DS 稽核 Step-by-Step 指南](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx)-描述 Windows Server 2008 中新的 Active Directory Domain Services (AD DS) 稽核功能。 它也會提供程序實作此新功能。  
  
### <a name="windows-audit-categories"></a>Windows 稽核類型  
之前 Windows Vista 和 Windows Server 2008、Windows 有只九事件登入稽核原則分類：  
  
-   Account 登入事件  
  
-   Account 管理  
  
-   Directory 服務的存取  
  
-   事件登入  
  
-   存取物件  
  
-   變更原則  
  
-   使用權限  
  
-   追蹤程序  
  
-   系統事件  
  
這些九傳統稽核分類組成稽核原則。 每個稽核原則分類均可的成功、失敗，成功或失敗事件。 下一節包含及其描述。  
  
#### <a name="audit-policy-category-descriptions"></a>稽核原則分類描述  
稽核原則分類讓下列事件登入郵件類型。  
  
##### <a name="audit-account-logon-events"></a>按兩下  
回報安全性原則（例如，使用者、電腦或服務 account）登入或登出一部電腦，在另一部電腦用來驗證 account 每個執行的個體。 安全性主體核對網域控制站驗證時，專 account 登入事件。 本機使用者的本機電腦上的驗證產生登入事件登入本機安全性登入。 不 account 登出事件是登入。  
  
這個分類產生「聲音」許多因為 Windows 會持續遇到帳號登入和本機和遠端電腦使用一般過程中的企業。 成功與此稽核分類的失敗，仍然，應包含任何安全性計劃。  
  
##### <a name="audit-account-management"></a>稽核 Account 管理  
此稽核設定判斷追蹤管理使用者和群組。 例如，使用者和群組應當追蹤時建立、變更或刪除; 使用者或電腦 account、安全性群組 distribution 群組 account 使用者或電腦時重新命名、停用，或功能。或當使用者或電腦已變更密碼。 事件可以因使用者或群組，若要新增或移除其他群組。  
  
##### <a name="audit-directory-service-access"></a>稽核 Directory 服務的存取  

這項原則設定判斷是否安全性主體存取 Active Directory 物件的稽核，有它自己指定的系統存取控制清單 (SACL)。 一般而言，這個分類應該只網域控制站上。 此設定時支援，產生許多「聲音」。  
  
##### <a name="audit-logon-events"></a>稽核登入事件  
登入事件專本機電腦上已本機安全性原則時。 事件記錄網域登入本機電腦上發生的登入。 專不 account 登出事件。 當功能，登入事件產生許多「聲音」，但它們應該會支援預設任何安全性稽核計劃中。  
  
##### <a name="audit-object-access"></a>存取物件的稽核  
存取物件可以產生後續定義的物件的稽核支援的存取（的範例、Opened、讀取、重新命名，刪除或已關閉）的活動。 支援的主要稽核分類是之後，系統管理員必須排列定義此物件會稽核支援。 稽核支援，讓這個分類通常會開始產生系統管理員所定義任何前的事件隨附許多 Windows 系統物件。  
  
這種是非常」吵的「，而且會發出 5 到 10 活動的每個物件存取。 就很難適用於系統管理員新物件的稽核以取得有用的資訊。 它應該只需要時。  
  
##### <a name="auditing-policy-change"></a>稽核原則的變更  
這項原則設定判斷稽核使用者權限指派原則、Windows 防火牆原則、信任原則或變更稽核原則的變更的每個發生率。 應該將這個分類支援所有的電腦上。 它會產生幾乎雜音。  
  
##### <a name="audit-privilege-use"></a>稽核權限使用  

有許多使用者權限」及「Windows（，例如登入為分批和部分作業系統作為）中的權限。 這項原則設定判斷是否稽核每個執行個體的安全性原則來執行正確的使用者或權限。 讓這個分類會導致許多「聲音」，但它可能有幫助的追蹤安全性主體帳號使用提高權限。  
  
##### <a name="audit-process-tracking"></a>審核  
這項原則設定判斷是否稽核詳細的程序追蹤事件程式啟用、結束處理程序，控點 20gb，和間接物件存取的資訊。 它是適用於追蹤惡意的使用者，他們使用的程式。  
  
讓稽核程序追蹤，通常是設定為產生了大量的事件，**無稽核**。 不過，這項設定可以提供事件回應詳細登入開始程序的期間變得更好的優點和已啟動的時間。 網域控制站及其他單一角色基礎結構伺服器，這個分類可以放心地已在任何時候。 伺服器角色單一不會產生追蹤資料傳輸期間一般他們責任多少處理程序。 因此，它們可以發生擷取未經授權的事件支援。  
  
##### <a name="system-events-audit"></a>系統事件稽核  

系統事件是幾乎一般包羅萬象分類，登記各種不同的電腦或系統安全性，安全性登影響活動。 它包括事件電腦的關機並重新開機，停電、時間的系統變更、驗證套件初始設定、登入空地所稽核構成、模擬問題，以及其他一般事件主機。 一般而言，讓這個稽核分類產生許多」雜音，」，但它產生不足，無法很有幫助的事件，很難以往，建議您不讓它。  
  
#### <a name="advanced-audit-policies"></a>進階的稽核原則  
開始使用 Windows Vista 和 Windows Server 2008、Microsoft 改善建立在每個主要稽核分類子進行分類的事件登入選項的方式。 子允許將更多遠比其他可能使用主要分類細微稽核。 使用子，您可以讓部分特定主要分類，並略過產生您有未使用的活動。 每個稽核原則子分類均可的成功、失敗，成功或失敗事件。  
  
請列出所有可用的稽核子，進階稽核原則容器在群組原則物件的檢視，或輸入下列命令提示字元中執行 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008、Windows 8、Windows 7 或 Windows Vista 的任何電腦上：  
  
**auditpol//list /subcategory: \ ***  
  
若要取得目前設定稽核子清單上的電腦執行的是 Windows Server 2012、Windows Server 2008 R2 或 Windows 2008，輸入下列動作：  
  
**auditpol//get /category: \ ***  
  
下圖顯示 auditpol.exe 列出目前稽核原則的範例。  
  
![監視廣告](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> 群組原則不會一定可以正確地報告的狀態的所有讓稽核原則，而會 auditpol.exe。 查看[取得有效 Windows 7 和 2008 R2 的稽核原則](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx)如需詳細資訊。  
  
每個主要分類有多個子。 以下是清單分類、其子，並描述的功能。  
  
### <a name="auditing-subcategories-descriptions"></a>稽核子描述  
稽核原則子讓下列事件登入郵件類型：  
  
#### <a name="account-logon"></a>Account 登入  
  
##### <a name="credential-validation"></a>Credential 驗證  
此子分類提交 account 登入的使用者要求的認證報告驗證測試的結果。 下列事件發生授權的認證的電腦上。 網域帳號，網域控制站是授權，而本機帳號，會在本機電腦是授權。  
  
在網域環境中，最 account 登入的登入的網域控制站的網域帳號授權的安全登入。 不過，這些事件可能是在組織中的其他電腦上時本機帳號用來登入。  
  
##### <a name="kerberos-service-ticket-operations"></a>Kerberos 服務票證作業  
此子分類報告 Kerberos 票證要求處理程序網域控制站的核對授權的事件。  
  
##### <a name="kerberos-authentication-service"></a>Kerberos 驗證服務  
此子分類報告事件 Kerberos 驗證服務。 下列事件發生授權的認證的電腦上。  
  
##### <a name="other-account-logon-events"></a>其他 Account 登入事件  
此子分類報告中不有關的認證驗證或 Kerberos 門票提交 account 登入的使用者要求的認證回應出現的活動。 下列事件發生授權的認證的電腦上。 網域帳號，網域控制站是授權，而本機帳號，會在本機電腦是授權。  
  
網域的環境中，最 account 登入事件被登入的網域控制站的網域帳號授權的安全登入。 不過，這些事件可能是在組織中的其他電腦上時本機帳號用來登入。 範例可以包含下列類型：  
  
-   遠端桌面服務工作階段中斷  
  
-   新遠端桌面服務工作階段  
  
-   鎖定及解除鎖定工作站  
  
-   叫用螢幕保護裝置  
  
-   關閉螢幕保護裝置  
  
-   偵測 Kerberos 重新執行的攻擊，兩次收到相同資訊要求 Kerberos 時  
  
-   Wireless 授權給使用者或電腦帳號網路的存取權  
  
-   存取有線 802.1 x 授權給使用者或電腦帳號網路  
  
#### <a name="account-management"></a>Account 管理  
  
##### <a name="user-account-management"></a>使用者 Account 管理  
此子分類報告每個使用者 account 管理使用者 account 時建立、變更或刪除; 例如事件使用者 account 重新命名，停用，或功能。或設定或變更密碼。 如果此稽核原則設定時，系統管理員可以曲目活動偵測到惡意、誤，並授權帳號建立。  
  
##### <a name="computer-account-management"></a>電腦 Account 管理  
此子分類報告電腦 account 管理，例如時電腦 account 是建立、變更、刪除、重新命名，停用，或讓每個的事件。  
  
##### <a name="security-group-management"></a>安全性群組管理  
此子分類報告每個事件安全性群組管理，例如或建立、變更或刪除安全性群組時時要新增或移除從安全性群組成員。 如果此稽核原則設定時，系統管理員可以曲目活動偵測到惡意、誤，並會在授權的安全性群組帳號建立。  
  
##### <a name="distribution-group-management"></a>管理通訊群組  
此子分類報告每個事件 distribution 群組管理，例如時建立、變更或刪除 distribution 群組或時新增或移除 distribution 群組成員。 如果此稽核原則設定時，系統管理員可以曲目活動偵測到惡意、誤，並會在授權的群組帳號建立。  
  
##### <a name="application-group-management"></a>管理應用程式群組  
此子分類報告應用程式群組管理每個的事件在電腦上，例如時建立、變更或刪除應用程式群組或時新增或移除應用程式群組成員。 如果此稽核原則設定時，系統管理員可以偵測到惡意、誤，並會在授權的應用程式群組帳號建立曲目事件。  
  
##### <a name="other-account-management-events"></a>其他 Account 管理事件  
此子分類報告其他 account 管理事件。  
  
#### <a name="detailed-process-tracking"></a>追蹤詳細的程序  
  
##### <a name="process-creation"></a>建立程序  
此子分類報告建立的處理程序，以及的程式建立的使用者名稱。  
  
##### <a name="process-termination"></a>處理程序終止  
此子分類報告時結束處理程序。  
  
##### <a name="dpapi-activity"></a>DPAPI 活動  
此子分類報告加密或解密呼叫到的資料保護應用程式開發介面 (DPAPI)。 DPAPI 用來保護的機密資訊，例如儲存密碼和資訊。  
  
##### <a name="rpc-events"></a>事件 RPC  
此子分類報告遠端程序呼叫 (RPC) 連接事件。  
  
#### <a name="directory-service-access"></a>Directory 服務的存取  
  
##### <a name="directory-service-access"></a>Directory 服務的存取  
此子分類報告時存取 AD DS 物件。 僅限的物件設定 Sacl 導致稽核事件存取它們的方式符合 SACL 項目時，會並只。 這些事件有在舊版的 Windows Server directory 服務存取事件類似。 此子分類只能套用至網域控制站。  
  
##### <a name="directory-service-changes"></a>變更 directory 服務  
此子分類報告變更 AD DS 中的物件。 會建立報告的變更的類型、修改、移動，以及取消對物件執行作業。 Directory 服務變更稽核，在適當的地方，表示舊新變更已變更物件的屬性的值。 僅限的物件 Sacl 導致稽核事件存取它們的方式符合他們 SACL 項目時，會並只。 某些物件和屬性並不會因為物件課程架構中設定被轉換稽核事件。 此子分類只能套用至網域控制站。  
  
##### <a name="directory-service-replication"></a>Directory 服務複寫  
此子分類回報兩個網域控制站之間複製開始與結束。  
  
##### <a name="detailed-directory-service-replication"></a>詳細的 Directory 服務複寫  
此子分類報告網域控制站之間複製資訊的詳細的資訊。 下列事件可以是非常高磁碟區中。  
  
#### <a name="logonlogoff"></a>登入/登出  
  
##### <a name="logon"></a>登入  
此子分類報告時使用者嘗試登入系統。 下列事件會發生存取電腦上。 下列事件代互動式登入，登入電腦上發生。 如果網路登入來存取共用發生，這些事件產生裝載資源存取電腦上。 若要有此設定**未稽核**，很難或無法以判斷有存取的使用者，或嘗試存取組織電腦。  
  
##### <a name="network-policy-server"></a>網路原則伺服器  
此子分類報告 RADIUS (IAS) 和網路存取保護 (NAP) 使用者存取要求事件。 可以將這些要求**授與**，**拒絕**，**捨棄**、**隔離**，**鎖定**，和**解除鎖定**。 稽核此設定會導致中或高記錄 NPS 及 IAS 的伺服器上的磁碟區。  
  
##### <a name="ipsec-main-mode"></a>主要 IPsec 模式  
此子分類主要模式交涉期間報告網際網路金鑰交換（ikemefuna udeze，綽號）通訊協定和驗證網際網路通訊協定 (AuthIP) 的結果。  
  
##### <a name="ipsec-extended-mode"></a>IPsec 延伸模式  
此子分類期間延伸模式交涉報告 AuthIP 的結果。  
  
##### <a name="other-logonlogoff-events"></a>其他登入/登出事件  
此子分類報告其他登入及登出相關的事件，例如遠端桌面服務中斷連接工作階段，並重新連接，使用 [執行身分執行處理程序在不同的帳號，鎖定及解除鎖定工作站。  
  
##### <a name="logoff"></a>登出  
此子分類報告時的使用者登入時系統。 下列事件會發生存取電腦上。 下列事件代互動式登入，登入電腦上發生。 如果網路登入來存取共用發生，這些事件產生裝載資源存取電腦上。 若要有此設定**未稽核**，很難或無法以判斷有存取的使用者，或嘗試存取組織電腦。  
  
##### <a name="account-lockout"></a>鎖定  
此子分類報告時帳號鎖定登入失敗次數過的結果。  
  
##### <a name="ipsec-quick-mode"></a>快速 IPsec 模式  
此子分類快速模式交涉期間報告 ikemefuna udeze，綽號通訊協定與 AuthIP 的結果。  
  
##### <a name="special-logon"></a>特殊登入  
此子分類報告時特殊登入。 特殊的登入是具有系統管理員相當權限，並且可用於更高等級台中的程序登入。  
  
#### <a name="policy-change"></a>變更原則  
  
##### <a name="audit-policy-change"></a>稽核原則變更]  
此子分類報告稽核原則 SACL 變更變更。  
  
##### <a name="authentication-policy-change"></a>驗證原則的變更  
此子分類報告驗證原則中的變更。  
  
##### <a name="authorization-policy-change"></a>授權原則變更  
此子分類報告變更授權原則，包括 (DACL) 的權限的變更。  
  
##### <a name="mpssvc-rule-level-policy-change"></a>Mpssvc 規則層級原則變更  
此子分類報告中使用 Microsoft 保護服務 (MPSSVC.exe) 原則規則的變更。 這項服務是由 Windows 防火牆。  
  
##### <a name="filtering-platform-policy-change"></a>篩選平台變更中原則  
此子分類報告新增與移除 WFP，包括開機篩選物件。 下列事件可以是非常高磁碟區中。  
  
##### <a name="other-policy-change-events"></a>其他原則變更事件  
此子分類報告其他類型的安全性原則變更，例如信賴平台模組 (TPM) 或密碼編譯提供者的設定。  
  
#### <a name="privilege-use"></a>使用權限  
  
##### <a name="sensitive-privilege-use"></a>使用機密的權限  
此子分類報告時帳號或服務會使用機密的權限。 機密的權限包括下列權利：做為作業系統的一部分，備份的檔案和目錄、建立權杖物件、程式進行偵錯、讓電腦和使用者受信任的委派產生安全性稽核、模擬驗證後的 client、載入與釋放裝置驅動程式、管理稽核帳號和安全性登入，修改 firmware 環境值、取代程序層級、還原的檔案和目錄，並取得檔案或其他物件的擁有權。 稽核這個子分類會建立大量事件。  
  
##### <a name="nonsensitive-privilege-use"></a>使用 nonsensitive 權限  
此子分類報告時帳號或服務使用 nonsensitive 權限。 Nonsensitive 權限包括下列權利：存取認證管理員為信任的本機號碼、存取這台電腦與網路、加入網域工作站、調整記憶體配額處理程序，允許登入本機、允許透過遠端桌面服務登入，略過周遊檢查、變更系統時間、建立分頁檔、建立通用物件、建立永久共用的物件、建立符號連結拒絕這台電腦與網路的存取、拒絕以分批登入、拒絕登入以服務、拒絕登入本機、拒絕登入透過遠端桌面服務、強制關機從遠端系統、增加程序運作設定、增加排程優先順序、鎖定記憶體中的網頁、分批身分登入，登入即服務，修改物件標籤執行音量維護工作設定檔單一程序，設定檔的系統效能，請移除電腦的連接基座、關機，並 directory 服務的資料同步處理。 稽核這個子分類會建立非常大量的活動。  
  
##### <a name="other-privilege-use-events"></a>其他雲端使用事件  
目前無法使用此安全性原則設定。  
  
#### <a name="object-access"></a>存取物件  
  
##### <a name="file-system"></a>檔案系統  
此子分類報告時存取檔案系統物件。 僅限檔案系統物件與 Sacl 導致稽核事件存取它們的方式有違符合他們 SACL 項目時，會並只。 本身這項原則設定不會造成的任何事件稽核。 它會判斷是否稽核使用者存取指定的系統存取控制清單 (SACL) 的檔案系統物件的事件有效地讓稽核才會生效。  
  
如果稽核物件存取設定設定為**成功**，稽核項目也每次的使用者順利存取指定 SACL 物件。 如果這項原則設定設定為**失敗**，稽核項目也每次嘗試存取物件 SACL 指定的使用者失敗。  
  
##### <a name="registry"></a>登錄  
此子分類報告時存取登錄物件。 僅限登錄物件 Sacl 導致稽核事件存取它們的方式有違符合他們 SACL 項目時，會並只。 本身這項原則設定不會造成的任何事件稽核。  
  
##### <a name="kernel-object"></a>核心物件  
此子分類報告時存取處理程序和 mutex 核心物件。 僅限使用 Sacl 核心物件導致稽核事件存取它們的方式有違符合他們 SACL 項目時，會並只。 通常核心物件只會提供 Sacl，如果 AuditBaseObjects 或 AuditBaseDirectories 稽核選項的功能。  
  
##### <a name="sam"></a>薩姆  
此子分類報告時存取本機安全性帳號 Manager（坡）驗證資料庫物件。  
  
##### <a name="certification-services"></a>認證服務  
此子分類報告時認證服務作業。  
  
##### <a name="application-generated"></a>產生應用程式  
此子分類報告時使用稽核應用程式開發介面 (Api) 的 Windows 產生稽核事件嘗試應用程式。  
  
##### <a name="handle-manipulation"></a>控點操作  
此子分類報告時開放或已關閉物件控點。 僅限的物件 Sacl 造成，並嘗試控點操作符合 SACL 項目只有將這些事件。 處理操作事件只專為物件類型（例如，檔案系統或登錄）的功能對應物件存取子分類的位置。  
  
##### <a name="file-share"></a>檔案共用  
此子分類報告時存取檔案共用。 本身這項原則設定不會造成的任何事件稽核。 它會判斷是否稽核使用者存取指定的系統存取控制清單 (SACL) 的檔案共用物件的事件有效地讓稽核才會生效。  
  
##### <a name="filtering-platform-packet-drop"></a>篩選平台封包拖放  
此子分類報告時封包會卸除的 Windows 篩選平台 (WFP)。 下列事件可以是非常高磁碟區中。  
  
##### <a name="filtering-platform-connection"></a>篩選平台連接  
此子分類報告時允許或封鎖 WFP 連接。 下列事件可能高磁碟區中。  
  
##### <a name="other-object-access-events"></a>其他物件存取事件  
此子分類報告其他物件存取相關事件工作排程器工作和 COM + 物件。  
  
#### <a name="system"></a>系統  
  
##### <a name="security-state-change"></a>變更安全性狀態  
此子分類報告變更系統，例如安全性子系統時開始和停止的安全狀態。  
  
##### <a name="security-system-extension"></a>安全性系統的擴充功能  
此子分類的安全性子系統報告載入驗證套件例如擴充功能程式碼。  
  
##### <a name="system-integrity"></a>系統整合  
此子分類的安全性子系統完整性違反報告。  
  
IPsec 驅動程式  
  
此子分類的「網際網路通訊協定的安全性 (IPsec) 驅動程式的活動報告。  
  
##### <a name="other-system-events"></a>其他系統事件  
此子分類其他系統活動報告。  
  
如需子分類描述的相關資訊，請參考[Microsoft Security Compliance Manager 工具](https://technet.microsoft.com/library/cc677002.aspx)。  
  
每個組織應該上述涵蓋的分類和子以及安裝的最適合自己的環境。 隨時應該 production 環境中部署之前測試稽核原則的變更。  
  
### <a name="configuring-windows-audit-policy"></a>設定 Windows 稽核原則  
使用群組原則、auditpol.exe、Api 或登錄編輯可以設定 Windows 稽核原則。 大部分的公司稽核原則設定的建議的方法的群組原則」或「auditpol.exe。 將系統稽核原則設定需要 account 系統管理員等級權限] 或 [委派適當權限。  
  
> [!NOTE]  
> **管理稽核和安全性登入**的安全性原則必須給予權限（系統管理員讓它預設）允許修改物件存取稽核個人資源，例如的檔案、Active Directory 物件和登錄鍵的選項。  
  
#### <a name="setting-windows-audit-policy-by-using-group-policy"></a>使用群組原則設定 Windows 稽核原則  
若要設定使用群組原則稽核原則，設定適當稽核分類位於**電腦 \windows 安全性設定本機稽核原則**（查看下列的本機群組原則編輯器 (gpedit.msc) 例如螢幕擷取畫面）。 每個稽核原則分類均可適用於**成功**，**失敗**，或**成功**和失敗事件。  
  
![監視廣告](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
進階的稽核原則可以使用 Active Directory 或的本機群組原則設定。 若要設定進階稽核原則，設定適當的子位於**電腦 \windows 安全性設定 Settings\Advanced 稽核原則**（查看下列的本機群組原則編輯器 (gpedit.msc) 例如螢幕擷取畫面）。 每個稽核原則子分類均可適用於**成功**，**失敗**，或**成功**和**失敗**活動。  
  
![監視廣告](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
#### <a name="setting-windows-audit-policy-using-auditpolexe"></a>使用 Auditpol.exe 設定 Windows 稽核原則  
Windows Server 2008 和 Windows Vista 中引進 Auditpol.exe（適用於設定 Windows 稽核原則）。 一開始 auditpol.exe 可用於設定進階稽核原則，但在 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008、Windows 8 和 Windows 7 中可以使用群組原則。  
  
Auditpol.exe 是一個命令列的公用程式。 語法如下：  
  
**auditpol//set 日 < 分類 |子分類 >:<audit category>日 < 成功 | 錯誤：> 日 < 讓 | 停用 >**  
  
Auditpol.exe 語法範例：  
  
**auditpol//set /subcategory:「使用者 account 管理」/success：讓 /failure：讓**  
  
**auditpol//set /subcategory:「登入「/success：讓 /failure：讓**  
  
**auditpol//set /subcategory:「IPSEC 主要模式」/failure：讓**  
  
> [!NOTE]  
> Auditpol.exe 本機設定進階稽核原則。 如果您本機原則衝突 Active Directory 或本機群組原則，群組原則設定通常優先適用透過 auditpol.exe 設定。 當會有多個群組] 或 [本機原則衝突時，只有一個原則將會優先適用（也就，請更換）。 不會將合併稽核原則。  
  
#### <a name="scripting-auditpol"></a>指令碼 Auditpol  
Microsoft 提供[樣本指令碼](https://support.microsoft.com/kb/921469)針對想要進階稽核原則設定使用指令碼，而不是每個 auditpol.exe 命令手動輸入系統管理員。  
  
**注意**群組原則不會不一定可以正確地報告狀態的所有讓稽核原則，而 auditpol.exe 會。 查看[取得有效 Windows 7 和 Windows 2008 R2 的稽核原則](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx)如需詳細資訊。  
  
#### <a name="other-auditpol-commands"></a>Auditpol 的其他命令  
Auditpol.exe 可用來儲存和還原本機稽核原則，以及檢視其他稽核相關的命令。 以下是另**auditpol**的命令。  
  
**auditpol/清除**-用來清除和本機稽核原則防重設  
  
**auditpol /backup /file:<filename>** -使用二進位檔案備份目前的本機稽核原則  
  
**auditpol /restore /file:<filename>** -使用匯入之前儲存的稽核原則檔案到本機稽核原則  
  
**auditpol / < 取得日設定 > /option:<CrashOnAuditFail> / < 讓或停用 >** -如果這個稽核原則設定時，它會導致系統立刻停止 (停止使用：C0000244 {稽核無法} 訊息) 是否有任何原因安全性稽核無法登入。 通常，便無法事件時安全性稽核已滿，指定的安全性登入保持方法是登入**不覆寫事件**或**覆寫依日期事件**。 通常它僅適，需要更高安全性登入登入保證的環境。 如果功能，系統管理員必須密切觀賞安全性登入的大小，並旋轉視需要登。 它也可以設定群組原則來修改的安全性選項**稽核：系統如果無法登入安全性稽核立即關機]** (預設值 = 停用)。  
  
**Auditpol / < 取得日設定 > /option:<AuditBaseObjects> / < 讓或停用 >** -此稽核原則設定判斷是否稽核物件全域系統的存取。 如果這項原則功能，它會導致系統物件，例如 mutex、誌，以及 DOS 裝置使用預設系統存取控制清單 (SACL) 來建立。 大部分的系統管理員考慮稽核全域系統物件太」吵，「，這些只可以讓它如果惡意駭客懷疑。 僅限命名的物件可以 SACL。 如果稽核物件存取稽核原則（或核心物件的稽核子分類）也功能，被稽核這些系統物件的存取權。 當這個安全性設定，變更會生效您重新開機的 Windows。 這項原則也可以設定群組原則來修改全域系統物件的安全性選項稽核存取 (預設值 = 停用)。  
  
**auditpol / < 取得日設定 > /option:<AuditBaseDirectories> / < 讓或停用 >** -此稽核原則設定指定命名的核心物件（例如 mutex 和誌）會在建立時提供 Sacl。 AuditBaseDirectories 影響容器物件時 AuditBaseObjects 影響不包含其他物件的物件。  
  
**Auditpol / < 取得日設定 > /option:<FullPrivilegeAuditing> / < 讓或停用 >** -  
  
此稽核原則設定指定 client 產生事件時一或多個這些權限已指派給使用者的安全性權杖：AssignPrimaryTokenPrivilege、AuditPrivilege、BackupPrivilege、CreateTokenPrivilege、DebugPrivilege、EnableDelegationPrivilege、ImpersonatePrivilege、LoadDriverPrivilege、RestorePrivilege、SecurityPrivilege、SystemEnvironmentPrivilege、TakeOwnershipPrivilege 及 TcbPrivilege。 如果不支援此選項會 (預設值 = 停用)，不會記錄 BackupPrivilege 和 RestorePrivilege 權限。 讓這個選項可以備份操作期間，讓安全性登入很吵（有時候數百種第二事件）。 這項原則也可以設定群組原則來修改的安全性選項**稽核：稽核使用備份與還原權限的**。  
  
> [!NOTE]  
> 以下提供一些資訊來自 Microsoft 的[稽核選項類型](https://msdn.microsoft.com/library/dd973862(prot.20).aspx)和 [Microsoft SCM 工具。  
  
### <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>執行傳統稽核或進階稽核  
在 Windows Server 2012、Windows Server 2008 R2、Windows Server 2008、Windows 8、Windows 7 和 Windows Vista，系統管理員可以選擇讓九傳統類型，或使用子。 這是每個 Windows 系統中進行必須二進位選擇。 主要分類均可或 subcategoriesit 不能同時。  
  
您必須以避免覆寫稽核原則子原則舊的傳統分類，讓**推動稽核原則子分類設定 (Windows Vista 或更新版本) 若要覆寫稽核原則分類設定**原則設定位於**電腦 \windows 安全性設定本機原則安全性選項**。  
  
我們建議功能，而不是九主要分類設定子。 這需要（若要允許覆寫稽核分類子）功能的群組原則設定，以及設定不同子支援稽核原則。  
  
稽核子可以透過數種方式，包括群組原則和的命令列程式 auditpol.exe 設定。  
  
如需關於 Windows 稽核，查看下列的文件：  
  
-   [進階的安全性稽核 windows 7 和 Windows Server 2008 R2](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
-   [稽核和 Windows Server 2008 的相容性](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
-   [如何使用群組原則設定詳細的安全性稽核網域 Windows Server 2008、Windows Server 2003 網域中，或 Windows 2000 網域中的 Windows vista 和 Windows Server 2008 為電腦設定](https://support.microsoft.com/kb/921469)  
  
-   [進階安全性稽核原則 Step-by-Step 指南](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  
  


