---
ms.assetid: 34244b53-1206-4f5b-8c4d-3ebf574d8e24
title: 常成為認證竊取目標的帳戶
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 6aab5eeb405a067cc2bb83bc5e92d777869ca349
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963109"
---
# <a name="attractive-accounts-for-credential-theft"></a>常成為認證竊取目標的帳戶

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

認證竊取攻擊是指攻擊者最初獲得最高許可權 (根、系統管理員或系統，視使用中的作業系統) 存取網路上的電腦，然後使用可自由使用的工具，從其他已登入的帳戶會話解壓縮認證。 根據系統組態而定，這些認證可擷取為雜湊、票證或甚至是純文字密碼的格式。 如果有任何搜集到認證適用于可能存在於網路上其他電腦上的本機帳戶 (例如，Windows 中的系統管理員帳戶，或是 OSX、UNIX 或 Linux) 中的根帳號，攻擊者會向網路上的其他電腦提供認證，以傳播到其他電腦，並嘗試取得兩個特定帳戶類型的認證：

1.  具有廣泛和深層許可權的特殊許可權網域帳戶 (也就是具有許多電腦和 Active Directory) 的系統管理員層級許可權的帳戶。 這些帳戶可能不是 Active Directory 中任何最高許可權群組的成員，但可能已被授與網域或樹系中許多伺服器和工作站的系統管理員層級許可權，如此一來，就能有效地成為 Active Directory 中特殊許可權群組的成員。 在大部分的情況下，在 Windows 基礎結構的廣泛大量上授與高階許可權的帳戶是服務帳戶，因此服務帳戶應該一律針對廣度和深度的許可權進行評估。

2.  「非常重要的人員」 (VIP) 網域帳戶。 在本檔的內容中，VIP 帳戶是指有權存取攻擊者想要 (智慧財產權和其他機密資訊的任何帳戶) ，或任何可用來授與攻擊者存取該資訊的帳戶。 這些使用者帳戶的範例包括：

    1.  其帳戶可以存取機密公司資訊的主管

    2.  負責維護主管所使用之電腦和應用程式的技術支援人員帳戶

    3.  可存取組織投標和合約檔的合法員工帳戶，不論檔是供他們自己的組織或用戶端組織使用

    4.  產品規劃人員無論公司所做的產品類型為何，都能存取公司開發管線產品的方案和規格

    5.  研究人員使用其帳戶來存取研究資料、產品公式，或對攻擊者感興趣的任何其他研究人員

由於 Active Directory 中的高特殊許可權帳戶可以用來傳播入侵，以及管理可存取的 VIP 帳戶或資料，因此認證遭竊攻擊最有用的帳戶就是 Active Directory 的 Enterprise Admins、Domain Admins 和 Administrators 群組成員的帳戶。

由於網域控制站是 AD DS 資料庫的儲存機制，而網域控制站具有 Active Directory 中所有資料的完整存取權，因此網域控制站也會以入侵的目標、是否與認證遭竊攻擊為目標，或在一或多個具有高許可權的 Active Directory 帳戶遭到入侵之後。 雖然有許多發行集 (和許多攻擊者在描述傳遞雜湊和其他認證竊取攻擊時，) 專注于 Domain Admins 群組成員資格 (如 [減少 Active Directory 攻擊面](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)) 中所述，您可以使用此處所列的任何群組成員的帳戶來危害整個 AD DS 安裝。

> [!NOTE]
> 如需傳遞雜湊和其他認證竊取攻擊的完整資訊，請參閱[附錄 M：檔連結和建議閱讀](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)中所列的[緩和傳遞雜湊 (PTH) 攻擊和其他認證竊取技術](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf)白皮書。 如需有關如何藉由判斷敵人（有時稱為「advanced persistent 威脅」） (Apt) 的詳細資訊，請參閱 [決定的敵人和目標攻擊](https://www.microsoft.com/download/details.aspx?id=34793)。

## <a name="activities-that-increase-the-likelihood-of-compromise"></a>提高入侵可能性的活動
因為認證遭竊的目標通常是具有高度許可權的網域帳戶和 VIP 帳戶，所以系統管理員很重要的是，系統管理員必須重視可提高認證遭竊攻擊之可能性的活動。 雖然攻擊者也會以 VIP 帳戶為目標，但如果 Vip 在系統或網域中沒有較高等級的許可權，則竊取認證需要其他類型的攻擊，例如社交將 VIP 設計為提供秘密資訊。 或者，攻擊者必須先取得快取 VIP 認證之系統的特殊許可權存取權。 因此，在這裡所述之認證遭竊的可能性提高的活動主要著重在防止取得高許可權的系統管理認證。 這些活動是攻擊者能夠危害系統以取得特殊許可權認證的常見機制。

### <a name="logging-on-to-unsecured-computers-with-privileged-accounts"></a>使用具特殊許可權的帳戶登入不安全的電腦
允許認證竊取攻擊的核心弱點是登入不安全的電腦，這些帳戶在整個環境中都具有廣泛且更具許可權的帳戶。 這些登入可能是此處所述的各種錯誤配置結果。

#### <a name="not-maintaining-separate-administrative-credentials"></a>不維護個別的系統管理認證
雖然這種情況並不常見，但在評估各種 AD DS 安裝時，我們發現它的員工會使用單一帳戶來執行所有工作。 帳戶是 Active Directory 中至少一個最具高度許可權的群組的成員，而且是員工在早上登入工作站所用的相同帳戶、檢查其電子郵件、流覽網際網路網站，以及將內容下載到他們的電腦。 當使用者以授與本機系統管理員許可權的帳戶執行時，他們會公開本機電腦以完成入侵。 當這些帳戶也是 Active Directory 中最具特殊許可權之群組的成員時，就會公開整個樹系進行入侵，讓攻擊者輕鬆取得 Active Directory 和 Windows 環境的完整控制權。

同樣地，在某些環境中，我們發現在非 Windows 電腦上的根帳號使用的使用者名稱和密碼與 Windows 環境中使用的相同，可讓攻擊者將從 UNIX 或 Linux 系統洩漏到 Windows 系統，反之亦然。

#### <a name="logons-to-compromised-workstations-or-member-servers-with-privileged-accounts"></a>使用具特殊許可權的帳戶登入遭盜用的工作站或成員伺服器
當使用高度特殊許可權的網域帳戶以互動方式登入遭盜用的工作站或成員伺服器時，該遭入侵的電腦可能會從任何登入系統的帳戶搜集認證。

#### <a name="unsecured-administrative-workstations"></a>不安全的系統管理工作站
在許多組織中，IT 人員使用多個帳戶。 您可以使用一個帳戶登入員工的工作站，因為這些是 IT 人員，他們的工作站上通常會有本機系統管理員許可權。 在某些情況下，UAC 會保持啟用狀態，如此一來，使用者至少會在登入時收到分割存取權杖，而且必須在需要許可權時提升許可權。 當這些使用者執行維護活動時，通常會使用本機安裝的管理工具，並藉由選取 [以 **系統管理員身分執行** ] 選項或在出現提示時提供認證，來提供其網域特殊許可權帳戶的認證。 雖然這種設定看起來可能很適合，但它會公開環境以犧牲，因為：

-   員工用來登入工作站的「一般」使用者帳戶具有本機系統管理員許可權，而電腦很容易受到 [下載](https://www.microsoft.com/security/sir/glossary/drive-by-download-sites.aspx) 攻擊，而使用者相信要安裝惡意程式碼。

-   惡意程式碼會安裝在系統管理帳戶的內容中，而電腦現在可以用來捕捉擊鍵、剪貼簿內容、螢幕擷取畫面和記憶體內建認證，其中任何一項都可能造成功能強大的網域帳號憑證。

此案例中的問題是雙重的。 首先，雖然不同的帳戶用於本機和網域管理，但電腦是不安全的，且不會保護帳戶免于遭竊。 其次，一般使用者帳戶和系統管理帳戶被授與過多的許可權和許可權。

### <a name="browsing-the-internet-with-a-highly-privileged-account"></a>使用具有高度許可權的帳戶流覽網際網路
登入電腦的使用者，其帳戶是電腦上本機系統管理員群組的成員，或是 Active Directory 中的特殊許可權群組的成員，然後流覽網際網路 (或遭入侵的內部網路) 公開本機電腦和要危害的目錄。

使用以系統管理許可權執行的瀏覽器來存取惡意製作的網站，可讓攻擊者在特殊許可權使用者的環境中，將惡意程式碼放在本機電腦上。 如果使用者在電腦上具有本機系統管理員許可權，攻擊者可能會欺騙使用者下載惡意程式碼或開啟電子郵件附件，以利用應用程式弱點並利用使用者的許可權，將電腦上所有作用中使用者的本機快取認證解壓縮。 如果使用者在目錄中由 Active Directory 的 Enterprise Admins、Domain Admins 或 Administrators 群組成員資格來擁有系統管理許可權，則攻擊者可以解壓縮網域認證，並使用這些認證來危害整個 AD DS 網域或樹系，而不需要危及樹系中的任何其他電腦。

### <a name="configuring-local-privileged-accounts-with-the-same-credentials-across-systems"></a>使用相同的認證跨系統設定本機特殊許可權帳戶
在許多或所有電腦上設定相同的本機系統管理員帳戶名稱和密碼，可讓認證從一部電腦上的 SAM 資料庫遭竊，用來危害所有其他使用相同認證的電腦。 在每個已加入網域的系統上，您至少應該針對本機系統管理員帳戶使用不同的密碼。 本機系統管理員帳戶也可以唯一命名，但針對每個系統的特殊許可權本機帳戶使用不同的密碼，足以確保認證不能用在其他系統上。

### <a name="overpopulation-and-overuse-of-privileged-domain-groups"></a>Overpopulation 和過度利用特殊許可權的網域群組
授與網域中 EA、DA 或 BA 群組的成員資格，可為攻擊者建立目標。 這些群組的成員數目愈大，特殊許可權的使用者可能會不小心誤用認證並公開認證竊取攻擊。 具有特殊許可權的網域使用者登入的每個工作站或伺服器，都有可能會被搜集到並用來危害 AD DS 網域和樹系的可能機制。

### <a name="poorly-secured-domain-controllers"></a>安全性不佳的網域控制站
網域控制站會存放網域 AD DS 資料庫的複本。 在唯讀網域控制站的情況下，資料庫的本機複本只包含目錄中帳戶子集的認證，而這些認證預設為特殊許可權的網域帳戶。 在讀寫網域控制站上，每個網域控制站都會維護 AD DS 資料庫的完整複本，包括除了 Domain Admins 等特殊許可權使用者的認證，但是具有特殊許可權的帳戶（例如網域控制站帳戶或網域的 Krbtgt 帳戶），也就是與網域控制站上的 KDC 服務相關聯的帳戶。 如果網域控制站上沒有安裝網域控制站功能所需的其他應用程式，或網域控制站未 stringently 修補和保護，攻擊者可能會透過未修補的弱點來入侵它們，或者可以利用其他攻擊媒介直接在其上安裝惡意軟體。

## <a name="privilege-elevation-and-propagation"></a>權限提高和傳播
無論使用何種攻擊方法，在 Windows 環境受到攻擊時，一律會將 Active Directory 設為目標，因為它最終會控制攻擊者所需的存取權。 但是，這並不表示整個目錄都是以目標為目標。 特定的帳戶、伺服器和基礎結構元件通常是針對 Active Directory 的主要攻擊目標。 這些帳戶的說明如下。

### <a name="permanent-privileged-accounts"></a>永久特殊許可權帳戶
由於引進了 Active Directory，因此您可以使用高度特殊許可權的帳戶來建立 Active Directory 樹系，然後將執行日常系統管理所需的許可權委派給較低許可權的帳戶。 Active Directory 中的 Enterprise Admins、Domain Admins 或 Administrators 群組的成員資格，只需要暫時且不常在執行每日系統管理最低許可權方法的環境中。

永久特殊許可權帳戶是已放在特殊許可權群組中的帳戶，且會從一天開始。 如果您的組織將五個帳戶放入網域的 Domain Admins 群組，這五個帳戶的目標可能是一天24小時、一周七天。 不過，實際使用具有網域系統管理員許可權的帳戶通常只適用于特定的全網域設定，而且很短一段時間。

### <a name="vip-accounts"></a>VIP 帳戶
Active Directory 缺口中經常被忽略的目標，就是組織中「非常重要的人員」 (或 Vip) 的帳戶。 特殊許可權帳戶的目標是因為這些帳戶可以授與存取權給攻擊者，如此一來，就能讓使用者入侵或甚至終結目標系統，如本節稍早所述。

### <a name="privilege-attached-active-directory-accounts"></a>「附加許可權」 Active Directory 帳戶
「附加許可權」 Active Directory 帳戶是網域帳戶，這些帳戶尚未被授與 Active Directory 中具有最高等級許可權的任何群組成員，而是在環境中的許多伺服器和工作站上被授與較高等級的許可權。 這些帳戶最常是以網域為基礎的帳戶，這些帳戶是設定為在已加入網域的系統上執行服務，通常是針對在基礎結構的大型區段上執行的應用程式。 雖然這些帳戶在 Active Directory 中沒有任何許可權，但如果授與大量系統的高許可權，則可以使用它們來危害或甚至終結基礎結構的大型區段，進而達成與具有特殊許可權的 Active Directory 帳戶的相同效果。
