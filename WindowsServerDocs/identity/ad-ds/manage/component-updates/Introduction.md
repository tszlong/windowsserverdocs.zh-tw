---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: 簡介
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ab21cca727342f6dc69ceecfb0c8991b30b0f227
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389876"
---
# <a name="introduction"></a>簡介

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

只要電腦擁有，針對運算基礎結構的攻擊（不論是簡單或複雜）就已經存在了。 不過，在過去十年以來，隨著各種規模組織數量的增加，在世界上所有部分都遭受以大幅變更威脅大環境的方式攻擊及洩露。 網路戰爭和網路犯罪以達到記錄的速率增加。 「Hacktivism」，其中的攻擊是由行動家的位置所積極處理，已經被宣告為用於公開組織秘密資訊、建立拒絕服務，或甚至損毀基礎結構的一些缺口動機。 針對公用和私用機構的攻擊，其目標是 exfiltrating 組織的智慧財產（IP）。  
  
不具資訊技術（IT）基礎結構的組織不會遭受攻擊，但如果適當的原則、程式和控制項是為了保護組織運算基礎結構的重要區段而實行，則會提升攻擊入侵以完成危害可能是預防。 因為源自組織外部的攻擊數目和規模已 eclipsed 內部威脅，所以這份檔通常會討論外部攻擊者，而不是由授權的使用者濫用環境。 不過，本檔中提供的原則和建議是為了協助保護您的環境免于遭受外部攻擊者和有誤導或惡意的內部人員。  
  
本檔中提供的資訊和建議是取自許多來源，並且衍生自設計來保護 Active Directory 安裝免于危害的做法。 雖然無法防止攻擊，但還是可以減少 Active Directory 攻擊面，並將危害目錄的控制措施變得更容易遭受攻擊。 本檔提供我們在遭入侵環境中觀察到的最常見弱點類型，以及我們對客戶提出的最常見建議，以改善其 Active Directory 安裝的安全性。  
  
## <a name="account-and-group-naming-conventions"></a>帳戶和群組命名慣例  
下表提供本檔中所述的命名慣例指南，用於檔中所參考的群組和帳戶。 資料表中包含每個帳戶/群組的位置、其名稱，以及這些帳戶/群組在這份檔中的參考方式。  
  


|**帳戶/群組位置**|**帳戶/群組的名稱**|**本檔中的參考方式**|
| --- | --- | --- |   
|Active Directory-每個網域|Administrator|內建的系統管理員帳戶|  
|Active Directory-每個網域|Administrators|內建的系統管理員（BA）群組|  
|Active Directory-每個網域|Domain Admins|網域系統管理員（DA）群組|  
|Active Directory 樹系根域|Enterprise Admins|Enterprise Admins （EA）群組|  
|執行 Windows Server 之電腦上的本機電腦安全性性帳戶管理員（SAM）資料庫，以及不是網域控制站的工作站|Administrator|本機系統管理員帳戶|  
|執行 Windows Server 之電腦上的本機電腦安全性性帳戶管理員（SAM）資料庫，以及不是網域控制站的工作站|Administrators|本機系統管理員群組|  
  
## <a name="about-this-document"></a>關於本檔  
Microsoft 資訊安全和風險管理（ISRM）組織是 Microsoft 資訊技術（MSIT）的一部分，可與內部業務單位、外部客戶和產業對等合作，以收集、散佈及定義原則。實務和控制項。 Microsoft 和我們的客戶可以使用這項資訊來增加安全性，並減少 IT 基礎結構的受攻擊面。 本檔中提供的建議是根據 MSIT 和 ISRM 中所使用的一些資訊來源和實務。 下列各節將提供有關此檔之來源的詳細資訊。  
  
### <a name="microsoft-it-and-isrm"></a>Microsoft IT 和 ISRM  
已在 MSIT 和 ISRM 中開發一些實務和控制項，以保護 Microsoft AD DS 樹系和網域。 這些控制項廣泛適用，已整合到這份檔中。 SAFE-T （適用于新興技術的解決方案加速器）是 ISRM 中的小組，其宗旨是用來識別新興技術，以及定義安全性需求和控制項以加速採用。  
  
### <a name="active-directory-security-assessments"></a>Active Directory 安全性評量  
在 Microsoft ISRM 中，評量、諮詢及工程（ACE）小組會與內部 Microsoft 業務單位和外部客戶合作，以評估應用程式和基礎結構的安全性，並提供策略性和策略指引來增加組織的安全性狀態。 一個 ACE 服務供應專案是 Active Directory 的安全性評估（ADSA），它是組織 AD DS 環境的整體評量，可評估人員、程式和技術，並產生客戶特有的建議。 客戶會根據組織的獨特特性、實務和風險胃口提供建議。 除了我們的客戶以外，已在 Microsoft 的 Active Directory 安裝中執行 ADSAs。 經過一段時間之後，就可以在不同大小和產業的客戶之間，找到一些建議。  
  
### <a name="content-origin-and-organization"></a>內容來源與組織  
本檔的大部分內容都是衍生自 ADSA 和其他 ACE 小組針對遭入侵的客戶所執行的評量，以及未遭遇重大危害的客戶。 雖然不會使用個別的客戶資料來建立這份檔，但我們已收集我們在評量中發現的最常見弱點，以及我們對客戶提出的建議，以改善其 AD DS 的安全性70，000. 並非所有的弱點都適用於所有環境，所有建議也無法在每個組織中實作。  
  
本檔的組織方式如下：  
  
## <a name="executive-summary"></a>執行摘要  
執行摘要（可當做獨立檔讀取或與完整檔結合）提供此檔的高階摘要。 包含在「主管」摘要中，是我們發現用來危害客戶環境的最常見攻擊媒介、保護 Active Directory 安裝的摘要建議，以及規劃部署新 AD DS 之客戶的基本目標現在或未來的樹系。  
  
### <a name="introduction"></a>簡介  
這是您目前正在閱讀的區段。  
  
### <a name="avenues-to-compromise"></a>危及系統安全的途徑  
本節提供一些我們發現攻擊者用來危害客戶基礎結構的最常見弱點的相關資訊。 本節一開始會列出弱點的一般類別，以及如何利用它們來一開始滲透客戶的基礎結構、在其他系統中傳播危害，最後以 AD DS 和網域控制站為目標，以取得完整的控制組織的樹系。  
  
本節不會提供有關解決每種弱點類型的詳細建議，特別是在未使用弱點直接以 Active Directory 為目標的區域中。 不過，針對每種類型的弱點，我們提供了其他資訊的連結，可讓您用來開發對策和減少貴組織的受攻擊面。  
  
### <a name="reducing-the-active-directory-attack-surface"></a>減少 Active Directory 的攻擊面  
本節一開始會提供有關中特殊許可權帳戶和群組的背景資訊 Active Directory 中，以提供資訊來協助闡明後續建議的原因，以保護和管理特殊許可權群組和帳目. 接著，我們會討論減少使用高許可權帳戶進行日常管理的方法，這不需要授與企業系統管理員（EA）、Domain Admins （DA）和內建等群組的許可權層級Active Directory 中的系統管理員（BA）群組。 接下來，我們會提供保護特殊許可權群組和帳戶，以及執行安全系統管理實務和系統的指導方針。  
  
雖然本節提供有關這些設定設定的詳細資訊，但我們也針對每個建議加入附錄，提供可「依原樣」使用的逐步設定指示，或可修改為組織的需求。 本節會提供資訊來安全地部署和管理網域控制站，這應該是基礎結構中最而更加嚴格的安全系統。  
  
### <a name="monitoring-active-directory-for-signs-of-compromise"></a>監視 Active Directory 遭到危害的徵兆  
無論您是否已在環境中執行穩健的安全性資訊和事件監視（SIEM），或使用其他機制來監視基礎結構的安全性，本節提供的資訊可用於識別 Windows 上的事件可能表示組織遭到攻擊的系統。 我們會討論傳統和先進的稽核原則，包括在 Windows 7 和 Windows Vista 作業系統中有效設定 audit 子類別目錄。 本節包含要進行審核的物件和系統的完整清單，以及相關聯的附錄會列出您應該監視的事件（如果目標是要偵測入侵嘗試）。  
  
### <a name="planning-for-compromise"></a>規劃危害因應措施  
本節一開始會從技術詳細資料中「回溯」，著重于可執行檔原則和程式，以找出不只是 IT 基礎結構，而是對企業而言最重要的使用者、應用程式和系統。 在識別出對您組織穩定性和營運最重要的事項之後，您可以專注于將這些資產分離並加以保護，不論它們是智慧財產、人員或系統。 在某些情況下，隔離和保護資產可能會在您現有的 AD DS 環境中執行，而在其他情況下，您應該考慮執行小型、個別的「資料格」，讓您能夠建立重要資產的安全界限，並監視這些資源資產比較不重要的元件更而更加嚴格。 一種稱為「創意性終結」的概念，這是一種機制，可讓您藉由建立新的解決方案來排除繼承應用程式和系統，並在一節結尾提供建議，協助維護更安全的環境。結合商務和 IT 資訊，以深入瞭解什麼是正常運作狀態。 藉由瞭解組織的正常情況，異常狀況可能表示攻擊和危害，可以更容易識別。  
  
### <a name="summary-of-best-practice-recommendations"></a>最佳做法建議的摘要  
本節所提供的資料表會摘要說明本檔中的建議，並依相對優先順序排序它們，除了提供有關每個建議的詳細資訊可在檔及其附錄中找到的連結。  
  
### <a name="appendices"></a>附錄  
本檔包含附錄，以加強檔本文中包含的資訊。 下表包含附錄清單和各項的簡短描述。  
  
 
|**附錄**|**描述**|
| --- | --- | 
|[附錄 B：Active Directory 中具特殊權限的帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|提供背景資訊，可協助您識別應專注于保護的使用者和群組，因為攻擊者可以利用它們來入侵，甚至終結您的 Active Directory 安裝。|  
|[附錄 C：Active Directory 中受保護的帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|包含 Active Directory 中受保護群組的相關資訊。 其中也包含被視為受保護群組且受到 AdminSDHolder 和 SDProp 影響之群組的有限自訂（移除）資訊。|  
|[附錄 D：保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|包含指導方針，以協助保護樹系中每個網域的系統管理員帳戶。|  
|[附錄 E：保護 Active Directory 中的 Enterprise Admins 群組](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|包含協助保護樹系中 Enterprise Admins 群組的指導方針。|  
|[附錄 F：保護 Active Directory 中的 Domain Admins 群組](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|包含協助保護樹系中每個網域中 Domain Admins 群組的指導方針。|  
|[附錄 G：保護 Active Directory 中的 Administrators 群組](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|包含指導方針，可協助保護樹系中每個網域內的內建系統管理員群組。|  
|[附錄 H：保護本機 Administrators 帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|包含指導方針，可協助保護已加入網域的伺服器和工作站上的本機系統管理員帳戶和系統管理員群組。|  
|[附錄 I：為 Active Directory 中的受保護帳戶和群組建立管理帳戶](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|提供資訊來建立具有有限許可權且可由而更加嚴格控制的帳戶，但當需要暫時提高許可權時，可以用來填入 Active Directory 中的特殊許可權群組。|  
|[附錄 L：要監視的事件](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|列出您的環境中應該監視的事件。|  
|[附錄 M：文件連結與建議閱讀資料](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|包含建議的閱讀清單。 此外，也包含外部檔和其 Url 的連結清單，讓這份檔的讀取者可以存取這項資訊。|  
  


