---
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: 減少 Active Directory 的攻擊面
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 5c151f0b152fadc4c86fc7bc0a414e9a190c0080
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941388"
---
# <a name="reducing-the-active-directory-attack-surface"></a>減少 Active Directory 的攻擊面

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本節著重于執行的技術控制項，以降低 Active Directory 安裝的受攻擊面。 此區段包含下列資訊：

- [執行最小許可權的系統管理模型](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) 著重于找出使用高特殊許可權帳戶進行日常管理的風險，以及提供建議來降低特殊許可權帳戶所呈現的風險。

- [執行安全的系統管理主機](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md) 描述部署專用、安全系統管理系統的準則，以及安全管理主機部署的一些範例方法。

- [保護網域控制站免于遭受攻擊](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md) 會討論原則和設定，雖然類似于執行安全管理主機的建議，但是包含一些網域控制站特定的建議，以協助確保用來管理它們的網域控制站和系統受到妥善保護。

## <a name="privileged-accounts-and-groups-in-active-directory"></a>Active Directory 中具特殊權限的帳戶和群組
本節提供有關 Active Directory 中的特殊許可權帳戶和群組的背景資訊，以說明 Active Directory 中的特殊許可權帳戶和群組之間的共通性和差異。 藉由瞭解這些差異，無論您是在執行 [最少許可權的系統管理模型](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md) ，或選擇為您的組織自訂它們時，都有必要的工具可以適當地保護每個群組和帳戶。

### <a name="built-in-privileged-accounts-and-groups"></a>內建的特殊許可權帳戶和群組
Active Directory 有助於委派管理，並支援指派權利和許可權的最低許可權原則。 依預設，在網域中具有帳戶的「一般」使用者可以讀取大部分儲存在目錄中的內容，但只能變更目錄中非常有限的資料集。 需要額外許可權的使用者可以被授與目錄內建的各種「特殊許可權」群組的成員資格，讓他們可以執行與其角色相關的特定工作，但無法執行與其職責無關的工作。 組織也可以建立針對特定工作責任量身訂做的群組，並授與更細微的權利和許可權，讓 IT 人員可以執行日常系統管理功能，而不授與超出這些功能所需的許可權。

在 Active Directory 中，三個內建組是目錄中的最高許可權群組： Enterprise Admins、Domain Admins 和 Administrator。 下列各節將說明每個群組的預設設定和功能：

#### <a name="highest-privilege-groups-in-active-directory"></a>Active Directory 中的最高許可權群組

##### <a name="enterprise-admins"></a>企業系統管理員
Enterprise Admins (EA) 是只存在於樹系根域中的群組，依預設，它是樹系中所有網域的 Administrators 群組成員。 樹系根域中的內建系統管理員帳戶是 EA 群組的唯一預設成員。 EAs 被授與許可權，可讓他們執行全樹系的變更， (也就是會影響) 樹系中所有網域的變更，例如新增或移除網域、建立樹系信任，或提高樹系功能等級。 在適當設計及實施的委派模型中，只有在第一次建立樹系或進行特定全樹系的變更（例如建立輸出樹系信任）時，才需要 EA 成員資格。 對 EA 群組授與的大部分權利和許可權，都可以委派給較低許可權的使用者和群組。

##### <a name="domain-admins"></a>網域管理員

樹系中的每個網域都有自己的 Domain Admins (DA) 群組，也就是該網域的 Administrators 群組成員，以及加入網域之每部電腦上的本機 Administrators 群組成員。 網域的 DA 群組唯一預設成員是該網域的內建系統管理員帳戶。 DAs 在其網域內是「功能強大」，而 EAs 具有全樹系的許可權。 在適當設計及實施的委派模型中，只有在「中斷玻璃」案例中才需要 Domain Admins 成員資格 (例如，需要在網域中的每一部電腦上具有高階許可權的帳戶) 。 雖然原生 Active Directory 委派機制允許委派可在緊急情況下使用 DA 帳戶的程度，但是建立有效的委派模型可能相當耗時，而且許多組織會利用協力廠商工具來加速程式。

##### <a name="administrators"></a>系統管理員
第三個群組是內建的網域本機系統管理員 (BA) 群組，其中的 DAs 和 EAs 會進行嵌套。 此群組被授與目錄和網域控制站上的許多直接許可權和許可權。 但是，網域的 Administrators 群組沒有成員伺服器或工作站上的許可權。 它是透過授與本機許可權的電腦本機系統管理員群組成員資格。

> [!NOTE]
> 雖然這些是這些特殊許可權群組的預設設定，但這三個群組中任一群組的成員都可以操作目錄，以得到任何其他群組的成員資格。 在某些情況下，取得其他群組的成員資格是很簡單的，但在其他群組中則比較困難，但是從潛在許可權的觀點來看，所有三個群組都應該視為有效的同等專案。

##### <a name="schema-admins"></a>Schema Admins

第四個特殊許可權群組（Schema Admins (SA) ）只存在於樹系根域中，而且只有該網域的內建系統管理員帳戶做為預設成員，類似于 Enterprise Admins 群組。 Schema Admins 群組只會暫時填入，而在需要修改 AD DS 架構) 時偶爾 (。

雖然 SA 群組是唯一可以修改 Active Directory 架構的群組 (也就是說，目錄的基礎資料結構（例如物件和屬性）) ，但 SA 群組的許可權範圍與先前所述的群組更受限制。 此外，也會發現組織開發了適當的作法來管理 SA 群組的成員資格，因為通常不會經常需要群組的成員資格，而且只會在短時間內進行。 這在技術上也是 Active Directory 中的 EA、DA 和 BA 群組，但較不常見的情況是找出組織為這些群組實行類似于 SA 群組的類似做法。

#### <a name="protected-accounts-and-groups-in-active-directory"></a>Active Directory 中受保護的帳戶和群組
在 Active Directory 中，一組稱為「受保護」的帳戶和群組的預設授權帳戶和群組，與目錄中的其他物件的保護方式不同。 不論成員資格是否衍生自安全性或通訊群組，任何可直接或可轉移的受保護群組成員資格 (的帳戶，) 繼承此限制的安全性。


例如，如果使用者是通訊群組的成員，而該群組是 Active Directory 中受保護群組的成員，則該使用者物件會標示為受保護的帳戶。 當帳戶被標示為受保護的帳戶時，物件上的 adminCount 屬性值會設定為1。

> [!NOTE]
> 雖然受保護群組中的可轉移成員資格包括嵌套散發和嵌套安全性群組，但屬於嵌套通訊群組成員的帳戶並不會在其存取權杖中收到受保護群組的 SID。 不過，您可以將通訊群組轉換成 Active Directory 中的安全性群組，這也是為什麼將通訊群組納入受保護群組成員列舉中的原因。 如果受保護的嵌套通訊群組已轉換成安全性群組，則為先前通訊群組成員的帳戶，接著會在下次登入時，于其存取權杖中收到父系受保護群組的 SID。

下表列出依作業系統版本和 service pack 層級 Active Directory 的預設受保護帳戶和群組。

**依作業系統和 Service Pack (SP) 版本 Active Directory 預設受保護的帳戶和群組**

|**Windows 2000 <SP4**|**Windows 2000 SP4-Windows Server 2003**|**Windows Server 2003 SP1 +**|**Windows Server 2008-Windows Server 2012**|
|--|--|--|--|
|系統管理員|Account Operators|Account Operators|Account Operators|
||系統管理員|系統管理員|系統管理員|
||系統管理員|系統管理員|系統管理員|
|網域管理員|Backup Operators|Backup Operators|Backup Operators|
||Cert Publishers|||
||網域管理員|網域管理員|網域管理員|
|企業系統管理員|網域控制站|網域控制站|網域控制站|
||企業系統管理員|企業系統管理員|企業系統管理員|
||Krbtgt|Krbtgt|Krbtgt|
||Print Operators|Print Operators|Print Operators|
||||Read-only Domain Controllers|
||Replicator|Replicator|Replicator|
|Schema Admins|Schema Admins||Schema Admins|

##### <a name="adminsdholder-and-sdprop"></a>AdminSDHolder 和 SDProp
在每個 Active Directory 網域的系統容器中，會自動建立名為 AdminSDHolder 的物件。 AdminSDHolder 物件的目的是要確保不論受保護的群組和帳戶位於網域中的哪個位置，都會一致地強制執行受保護帳戶和群組的許可權。

每隔60分鐘 (預設) ，稱為安全描述項傳播程式 (SDProp) 的程式會在持有網域 PDC 模擬器角色的網域控制站上執行。 SDProp 會比較網域的 AdminSDHolder 物件使用權限與網域中受保護帳戶和群組的許可權。 如果任何受保護帳戶和群組的許可權不符合 AdminSDHolder 物件的許可權，則會重設受保護帳戶和群組的許可權，以符合網域 AdminSDHolder 物件的許可權。

受保護的群組和帳戶已停用許可權繼承，這表示即使將帳戶或群組移至目錄中的不同位置，它們也不會繼承其新父物件的許可權。 AdminSDHolder 物件上的繼承也會停用，因此父物件的許可權變更不會變更 AdminSDHolder 的許可權。

> [!NOTE]
> 從受保護的群組移除帳戶時，該帳戶不再被視為受保護的帳戶，但如果未手動變更，其 adminCount 屬性仍會設定為1。 這項設定的結果是 SDProp 不會再更新物件的 Acl，但是該物件仍不會繼承其父物件的許可權。 因此，物件可能位於組織單位 (OU) 已委派許可權，但先前受保護的物件將不會繼承這些委派的許可權。 若要在網域中尋找及重設先前受保護物件的腳本，請參閱 [Microsoft 支援服務文章 817433](https://support.microsoft.com/?id=817433)。

###### <a name="adminsdholder-ownership"></a>AdminSDHolder 擁有權
Active Directory 中的大部分物件都是由網域的 BA 群組所擁有。 不過，根據預設，AdminSDHolder 物件是由網域的 DA 群組所擁有。  (這種情況下，DAs 不會透過網域的 Administrators 群組成員資格衍生其權利和許可權。 ) 

在 Windows Server 2008 之前的 Windows 版本中，物件的擁有者可以變更物件的許可權，包括授與本身原本沒有的許可權。 因此，網域的 AdminSDHolder 物件上的預設許可權會防止屬於 BA 或 EA 群組成員的使用者變更網域 AdminSDHolder 物件的許可權。 不過，網域的 Administrators 群組成員可以取得物件的擁有權，並授與自己額外的許可權，這表示這項保護是基本的，而且只有在網域中不是 DA 群組成員的使用者不會意外修改時，才會保護物件。 此外，您可以在適用的) 群組有權變更 EA) 之本機網域 (根域中 AdminSDHolder 物件的屬性時，使用 BA 和 EA (。

> [!NOTE]
> AdminSDHolder 物件（dSHeuristics）上的屬性允許有限的自訂 () 移除被視為受保護群組的群組，並受到 AdminSDHolder 和 SDProp 的影響。 這項自訂應謹慎考慮（如果有的話），但在某些情況下，dSHeuristics on AdminSDHolder 的修改會很有用。 如需有關在 AdminSDHolder 物件上修改 dSHeuristics 屬性的詳細資訊，請參閱 Microsoft 支援服務文章 [817433](https://support.microsoft.com/?id=817433) 和 [973840](https://support.microsoft.com/kb/973840)，以及 [附錄 C： Active Directory 中受保護的帳戶和群組](Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。

雖然此處描述 Active Directory 中最具特殊許可權的群組，但有一些其他群組已獲得提高許可權的許可權等級。 如需 Active Directory 中所有預設和內建組的詳細資訊，以及指派給每個群組的使用者權限，請參閱 [附錄 B： Active Directory 中的特殊許可權帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)。
