---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: 簡介
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8e2717af6183944b26a71e55b36f31cef51cf2e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831869"
---
# <a name="introduction"></a>簡介

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

只要電腦有，則已經存在於對運算基礎結構，不論是簡單或複雜的攻擊。 不過，在過去十年以來，隨著各種規模組織數量的增加，在世界上所有部分都遭受以大幅變更威脅大環境的方式攻擊及洩露。 網路戰爭和網路犯罪以達到記錄的速率增加。 "Hacktivism，「 說明攻擊的就由激進份子發動，促成已領取的違規數的動機打算公開組織的機密資訊，來建立服務拒絕，或甚至摧毀基礎結構。 攻擊公用和私人機構，其目標是要滲透組織的智慧財產權 (IP) 變得非常普遍。  
  
沒有任何組織的資訊技術 (IT) 基礎結構是免受攻擊，但如果實作適當的原則、 流程和控制項來保護組織的運算基礎結構，從攻擊的重大問題的重要片段若要完成洩露的滲透可能可以預防。 小數位數來自組織外部的攻擊與數量有 eclipsed 內部威脅，近幾年來，因為這份文件通常討論外部攻擊者，而不是由授權的使用者環境的誤用。 然而，原則與本文件中提供的建議，被用來協助保護您的環境免受外部攻擊者和被誤導或惡意內部人士中。  
  
本文件中提供的建議與資訊會從各種來源描繪，而衍生自設計用來保護 Active Directory 安裝免於遭受洩露的作法。 雖然不可能防止攻擊，就可以降低的 Active Directory 的攻擊面，並實作控制項，讓攻擊者更難以目錄的危害。 觀察在遭洩露的環境和最常見的建議，我們對客戶提升其 Active Directory 安裝的安全性，這份文件會顯示我們有的弱點的最常見的類型。  
  
## <a name="account-and-group-naming-conventions"></a>帳戶和群組的命名慣例  
下表提供本文件中使用群組和帳戶整個文件中參考的命名慣例的指南。 包含的資料表是每個帳戶/群組，其名稱，以及如何將這些帳戶/群組參考這份文件中的位置。  
  


|**帳戶/群組的位置**|**帳戶/群組的名稱**|**如何在這份文件中參考它**|
| --- | --- | --- |   
|Active Directory-每個網域|Administrator|內建的 Administrator 帳戶|  
|Active Directory-每個網域|Administrators|內建的系統管理員 (BA) 群組|  
|Active Directory-每個網域|Domain Admins|網域系統管理員 (DA) 群組|  
|Active Directory-樹系根網域|Enterprise Admins|Enterprise Admins (EA) 群組|  
|在執行 Windows Server 並不是網域控制站的工作站電腦上本機電腦的安全性帳戶管理員 (SAM) 資料庫|Administrator|本機系統管理員帳戶|  
|在執行 Windows Server 並不是網域控制站的工作站電腦上本機電腦的安全性帳戶管理員 (SAM) 資料庫|Administrators|本機 Administrators 群組|  
  
## <a name="about-this-document"></a>關於此文件  
Microsoft 資訊安全與風險管理 (ISRM) 組織，也就是組件的 Microsoft 資訊技術 (MSIT)，適用於內部的業務單位、 外部客戶和業內同行，以收集、 傳播，並定義原則，作法，以及控制項。 這項資訊可以使用由 Microsoft 和我們的客戶，以提高安全性並降低 IT 基礎結構的受攻擊面。 本文件中提供的建議是以許多資訊來源和所 MSIT 及 ISRM 作法為基礎。 下列各節提供此文件的原始來源的詳細資訊。  
  
### <a name="microsoft-it-and-isrm"></a>Microsoft IT 和 ISRM  
MSIT 和 ISRM 來保護 Microsoft AD DS 樹系和網域內已開發出一些做法和控制項。 在這些控制項可廣泛套用的它們已整合到這份文件。 安全-T （新興技術的解決方案加速器） 是的 ISRM 其規範是要識別新興技術，並定義加速其採用的安全性需求和控制項內的小組。  
  
### <a name="active-directory-security-assessments"></a>Active Directory 安全性評定  
在內部的 Microsoft 業務單位或外部客戶評估應用程式和基礎結構的安全性，並且提供策略性指引以提高與 Microsoft ISRM、 評估、 諮詢及工程 (ACE) 團隊的運作方式組織的安全性狀態。 一個 ACE 服務供應項目是 Active Directory 安全性評定 (ADSA)，即會評估人員、 程序和技術，並產生客戶專屬建議組織的 AD DS 環境的全面評估。 建議根據組織的獨特特性、 做法及風險愛好提供客戶。 除了我們的客戶的 Active Directory 安裝在 Microsoft 已經執行 ADSAs。 經過一段時間，可套用不同的大小和產業的客戶之間找到的建議數目。  
  
### <a name="content-origin-and-organization"></a>內容來源與組織  
本文件的內容多數衍生自 ADSA 和遭入侵的客戶和客戶不會發生顯著的折衷執行其他 ACE 團隊評估。 雖然個別客戶的資料不用來建立這份文件中，我們已經收集到的最常被入侵的弱點，我們已找出在我們的評定與建議我們對客戶以改善其 AD DS 的安全性安裝。 並非所有的弱點都適用於所有環境，所有建議也無法在每個組織中實作。  
  
這份文件的組織方式如下：  
  
## <a name="executive-summary"></a>執行摘要  
執行摘要，可以讀取作為獨立文件，或搭配完整的文件，提供本文件的高階摘要。 Executive 摘要包括最常見的攻擊方式，我們觀察到用來危害客戶環境中，摘要建議客戶想要部署新的 AD DS 來保護 Active Directory 安裝與基本的目標現在或未來的樹系。  
  
### <a name="introduction"></a>簡介  
這是您現在閱讀的區段。  
  
### <a name="avenues-to-compromise"></a>危及系統安全的途徑  
本節提供資訊部分最常利用我們發現有攻擊者使用來危害客戶的基礎結構中的弱點。 本節開頭的弱點，以及如何運用一開始滲入客戶的基礎結構、 洩露散佈額外的系統，及最終目標 AD DS 和網域控制站以取得完整的一般類別組織的樹系的控制項。  
  
本節並不會提供有關解決每一種弱點，特別是在區域中的弱點不會以直接鎖定 Active Directory 的詳細的建議。 不過，針對每個類型的弱點可能會中，我們提供了開發因應對策，並減少貴組織的受攻擊面，您可以使用的其他資訊的連結。  
  
### <a name="reducing-the-active-directory-attack-surface"></a>減少 Active Directory 的攻擊面  
本節一開始會提供中 Active Directory 提供的資訊可協助釐清保護和管理特殊權限的群組的後續建議的原因有權限的帳戶和群組的背景資訊和帳戶。 接著會討論方法可以減少需要使用高權限帳戶對日常的管理，不需要的例如 Enterprise Admins (EA)、 網域系統管理員 (DA) 和內建群組授與的權限層級Active Directory 中的系統管理員 (BA) 群組。 接下來，我們提供指引，來保護特殊權限的群組和帳戶以及實作安全的系統管理實務與系統。  
  
雖然本節提供這些組態設定的詳細的資訊，我們也已包含之每個建議，提供逐步設定指示，可使用 「 現狀 」，或可以修改的附錄組織的需求。 本章節會完成藉由提供安全地部署和管理網域控制站，應該會在基礎結構中最嚴格安全的系統之間的資訊。  
  
### <a name="monitoring-active-directory-for-signs-of-compromise"></a>監視 Active Directory 遭到危害的徵兆  
此區段是否您已實作強固的安全性資訊及您環境中的監視 (SIEM) 的事件，或使用其他機制來監視基礎結構的安全性，提供可用來識別事件在 Windows 上的資訊可能表示組織遭受攻擊的系統。 我們會討論傳統和進階稽核原則，包括 Windows 7 和 Windows Vista 作業系統中的稽核子類別目錄的有效設定。 本節包含的物件和系統来稽核的完整清單和相關聯的附錄列出，您應該監視如果目標是要偵測入侵嘗試的事件。  
  
### <a name="planning-for-compromise"></a>規劃危害因應措施  
本節一開始會從將焦點放在 原則和程序，可實作來識別使用者、 應用程式，以及不只是以 IT 基礎結構中，最重要的系統上的技術詳細資料的 「 逐步執行後 」 但業務。 找出哪些最重要的穩定性和組織的作業之後, 您可以專心隔離和保護這些資產，無論是智慧財產、 人員或系統。 在某些情況下，隔離和保護資產可能會執行在您現有的 AD DS 環境中，在其他情況下，您應該考慮實作小型的個別 「 儲存格 」 可讓您建立周圍重要資產的安全界限，並監視這些資產更嚴格比較不重要元件。 討論的概念稱為 「 creative 解構 」，是用舊版的應用程式和系統要消除，可以藉由建立新的解決方案的機制，而一節結尾有助於維護由更安全的環境的建議結合商務和 IT 的資訊，以創造什麼是正常的作業狀態的詳細的描述。 藉由得知哪些正常的組織，可以更輕鬆地識別攻擊和入侵，可能表示異常狀況。  
  
### <a name="summary-of-best-practice-recommendations"></a>最佳做法建議的摘要  
本節提供摘要說明在這份文件中所做的建議的資料表，並加以排序依相對優先權，除了提供每個建議的詳細資訊可以找到文件和其附錄中的連結。  
  
### <a name="appendices"></a>附錄  
附錄包含在這份文件，來加強的文件內所包含的資訊。 附錄和每個的簡短描述的清單是否包含下表。  
  
 
|**附錄**|**描述**|
| --- | --- | 
|[附錄 b:Active Directory 中具有特殊權限的帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|提供可協助您識別的使用者和群組，您應該專注於保障安全，因為它們可以利用攻擊者危害，並甚至摧毀您的 Active Directory 安裝的背景資訊。|  
|[附錄 c:Active Directory 中受保護的帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|包含 Active Directory 中的受保護群組的相關資訊。 它也包含資訊的群組會被視為受保護的群組，而且會受到 AdminSDHolder 和 SDProp 有限的自訂 （移除）。|  
|[附錄 d:保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|包含指導方針來協助保護樹系中每個網域中的系統管理員帳戶。|  
|[附錄 e:保護 Active Directory 中的 Enterprise Admins 群組](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|包含指導方針來協助保護樹系中的 Enterprise Admins 群組。|  
|[附錄 f:保護 Active Directory 中的 Domain Admins 群組](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|包含指導方針來協助保護樹系中每個網域中的 Domain Admins 群組。|  
|[附錄 g:保護 Active Directory 中的系統管理員群組](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|包含指導方針來協助保護樹系中的每個網域中的內建的 Administrators 群組。|  
|[附錄 h:保護本機系統管理員帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|包含指導方針來協助安全的本機系統管理員帳戶和已加入網域的伺服器和工作站上的系統管理員群組。|  
|[附錄 i:建立管理帳戶的受保護的帳戶和 Active Directory 中的群組](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|提供資訊以建立具有有限的權限，也能嚴格控制，但可以用來填入 Active Directory 中的特殊權限的群組，需要暫時提高權限時的帳戶。|  
|[附錄 l:要監視的事件](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|列出，您應該監視您的環境中的事件。|  
|[附錄 m︰文件連結與建議閱讀資料](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|包含一份建議您先閱讀。 也包含連結至外部的文件的清單，以及其 Url，以便讀取器的這份文件的書面複本可存取此資訊。|  
  


