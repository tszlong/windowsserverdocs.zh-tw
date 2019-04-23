---
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: 減少 Active Directory 的攻擊面
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d692641d316b5fe7206cc3f413bdcfc9b74675b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874149"
---
# <a name="reducing-the-active-directory-attack-surface"></a>減少 Active Directory 的攻擊面

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本節著重於技術的控制項，以減少受攻擊面，Active Directory 安裝的實作。 區段包含下列資訊：  
  
-   [實作最低權限系統管理模型](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)著重於用來識別使用具有高度權限風險帳戶以獲得呈現日常管理工作，除了提供建議來實作，以降低風險該特殊權限帳戶存在。  
  
-   [實作安全的系統管理主機](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)描述原則的安全的系統管理主機的部署方法部署專用、 安全管理系統，除了一些範例。  
  
-   [保護網域控制站針對攻擊](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)討論原則和設定，雖然類似於實作安全的系統管理主機的建議包含某些網域控制站的特定建議，可幫助請確定網域控制站和用來管理這些系統是安全無虞。  
  
## <a name="privileged-accounts-and-groups-in-active-directory"></a>Active Directory 中具有特殊權限的帳戶和群組  
本節提供特殊權限帳戶的背景資訊，Active Directory 中的群組的共通性和 Active Directory 中具有特殊權限的帳戶與群組之間差異的說明。 了解這些差異，是否您實作中的建議[實作最低權限系統管理模型](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)逐字或選擇以針對您的組織進行自訂，您有您所需的工具適當地保護每個群組和帳戶。  
  
### <a name="built-in-privileged-accounts-and-groups"></a>內建特殊權限帳戶和群組  
Active Directory 可協助管理委派，並支援在指派權利與權限的最低權限原則。 在網域中擁有帳戶的 「 一般 」 使用者是，根據預設，能夠讀取大多在目錄中，儲存的內容，但能夠變更只有非常有限的目錄中的資料。 需要額外的權限的使用者可以授與內建目錄，讓它們可能會執行特定工作相關的角色，但無法執行其職責無關的工作的各種 「 特許 」 群組的成員資格。 組織也可以建立以特定的職務量身訂做且細微的權限和權限可讓 IT 人員來執行日常的管理功能，但不授與權限和超過的權限授與的群組需要這些函式。  
  
在 Active Directory 中，三個內建群組就是目錄中最高的權限群組：Enterprise Admins、 Domain Admins 和系統管理員。 預設組態和功能，每個群組的下列各節所述：  
  
#### <a name="highest-privilege-groups-in-active-directory"></a>Active Directory 中最高的權限群組  
  
##### <a name="enterprise-admins"></a>Enterprise Admins  
Enterprise Admins (EA) 是只存在於樹系根網域的群組，而且根據預設，樹系中的所有網域中的 Administrators 群組的成員。 內建的 Administrator 帳戶的樹系根網域中是唯一的預設成員的 EA 群組。 EAs 會授與權限，讓他們能夠實作全樹系的變更 （亦即，變更會影響所有的網域樹系中），例如加入或移除網域、 建立樹系信任或提高樹系功能等級。 正確設計及實作委派模型，在 EA 成員資格是必要的或只在第一次建構樹系時，才進行特定的全樹系的變更，例如建立輸出樹系信任時。 大部分的權限和 EA 群組授與權限可以將委派權限較低的使用者和群組。  
  
##### <a name="domain-admins"></a>Domain Admins  

每個樹系中的網域有它自己的網域系統管理員 (DA) 群組，也就是該網域系統管理員群組的成員和每個已加入網域的電腦上本機 Administrators 群組的成員。 只有預設群組的成員資料網域是內建的 Administrator 帳戶，該網域。 DAs 是"全能"在其網域、 EAs 具有全樹系的權限。 在適當地設計和實作委派模型中，應該只有在 「 急用 」 （例如在網域中的每一部電腦上具高的層級的權限的帳戶所需的情況下） 的情況下需要 Domain Admins 的成員資格。 雖然就能夠使用 DA 帳戶只有在緊急情況下，原生 Active Directory 委派機制會允許委派，建構有效的委派模型可能非常耗時，而且許多組織中運用若要加速此程序的第三方工具。  
  
##### <a name="administrators"></a>Administrators  
第三個群組是在其中 DAs 和 EAs 巢狀的內建的網域本機系統管理員 (BA) 群組。 此群組會授與許多直接的權限和權限在目錄和網域控制站上。 不過，網域系統管理員群組沒有任何權限，在成員伺服器或工作站上。 它是透過電腦的本機系統管理員群組的成員資格本機權限會授與。  
  
> [!NOTE]  
> 雖然這些都是這些特殊權限群組的預設組態，任何三個群組的成員可以管理的目錄，以取得在任何其他群組的成員資格。 在某些情況下，它是 trivial 取得其他群組的成員資格，而在其他使用案例就很難，但從可能的權限的觀點來看，所有三個群組應視為有效等同項目。  
  
##### <a name="schema-admins"></a>Schema Admins  

第四個特殊權限群組中，結構描述系統管理員 (SA)，只存在於樹系根網域，且只有該網域內建系統管理員帳戶做為預設成員，類似於 Enterprise Admins 群組。 Schema Admins 群組被要只能暫時和偶爾 （AD DS 結構描述修改需要時） 時，才會填入。  
  
雖然 SA 群組是唯一可以修改 Active Directory 結構描述 （該現有的設定，例如物件和屬性的目錄的基礎資料結構） 的群組，但 [SA] 群組的權利與權限的範圍是限制高於先前所述群組。 它也是時間的常見尋找組織所開發的 SA 群組的成員資格管理的適當作法，因為通常不常需要群組的成員資格，而且只適合短。 這是 EA、 DA 和 BA 群組的 Active Directory 中，技術上則為 true，但通常會遠低於以尋找組織已實作類似的作法，才能讓這些群組與 [SA] 群組。  
  
#### <a name="protected-accounts-and-groups-in-active-directory"></a>Active Directory 中受保護的帳戶和群組  
Active Directory，內一組預設的特殊權限的帳戶和群組稱為 「 受保護 」 的帳戶，並在目錄中其他物件不同保護群組。 具有直接或可轉移的成員資格 （不論是否成員資格衍生自安全性或通訊群組的群組） 的任何受保護群組中的任何帳戶都會繼承此限制的安全性。  

  
比方說，如果使用者是通訊群組的成員也就是，接著，在 Active Directory 中，該使用者物件的受保護群組的成員標示為受保護的帳戶。 當帳戶被標示為受保護的帳戶時，在物件上 adminCount 屬性的值是設定為 1。  
  
> [!NOTE]
> 雖然受保護群組中的可轉移成員資格包含巢狀通訊群組和巢狀的安全性群組，巢狀的通訊群組之成員的帳戶不會收到受保護的群組的 SID 的存取權杖中。 不過，發佈群組可以轉換到 Active Directory，這就是為什麼發佈群組包含在受保護的群組成員的列舉型別中的安全性群組。 先前發佈群組的成員後續將會收到父代的帳戶應該受保護的巢狀的通訊群組曾經轉換為安全性群組，保護群組的 SID，在下次登入時的存取權杖中。  
  
下表列出受保護的預設帳戶及群組的作業系統版本和 service pack 層級的 Active Directory 中。  
  
**預設的作業系統和 Service Pack (SP) 版本的 Active Directory 中受保護的帳戶和群組**  
  
|||||  
|-|-|-|-|  
|**Windows 2000 <SP4**|**Windows 2000 SP4 -Windows Server 2003**|**Windows Server 2003 SP1+**|**Windows Server 2008 -Windows Server 2012**|  
|Administrators|Account Operators|Account Operators|Account Operators|  
||Administrator|Administrator|Administrator|  
||Administrators|Administrators|Administrators|  
|Domain Admins|Backup Operators|Backup Operators|Backup Operators|  
||Cert Publishers|||  
||Domain Admins|Domain Admins|Domain Admins|  
|Enterprise Admins|網域控制站|網域控制站|網域控制站|  
||Enterprise Admins|Enterprise Admins|Enterprise Admins|  
||Krbtgt|Krbtgt|Krbtgt|  
||Print Operators|Print Operators|Print Operators|  
||||Read-only Domain Controllers|  
||Replicator|Replicator|Replicator|  
|Schema Admins|Schema Admins||Schema Admins|  
  
##### <a name="adminsdholder-and-sdprop"></a>AdminSDHolder 和 SDProp  
在每個 Active Directory 網域系統容器中，會自動建立一個稱為 AdminSDHolder 物件。 AdminSDHolder 物件的目的是要確保以一致的方式執行的受保護的帳戶和群組權限，不論其中位於網域中的受保護的群組和帳戶。  

（根據預設值），每隔 60 分鐘會持有網域的 PDC 模擬器角色的網域控制站上執行安全性描述元傳播 (SDProp) 這道程序。 SDProp 比較網域的 AdminSDHolder 物件上的受保護的帳戶和網域中的群組的權限的權限。 如果任何受保護的帳戶和群組的權限不相符的 AdminSDHolder 物件的權限，受保護的帳戶和群組的權限會重設以符合那些網域的 AdminSDHolder 物件。  
  
權限繼承已停用受保護的群組和帳戶，這表示，即使帳戶或群組會移到不同位置的目錄中，它們不會繼承權限自其新的父物件。 繼承也會停用的 AdminSDHolder 物件上，讓父物件的權限變更不會變更 AdminSDHolder 的權限。  
  
> [!NOTE]
> 當從受保護群組移除帳戶時，它不再被視為受保護的帳戶，但設定為 1，如果未以手動方式變更其 adminCount 屬性會維持。 這項設定的結果是 SDProp，不會再更新物件的 Acl，但該物件仍然不會繼承的權限其父物件。 因此，物件可能位於組織單位 (OU) 的委派的權限，但先前受保護的物件將不會繼承這些委派的權限。 找出及重設之前受保護的物件，在網域中的指令碼可在[Microsoft 支援服務文章 817433](https://support.microsoft.com/?id=817433)。  
  
###### <a name="adminsdholder-ownership"></a>AdminSDHolder 擁有權  
Active Directory 中的大部分物件是由網域的 BA 群組所擁有。 不過，AdminSDHolder 物件，根據預設，屬於網域的資料群組。 （這是在其中 DAs 不是衍生其權限和透過網域系統管理員群組的成員資格的權限的一種情況）。  
  
在版本 Windows 的 Windows Server 2008 之前，物件的擁有者可以變更物件，包括它們原本沒有的權限授與自己的權的限。 因此，網域的 AdminSDHolder 物件的預設權限會防止 BA 或 EA 變更網域的 AdminSDHolder 物件的權限的群組之成員的使用者。 不過，網域系統管理員群組的成員可以取得物件的擁有權，並授與自己額外的權限，這表示這項保護是基本，也只會保護的使用者是意外修改物件不在網域中的資料群組的成員。 此外，BA 和 EA （如果適用） 群組已變更的屬性 （根網域為 EA） 的本機網域中的 AdminSDHolder 物件的權限。  
  
> [!NOTE]  
> AdminSDHolder 物件，dSHeuristics，屬性允許的群組會被視為受保護的群組，而且會受到 AdminSDHolder 和 SDProp 有限的自訂 （移除）。 這項自訂應該要謹慎考量實作它時，如果雖然修改 dSHeuristics AdminSDHolder 在適合的情況下有效。 AdminSDHolder 物件上修改 dSHeuristics 屬性的詳細資訊可在 Microsoft 支援服務文章[817433](https://support.microsoft.com/?id=817433)並[973840](https://support.microsoft.com/kb/973840)，然後在[附錄 c:受保護的帳戶和 Active Directory 中的群組](Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
雖然此處所述的 Active Directory 中的最高權限的群組，有幾個其他的群組已經被授與提高權限層級的權限。 如需所有的預設和 Active Directory 與指派給每個使用者權限中的內建群組的詳細資訊，請參閱[附錄 b:特殊權限的帳戶和 Active Directory 中的群組](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)。  
  


