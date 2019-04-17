---
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: "減少 Active Directory 攻擊"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2de254076b10a1a75d658f006c2245d523de6b7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="reducing-the-active-directory-attack-surface"></a>減少 Active Directory 攻擊

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本章節焦某技術控制實作減少 Active Directory 安裝的攻擊。 一節包含下列資訊：  
  
-   [實作最低權限管理機型](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)重點在於提供使用的高特殊權限帳號日常的系統管理，除了提供建議實施減少帳號有特殊權限的風險。  
  
-   [實作安全管理主機](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)的方法安全管理主機部署部署專用、安全管理系統，除了一些範例描述原則。  
  
-   [保護針對攻擊網域控制站](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)討論原則和設定，類似的建議的實作系統主機安全，但包含來協助確保您的網域控制站和可用來管理他們的系統都是安全的一些網域控制站的特定建議。  
  
## <a name="privileged-accounts-and-groups-in-active-directory"></a>有特殊權限的帳號，並 Active Directory 中的群組  
本章節提供權限帳號的背景資訊，Active Directory 中的群組適合解釋共通性及特殊權限的帳號與不同群組 Active Directory 中。 了解這些區別後，是否您執行的建議在[實作最低權限管理型號](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)逐字或選擇以自訂您的組織，您有安全每個群組，並考慮適當所需的工具。  
  
### <a name="built-in-privileged-accounts-and-groups"></a>建帳號及群組特殊權限  
Active Directory 加速的管理委派，並且支援原則權限指派權利與權限。 [一般] 使用者網域中有帳號，根據預設，可以朗讀的多的項目儲存在 directory，但無法變更只非常有限的資料 directory。 使用者需要額外的權限授與到 directory 建置，所以它們可能會執行他們的角色的相關特定的工作，但無法執行工作，不會與他們責任各種」有特殊權限」群組成員資格。 組織也可以建立的量身打造，以特定工作責任和細微的權利和執行日常的管理功能，而不會授與的權利和超過功能這些功能的權限 IT 人員的權限授與的群組。  
  
在 Active Directory 中建的三個群組的最高的權限群組中 directory：企業系統管理員，網域系統管理員及系統管理員。 預設設定，並且每個群組的功能下列各節所述：  
  
#### <a name="highest-privilege-groups-in-active-directory"></a>在 Active Directory 中的最高的權限群組  
  
##### <a name="enterprise-admins"></a>企業系統管理員  
企業系統管理員 (EA) 是群組的只存在於森林根網域中，而且預設森林中的所有網域中的系統管理員群組成員。 森林根網域中的建是只預設 EA 群組成員。 EAs 會授與權限，讓他們實作樹系的變更（也就是，變更會影響所有網域森林中的），例如新增或移除網域、建立信任的樹系，或引發森林功能層級。 在適當地設計和實作委派模式下，僅第一次建構樹系時才或特定例如建立輸出森林信任的樹系變更，就需要 EA 成員資格。 大部分的權限與權限授與 EA 群組可以委派給權限使用者和群組。  
  
##### <a name="domain-admins"></a>網域系統管理員 」  

每個網域中的有它自己的網域系統管理員 (DA) 群組，也就是該網域中的系統管理員群組成員，並在每個已經加入網域的電腦上系統管理員本機群組成員。 僅限預設群組成員的 DA 網域是該網域建。 EAs 有樹系權限時，DAs 是他們網域中的「雖然」。 在適當地設計和實作委派模式下，應該只在「中斷玻璃「案例中（例如網域中的每一部電腦上的權限等級高帳號所的情形）需要網域系統管理員的資格。 原生 Active Directory 委派機制允許委派可能只在緊急案例中使用 DA 帳號，雖然建構生效委派型號可能會花時間，以及許多組織利用第三方促進此程序的工具。  
  
##### <a name="administrators"></a>系統管理員  
第三個群組是建網域本機系統管理員 (BA) 的 DAs 和 EAs 的巢的群組。 此群組會授與的許多直接存取權限和 directory 和上網域控制站的權限。 不過，系統管理員群組網域權限不或工作站成員伺服器上。 它是透過群組成員資格在電腦本機系統管理員本機權限授與。  
  
> [!NOTE]  
> 雖然這些都是個特殊權限群組預設設定的三個群組成員可以管理 directory 取得任何其他群組成員資格。 有時很一般取得成員資格群組，而在其他很難，但潛在的權限的角度，從三個群組被視為相等有效地。  
  
##### <a name="schema-admins"></a>架構系統管理員  

第四個特殊權限的群組，架構系統管理員（索）森林根網域只存在於且只該網域的建做為預設成員類似企業系統管理員」群組。 只會暫時有時（AD DS 架構修改需要時）和填入是架構管理員群組。  
  
索的可以修改 Active Directory 架構（該現有的設定，例如物件和屬性 directory 的基礎資料結構）僅限群組，但索群組的權利和權限的範圍是更限制比之前所述的群組。 通常也會尋找組織已經開發適當做法管理索群組成員資格群組成員資格通常不常需要因為，且僅供簡短一段時間。 這是技術 EA、DA 及 BA 群組的 Active Directory 中，則為 true，但最較少見，以尋找您的組織已經實作類似做法索群組和這些群組。  
  
#### <a name="protected-accounts-and-groups-in-active-directory"></a>受保護的帳號，並 Active Directory 中的群組  
Active Directory 中有特殊權限的帳號及群組預設設定稱為「保護」帳號，並群組中 directory 其他物件不同的安全。 任何 account（無論是否成員資格從安全性或 distribution 群組）受保護的群組中的直接或轉移成員資格繼承這個限制的安全性。  

  
例如，如果使用者通訊群組成員也就是、的受保護的 Active Directory，該使用者物件群組成員標示為受保護的 account。 當 account 被標示為受保護的帳號時，物件 adminCount 屬性的值為 1。  
  
> [!NOTE]
> 轉移受保護的群組成員資格包含巢的 distribution 和巢的安全性群組，雖然帳號巢的 distribution 群組成員，將不會收到他們存取權杖中的受保護的群組 SID。 不過，可以在也就是為何 distribution 群組均受保護的群組成員列舉在 Active Directory 安全性群組轉換 distribution 群組。 應受保護的巢的 distribution 群組曾經轉換成安全性群組，帳號先前通訊群組成員後續將會收到家長的保護，下次登入他們存取權杖中的群組 SID。  
  
下表列出的受保護的預設帳號和作業系統版本與 service pack 層級 Active Directory 中的群組。  
  
**預設的受保護的帳號和作業系統或 Service Pack (SP) 版本的 Active Directory 中的群組**  
  
|||||  
|-|-|-|-|  
|**Windows 2000 < SP4**|**Windows 2000 SP4-Windows Server 2003**|**Windows Server 2003 SP1 +**|**Windows Server 2008 的 Windows Server 2012**|  
|系統管理員|Account 電信業者|Account 電信業者|Account 電信業者|  
||系統管理員|系統管理員|系統管理員|  
||系統管理員|系統管理員|系統管理員|  
|網域系統管理員 」|備份電信業者|備份電信業者|備份電信業者|  
||憑證的發行者|||  
||網域系統管理員 」|網域系統管理員 」|網域系統管理員 」|  
|企業系統管理員|網域控制站|網域控制站|網域控制站|  
||企業系統管理員|企業系統管理員|企業系統管理員|  
||Krbtgt|Krbtgt|Krbtgt|  
||列印電信業者|列印電信業者|列印電信業者|  
||||唯讀模式網域控制站|  
||複製者|複製者|複製者|  
|架構系統管理員|架構系統管理員||架構系統管理員|  
  
##### <a name="adminsdholder-and-sdprop"></a>AdminSDHolder 和 SDProp  
在系統容器中的每個 Active Directory domain，會自動建立稱為 AdminSDHolder 物件。 AdminSDHolder 物件的目的是為了確保持續執行受保護的帳號群組的權限，無論的受保護的群組和帳號網域中的所在位置。  

每個 60 分鐘（預設）處理程序稱為「安全性描述傳播 (SDProp) 上執行，網域控制站擁有的網域肯定角色。 SDProp 比較網域的 AdminSDHolder 物件的權限的權限的受保護的帳號及網域中的群組。 如果任何受保護的帳號及群組的權限不符合 AdminSDHolder 物件的權限的權限的受保護的帳號和群組會重設以符合網域的 AdminSDHolder 物件。  
  
權限繼承上已停用受保護的群組和帳號，這表示，即使移到不同的位置在 directory 帳號或群組，它們未繼承權限新父物件。 繼承也停用 AdminSDHolder 物件，讓家長物件的權限變更不會變更 AdminSDHolder 的權限。  
  
> [!NOTE]
> 從一個受保護的群組移除帳號時, 不再是受保護的帳號，但不是手動變更為 1 其 adminCount 屬性保留。 這項設定的結果是物件的 Acl 所不再更新 SDProp，但物件仍然不會繼承權限從其家長物件。 因此，物件可能位於單位（組織單位）的已經委派權限，但不是會有前身為受保護的物件繼承這些委派權限。 指令碼以找出並重設保護前身為物件網域中的可以找到[Microsoft 的支援文章 817433](https://support.microsoft.com/?id=817433)。  
  
###### <a name="adminsdholder-ownership"></a>AdminSDHolder 擁有權  
大部分 Active Directory 物件的擁有的網域的 BA 群組。 不過，AdminSDHolder 物件，預設所擁有的網域 DA 群組。 （這是的還中 DAs 執行衍生他們的權限和系統管理員的網域群組成員資格透過權限）。  
  
在之前的版本 Windows Windows Server 2008、物件的擁有者可以變更權限的物件，包括其原始不需要的權限授與本身。 因此，預設的網域 AdminSDHolder 物件的權限防止使用者變更網域 AdminSDHolder 物件的權限 BA 或 EA 群組成員。 不過，系統管理員」的網域群組成員可以取得物件的擁有權，而且本身其他權限授與，也就是基本保護，只保護的使用者網域中的 DA 群組成員意外修改對物件。 此外，BA 和 EA（如果適用）群組已變更物件的屬性 AdminSDHolder（根 EA 網域）本機網域中的權限。  
  
> [!NOTE]  
> 屬性 AdminSDHolder 物件，dSHeuristics，可讓您的群組視為受保護的群組 AdminSDHolder 和 SDProp 會受到限制的自訂（移除）。 這個自訂仔細考慮如果實作，雖然上 AdminSDHolder dSHeuristics 修改適合有效的環境。 Microsoft 的支援文章中找到 AdminSDHolder 物件修改 dSHeuristics 屬性的其他資訊[817433](https://support.microsoft.com/?id=817433)和[973840](https://support.microsoft.com/kb/973840)，在[附錄 c：保護帳號，並 Active Directory 中的群組](Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
Active Directory 中最有特殊權限的群組如下所示，但有一些其他被授與的群組提高權限等級。 如需有關的所有預設和建 Active Directory 與使用者的權限指派給每個群組，請查看[附錄 b 特殊權限帳號，並在 Active Directory 中的群組](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)。  
  


