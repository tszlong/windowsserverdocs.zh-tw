---
ms.assetid: 84754c23-f039-4de4-a378-853942e662df
title: "簡介"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dc89afc47eb78a388238e8edf5059b0bec3006ad
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="introduction"></a>簡介

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

攻擊運算基礎結構，不論是簡單或複雜曾在的電腦。 但是，在過去十，越來越規模的組織所有，在所有部分的世界中已攻擊並危害有明顯的變更威脅景致的方式。 使用碼表進行速率增加充滿網路-戰爭和網路犯罪。 「Hacktivism，」中攻擊引發的 activist 位置，已聲稱之破壞數目的動機是透過組織的機密資訊，若要建立服務指，或甚至破壞基礎結構。 使用 exfiltrating 的公開和私人機構攻擊組織的診斷的作業」(IP) 已成為普遍。  
  
不組織的基礎結構的資訊 (IT) 技術會受到攻擊，但如果實作適當的原則、處理程序，以及控制保護組織運算基礎結構金鑰區段，來自攻擊入侵完成危害的重大問題可能預防。 數字和規模的組織外部來自攻擊最近幾年已經 eclipsed 測試人員威脅，因為這份文件通常討論外部攻擊，而非不當環境授權使用者。 儘管如此，這份文件中所提供的建議與原則是以協助保護您的環境針對外部攻擊者和誤導或惡意的測試人員。  
  
資訊和建議提供，本文件中的一些來源繪製和推斷的做法是設計用來保護中毒 Active Directory 安裝。 雖然它並不可避免攻擊，以減少 Active Directory 攻擊，以及執行控制項，可讓可能是更難 directory 危害的攻擊。 本文件會提供我們的安全漏洞的最常見的類型危害的環境中觀察到，我們已經針對以改善安全性其 Active Directory 安裝的最常見的建議。  
  
## <a name="account-and-group-naming-conventions"></a>慣例 account 和命名群組  
下表提供命名規格使用本文件中的群組與帳號在文件中所參照指南。 下表包含是每個 account 日群組，其名稱，以及如何這些帳號日群組參考本文件中的位置。  
  


|**Account 群組的位置**|**Account 日群組的名稱**|**其參考本文件中的方式**|
| --- | --- | --- |   
|Active Directory-每個網域|系統管理員|建管理員|  
|Active Directory-每個網域|系統管理員|建系統管理員 (BA) 群組|  
|Active Directory-每個網域|網域系統管理員 」|系統管理員 (DA) 群組|  
|Active Directory-森林根網域|企業系統管理員|企業系統管理員 (EA) 群組|  
|本機電腦安全性帳號管理程式資料庫上執行 Windows Server 和工作站未網域控制站的電腦|系統管理員|本機系統管理員 account|  
|本機電腦安全性帳號管理程式資料庫上執行 Windows Server 和工作站未網域控制站的電腦|系統管理員|本機|  
  
## <a name="about-this-document"></a>關於此文件  
Microsoft 資訊安全風險管理 (ISRM) 組織，也就是部分的 Microsoft 的資訊技術 (MSIT)，適用於內部商務單位、外部針對，以及 industry 同儕收集、散播，並定義原則、做法的規範，以及控制。 此資訊可用於透過 Microsoft 和我們針對提高安全性，並減少他們 IT 基礎架構的攻擊。 本文件中所提供的建議依據一些資訊來源，用於 MSIT 和 ISRM 做法的規範。 下列章節提供詳細資訊出處本文件。  
  
### <a name="microsoft-it-and-isrm"></a>Microsoft IT 和 ISRM  
已在安全的 Microsoft AD DS 森林和網域 MSIT 和 ISRM 開發出的做法，以及控制。 這些控制項所廣泛適用，其已經整合至本文件。 安全 T（新興技術方案加速器）是在其許可是找出新的技術，以及定義安全性需求與控制項，來加速採用 ISRM 團隊。  
  
### <a name="active-directory-security-assessments"></a>Active Directory 安全性評估  
在 Microsoft ISRM、評定、查閱和工程 (A) 小組與內部 Microsoft 商務單位外部針對評估應用程式和安全性基礎結構及提供策略與策略的指導方針增加組織的安全性狀態運作。 一個 a 服務提供是 Active Directory 安全性評定 (ADSA)，也就是組織的 AD DS 環境中的人員、程序和技術評估並製作客戶特定建議的整體評估。 針對提供的組織的唯一特性、方式，以及風險嚮往為基礎的建議。 將安裝 Microsoft Active Directory 除了我們針對的執行 ADSAs。 隨著時間，建議的一些找到跨不同的大小和業界針對會適用。  
  
### <a name="content-origin-and-organization"></a>內容原點與組織  
大部分的 content 本文件被從 ADSA 和危害的針對和尚未發生重大入侵者針對執行其他 a 小組評量。 客戶個人資料無法用來建立這份文件，雖然我們已經收集我們找出利用最常弱點我們評估和建議我們已經針對改善其 AD DS 安裝的安全性。 並非所有的安全漏洞適用於所有環境，也不是所有建議可以在每個組織實作。  
  
本文件是來整理，如下所示：  
  
## <a name="executive-summary"></a>執行摘要  
高階主管摘要，可讀取獨立文件或搭配完整的文件，提供高階摘要本文件。 高階主管摘要包括我們已經觀察到用來危害客戶環境，摘要建議保護 Active Directory 安裝以及基本目標針對人員計劃部署新 AD DS 森林現在或在未來的最常見的攻擊。  
  
### <a name="introduction"></a>簡介  
這是您會立即朗讀的區段。  
  
### <a name="avenues-to-compromise"></a>途徑危害  
本章節提供資訊一些最常運用我們發現攻擊者會用來危害針對的基礎結構的資訊安全風險。 本章節弱點，以及他們如何運用一開始侵入針對的基礎結構，額外的系統上傳播危害和最後為目標，以取得組織的樹系的完整控制權 AD DS 和網域控制站的一般分類的開頭。  
  
本章節不提供詳細位址各種弱點，尤其是在區域中的弱點不用來直接目標 Active Directory 中相關的建議。 不過，適用於各種弱點，我們也提供其他資訊可供您開發措施並降低您的組織攻擊 surface 的連結。  
  
### <a name="reducing-the-active-directory-attack-surface"></a>減少 Active Directory 攻擊  
本章節開始提供權限的帳號及群組以提供的資訊，協助澄清保護和管理特殊權限的群組帳號後續建議的原因 Active Directory 中的背景資訊。 我們然後討論方法，以減少使用高度授權的帳號日常的系統管理，不需要的權限授與給群組，例如企業系統管理員 (EA)、網域系統管理員 (DA)，以及建系統管理員 (BA) 群組 Active Directory 中層級。 接下來，我們提供指導方針保護的特殊權限的群組和帳號和實作安全管理的方式與系統。  
  
雖然這個區段會提供這些設定的詳細的資訊，我們也包含逐步設定指示操作，可以使用「現狀」，或者可以經過修改，提供的組織所需的每個建議附錄。 本章節完成提供安全地部署與管理網域控制站，應該會在系統嚴格最安全的基礎結構的資訊。  
  
### <a name="monitoring-active-directory-for-signs-of-compromise"></a>Active Directory 監視危害的符號  
無論您已實作穩定的安全性資訊與事件 (SIEM) 監視您的環境中使用其他機制來監視基礎結構的安全性，本節可用來辨識在 Windows 系統可能會被攻擊組織的活動的資訊。 我們討論傳統和進階稽核原則，包括有效的稽核子設定，在 Windows 7 和 Windows Vista 作業系統。 本節物件的稽核，系統的完整清單，並相關的附錄會列出的活動的您應該會監視目標是否偵測入侵嘗試。  
  
### <a name="planning-for-compromise"></a>規劃區域的入侵  
本章節一開始先從技術的詳細資料，即可對焦於原則和可實作找出使用者、應用程式，以及限於 IT 基礎結構最重要的系統處理程序「逐步返回「企業，但。 檢測軍人最重要的穩定性和您的組織的作業之後, 您可以專注於分離和保護資產是否有診斷作業人員或系統。 有時候，分離和保護資產可能會執行在現有的 AD DS 環境，而在其他案例，您應該考慮實作小的不同「儲存格」可讓您建立重大資產在安全的邊界，比較不重要元件嚴格監視資產。 討論稱為「創意破壞]，是一種機制來傳統應用程式和系統可以排除來建立新的方案的概念，並區段結尾的建議，可協助以更安全的環境維護藉由組合企業及 IT 建構圖片功能正常運作狀態的詳細的資訊。 藉由行車建議等項目正常現象組織，可以比較容易辨識異常攻擊及折衷可能表示。  
  
### <a name="summary-of-best-practice-recommendations"></a>最佳做法建議的摘要  
本節提供表摘要本文件中所提供的建議，並訂單它們來優先權，除了提供位置可以找到詳細的資訊，有關每個建議，其附錄文件中的連結。  
  
### <a name="appendices"></a>附錄  
本文件擴大本文件中所包含的資訊，均附錄。 附錄和每的簡短描述清單會包含表。  
  
 
|**附錄**|**描述**|
| --- | --- | 
|[B 附錄特殊權限的帳號及 Active Directory 中的群組](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)|提供協助找出使用者和群組，您應該會對焦於因為它們可以利用攻擊者甚至破壞 Active Directory 安裝侵入您，並保護您的背景資訊。|  
|[C：附錄受保護的帳號及 Active Directory 中的群組](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)|包含受保護的群組 Active Directory 中相關資訊。 它也包含群組視為受保護的群組 AdminSDHolder 和 SDProp 會受到限制自訂 （移除） 的資訊。|  
|[在 Active Directory 中附錄 d 保護建系統管理員帳號](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)|包含可協助保護森林中的每個網域中管理員指導方針。|  
|[在 Active Directory 中附錄 e 保護企業管理員群組](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)|包含指導方針操作，以協助保護森林中的企業系統管理員 」 群組。|  
|[在 Active Directory 中附錄 f︰ 保護網域管理員群組](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)|包含指導方針操作，以協助保護森林中的每個網域中的網域系統管理員 」 群組。|  
|[在 Active Directory 中附錄 g：保護系統管理員群組](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)|包含可協助保護建系統管理員群組森林中的每個網域中的指導方針。|  
|[附錄 H:WINDOWS 保護本機系統管理員帳號，並群組](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)|包含指導方針操作，以協助安全本機系統管理員帳號，並加入網域的伺服器上工作站系統管理員 」 群組。|  
|[附錄 i：建立管理帳號受保護的帳號和 Active Directory 中的群組](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)|提供資訊，以建立帳號，有限的權限但可以嚴格控制，可以用來需要暫時提高權限時填入 Active Directory 中有特殊權限的群組。|  
|[事件監視器附錄 l:](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)|列出的活動，您應該會監視您的環境中。|  
|[附錄 m：文件的連結，並建議朗讀](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)|包含建議朗讀的清單。 也包含清單連結到外部文件，以及他們的 Url，讀卡機的硬碟份文件可以存取此資訊。|  
  


