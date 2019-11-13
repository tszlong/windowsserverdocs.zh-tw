---
ms.assetid: 864ad4bc-8428-4a8b-8671-cb93b68b0c03
title: 減少 Active Directory 的攻擊面
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 94bc65d42fa90dd7c93ba759a41d34edec10de09
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367652"
---
# <a name="reducing-the-active-directory-attack-surface"></a>減少 Active Directory 的攻擊面

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本節著重于要執行的技術控制項，以減少 Active Directory 安裝的受攻擊面。 區段包含下列資訊：  
  
-   [執行最低許可權的系統管理模型](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)著重于找出使用高許可權帳戶進行日常管理的風險，以及提供建議來降低有許可權的帳戶所帶來的風險。  
  
-   除了安全的系統管理主機部署的一些範例方法以外，[執行安全的系統管理主機](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)也會說明部署專用、安全管理系統的原則。  
  
-   [保護網域控制站免于遭受攻擊](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)的討論原則和設定，雖然類似于安全系統管理主機的執行建議，但包含一些網域控制站特定的建議，可協助確保用來管理這些網域控制站和系統的安全性受到妥善保護。  
  
## <a name="privileged-accounts-and-groups-in-active-directory"></a>Active Directory 中的特殊許可權帳戶和群組  
本節提供 Active Directory 中的特殊許可權帳戶和群組的背景資訊，以說明 Active Directory 中的特殊許可權帳戶和群組之間的共通性和差異。 瞭解這些差異後，無論您是實作為原實行[最低許可權系統管理模型](../../../ad-ds/plan/security-best-practices/../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)的建議，或是選擇為組織自訂它們，您都擁有適當的工具來保護每個群組和帳戶。  
  
### <a name="built-in-privileged-accounts-and-groups"></a>內建的特殊許可權帳戶和群組  
Active Directory 有助於管理委派，並支援指派權利和許可權的最低許可權原則。 根據預設，在網域中具有帳戶的「一般」使用者，可以讀取儲存在目錄中的大部分內容，但只能變更目錄中非常有限的資料集。 需要額外許可權的使用者可以被授與目錄內建之各種「特殊許可權」群組的成員資格，使其可以執行與其角色相關的特定工作，但無法執行與其責任無關的工作。 組織也可以建立專為特定工作責任量身打造的群組，並獲得更細微的權利和許可權，讓 IT 人員能夠執行日常的系統管理功能，而不需授與的權利和許可權超過這是這些函式的必要參數。  
  
在 Active Directory 中，三個內建群組是目錄中最高的許可權群組： Enterprise Admins、Domain Admins 和 Administrators。 下列各節將說明每個群組的預設設定和功能：  
  
#### <a name="highest-privilege-groups-in-active-directory"></a>Active Directory 中的最高許可權群組  
  
##### <a name="enterprise-admins"></a>Enterprise Admins  
Enterprise Admins （EA）是只存在於樹系根域中的群組，而且根據預設，它是樹系中所有網域內 Administrators 群組的成員。 樹系根域中的內建 Administrator 帳戶是 EA 群組的唯一預設成員。 EAs 會被授與許可權，讓他們能夠執行全樹系變更（也就是影響樹系中所有網域的變更），例如新增或移除網域、建立樹系信任，或提高樹系功能等級。 在適當設計和實行的委派模型中，只有在第一次建立樹系或進行特定樹系的變更（例如建立輸出樹系信任）時，才需要 EA 成員資格。 授與 EA 群組的大部分權利和許可權，都可以委派給較低許可權的使用者和群組。  
  
##### <a name="domain-admins"></a>Domain Admins  

樹系中的每個網域都有自己的網域系統管理員（DA）群組，這是該網域的系統管理員群組成員，以及每一部加入網域的電腦上本機 Administrators 群組的成員。 網域的 DA 群組唯一的預設成員是該網域的內建系統管理員帳戶。 DAs 在其網域內具有「全功能」，而 EAs 則具備全樹系許可權。 在適當設計和實行的委派模型中，只有在「中斷玻璃」的情況下才需要網域系統管理員成員資格（例如，需要在網域中的每一部電腦上具有高階許可權的帳戶）。 雖然原生 Active Directory 委派機制允許委派的範圍，只有在緊急情況下才能夠使用 DA 帳戶，而建立有效的委派模型可能相當耗時，而許多組織都利用可加速程式的協力廠商工具。  
  
##### <a name="administrators"></a>Administrators  
第三個群組是在其中嵌套 DAs 和 EAs 的內建網域本機系統管理員（BA）群組。 此群組會被授與目錄和網域控制站上的許多直接權利和許可權。 不過，網域的 Administrators 群組在成員伺服器或工作站上沒有許可權。 它是透過授與本機許可權的電腦本機系統管理員群組中的成員資格。  
  
> [!NOTE]  
> 雖然這些是這些特殊許可權群組的預設設定，但其中任何一個群組的成員都可以操控目錄，以取得任何其他群組中的成員資格。 在某些情況下，取得其他群組中的成員資格是很簡單的，而在其他群組中則比較困難，但是從潛在許可權的觀點來看，這三個群組都應該被視為有效的對應專案。  
  
##### <a name="schema-admins"></a>Schema Admins  

第四個特殊許可權群組（架構系統管理員（SA））只存在於樹系根域中，而且只有該網域的內建 Administrator 帳戶作為預設成員，類似于 Enterprise Admins 群組。 Schema Admins 群組僅供暫時和偶爾填入（需要修改 AD DS 架構時）。  
  
雖然 SA 群組是唯一能夠修改 Active Directory 架構的群組（也就是目錄的基礎資料結構，例如物件和屬性），但 SA 群組的許可權與許可權的範圍比先前所述的限制更多。部門. 同時也會發現組織已針對 SA 群組的成員資格而開發適當的作法，因為通常不常需要群組中的成員資格，而且只是短時間內。 這在技術上，這在 Active Directory 的 EA、DA 和 BA 群組中都是如此，但在某些情況下，發現組織已為這些群組實作為 SA 群組的類似做法，這種方式也比較不常見。  
  
#### <a name="protected-accounts-and-groups-in-active-directory"></a>Active Directory 中的受保護帳戶和群組  
在 Active Directory 中，一組稱為「受保護」帳戶和群組的預設特殊許可權帳戶和群組，與目錄中的其他物件的保護方式不同。 任何在任何受保護群組中具有直接或可轉移成員資格的帳戶（不論成員資格是否衍生自安全性或通訊群組）都會繼承此限制的安全性。  

  
例如，如果使用者是通訊群組的成員，而後者又是 Active Directory 中受保護群組的成員，則該使用者物件會被標示為受保護的帳戶。 當帳戶標示為受保護的帳戶時，物件上 adminCount 屬性的值會設定為1。  
  
> [!NOTE]
> 雖然受保護群組中的可轉移成員資格包含嵌套散發和嵌套安全性群組，但屬於嵌套通訊群組成員的帳戶將不會在其存取權杖中收到受保護群組的 SID。 不過，通訊群組可以轉換成 Active Directory 中的安全性群組，這就是為什麼發佈群組會包含在受保護的群組成員列舉中。 如果受保護的嵌套通訊群組已轉換成安全性群組，則先前通訊群組成員的帳戶會在下次登入時，于其存取權杖中收到父系受保護群組的 SID。  
  
下表依作業系統版本和 Service Pack 層級列出 Active Directory 中的預設受保護帳戶和群組。  
  
**依作業系統和 Service Pack （SP）版本 Active Directory 中的預設受保護帳戶和群組**  
  
|||||  
|-|-|-|-|  
|**Windows 2000 < SP4**|**Windows 2000 SP4-Windows Server 2003**|**Windows Server 2003 SP1 +**|**Windows Server 2008-Windows Server 2012**|  
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
在每個 Active Directory 網域的系統容器中，會自動建立名為 AdminSDHolder 的物件。 AdminSDHolder 物件的目的是要確保受保護帳戶和群組的許可權會一致地強制執行，不論受保護的群組和帳戶位於網域中的哪個位置。  

每隔60分鐘（根據預設），稱為安全描述項傳播程式（SDProp）的程式會在持有網域 PDC 模擬器角色的網域控制站上執行。 SDProp 會比較網域的 AdminSDHolder 物件使用權限與網域中受保護帳戶和群組的許可權。 如果任何受保護帳戶和群組的許可權不符合 AdminSDHolder 物件上的許可權，則會重設受保護帳戶和群組的許可權，以符合網域的 AdminSDHolder 物件。  
  
已停用受保護群組和帳戶的許可權繼承，這表示即使帳戶或群組已移至目錄中的不同位置，它們也不會繼承其新父物件的許可權。 AdminSDHolder 物件上的繼承也已停用，因此父物件的許可權變更不會變更 AdminSDHolder 的許可權。  
  
> [!NOTE]
> 當帳戶從受保護的群組中移除時，不會再將它視為受保護的帳戶，但其 adminCount 屬性若未手動變更，仍會設定為1。 此設定的結果是 SDProp 不會再更新物件的 Acl，但物件仍不會繼承其父物件的許可權。 因此，物件可能位於已委派許可權的組織單位（OU）中，但先前受保護的物件將不會繼承這些委派的許可權。 在[Microsoft 支援服務文章 817433](https://support.microsoft.com/?id=817433)中，可以找到用來找出並重設網域中先前受保護物件的腳本。  
  
###### <a name="adminsdholder-ownership"></a>AdminSDHolder 擁有權  
Active Directory 中的大部分物件都是由網域的 BA 群組所擁有。 不過，根據預設，AdminSDHolder 物件是由網域的 DA 群組所擁有。 （這是 DAs 不會透過網域的 Administrators 群組中的成員資格來衍生其權利和許可權的情況）。  
  
在 Windows Server 2008 之前的 Windows 版本中，物件的擁有者可以變更物件的許可權，包括授與本身原本不具有的許可權。 因此，網域 AdminSDHolder 物件的預設許可權會防止屬於 BA 或 EA 群組成員的使用者變更網域 AdminSDHolder 物件的許可權。 不過，網域系統管理員群組的成員可以取得物件的擁有權，並授與自己其他許可權，這表示這項保護是基本的，而且只會保護物件免于遭到下列情況的使用者意外修改：不是網域中的 DA 群組成員。 此外，BA 和 EA （若適用）群組具有在本機網域（EA 的根域）中變更 AdminSDHolder 物件屬性的許可權。  
  
> [!NOTE]  
> AdminSDHolder 物件（dSHeuristics）上的屬性（attribute）允許有限的自訂（移除）被視為受保護群組且受到 AdminSDHolder 和 SDProp 影響的群組。 這項自訂應該仔細考慮是否已實行，雖然在某些情況下，在 AdminSDHolder 上修改 dSHeuristics 很有用。 如需有關在 AdminSDHolder 物件上修改 dSHeuristics 屬性的詳細資訊，請參閱 Microsoft 支援服務文章[817433](https://support.microsoft.com/?id=817433)和[973840](https://support.microsoft.com/kb/973840)，以及[附錄 C： Active Directory 中的受保護帳戶和群組](Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
雖然此處描述 Active Directory 中最具特殊許可權的群組，但有一些其他群組已授與較高的許可權層級。 如需 Active Directory 中的所有預設和內建組，以及指派給每個群組之使用者權限的詳細資訊，請參閱[附錄 B： Active Directory 中的特殊許可權帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)。  
  


