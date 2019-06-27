---
ms.assetid: 79b9c912-ea3e-4679-ab41-893e096c4d09
title: 附錄 B-特殊權限帳戶和 Active Directory 中群組
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d6a4bfa6d59bd8049d39106a4cbd4f8a4dee8ba8
ms.sourcegitcommit: 545dcfc23a81943e129565d0ad188263092d85f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2019
ms.locfileid: "67407679"
---
# <a name="appendix-b-privileged-accounts-and-groups-in-active-directory"></a>附錄 B：Active Directory 中具有特殊權限的帳戶和群組

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012


## <a name="appendix-b-privileged-accounts-and-groups-in-active-directory"></a>附錄 B：Active Directory 中具有特殊權限的帳戶和群組  
「 特許 」 的帳戶和 Active Directory 中的群組是功能強大的權限、 權限，以及權限會授予，讓他們在 Active Directory 中和已加入網域的系統上，執行幾乎任何動作。 本附錄一開始會討論權限，權限和權限，後面的 「 最高的權限 」 的帳戶和 Active Directory 中的群組的相關資訊也就是最強大的帳戶和群組。  

也提供關於內建和預設的帳戶和 Active Directory 中的群組，除了其權限的資訊。 雖然是以個別的附錄提供保護的最高的權限的帳戶和群組的特定組態建議，本附錄提供背景資訊可協助您識別的使用者和群組，您應該專注於保護. 因為可以利用攻擊者危害，並甚至摧毀您的 Active Directory 安裝，您應該這麼做。  

### <a name="rights-privileges-and-permissions-in-active-directory"></a>權限、 權限，以及在 Active Directory 中的權限  
權限、 權限和權限之間的差異可以是既容易混淆又矛盾，甚至在 microsoft 的文件。 使用本文件中，本節會描述一些每個特性。 這些描述不應視為授權的其他 Microsoft 文件，因為它可能會以不同的方式使用這些詞彙。  

#### <a name="rights-and-privileges"></a>權利和權限  
實際上是相同的全系統的功能，例如使用者、 服務、 電腦或群組的安全性主體授與的權利和權限。 在通常由 IT 專業人員所使用的介面，這些通常稱為 「 權限 > 或 < 使用者權限 >，它們通常會指派由群組原則物件。 下列螢幕擷取畫面顯示一些最常見的使用者權限，可以指派給安全性主體 （它代表在 Windows Server 2012 網域中預設網域控制站 GPO）。 部分這些權限將套用到 Active Directory 中，例如**讓電腦和使用者帳戶，以受信任可以委派**使用者權限，而其他權限將套用到 Windows 作業系統，例如**變更的系統時間**。  

![特殊權限的帳戶和群組](media/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory/SAD_8.gif)  

例如 「 群組原則物件編輯器 」 中的介面，在所有這些可指派的功能稱為廣泛的使用者權限。 實際上不過，某些使用者權限以程式設計的方式稱為權限，而其他人以程式設計的方式就是權限。 資料表 B-1:使用者權利和權限提供的一些最常見的可指派的使用者權限和相關的程式設計常數。 雖然群組原則和其他介面，請參閱所有的這些使用者權限，部分以程式設計方式識別為權限，而有些則會定義為權限。  

如需每個使用者的權限下表所列的詳細資訊，請使用資料表中的連結，或參閱[威脅與對策指南：使用者權限](https://technet.microsoft.com/library/hh125917(v=ws.10).aspx)中[安全威脅與弱點防護](https://technet.microsoft.com/library/cc755181(v=ws.10).aspx)Microsoft TechNet 網站上的 Windows Server 2008 R2 的指南。 適用於 Windows Server 2008 的資訊，請參閱[使用者權限](https://technet.microsoft.com/library/dd349804(v=WS.10).aspx)中[安全威脅與弱點防護](https://technet.microsoft.com/library/cc755181(v=ws.10).aspx)Microsoft TechNet 網站上的文件。 在撰寫本文件，適用於 Windows Server 2012 的對應文件尚未發佈。  

> [!NOTE]  
> 基於本文的目的，條款 」 權限 > 和 < 使用者權限 > 會用來識別權限和權限，除非另有指定。  

##### <a name="table-b-1-user-rights-and-privileges"></a>資料表 B-1:使用者權利和權限  

|||  
|-|-|  
|**群組原則中的權限的使用者**|**常數的名稱**|  
|[為受信任的呼叫者的存取認證管理員](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_2)|SeTrustedCredManAccessPrivilege|  
|[從網路存取這台電腦](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_1)|SeNetworkLogonRight|  
|[做為作業系統的一部分](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_3)|SeTcbPrivilege|  
|[新增工作站到網域](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_4)|SeMachineAccountPrivilege|  
|[調整處理序的記憶體配額](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_5)|SeIncreaseQuotaPrivilege|  
|[允許本機登入](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_6)|SeInteractiveLogonRight|  
|[允許透過終端機服務登入](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_7)|SeRemoteInteractiveLogonRight|  
|[備份檔案和目錄](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_8)|SeBackupPrivilege|  
|[略過周遊檢查](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_9)|SeChangeNotifyPrivilege|  
|[變更系統時間](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_10)|SeSystemtimePrivilege|  
|[變更時區](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_11)|SeTimeZonePrivilege|  
|[建立分頁檔](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_12)|SeCreatePagefilePrivilege|  
|[建立權杖的物件](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_13)|SeCreateTokenPrivilege|  
|[建立全域物件](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_14)|SeCreateGlobalPrivilege|  
|[建立永久共用的物件](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_15)|SeCreatePermanentPrivilege|  
|[建立符號連結](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_16)|SeCreateSymbolicLinkPrivilege|  
|[偵錯程式](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_17)|SeDebugPrivilege|  
|[拒絕從網路存取這台電腦](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_18)|SeDenyNetworkLogonRight|  
|[拒絕以批次工作登入](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_18a)|SeDenyBatchLogonRight|  
|[拒絕以服務登入](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_19)|SeDenyServiceLogonRight|  
|[拒絕本機登入](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_20)|SeDenyInteractiveLogonRight|  
|[拒絕透過終端機服務登入](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_21)|SeDenyRemoteInteractiveLogonRight|  
|[讓電腦和使用者帳戶，以受信任可以委派](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_22)|SeEnableDelegationPrivilege|  
|[從遠端系統強制關機](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_23)|SeRemoteShutdownPrivilege|  
|[產生安全性稽核](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_24)|SeAuditPrivilege|  
|[在驗證後模擬用戶端](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_25)|SeImpersonatePrivilege|  
|[增加處理序工作集](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_26)|SeIncreaseWorkingSetPrivilege|  
|[增加排程優先順序](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_27)|SeIncreaseBasePriorityPrivilege|  
|[載入和卸載的裝置驅動程式](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_28)|SeLoadDriverPrivilege|  
|[鎖定記憶體分頁](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_29)|SeLockMemoryPrivilege|  
|[以批次工作登入](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_30)|SeBatchLogonRight|  
|[以服務方式登](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_31)|SeServiceLogonRight|  
|[管理稽核及安全性記錄檔](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_32)|SeSecurityPrivilege|  
|[修改物件標籤](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_33)|SeRelabelPrivilege|  
|[修改韌體環境值](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_34)|SeSystemEnvironmentPrivilege|  
|[執行磁碟區維護工作](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_35)|SeManageVolumePrivilege|  
|[設定檔的單一處理程序](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_36)|SeProfileSingleProcessPrivilege|  
|[監視系統效能](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_37)|SeSystemProfilePrivilege|  
|[從銜接站移除電腦](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_38)|SeUndockPrivilege|  
|[取代處理程序等級權杖](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_39)|SeAssignPrimaryTokenPrivilege|  
|[還原檔案和目錄](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_40)|SeRestorePrivilege|  
|[將系統關機](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_41)|SeShutdownPrivilege|  
|[同步處理目錄服務的資料](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_42)|SeSyncAgentPrivilege|  
|[取得檔案或其他物件的擁有權](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_43)|SeTakeOwnershipPrivilege|  

#### <a name="permissions"></a>Permissions  
權限會套用至 安全性實體的物件，例如檔案系統、 登錄、 服務和 Active Directory 物件的存取控制。 每個安全性實體的物件有相關聯的存取控制清單 (ACL)，其中包含存取控制項目 (Ace)，授與或拒絕安全性主體 （使用者、 服務、 電腦或群組） 的物件上執行各種作業的能力。 例如，Active Directory 中的許多物件的 Acl 包含 Ace 允許已驗證的使用者，若要讀取之物件中，相關的一般資訊，但不會授與它們能夠讀取的敏感資訊或變更的物件。
除了內建 Guest 帳戶每個網域，每個安全性主體的登入，而由 Active Directory 樹系或受信任的樹系的網域控制站驗證已驗證的使用者安全性識別碼 (SID) 新增至其存取權預設的語彙基元。 因此，使用者、 服務或電腦帳戶嘗試讀取網域中的使用者物件上的一般屬性，讀取的作業成功。  

定義安全性主體嘗試存取物件的任何 ace，且包含主體的存取權杖中存在的 SID，如果主體無法存取的物件。 此外，如果符合使用者的存取權杖，「 拒絕 」 ACE 的 SID 的 ACE 的物件 ACL 中包含的 deny 項目將通常覆寫衝突的 「 允許 」 ACE。 如需有關在 Windows 中的存取控制的詳細資訊，請參閱[存取控制](https://msdn.microsoft.com/library/aa374860(v=VS.85).aspx)MSDN 網站上。  

本文件中，權限是指授與或拒絕安全性實體物件上的安全性主體的功能。 使用者權限與權限之間的衝突時，使用者的權限通常會優先。 比方說，如果 Active Directory 中的物件已設有拒絕系統管理員，所有的讀取和寫入物件的存取權的 ACL，身為網域的系統管理員群組成員的使用者將無法檢視之物件相關資訊。 不過，因為系統管理員群組授與使用者權限 」 取得擁有權的檔案或其他物件 」，使用者可以直接取得之物件的擁有權，然後重寫物件的 ACL，以系統管理員之物件的完整控制權授與。  

它是基於這個理由，這份文件鼓勵您避免使用功能強大的帳戶和群組對於日常管理工作，而不是想要限制的帳戶和群組的功能。 不是有效可停止下定決心的使用者存取的人員強大的認證無法使用這些認證來存取任何安全性實體的資源。  

### <a name="built-in-privileged-accounts-and-groups"></a>內建特殊權限帳戶和群組  
Active Directory 被要協助管理委派並在指派權利與權限的最低權限原則。 在 Active Directory 網域中擁有帳戶的 「 一般 」 使用者是，根據預設，能夠讀取大多在目錄中，儲存的內容，但能夠變更只有非常有限的目錄中的資料。 需要額外的權限的使用者可以授與內建目錄，讓它們可能會執行特定工作相關的角色，但無法執行其職責無關的工作的各種權限群組中的成員資格。  

Active Directory，內有包含目錄中最高的權限群組的三個內建群組： Enterprise Admins (EA) 群組、 網域系統管理員 (DA) 群組，以及內建的系統管理員 (BA) 群組。  

第四個群組，[結構描述系統管理員 (SA)] 群組中的權限，如果遭到濫用，可能會損壞或摧毀整個 Active Directory 樹系，但此群組是多個限制中它的功能比 EA、 DA 和 BA 群組。  

除了這些四個群組中，還有許多其他的內建和預設帳戶和 Active Directory，其中每一個會授與權限可讓特定的系統管理工作，以執行中的群組。 雖然本附錄中未提供 Active Directory 中的每個內建或預設群組的完整討論，但是它提供您最可能會看到在您安裝的帳戶與群組的資料表。  

比方說，如果您將 Microsoft Exchange Server 安裝到 Active Directory 樹系時，其他帳戶和群組可能會建立在您網域中的內建和使用者的容器。 本附錄說明只有群組和原生的角色和功能為基礎的 Active Directory 中的內建和使用者的容器中建立的帳戶。 帳戶和群組所建立的企業軟體的安裝不包含在內。  

#### <a name="enterprise-admins"></a>Enterprise Admins  
Enterprise Admins (EA) 群組位於樹系根網域中，並根據預設，它是樹系中每個網域內建的 Administrators 群組的成員。 樹系根網域中的內建的 Administrator 帳戶是唯一的預設成員的 EA 群組。 EAs 會授與權限，讓他們能夠影響整個樹系的變更。 這些是會影響所有的網域樹系，例如加入或移除網域、 建立樹系信任或提高樹系功能等級中的變更。 正確設計及實作委派模型，在 EA 成員資格是必要的或只在第一次建構樹系時，才進行特定的全樹系的變更，例如建立輸出樹系信任時。  

EA 群組位於預設會在樹系根網域中的 [使用者] 容器，它是通用安全性群組，除非樹系根網域以 Windows 2000 Server 混合模式執行，則群組這個是通用安全性群組。 雖然某些權限會授與直接 EA 群組，此群組的權限的許多實際的繼承 EA 群組因為它是樹系中的每個網域中的 Administrators 群組的成員。 企業系統管理員在工作站或成員伺服器上有任何預設權限。  

#### <a name="domain-admins"></a>Domain Admins  
每個樹系中的網域有它自己的網域系統管理員 (DA) 群組，也就是每個已加入網域的電腦上本機 Administrators 群組的成員除了該網域內建系統管理員 (BA) 群組的成員。 網域的資料群組的唯一預設成員是該網域內建的 Administrator 帳戶。  

DAs 是全能自己網域中，內部的而 EAs 具有全樹系的權限。 在適當地設計和實作委派模型中，DA 成員資格應該只在 「 急用 」 案例中，也就是需要該資料行具有高的層級的權限的帳戶，在網域中的每一部電腦上，或是特定的情況下需要全網域必須進行變更。 雖然原生 Active Directory 委派機制並允許委派，就可以使用資料的帳戶只有在緊急情況下，建構有效的委派模型可能非常耗時，而且許多組織會使用第三方若要加速此程序的應用程式。  

資料群組是位於網域的 Users 容器中的全域安全性群組。 針對每個網域、 樹系的一個資料群組和資料群組的唯一預設成員是網域的內建的 Administrator 帳戶。 因為網域的資料群組巢狀方式置於網域的 BA 群組和每個已加入網域的系統的本機 Administrators 群組、 DAs 不僅為 Domain Admins，特別授與的權限，但它們也會繼承所有的權限和權限授與網域的系統管理員群組和本機 Administrators 群組已加入網域的所有系統上。  

#### <a name="administrators"></a>Administrators  
內建的系統管理員 (BA) 群組是網域本機群組所在 DAs 和 EAs 為巢狀，而且被授與許多直接的權限和權限在目錄和網域控制站上的此群組的網域內建容器中。 不過，網域系統管理員群組沒有任何權限的成員伺服器或工作站上。 已加入網域的電腦的本機 Administrators 群組的成員資格為其中本機權限會授與;因此討論群組，只有 DAs 預設所有已加入網域的電腦的本機 Administrators 群組的成員。  

系統管理員群組是網域的內建容器中的網域本機群組。 根據預設，每個網域 BA 群組包含本機網域的內建的 Administrator 帳戶、 本機網域的資料群組和樹系根網域的 EA 群組。 在 Active Directory 和網域控制站上許多的使用者權限會授與專為系統管理員群組，不能 EAs 或 DAs。 網域的 BA 群組授與大部分的目錄物件的完全控制權限，而且可以取得的目錄物件的擁有權。 EA 和 DA 群組授與特定樹系和網域中的特定物件的權限，雖然大部分的群組是實際上 「 繼承 」 從 BA 群組中的成員資格。  

> [!NOTE]  
> 雖然這些都是這些特殊權限群組的預設組態，其中的三個群組的成員可以操作的目錄，以取得在任何其他群組的成員資格。 在某些情況下，它是 trivial 達到，而在其他使用案例就很難，但從可能的權限的觀點來看，所有三個群組應視為有效等同項目。  

#### <a name="schema-admins"></a>Schema Admins  
結構描述系統管理員 (SA) 群組是一個樹系根網域中的萬用群組，並做為預設成員，類似的 EA 群組擁有該網域內建的 Administrator 帳戶。 雖然 SA 群組的成員資格可以讓攻擊者危害 Active Directory 架構，也就是整個 Active Directory 樹系的架構，SAs 會有一些預設的權限和結構描述以外的權限。  

您應該仔細管理及監視 [SA] 群組中的成員資格，但在某些方面，此群組是 「 較低權限 」 比最高的三個特殊權限群組，因為其權限範圍較窄; 稍早所述也就是說，SAs 有無結構描述以外的任何位置的系統管理權限。  

#### <a name="additional-built-in-and-default-groups-in-active-directory"></a>其他內建和 Active Directory 中的預設群組  
為了方便委派管理目錄中的，Active Directory 隨附的特定權限和權限已授與的各種內建和預設群組。 下表會簡短說明這些群組。  

下表列出 Active Directory 中的內建和預設群組。 這兩組群組存在的預設值;不過，內建群組位於 （依預設） 的內建的容器中 Active Directory 中，雖然預設群組位於 Active Directory 中的 [使用者] 容器中時 （依預設）。 內建的容器中的群組會是所有的網域本機群組，而 [使用者] 容器中的群組是網域本機、 全域、 和萬用群組，除了三個個別的使用者帳戶 （Administrator、 Guest 以及 Krbtgt） 的混合。  

本附錄稍早所述的最高特殊權限群組，除了一些內建和預設的帳戶和群組會授與提高的權限應該也是受保護及使用只在安全的系統管理主機上。 這些群組與帳戶可在資料表 B-1 陰影資料列中：內建和預設群組和 Active Directory 中的帳戶。 其中一些群組和帳戶會授與權限，可能會誤用來危害到 Active Directory 或網域控制站，因為它們就會獲得額外的保護，如中所述[附錄 c:受保護的帳戶和 Active Directory 中的群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  

##### <a name="table-b-1-built-in-and-default-accounts-and-groups-in-active-directory"></a>資料表 B-1:內建的預設帳戶和 Active Directory 中的群組  

||||  
|-|-|-|  
|**帳戶或群組**|**預設容器，群組範圍和類型**|**描述和預設的使用者權限**|  
|存取控制協助運算子 (在 Windows Server 2012 中的 Active Directory)|內建的容器<br /><br />網域本機安全性群組|授權屬性和資源，在此電腦上的權限，此群組的成員可以從遠端查詢。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Account Operators|內建的容器<br /><br />網域本機安全性群組|成員可以管理網域使用者和群組帳戶。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Administrator 帳戶|Users 容器<br /><br />不是群組|管理網域的內建帳戶。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />調整處理程序的記憶體配額<br /><br />允許本機登入<br /><br />允許透過遠端桌面服務登入<br /><br />備份檔案及目錄<br /><br />略過周遊檢查<br /><br />變更系統時間<br /><br />變更時區<br /><br />建立分頁檔<br /><br />建立通用物件<br /><br />建立符號連結<br /><br />偵錯程式<br /><br />讓電腦及使用者帳戶受信賴，以進行委派<br /><br />強制從遠端系統進行關閉<br /><br />在驗證後模擬用戶端<br /><br />增加處理程序工作組<br /><br />增加排程優先順序<br /><br />載入及解除載入裝置驅動程式<br /><br />以批次工作登入<br /><br />管理稽核及安全性記錄檔<br /><br />修改韌體環境值<br /><br />執行磁碟區維護工作<br /><br />監視單一處理程序<br /><br />監視系統效能<br /><br />從擴充座移除電腦<br /><br />還原檔案及目錄<br /><br />關閉系統<br /><br />取得檔案或其他物件的擁有權|  
|系統管理員群組|內建的容器<br /><br />網域本機安全性群組|系統管理員在網域中需要完整且不受限制的存取。<br /><br />**直接的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />調整處理程序的記憶體配額<br /><br />允許本機登入<br /><br />允許透過遠端桌面服務登入<br /><br />備份檔案及目錄<br /><br />略過周遊檢查<br /><br />變更系統時間<br /><br />變更時區<br /><br />建立分頁檔<br /><br />建立通用物件<br /><br />建立符號連結<br /><br />偵錯程式<br /><br />讓電腦及使用者帳戶受信賴，以進行委派<br /><br />強制從遠端系統進行關閉<br /><br />在驗證後模擬用戶端<br /><br />增加排程優先順序<br /><br />載入及解除載入裝置驅動程式<br /><br />以批次工作登入<br /><br />管理稽核及安全性記錄檔<br /><br />修改韌體環境值<br /><br />執行磁碟區維護工作<br /><br />監視單一處理程序<br /><br />監視系統效能<br /><br />從擴充座移除電腦<br /><br />還原檔案及目錄<br /><br />關閉系統<br /><br />取得檔案或其他物件的擁有權<br /><br />繼承的使用者權限：<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|允許的 RODC 密碼複寫群組|Users 容器<br /><br />網域本機安全性群組|在此群組的成員可以有其密碼複寫至網域中的所有唯讀網域控制站。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Backup Operators|內建的容器<br /><br />網域本機安全性群組|備份操作員可以覆寫安全性限制，唯一的目的是要備份或還原檔案。<br /><br />**直接的使用者權限：**<br /><br />允許本機登入<br /><br />備份檔案及目錄<br /><br />以批次工作登入<br /><br />還原檔案及目錄<br /><br />關閉系統<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Cert Publishers|Users 容器<br /><br />網域本機安全性群組|此群組的成員可以將憑證發行至目錄。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|憑證服務 DCOM 存取|內建的容器<br /><br />網域本機安全性群組|如果 （不建議） 在網域控制站上安裝憑證服務，此群組授與 DCOM 註冊存取權網域使用者和網域電腦。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|可複製的網域控制站 (在 Windows Server 2012AD DS 的 AD DS)|Users 容器<br /><br />全域安全性群組|此群組的成員，都可能複製網域控制站。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Cryptographic Operators|內建的容器<br /><br />網域本機安全性群組|成員有權執行密碼編譯作業。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|偵錯工具的使用者|這不是預設值或內建群組，但時存在於 AD DS，以便進一步調查原因。|偵錯工具的使用者群組的目前狀態指出，偵錯工具已安裝在某個時間點，在系統上是否透過 Visual Studio、 SQL、 Office 或其他應用程式的需要和支援偵錯環境。 此群組可讓遠端偵錯電腦的存取權。 當此群組存在網域層級時，它會指出的偵錯工具或應用程式，其中包含偵錯工具已經安裝在網域控制站。|  
|拒絕的 RODC 密碼複寫群組|Users 容器<br /><br />網域本機安全性群組|這個群組的成員不能有其密碼複寫至網域中任何唯讀網域控制站。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|DHCP 系統管理員|Users 容器<br /><br />網域本機安全性群組|此群組的成員具有對 DHCP 伺服器服務的系統管理權限。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|DHCP Users|Users 容器<br /><br />網域本機安全性群組|此群組的成員具有僅限檢視存取 DHCP 伺服器服務。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Distributed COM Users|內建的容器<br /><br />網域本機安全性群組|此群組的成員可以啟動、 啟動及使用這部電腦上的 分散式的 COM 物件。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|DnsAdmins|Users 容器<br /><br />網域本機安全性群組|此群組的成員具有對 DNS 伺服器服務的系統管理權限。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|DnsUpdateProxy|Users 容器<br /><br />全域安全性群組|此群組的成員會獲准執行代表本身無法執行動態更新的用戶端的動態更新 DNS 用戶端。 此群組的成員通常是 DHCP 伺服器。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Domain Admins|Users 容器<br /><br />全域安全性群組|指定的系統管理員的網域，網域系統管理員 」 是每個已加入網域的電腦的本機 Administrators 群組的成員，以及接收權限授與權限到本機的 Administrators 群組，除了網域的系統管理員群組。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />調整處理程序的記憶體配額<br /><br />允許本機登入<br /><br />允許透過遠端桌面服務登入<br /><br />備份檔案及目錄<br /><br />略過周遊檢查<br /><br />變更系統時間<br /><br />變更時區<br /><br />建立分頁檔<br /><br />建立通用物件<br /><br />建立符號連結<br /><br />偵錯程式<br /><br />讓電腦及使用者帳戶受信賴，以進行委派<br /><br />強制從遠端系統進行關閉<br /><br />在驗證後模擬用戶端<br /><br />增加處理程序工作組<br /><br />增加排程優先順序<br /><br />載入及解除載入裝置驅動程式<br /><br />以批次工作登入<br /><br />管理稽核及安全性記錄檔<br /><br />修改韌體環境值<br /><br />執行磁碟區維護工作<br /><br />監視單一處理程序<br /><br />監視系統效能<br /><br />從擴充座移除電腦<br /><br />還原檔案及目錄<br /><br />關閉系統<br /><br />取得檔案或其他物件的擁有權|  
|網域的電腦|Users 容器<br /><br />全域安全性群組|所有的工作站和已加入網域的伺服器是此群組的預設成員。<br /><br />**預設值直接使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|網域控制站|Users 容器<br /><br />全域安全性群組|在網域中的所有網域控制站。 注意:網域控制站不是網域的電腦群組的成員。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Domain Guests|Users 容器<br /><br />全域安全性群組|在網域中的所有來賓<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|網域使用者|Users 容器<br /><br />全域安全性群組|網域中的所有使用者<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Enterprise Admins （只存在於樹系根網域）|Users 容器<br /><br />萬用安全性群組|企業系統管理員有權限變更全樹系的組態設定;企業系統管理員 」 是每個網域系統管理員群組的成員，以及接收權利與權限授與該群組。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />調整處理程序的記憶體配額<br /><br />允許本機登入<br /><br />允許透過遠端桌面服務登入<br /><br />備份檔案及目錄<br /><br />略過周遊檢查<br /><br />變更系統時間<br /><br />變更時區<br /><br />建立分頁檔<br /><br />建立通用物件<br /><br />建立符號連結<br /><br />偵錯程式<br /><br />讓電腦及使用者帳戶受信賴，以進行委派<br /><br />強制從遠端系統進行關閉<br /><br />在驗證後模擬用戶端<br /><br />增加處理程序工作組<br /><br />增加排程優先順序<br /><br />載入及解除載入裝置驅動程式<br /><br />以批次工作登入<br /><br />管理稽核及安全性記錄檔<br /><br />修改韌體環境值<br /><br />執行磁碟區維護工作<br /><br />監視單一處理程序<br /><br />監視系統效能<br /><br />從擴充座移除電腦<br /><br />還原檔案及目錄<br /><br />關閉系統<br /><br />取得檔案或其他物件的擁有權|  
|企業唯讀網域控制站|Users 容器<br /><br />萬用安全性群組|此群組包含樹系中的所有唯讀網域控制站的帳戶。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Event Log Readers|內建的容器<br /><br />網域本機安全性群組|在此群組的成員可以讀取事件記錄檔，在網域控制站上。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Group Policy Creator Owners|Users 容器<br /><br />全域安全性群組|此群組的成員可以建立和修改網域中的群組原則物件。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Guest|Users 容器<br /><br />不是群組|這是唯一的帳戶在 AD DS 網域，並沒有已驗證的使用者 SID，而此一加入至其的存取權杖。 因此，任何已授與存取權的已驗證的使用者群組的資源將無法存取此帳戶。 這個行為並不允許來賓群組與 Domain Guests 的成員，則為 true，不過-這些群組的成員可以通過驗證的使用者 SID，而此一加入他們的存取權杖。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Guests|內建的容器<br /><br />網域本機安全性群組|遊客有相同的存取權，為使用者群組的成員依預設，除了來賓帳戶，其中會進一步限制如先前所述。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|HYPER-V 系統管理員 (Windows Server 2012)|內建的容器<br /><br />網域本機安全性群組|此群組的成員具有 HYPER-V 的所有功能完整且不受限制的存取權。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|IIS_IUSRS|內建的容器<br /><br />網域本機安全性群組|Internet Information Services 所使用的內建群組。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|連入 Forest Trust Builders （只存在於樹系根網域）|內建的容器<br /><br />網域本機安全性群組|此群組的成員可以建立到此樹系的內送、 單向信任。 （適用於企業系統管理員會保留建立輸出的樹系信任）。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Krbtgt|Users 容器<br /><br />不是群組|Krbtgt 帳戶是網域中的 Kerberos 金鑰發佈中心服務帳戶。 此帳戶可以存取儲存在 Active Directory 中的所有帳戶的認證。 此帳戶預設為停用，而且絕不應該啟用<br /><br />**使用者權限：** N/A|  
|Network Configuration Operators|內建的容器<br /><br />網域本機安全性群組|此群組的成員會授與權限，讓他們能夠管理的網路功能的設定。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Performance Log Users|內建的容器<br /><br />網域本機安全性群組|此群組的成員可以的效能計數器排程記錄，啟用追蹤提供者，並在本機和遠端存取透過收集事件追蹤的電腦。<br /><br />**直接的使用者權限：**<br /><br />以批次工作登入<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Performance Monitor Users|內建的容器<br /><br />網域本機安全性群組|此群組的成員可以在本機和遠端存取效能計數器資料。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Pre-Windows 2000 Compatible Access|內建的容器<br /><br />網域本機安全性群組|此群組存在，可回溯相容於 Windows 2000 Server 之前, 的作業系統，並提供成員，才能讀取使用者與網域中的群組資訊的功能。<br /><br />**直接的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />略過周遊檢查<br /><br />**繼承的使用者權限：**<br /><br />將工作站新增至網域<br /><br />增加處理程序工作組|  
|Print Operators|內建的容器<br /><br />網域本機安全性群組|此群組的成員可以管理網域的印表機。<br /><br />**直接的使用者權限：**<br /><br />允許本機登入<br /><br />載入及解除載入裝置驅動程式<br /><br />關閉系統<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|RAS 與 IAS 伺服器|Users 容器<br /><br />網域本機安全性群組|此群組中的伺服器都可以讀取網域中的使用者帳戶上的遠端存取內容。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|RDS 結束點的伺服器 (Windows Server 2012)|內建的容器<br /><br />網域本機安全性群組|此群組中的伺服器執行的虛擬機器和主機工作階段，執行使用者 RemoteApp 程式與個人虛擬桌面。 此群組必須填入執行 RD 連線代理人伺服器上。 RD 工作階段主機伺服器和部署中所使用的 rd 工作階段主機伺服器必須是此群組中。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|RDS 的管理伺服器 (Windows Server 2012)|內建的容器<br /><br />網域本機安全性群組|此群組中的伺服器可以執行遠端桌面服務的伺服器上執行例行的系統管理動作。 此群組必須在遠端桌面服務部署中的所有伺服器上填入。 執行中央 RDS 的管理服務的伺服器必須包含在此群組。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|RDS 的遠端存取伺服器 (Windows Server 2012)|內建的容器<br /><br />網域本機安全性群組|此群組中的伺服器啟用的 RemoteApp 程式與個人虛擬桌面存取這些資源的使用者。 在網際網路對向部署中，這些伺服器通常會部署在邊緣網路中。 此群組必須填入執行 RD 連線代理人伺服器上。 RD 閘道伺服器和部署中所使用的 RD Web 存取伺服器必須是此群組中。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Read-only Domain Controllers|Users 容器<br /><br />全域安全性群組|此群組包含在網域中的所有唯讀網域控制站。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Remote Desktop Users|內建的容器<br /><br />網域本機安全性群組|從遠端使用 RDP 登入的權限會授與此群組的成員。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Remote Management Users (Windows Server 2012)|內建的容器<br /><br />網域本機安全性群組|此群組的成員可以存取 WMI 資源，透過管理通訊協定 （例如透過 Windows 遠端管理服務的 WS 管理）。 只適用於存取權授與使用者的 WMI 命名空間。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Replicator|內建的容器<br /><br />網域本機安全性群組|支援舊版的檔案複寫在網域中。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|（只存在於樹系根網域） 的結構描述系統管理員|Users 容器<br /><br />萬用安全性群組|結構描述管理員是唯一可以進行修改 Active Directory 架構，而且只有當結構描述的使用者已啟用寫入。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|Server Operators|內建的容器<br /><br />網域本機安全性群組|此群組的成員可以管理網域的伺服器。<br /><br />**直接的使用者權限：**<br /><br />允許本機登入<br /><br />備份檔案及目錄<br /><br />變更系統時間<br /><br />變更時區<br /><br />強制從遠端系統進行關閉<br /><br />還原檔案及目錄<br /><br />關閉系統<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|終端機伺服器授權伺服器|內建的容器<br /><br />網域本機安全性群組|此群組的成員可以更新 Active Directory 中的使用者帳戶授權發行，用於追蹤和報告 TS 每個使用者 CAL 使用量的相關資訊<br /><br />**預設值直接使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|使用者|內建的容器<br /><br />網域本機安全性群組|使用者有權限，讓他們能夠讀取許多物件和屬性在 Active Directory，雖然它們不能變更大部分。 使用者無法進行無意或有意的全系統變更，且可以執行大部分的應用程式。<br /><br />**直接的使用者權限：**<br /><br />增加處理程序工作組<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查|  
|Windows Authorization Access Group|內建的容器<br /><br />網域本機安全性群組|此群組的成員會對使用者物件中的計算的 tokenGroupsGlobalAndUniversal 屬性的存取權<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
|WinRMRemoteWMIUsers_ (Windows Server 2012)|Users 容器<br /><br />網域本機安全性群組|此群組的成員可以存取 WMI 資源，透過管理通訊協定 （例如透過 Windows 遠端管理服務的 WS 管理）。 只適用於存取權授與使用者的 WMI 命名空間。<br /><br />**直接的使用者權限：** None<br /><br />**繼承的使用者權限：**<br /><br />從網路存取這台電腦<br /><br />將工作站新增至網域<br /><br />略過周遊檢查<br /><br />增加處理程序工作組|  
