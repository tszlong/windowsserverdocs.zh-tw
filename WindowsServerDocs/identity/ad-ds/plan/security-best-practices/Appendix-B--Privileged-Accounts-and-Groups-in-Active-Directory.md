---
ms.assetid: 79b9c912-ea3e-4679-ab41-893e096c4d09
title: "附錄 B-權限帳號，並在 Active Directory 中群組"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c4f98b1b68ebdf290ba15b75274d7a876912565b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-b-privileged-accounts-and-groups-in-active-directory"></a>B 附錄特殊權限的帳號及 Active Directory 中的群組

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-b-privileged-accounts-and-groups-in-active-directory"></a>B 附錄特殊權限的帳號及 Active Directory 中的群組  
「 特殊權限 」 帳號及群組 Active Directory 中的這些的強大權限，權限，以及權限授與，讓它們在 Active Directory 和加入網域的系統上執行幾乎任何動作。 這個附錄開始討論權限，權限，而權限，緊接著 」 高權限來執行 「 帳號和群組 Active Directory 中的相關資訊也就是群組與最有效帳號。  

資訊也會提供有關建和預設帳號，並在 Active Directory，除了他們的權限的群組。 以不同附錄提供保護的最高的權限帳號及群組特定的設定的建議，雖然這附錄提供可協助您找出使用者和群組，您應該重點放在安全的背景資訊。 您應該做，因為它們可以利用攻擊者甚至破壞 Active Directory 安裝侵入您，並。  

### <a name="rights-privileges-and-permissions-in-active-directory"></a>權限、 權限和 Active Directory 中的權限  
不同的權限，權限 」 及權限可以是令人混淆和及清楚，甚至在來自 Microsoft 的文件。 本節特性每個部分它們是使用本文件。 這些描述不能視為授權的其他 Microsoft 文件，因為它可能會使用這些條款，以不同方式。  

#### <a name="rights-and-privileges"></a>權利和權限  
權利和權限的有效授權給使用者、 服務、 電腦或群組例如的安全性原則相同全系統功能。 在通常會由 IT 專業人員的介面，這些通常稱為 [權限 」 或 「 使用者權限]，通常指派給群組原則物件。 下圖顯示一些最常見的權利，可指派到安全性原則 （代表 Windows Server 2012 網域中預設的網域控制站 GPO）。 這些權限部分適用於 Active Directory，例如**讓電腦和使用者帳號受信任的委派**使用者權限時其他權利套用到 Windows 作業系統，例如,**變更系統時間**。  

![有特殊權限的帳號，並群組](media/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory/SAD_8.gif)  

在介面例如 「 群組原則物件編輯器] 中所有的這些指派功能稱為廣泛使用者權利。 事實上不過，有些使用者權限以程式設計方式稱為權限，其他人以程式設計方式就是權限時。 下表 B-1： 使用者權利和權限提供的一些最常見的指派使用者權利和他們程式設計常數。 群組原則和其他介面參考這些為使用者權限，但部分的以程式設計方式視為權限時其他定義為權限。  

針對每個使用者的權限在下表中列出的相關詳細資訊，如下表所使用的連結，或查看[威脅和措施指南： 使用者權利](https://technet.microsoft.com/library/hh125917(v=ws.10).aspx)在[威脅和弱點防護](https://technet.microsoft.com/library/cc755181(v=ws.10).aspx)指南 Windows Server 2008 R2 上的 Microsoft TechNet 網站。 適用於 Windows Server 2008 的詳細資訊，請查看[使用者權利](https://technet.microsoft.com/library/dd349804(v=WS.10).aspx)在[的威脅，弱點防護](https://technet.microsoft.com/library/cc755181(v=ws.10).aspx)Microsoft TechNet 網站上的文件。 截至本文件撰寫、 Windows Server 2012 對應的文件不尚未發行。  

> [!NOTE]  
> 針對這份文件，條款 」 權限 」 及 「 使用者權利 」 用來辨識權利和權限，除非另有指定。  

##### <a name="table-b-1-user-rights-and-privileges"></a>下表 B-1： 使用者權利和權限  

|||  
|-|-|  
|**立即在群組原則中的使用者**|**常數名稱**|  
|[存取認證管理員做受信任的本機號碼](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_2)|SeTrustedCredManAccessPrivilege|  
|[從網路存取此電腦](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_1)|SeNetworkLogonRight|  
|[做為作業系統的一部分](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_3)|SeTcbPrivilege|  
|[加入網域工作站](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_4)|SeMachineAccountPrivilege|  
|[調整記憶體配額處理程序](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_5)|SeIncreaseQuotaPrivilege|  
|[在本機允許登入](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_6)|SeInteractiveLogonRight|  
|[允許透過車票服務登入](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_7)|SeRemoteInteractiveLogonRight|  
|[備份的檔案和目錄](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_8)|SeBackupPrivilege|  
|[略過周遊檢查](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_9)|SeChangeNotifyPrivilege|  
|[變更系統時間](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_10)|SeSystemtimePrivilege|  
|[變更時區](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_11)|SeTimeZonePrivilege|  
|[建立分頁檔](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_12)|SeCreatePagefilePrivilege|  
|[建立權杖物件](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_13)|SeCreateTokenPrivilege|  
|[建立通用物件](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_14)|SeCreateGlobalPrivilege|  
|[建立永久共用的物件](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_15)|SeCreatePermanentPrivilege|  
|[建立符號的連結](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_16)|SeCreateSymbolicLinkPrivilege|  
|[程式進行偵錯](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_17)|SeDebugPrivilege|  
|[拒絕從網路存取此電腦](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_18)|SeDenyNetworkLogonRight|  
|[拒絕以分批登入](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_18a)|SeDenyBatchLogonRight|  
|[拒絕登入即服務](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_19)|SeDenyServiceLogonRight|  
|[在本機拒絕登入](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_20)|SeDenyInteractiveLogonRight|  
|[透過車票服務拒絕登入](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_21)|SeDenyRemoteInteractiveLogonRight|  
|[讓電腦和使用者帳號受信任的委派](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_22)|SeEnableDelegationPrivilege|  
|[從遠端系統推動關機](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_23)|SeRemoteShutdownPrivilege|  
|[產生安全性稽核](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_24)|SeAuditPrivilege|  
|[驗證後模擬 client](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_25)|SeImpersonatePrivilege|  
|[增加程序運作設定](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_26)|SeIncreaseWorkingSetPrivilege|  
|[增加排定優先順序](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_27)|SeIncreaseBasePriorityPrivilege|  
|[載入，而且釋放裝置驅動程式](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_28)|SeLoadDriverPrivilege|  
|[在記憶體中的鎖定頁面](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_29)|SeLockMemoryPrivilege|  
|[分批身分登入](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_30)|SeBatchLogonRight|  
|[登入即服務](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_31)|SeServiceLogonRight|  
|[管理稽核及安全的登入](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_32)|SeSecurityPrivilege|  
|[修改物件標籤](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_33)|SeRelabelPrivilege|  
|[修改 firmware 環境值](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_34)|SeSystemEnvironmentPrivilege|  
|[執行音量維護工作](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_35)|SeManageVolumePrivilege|  
|[設定檔單一程序](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_36)|SeProfileSingleProcessPrivilege|  
|[設定檔的系統效能](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_37)|SeSystemProfilePrivilege|  
|[連接基座移除電腦](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_38)|SeUndockPrivilege|  
|[取代程序層級](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_39)|SeAssignPrimaryTokenPrivilege|  
|[還原的檔案和目錄](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_40)|SeRestorePrivilege|  
|[關機](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_41)|SeShutdownPrivilege|  
|[同步處理 directory 服務的資料](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_42)|SeSyncAgentPrivilege|  
|[取得檔案或其他物件的擁有權](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_43)|SeTakeOwnershipPrivilege|  

#### <a name="permissions"></a>權限  
權限的存取控制項，可套用至安全物件，例如檔案系統、 登錄、 服務及 Active Directory 物件。 每個安全物件已相關的存取控制清單 (ACL)，其中包含存取控制項目 (a) 的授權或拒絕 （使用者、 服務、 電腦或群組） 的安全性原則來執行各種運算物件的能力。 例如的許多 Active Directory 物件的 Acl 包含 a 允許 Authenticated Users 朗讀一般資訊物件，但請勿授與這些讀取機密資訊，或變更物件的能力。
每個網域的建來賓，除了登入且通過網域控制站受信任的樹系的 Active Directory 森林中的每個安全性主體已驗證使用者安全性 （識別碼） 新增至其存取權杖預設。 因此，使用者、 服務或電腦 account 嘗試朗讀一般使用者網域中的物件的屬性，是否已成功讀取的作業。  

安全性主體嘗試不存取的任何 a 物件定義和包含存在於主體的存取權杖 SID，如果主體無法存取物件。 此外，如果 a 物件的 ACL 中的使用者存取權杖 「 deny 「 A 符合 SID 包含拒絕項目將會通常衝突覆寫 」 可讓 「 A。 如需 Windows 中存取控制的詳細資訊，請查看[存取控制](https://msdn.microsoft.com/library/aa374860(v=VS.85).aspx)MSDN 網站上。  

本文件中的權限指的是功能的授權或拒絕上安全物件的安全性原則。 使用者和權限之間發生衝突時，使用者權利通常會優先。 例如，如果 acl 所有讀取和寫入存取物件拒絕系統管理員已在 Active Directory 物件，使用者網域中的系統管理員群組成員將無法檢視物件的相關資訊。 不過，系統管理員群組授與使用者向 「 拍攝的擁有權檔案或其他物件 」，因為使用者可以只將物件的擁有權有問題，然後重新寫入物件的 ACL 授與系統管理員完全控制物件。  

它是針對這個原因，此文件鼓勵您避免日常的系統管理，使用強大帳號，並群組，而不是嘗試限制帳號及群組的功能。 不是有效可能停止有心的使用者可以使用這些認證存取的任何安全資源強大認證存取。  

### <a name="built-in-privileged-accounts-and-groups"></a>建帳號及群組特殊權限  
Active Directory 是協助管理委派和的權限指派權利與權限的原則。 有帳號 Active Directory domain 在 [一般] 使用者，根據預設，可以朗讀的多的項目儲存在 directory，但無法變更只非常有限的資料 directory。 使用者需要額外的權限授與到 directory 建置，所以它們可能會執行他們的角色的相關特定的工作，但無法執行工作，不會與他們責任各種有特殊權限群組成員資格。  

在 Active Directory 中有組成 directory 中的最高的權限群組建三個群組： 企業系統管理員 (EA) 群組、 群組網域系統管理員 (DA) 和建系統管理員 (BA) 群組。  

第四個群組，架構系統管理員 （索） 群組，擁有權限的如果濫用，損壞或破壞整個 Active Directory 樹系，但此群組多限制該功能比 EA、 DA 及 BA 群組。  

除了這些四個群組，有一些其他建及預設帳號，每一個都授與權限，允許執行特定管理工作 Active Directory 中的群組。 雖然這個附錄不提供完整的 Active Directory 中的每個建或預設群組討論，它提供的群組和帳號，您應該會看到在您安裝的。  

例如如果您安裝 Microsoft Exchange Server Active Directory 樹系到時，其他帳號和群組可能會建立建和使用者容器在您的網域中。 這個附錄描述僅限群組和帳號所建立的建和使用者容器在 Active Directory，根據原生角色及功能。 不包含帳號，並安裝企業版軟體所建立的群組的。  

#### <a name="enterprise-admins"></a>企業系統管理員  
企業系統管理員 (EA) 群組位於森林根網域中，而且預設建森林中的每個網域中的系統管理員群組成員。 森林根網域中的建管理員是的唯一預設 EA 群組成員。 EAs 會授與權限，讓他們樹系的變更會影響。 以下是變更會影響所有網域中的樹系，例如新增或移除網域、 建立信任的樹系，或引發森林功能層級。 在適當地設計和實作委派模式下，僅第一次建構樹系時才或特定例如建立輸出森林信任的樹系變更，就需要 EA 成員資格。  

EA 群組位於森林根網域中的使用者容器中的預設且萬用安全性群組，除非森林根網域執行 Windows 2000 Server 混合模式，案例群組是安全性的全域群組中。 某些權限授與直接 EA 群組，但繼承很多此群組的權限的實際 EA 群組，因為它是森林中的每個網域中的系統管理員群組成員。 企業系統管理員具備工作站或成員伺服器無預設權限。  

#### <a name="domain-admins"></a>網域系統管理員 」  
每個網域中的有它自己的網域系統管理員 (DA) 群組，也就是在每個已經加入網域的電腦上系統管理員本機群組成員除了網域的建系統管理員 (BA) 群組成員。 僅限預設群組成員的 DA 網域是該網域建系統管理員負責。  

EAs 有樹系權限時，會在他們網域，雖然 DAs。 在正常運作設計和實作委派模式下，DA 成員資格應該需要只能在 「 中斷玻璃 「 案例中，這情形需要網域中的每一部電腦上的權限等級高帳號，或在特定網域寬需要進行變更。 可能只在緊急案例中使用 DA 帳號，原生 Active Directory 委派機制並允許委派，雖然建構生效委派型號可能會花時間，以及許多組織促進此程序使用第三方應用程式。  

DA 群組是位於網域中的使用者容器安全性的全域群組。 有一個 DA 群組的每個網域中的樹系，而且只預設 DA 群組成員建系統管理員核對的。 因為您的網域 DA 群組位 BA 群組的所有加入網域的系統本機系統管理員群組中，DAs 不只需要明確授與網域系統管理員權限，但它們也繼承所有權限和權限授與的網域中的系統管理員群組和本機系統管理員群組所有加入網域的系統上。  

#### <a name="administrators"></a>系統管理員  
系統管理員 (BA) 建群組是網域本機群組建網域的容器至的 DAs 和 EAs 巢方式，且會授與的許多直接存取權限和權限 directory 和網域控制站在這個群組中。 不過，系統管理員群組網域不會有任何權限，或工作站成員伺服器上。 加入網域的電腦本機系統管理員群組成員資格是本機的權限授與位置。而且群組討論，只有 DAs 預設，所有加入網域的電腦本機系統管理員群組成員。  

系統管理員群組是網域本機群組中的網域建容器。 根據預設，每個網域的 BA 群組包含本機網域建管理員、 本機網域 DA 群組中和森林根網域 EA 群組。 專為系統管理員群組，不適用於 EAs 或 DAs 授與和網域控制站在 Active Directory 中的許多使用者權利。 完全控制大部分的 directory 物件的權限授與的網域 BA 群組，並可以取得 directory 物件的擁有權。 EA 和 DA 群組某些特定物件的權限的樹系和網域中，雖然大部分的群組的功能確實 」 繼承 」 從他們的 BA 群組成員資格。  

> [!NOTE]  
> 雖然這些都是個特殊權限群組預設設定的其中一種三個群組成員可以管理 directory 取得任何其他群組成員資格。 有時很一般達成，而在其他很難，但潛在的權限的角度，從三個群組被視為相等有效地。  

#### <a name="schema-admins"></a>架構系統管理員  
架構系統管理員 （索） 群組通用群組森林根網域中的，該網域的建系統管理員 account 做為預設成員類似 EA 群組。 雖然索群組成員資格可以允許危害，framework 整個 Active Directory 樹系的 Active Directory 區結構描述 SAs 有幾個預設權限和架構以外的權限。  

您應該會仔細管理及監視成員資格索群組中，但在某些方面，此群組 」 較低權限 」 比最高的三個特殊權限群組前述因為其權限的範圍是非常縮小問題。也就是 SAs 有架構以外的任何位置點一下不系統管理員權限。  

#### <a name="additional-built-in-and-default-groups-in-active-directory"></a>其他建和 Active Directory 中的預設群組  
若要加速 directory 中的管理委派，Active Directory 隨附各種建和預設群組授與的特定權限與權限。 下表描述短暫這些群組。  

下表列出 Active Directory 中建和預設的群組。 這兩個組群組存在預設;不過，建群組位於 （預設） 建容器 Active Directory 中時預設群組位於 （預設） 中的使用者容器 Active Directory 中。 建容器中的群組是所有網域本機群組，而使用者容器中的群組混合網域區域、 通用，與通用群組除了三個登入電腦 （系統管理員，來賓，以及 Krbtgt）。  

除了此附錄之前所述的最高有特殊權限群組，某些建和預設帳號群組會授與和提高權限和應也保護並只有在安全的系統管理主機上使用。 找到這些群組及帳號灰色列表 B-1： 建及帳號 Active Directory 中的預設的群組。 一些群組及帳號會授與權限可以危害 Active Directory 或網域控制站濫用，因為它們也可以用額外的保護中所述[附錄 c： 保護帳號，並 Active Directory 中的群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  

##### <a name="table-b-1-built-in-and-default-accounts-and-groups-in-active-directory"></a>下表 B-1： 建預設帳號及 Active Directory 中的群組  

||||  
|-|-|-|  
|**Account 或群組**|**預設容器、 群組領域並輸入**|**描述與預設使用者權限**|  
|存取控制協助者 (在 Windows Server 2012 中 Active Directory)|建容器<br /><br />網域本機安全性群組|授權屬性與權限此電腦上的資源，可以從遠端查詢此群組成員。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|Account 電信業者|建容器<br /><br />網域本機安全性群組|成員可以管理網域使用者和群組帳號。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|管理員|使用者容器<br /><br />不是群組|建負責管理網域。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />調整記憶體配額處理程序<br /><br />在本機允許登入<br /><br />允許登入透過遠端桌面服務<br /><br />備份的檔案和目錄<br /><br />略過周遊檢查<br /><br />變更系統時間<br /><br />變更時區<br /><br />建立分頁檔<br /><br />建立通用物件<br /><br />建立符號的連結<br /><br />程式進行偵錯<br /><br />讓電腦和使用者帳號受信任的委派<br /><br />從遠端系統推動關機<br /><br />驗證後模擬 client<br /><br />增加程序運作設定<br /><br />增加排定優先順序<br /><br />載入，而且釋放裝置驅動程式<br /><br />分批身分登入<br /><br />管理稽核及安全的登入<br /><br />修改 firmware 環境值<br /><br />執行音量維護工作<br /><br />設定檔單一程序<br /><br />設定檔的系統效能<br /><br />連接基座移除電腦<br /><br />還原的檔案和目錄<br /><br />關機<br /><br />取得檔案或其他物件的擁有權|  
|系統管理員群組|建容器<br /><br />網域本機安全性群組|系統管理員可以網域完整，且不受限制的存取。<br /><br />**直接使用者權限：**<br /><br />從網路存取此電腦<br /><br />調整記憶體配額處理程序<br /><br />在本機允許登入<br /><br />允許登入透過遠端桌面服務<br /><br />備份的檔案和目錄<br /><br />略過周遊檢查<br /><br />變更系統時間<br /><br />變更時區<br /><br />建立分頁檔<br /><br />建立通用物件<br /><br />建立符號的連結<br /><br />程式進行偵錯<br /><br />讓電腦和使用者帳號受信任的委派<br /><br />從遠端系統推動關機<br /><br />驗證後模擬 client<br /><br />增加排定優先順序<br /><br />載入，而且釋放裝置驅動程式<br /><br />分批身分登入<br /><br />管理稽核及安全的登入<br /><br />修改 firmware 環境值<br /><br />執行音量維護工作<br /><br />設定檔單一程序<br /><br />設定檔的系統效能<br /><br />連接基座移除電腦<br /><br />還原的檔案和目錄<br /><br />關機<br /><br />取得檔案或其他物件的擁有權<br /><br />繼承的使用者權限：<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|允許的 RODC 密碼複寫群組|使用者容器<br /><br />網域本機安全性群組|此群組成員可以讓它們複製到網域中的所有唯讀網域控制站的密碼。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|備份電信業者|建容器<br /><br />網域本機安全性群組|備份電信業者可以備份或還原檔案的專為覆寫安全性限制。<br /><br />**直接使用者權限：**<br /><br />在本機允許登入<br /><br />備份的檔案和目錄<br /><br />分批身分登入<br /><br />還原的檔案和目錄<br /><br />關機<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|憑證的發行者|使用者容器<br /><br />網域本機安全性群組|此群組成員允許發行至 「 directory 的憑證。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|憑證服務 DCOM 存取|建容器<br /><br />網域本機安全性群組|如果憑證服務 （不建議使用） 的網域控制站安裝，此群組授與 DCOM 註冊存取網域使用者與網域的電腦。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|複製網域控制站 (在 Windows Server 2012AD DS AD DS)|使用者容器<br /><br />安全性的全域群組|此群組成員可以複製網域控制站的。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|密碼編譯電信業者|建容器<br /><br />網域本機安全性群組|獲准執行密碼編譯作業。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|使用者偵錯工具|這是預設都建群組，但時在於 AD DS，以深入調查的原因。|偵錯工具使用者群組的指示，偵錯工具上已安裝的系統有些時候，是否透過 Visual Studio、 SQL，Office 或其他應用程式所需要支援的偵錯環境。 此群組，可讓遠端偵錯的存取權的電腦。 此群組網域層級時，表示您的偵錯工具或包含偵錯工具的應用程式上已安裝的網域控制站。|  
|拒絕的 RODC 密碼複寫群組|使用者容器<br /><br />網域本機安全性群組|此群組成員能它們複製到網域中的任何唯讀網域控制站的密碼。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|DHCP 系統管理員|使用者容器<br /><br />網域本機安全性群組|此群組成員存取管理 DHCP 伺服器服務。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|DHCP 使用者|使用者容器<br /><br />網域本機安全性群組|此群組成員擁有僅檢視存取 DHCP 伺服器服務。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|COM 使用者|建容器<br /><br />網域本機安全性群組|此群組成員可以上市、 啟動及這台電腦上使用分散式的 COM 物件。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|DnsAdmins|使用者容器<br /><br />網域本機安全性群組|此群組成員存取管理 DNS 伺服器服務。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|DnsUpdateProxy|使用者容器<br /><br />安全性的全域群組|此群組成員的 DNS 用允許執行的無法自動執行動態更新戶端代表動態更新。 此群組成員通常 DHCP 伺服器。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|網域系統管理員 」|使用者容器<br /><br />安全性的全域群組|指定的系統管理員的網域。網域系統管理員是每個加入網域的電腦的本機群組成員的系統管理員，及接收權利和權限授與本機系統管理員群組，除了網域中的系統管理員 」 群組。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />調整記憶體配額處理程序<br /><br />在本機允許登入<br /><br />允許登入透過遠端桌面服務<br /><br />備份的檔案和目錄<br /><br />略過周遊檢查<br /><br />變更系統時間<br /><br />變更時區<br /><br />建立分頁檔<br /><br />建立通用物件<br /><br />建立符號的連結<br /><br />程式進行偵錯<br /><br />讓電腦和使用者帳號受信任的委派<br /><br />從遠端系統推動關機<br /><br />驗證後模擬 client<br /><br />增加程序運作設定<br /><br />增加排定優先順序<br /><br />載入，而且釋放裝置驅動程式<br /><br />分批身分登入<br /><br />管理稽核及安全的登入<br /><br />修改 firmware 環境值<br /><br />執行音量維護工作<br /><br />設定檔單一程序<br /><br />設定檔的系統效能<br /><br />連接基座移除電腦<br /><br />還原的檔案和目錄<br /><br />關機<br /><br />取得檔案或其他物件的擁有權|  
|網域的電腦|使用者容器<br /><br />安全性的全域群組|所有工作站和加入網域的伺服器都都預設此群組成員。<br /><br />**預設直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|網域控制站|使用者容器<br /><br />安全性的全域群組|網域中的所有網域控制站。 注意： 網域控制站並非的網域電腦群組成員。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|網域來賓|使用者容器<br /><br />安全性的全域群組|網域中的所有來賓<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|使用者網域|使用者容器<br /><br />安全性的全域群組|所有使用者網域中<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|企業的系統管理員 （存在只能在森林根網域中）|使用者容器<br /><br />萬用安全性群組|企業系統管理員權能變更樹系設定。企業系統管理員是每個網域中的系統管理員群組成員，並接收權利與權限授與該群組。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />調整記憶體配額處理程序<br /><br />在本機允許登入<br /><br />允許登入透過遠端桌面服務<br /><br />備份的檔案和目錄<br /><br />略過周遊檢查<br /><br />變更系統時間<br /><br />變更時區<br /><br />建立分頁檔<br /><br />建立通用物件<br /><br />建立符號的連結<br /><br />程式進行偵錯<br /><br />讓電腦和使用者帳號受信任的委派<br /><br />從遠端系統推動關機<br /><br />驗證後模擬 client<br /><br />增加程序運作設定<br /><br />增加排定優先順序<br /><br />載入，而且釋放裝置驅動程式<br /><br />分批身分登入<br /><br />管理稽核及安全的登入<br /><br />修改 firmware 環境值<br /><br />執行音量維護工作<br /><br />設定檔單一程序<br /><br />設定檔的系統效能<br /><br />連接基座移除電腦<br /><br />還原的檔案和目錄<br /><br />關機<br /><br />取得檔案或其他物件的擁有權|  
|企業唯讀網域控制站|使用者容器<br /><br />萬用安全性群組|此群組包含森林中的所有唯讀網域控制站帳號。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|事件登入助讀程式|建容器<br /><br />網域本機安全性群組|此群組成員可以讀取網域控制站事件登。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|群組原則 Creator 擁有者|使用者容器<br /><br />安全性的全域群組|此群組成員可以建立和修改網域中的群組原則物件。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|客體|使用者容器<br /><br />不是群組|這是 account 僅 AD DS 網域中，不需要新增到其存取權杖驗證使用者 SID。 因此，任何設定為權限授與 Authenticated Users 群組的資源將不會存取此 account。 此行為不正確的網域來賓與來賓群組成員，不過-那些群組成員擁有加入他們的存取權杖驗證使用者 SID。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|來賓|建容器<br /><br />網域本機安全性群組|來賓有相同的存取權的使用者群組成員預設，除了來賓，然後再限制之前所述。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|HYPER-V 系統管理員 (Windows Server 2012)|建容器<br /><br />網域本機安全性群組|此群組成員存取完整，且不受限制 HYPER-V 中的所有功能。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|IIS_IUSRS|建容器<br /><br />網域本機安全性群組|使用網際網路資訊服務建群組。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|連入森林信任建造商 （存在只能在森林根網域中）|建容器<br /><br />網域本機安全性群組|此群組成員可以建立這個樹系傳入、 單向信任。 （建立輸出樹系信任保留適用於企業系統管理員 」）。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|Krbtgt|使用者容器<br /><br />不是群組|Krbtgt account 是網域中 Kerberos 金鑰 Distribution 中心服務負責。 這個 account 可以存取所有帳號認證儲存在 Active Directory 中。 這個 account 預設停用，以及應該不支援<br /><br />**使用者權限：**不適用|  
|網路設定電信業者|建容器<br /><br />網域本機安全性群組|此群組成員權限，讓他們管理設定的網路功能。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|效能登入的使用者|建容器<br /><br />網域本機安全性群組|此群組成員可以排程效能計數器登入，讓追蹤提供者，並在電腦本機或透過遠端存取收集事件追蹤。<br /><br />**直接使用者權限：**<br /><br />分批身分登入<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|效能監視器使用者|建容器<br /><br />網域本機安全性群組|在本機或遠端此群組成員可以存取計數器效能的資料。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|Windows 2000 相容存取|建容器<br /><br />網域本機安全性群組|此群組存在回溯相容性 Windows 2000 Server、 之前的作業系統，並提供成員朗讀使用者和群組資訊網域中的功能。<br /><br />**直接使用者權限：**<br /><br />從網路存取此電腦<br /><br />略過周遊檢查<br /><br />**繼承的使用者權限：**<br /><br />加入網域工作站<br /><br />增加程序運作設定|  
|列印電信業者|建容器<br /><br />網域本機安全性群組|此群組成員可以管理網域印表機。<br /><br />**直接使用者權限：**<br /><br />在本機允許登入<br /><br />載入，而且釋放裝置驅動程式<br /><br />關機<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|RAS 及 IAS 伺服器|使用者容器<br /><br />網域本機安全性群組|此群組中的伺服器可讀取使用者網域中的帳號遠端存取屬性。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|RDS 端點伺服器 (Windows Server 2012)|建容器<br /><br />網域本機安全性群組|此群組中的伺服器執行虛擬電腦及主機活動執行使用者 RemoteApp 程式和個人 virtual 桌面。 此群組需要會填入執行 RD 連接代理人伺服器上。 RD 工作階段主機伺服器和部署中所使用的 RD 模擬主機伺服器需要將這個群組中。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|RDS 管理伺服器 (Windows Server 2012)|建容器<br /><br />網域本機安全性群組|此群組中的伺服器可以執行一般管理動作執行遠端桌面服務的伺服器上。 此群組需要填入在遠端桌面服務部署所有伺服器上。 必須在此群組包含執行 RDS 中央管理服務的伺服器。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|RDS 遠端存取伺服器 (Windows Server 2012)|建容器<br /><br />網域本機安全性群組|此群組中的伺服器讓 RemoteApp 程式與這些資源個人 virtual 桌面存取的使用者。 在 [網際網路的部署，通常會在 edge 網路部署這些伺服器。 此群組需要會填入執行 RD 連接代理人伺服器上。 伺服器 RD 閘道和部署中所使用的 RD 網路存取伺服器需要將這個群組中。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|唯讀模式網域控制站|使用者容器<br /><br />安全性的全域群組|此群組包含所有的唯讀的網域控制站網域中。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|遠端桌面服務使用者|建容器<br /><br />網域本機安全性群組|從遠端使用 RDP 登入的權限授與此群組成員。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|管理遠端伺服器 (Windows Server 2012)|建容器<br /><br />網域本機安全性群組|此群組成員可以存取 WMI 資源透過管理通訊協定 （例如 Ws-management 透過 Windows 遠端管理服務)。 僅適用於的權限授與對使用者 WMI 命名空間。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|複製者|建容器<br /><br />網域本機安全性群組|支援網域中的舊版檔案複製。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|架構系統管理員 （存在只能在森林根網域中）|使用者容器<br /><br />萬用安全性群組|架構系統管理員的使用者，就可以進行修改 Active Directory 結構描述與才架構是寫入。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|伺服器電信業者|建容器<br /><br />網域本機安全性群組|此群組成員可以管理網域伺服器。<br /><br />**直接使用者權限：**<br /><br />在本機允許登入<br /><br />備份的檔案和目錄<br /><br />變更系統時間<br /><br />變更時區<br /><br />從遠端系統推動關機<br /><br />還原的檔案和目錄<br /><br />關機<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|終端 Server 授權伺服器|建容器<br /><br />網域本機安全性群組|此群組成員可以更新帳號在 Active Directory 授權發行，為了追蹤及報告 TS 每個使用者 CAL 使用方式的相關資訊<br /><br />**預設直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|使用者|建容器<br /><br />網域本機安全性群組|使用者必須允許讀取許多物件和屬性在 Active Directory，雖然他們無法變更大部分的權限。 使用者防止誤或故意的系統變更，且可以執行大部分的應用程式。<br /><br />**直接使用者權限：**<br /><br />增加程序運作設定<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查|  
|Windows 授權的存取群組|建容器<br /><br />網域本機安全性群組|此群組成員存取計算的 tokenGroupsGlobalAndUniversal 屬性對使用者物件<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
|WinRMRemoteWMIUsers_ (Windows Server 2012)|使用者容器<br /><br />網域本機安全性群組|此群組成員可以存取 WMI 資源透過管理通訊協定 （例如 Ws-management 透過 Windows 遠端管理服務)。 僅適用於的權限授與對使用者 WMI 命名空間。<br /><br />**直接使用者權限：**無<br /><br />**繼承的使用者權限：**<br /><br />從網路存取此電腦<br /><br />加入網域工作站<br /><br />略過周遊檢查<br /><br />增加程序運作設定|  
