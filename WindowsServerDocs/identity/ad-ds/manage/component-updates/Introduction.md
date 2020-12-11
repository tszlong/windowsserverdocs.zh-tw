---
description: 深入瞭解：簡介
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: 簡介
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 9f022f30b977fee15d7d20ba3f9330806a3570a4
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048816"
---
# <a name="introduction"></a>簡介

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

只要電腦有的話，對運算基礎結構的攻擊（無論是簡單或複雜的）都已存在。 不過，在過去十年以來，隨著各種規模組織數量的增加，在世界上所有部分都遭受以大幅變更威脅大環境的方式攻擊及洩露。 網路戰爭和網路犯罪以達到記錄的速率增加。 「Hacktivism」，這是由行動家職位負責的攻擊，已被宣告為一些缺口的動機，目的是要公開組織的秘密資訊、建立拒絕服務，甚至終結基礎結構。 針對公共和私用機構的攻擊，其目標為洩漏組織的智慧財產 (IP) 已成為普遍。

沒有任何組織的資訊技術 (IT) 基礎結構不會受到攻擊，但是如果執行適當的原則、程式和控制來保護組織計算基礎結構的重要區段，則入侵入侵的攻擊可能會 preventable。 由於來自組織外部的攻擊數量和規模在最近幾年都有 eclipsed 的內部威脅，因此本檔通常會討論外部攻擊者，而不是由已授權的使用者誤用環境。 不過，本檔中所提供的原則和建議旨在協助保護您的環境，使其免于遭受外部攻擊者和誤導或惡意的內部人員攻擊。

本檔中提供的資訊和建議是從數個來源進行繪製，並衍生自設計來保護 Active Directory 安裝免于洩漏的實務。 雖然無法防止攻擊，但是可以減少 Active Directory 的攻擊面，以及執行對攻擊者更難入侵目錄的控制項。 本檔提供我們在遭入侵環境中觀察到的最常見弱點類型，以及我們對客戶提出的最常見建議，以改善其 Active Directory 安裝的安全性。

## <a name="account-and-group-naming-conventions"></a>帳戶和群組命名慣例
下表提供本檔中用於檔中所參考之群組和帳戶的命名慣例指南。 表格中包含每個帳戶/群組的位置、其名稱，以及在本檔中參考這些帳戶/群組的方式。



|**帳戶/群組位置**|**帳戶/群組的名稱**|**本檔的參考方式**|
| --- | --- | --- |
|Active Directory-每個網域|系統管理員|內建的系統管理員帳戶|
|Active Directory-每個網域|Administrators|內建系統管理員 (BA) 群組|
|Active Directory-每個網域|Domain Admins|Domain Admins (DA) 群組|
|Active Directory-樹系根域|Enterprise Admins|企業系統管理員 (EA) 群組|
|本機電腦安全性性帳戶管理員 (SAM 在執行 Windows Server 的電腦和非網域控制站的工作站上) 資料庫|系統管理員|本機系統管理員帳戶|
|本機電腦安全性性帳戶管理員 (SAM 在執行 Windows Server 的電腦和非網域控制站的工作站上) 資料庫|Administrators|本機系統管理員群組|

## <a name="about-this-document"></a>關於本檔
Microsoft 資訊安全與風險管理 (ISRM) 的組織，這是 Microsoft 資訊技術 (MSIT) 的一部分，可與內部營業單位、外部客戶和產業對等互連，以收集、散佈和定義原則、實務和控制項。 Microsoft 與我們的客戶可以使用這項資訊來提高安全性，並減少 IT 基礎結構的受攻擊面。 本檔中提供的建議是以 MSIT 和 ISRM 中使用的一些資訊來源與實務為基礎。 下列各節提供有關本檔之來源的詳細資訊。

### <a name="microsoft-it-and-isrm"></a>Microsoft IT 和 ISRM
在 MSIT 和 ISRM 中開發了許多作法和控制項，以保護 Microsoft AD DS 的樹系和網域。 這些控制項廣泛適用，已整合至本檔中。 適用于新興技術的安全-T (解決方案加速器) 是 ISRM 內的團隊，其職責是找出新興的技術，並定義安全性需求和控制項來加速採用。

### <a name="active-directory-security-assessments"></a>Active Directory 安全性評定
在 Microsoft ISRM 中，評量、諮詢和工程 (ACE) 小組與內部的 Microsoft 業務單位和外部客戶合作，以評估應用程式和基礎結構安全性，並提供策略性和策略性的指導方針來提高組織的安全性狀態。 其中一個 ACE 服務供應專案是 Active Directory 的安全性評量 (ADSA) ，這是組織的 AD DS 環境的整體評量，可評估人員、程式和技術，並產生客戶特定的建議。 客戶會根據組織的獨特特性、實務和風險胃口提供建議。 除了我們的客戶以外，ADSAs 還已在 Microsoft 執行 Active Directory 安裝。 經過一段時間之後，您就可以在各種不同規模和產業的客戶上，找到一些建議。

### <a name="content-origin-and-organization"></a>內容來源與組織
這份檔的大部分內容都是衍生自 ADSA 和其他 ACE 小組評定，以針對遭盜用的客戶及未嚴重入侵的客戶進行。 雖然不使用個別的客戶資料來建立這份檔，但我們收集了我們在評量中找出的最常被利用的弱點，以及我們對客戶的建議，以改善其 AD DS 安裝的安全性。 並非所有的弱點都適用於所有環境，所有建議也無法在每個組織中實作。

這份檔的組織方式如下：

## <a name="executive-summary"></a>執行摘要
可讀取為獨立檔或結合完整檔的執行摘要可提供本檔的高階摘要。 在執行摘要中，是我們觀察到用來危害客戶環境的最常見攻擊媒介、保護 Active Directory 安裝的摘要建議，以及打算立即部署新 AD DS 樹系或未來的客戶的基本目標。

### <a name="introduction"></a>簡介
這是您目前閱讀的區段。

### <a name="avenues-to-compromise"></a>危及系統安全的途徑
本節提供我們發現攻擊者入侵客戶的基礎結構時，最常使用的弱點的相關資訊。 本節一開始會先探討弱點的一般類別，以及如何利用這些弱點來開始滲透客戶的基礎結構、在其他系統間傳播折衷，以及最終以 AD DS 和網域控制站為目標，以取得組織樹系的完整控制權。

本節不會提供有關處理每種類型弱點的詳細建議，特別是在不使用弱點直接鎖定 Active Directory 的區域中。 不過，針對每種類型的弱點，我們提供了其他資訊的連結，可讓您用來開發對策並減少組織的受攻擊面。

### <a name="reducing-the-active-directory-attack-surface"></a>減少 Active Directory 的攻擊面
本節一開始會提供有關 Active Directory 中的特殊許可權帳戶和群組的背景資訊，以提供資訊，協助說明後續建議保護和管理特殊許可權群組和帳戶的原因。 接著，我們會討論如何減少使用高許可權帳戶進行日常管理的需求，而不需要授與群組的許可權層級，例如企業系統管理員 (EA) 、Domain Admins (DA) ，以及 (中的內建系統管理員) BA Active Directory 群組。 接下來，我們會提供指引來保護具特殊許可權的群組和帳戶，以及執行安全的管理實務和系統。

雖然本節提供有關這些設定設定的詳細資訊，但我們也包含每個建議的附錄，提供可「依原樣」使用的逐步設定指示，或可針對組織的需求進行修改。 本節的完成方式是提供資訊來安全地部署和管理網域控制站，這應該是基礎結構中最 stringently 的安全系統之一。

### <a name="monitoring-active-directory-for-signs-of-compromise"></a>監視 Active Directory 遭到危害的徵兆
無論您是否已在環境中實施穩固的安全性資訊和事件監視 (SIEM) ，或使用其他機制來監視基礎結構的安全性，本節提供的資訊可用來識別 Windows 系統上可能表示組織遭受攻擊的事件。 我們會討論傳統和 advanced audit 原則，包括在 Windows 7 和 Windows Vista 作業系統中有效設定 audit 子類別。 本節包含要進行審核之物件和系統的完整清單，以及相關的附錄會列出您應該監視的事件（如果目標是偵測入侵嘗試）。

### <a name="planning-for-compromise"></a>規劃危害因應措施
本節一開始是從技術詳細資料「回溯回」，著重于可執行檔原則和程式，以找出最重要的使用者、應用程式和系統，而不只是 IT 基礎結構，而是企業。 識別貴組織的穩定性和作業最重要的事項之後，您就可以專注于隔離和保護這些資產，不論它們是智慧財產、人員或系統。 在某些情況下，您可以在現有的 AD DS 環境中，隔離和保護資產，而在其他情況下，您應該考慮執行小規模的個別「資料格」，讓您能夠在重要資產周圍建立安全的界限，並監視這些資產的 stringently 比不重要的元件更多。 一種稱為「創意式終結」的概念，它是一種機制，可讓您藉由建立新的解決方案來消除繼承應用程式和系統，並以建議的方式，藉由結合商務和 IT 資訊來協助維護更安全的環境，以構成正常運作狀態的詳細資料。 藉由瞭解組織的一般情況，可能表示攻擊和入侵的異常狀況可能更容易識別。

### <a name="summary-of-best-practice-recommendations"></a>最佳做法建議的摘要
本節提供摘要說明本檔中所做之建議的表格，並依相對優先權排序它們，以及提供有關每個建議的詳細資訊可在檔及其附錄中找到的連結。

### <a name="appendices"></a>附錄
本檔包含附錄，以增強檔主體中所包含的資訊。 下表包含附錄清單和每個附錄的簡短描述。


|**附錄**|**說明**|
| --- | --- |
|[附錄 B：Active Directory 中具特殊權限的帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|提供背景資訊，協助您識別應該專注于保護的使用者和群組，因為攻擊者可以利用它們來入侵，甚至終結您的 Active Directory 安裝。|
|[附錄 C：Active Directory 中受保護的帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|包含 Active Directory 中受保護群組的相關資訊。 其中也包含受限制的自訂 (移除) 被視為受保護群組的群組，並受到 AdminSDHolder 和 SDProp 影響的資訊。|
|[附錄 D：保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|包含的指導方針可協助保護樹系中每個網域的系統管理員帳戶。|
|[附錄 E：保護 Active Directory 中的 Enterprise Admins 群組](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|包含協助保護樹系中 Enterprise Admins 群組的指導方針。|
|[附錄 F：保護 Active Directory 中的 Domain Admins 群組](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|包含的指導方針可協助保護樹系中每個網域中的 Domain Admins 群組。|
|[附錄 G：保護 Active Directory 中的 Administrators 群組](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|包含的指導方針可協助保護樹系中每個網域內的內建系統管理員群組。|
|[附錄 H：保護本機 Administrators 帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|包含的指導方針可協助保護已加入網域之伺服器和工作站上的本機系統管理員帳戶和系統管理員群組。|
|[附錄 I：為 Active Directory 中的受保護帳戶和群組建立管理帳戶](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|提供的資訊可讓您建立具有有限許可權的帳戶，並可進行 stringently 控制，但在需要暫時提高許可權時，可以用來填入 Active Directory 中的特殊許可權群組。|
|[附錄 L：要監視的事件](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|列出您要在環境中監視的事件。|
|[附錄 M：文件連結與建議閱讀資料](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|包含建議閱讀的清單。 也包含外部檔和其 Url 的連結清單，讓這份檔的讀取者可以存取此資訊。|



