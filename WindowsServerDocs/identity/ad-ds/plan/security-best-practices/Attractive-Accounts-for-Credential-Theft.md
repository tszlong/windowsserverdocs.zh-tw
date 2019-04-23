---
ms.assetid: 34244b53-1206-4f5b-8c4d-3ebf574d8e24
title: 常成為認證竊取目標的帳戶
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8e5d2b01f73127f84f700dab3518b66687f0294f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863099"
---
# <a name="attractive-accounts-for-credential-theft"></a>常成為認證竊取目標的帳戶

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

認證竊取攻擊是指以攻擊者一開始取得最高的權限 （「 根 」、 「 管理員 」 或 「 系統中，根據使用中的作業系統 」） 存取網路，然後再使用免費工具，來擷取認證的電腦工作階段內的其他登入的帳戶。 根據系統組態，可擷取這些認證的雜湊、 票證或甚至是純文字密碼形式。 如果任何蒐集到的認證可能會存在於網路 （例如，在 Windows，系統管理員帳戶或在 OSX、 UNIX 或 Linux 根帳戶） 上的其他電腦上的本機帳戶，攻擊者會提供認證給其他電腦上網路傳播至其他電腦遭到入侵，並嘗試取得的兩個特定類型的帳戶認證：  

1.  使用廣泛並深入的權限 （也就是帳戶具有系統管理員層級權限，多部電腦上與 Active Directory） 的特殊權限的網域帳戶。 這些帳戶可能不到任何 Active Directory 中的最高特殊權限群組的成員，但他們可能已被授與系統管理員層級權限跨許多伺服器和工作站的網域或樹系，使其可有效地一樣強大在 Active Directory 中的特殊權限群組的成員。 在大部分情況下，獲得高的層級的權限在廣泛的 Windows 基礎結構的通關的帳戶是服務帳戶，讓服務帳戶應該一律會評估這部伺服器的廣度與深度的權限。  

2.  「 很重要的 Person 」 (VIP) 的網域帳戶。 在本文件的內容中，VIP 帳戶就可以存取資訊 （智慧財產權和其他機密資訊），攻擊者想要的任何帳戶可用來授與攻擊者存取權，該資訊的任何帳戶。 這些使用者帳戶的範例包括：  

    1.  帳戶可以存取公司機密資訊的主管  

    2.  技術支援人員負責維護的電腦和主管所使用的應用程式的帳戶  

    3.  法律的人員具有存取組織的 bid 和合約文件，文件是否為自己的組織或用戶端組織的帳戶  

    4.  產品規劃人員可存取的產品計劃與規格中的公司開發管線，無論公司的產品類型  

    5.  研究人員的帳戶用來存取研究資料、 產品的公式或攻擊者感興趣的任何其他參考資料  

最有用的帳戶的認證竊取攻擊傳播入侵和操作 VIP 帳戶或他們可以存取的資料，則可以使用 Active Directory 中的高特殊權限的帳戶，因為是 Enterprise Admins 的成員的帳戶Domain Admins 和系統管理員群組在 Active Directory 中。  

AD DS 資料庫的儲存機制的網域控制站和網域控制站在 Active Directory 中具有完整存取權的所有資料，因為網域控制站也目標的入侵，無論是在平行使用認證竊取攻擊，或之後一或多個高特殊權限的 Active Directory 帳戶已遭盜用。 雖然許多發行集 （和許多攻擊者） 著重於 Domain Admins 群組成員資格時描述 pass-雜湊和其他認證竊取攻擊 (中所述[減少 Active Directory 的攻擊面](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md))其中任一此處所列的群組成員的帳戶可以用來危害整個 AD DS 安裝。  

> [!NOTE]  
> 完整傳遞-雜湊和其他認證竊取攻擊的詳細資訊，請參閱[緩解傳遞-雜湊 (PTH) 攻擊和其他認證竊取技術](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf)（英文） 白皮書中所列[附錄 M:文件連結與建議閱讀資料](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)。 如需詳細的敵人攻擊的詳細資訊，這有時稱為 「 進階持續威脅 」 (Apt)，請參閱[決定敵人和目標的攻擊](https://www.microsoft.com/download/details.aspx?id=34793)。  

## <a name="activities-that-increase-the-likelihood-of-compromise"></a>增加的洩露可能性的活動  
因為高特殊權限的網域帳戶和 VIP 帳戶的認證竊取目標通常是，務必讓系統管理員的活動，增加的認證竊取攻擊成功的可能性。 雖然如果 Vip 系統不會提供高水準的系統上，或在網域中的權限，攻擊者也會以 VIP 帳戶，其認證遭竊會需要其他類型的攻擊，例如社交工程提供祕密資訊的 VIP。 或者，攻擊者必須先取得特殊權限的存取的 VIP 快取認證的系統。 因為這個緣故，提高此處所述的認證竊取的可能性的活動是主要著重於防止取得高特殊權限的系統管理認證的作業。 這些活動是常見的攻擊者會如何破壞系統，以取得特殊權限的認證的機制。  

### <a name="logging-on-to-unsecured-computers-with-privileged-accounts"></a>不安全的電腦，使用具有特殊權限的帳戶登入  
可讓認證竊取攻擊成功的核心弱點可能會是登入電腦不是安全使用大致的帳戶和整個環境深特殊權限的動作。 這些登入可以是此處所述的各種設定錯誤的結果。  

#### <a name="not-maintaining-separate-administrative-credentials"></a>不維護個別的系統管理認證  
雖然這是相當罕見，評估不同的 AD DS 安裝，我們發現使用單一帳戶的所有工作的 IT 員工。 至少其中一個 Active Directory 中的最高特殊權限群組的成員，並且是相同的帳戶的員工用來在早上登入他們的工作站、 檢查其電子郵件、 瀏覽網際網路網站，並下載內容，其電腦。 當使用者執行與被授與本機系統管理員權限和權限的帳戶時，它們會公開本機電腦，才能完成洩露。 當這些帳戶也是在 Active Directory 中的最高權限群組的成員時，它們會公開入侵，整個樹系，輕鬆地透過極簡方式攻擊者取得完整的控制權，Active Directory 和 Windows 環境的集。  

同樣地，在某些環境中，我們發現，相同的使用者名稱和密碼會用於在非 Windows 電腦上的根帳戶因為會使用在 Windows 環境中，可讓攻擊者若要從 UNIX 或 Linux 系統的入侵延伸到 Windows 系統反之亦然。

#### <a name="logons-to-compromised-workstations-or-member-servers-with-privileged-accounts"></a>遭盜用的工作站或成員伺服器，使用具有特殊權限的帳戶登入  
高特殊權限的網域帳戶來登入以互動方式的入侵的工作站或成員伺服器，該遭到入侵的電腦，可能會取得從登入系統的任何帳戶的認證。  

#### <a name="unsecured-administrative-workstations"></a>不安全的系統管理工作站  
在許多組織來說，IT 人員會使用多個帳戶。 一個帳戶用於員工的工作站，登入，他們因為這些都是 IT 人員，通常會有本機系統管理員權限在其工作站上。 在某些情況下，UAC 會保持啟用，讓使用者至少會收到分割存取權杖在登入時，並必須提高權限時所需權限。 當這些使用者正在執行維護活動時，它們通常使用本機安裝的管理工具，並提供其網域權限的帳戶，方法是選取的認證**系統管理員身分執行**選項或提供的認證，當系統提示您。 雖然這項設定可能都不適合的它會公開入侵，因為環境：  

-   員工使用自己的工作站登入的 「 一般 」 的使用者帳戶具有本機系統管理員權限、 電腦容易遭受[偷渡式下載](https://www.microsoft.com/security/sir/glossary/drive-by-download-sites.aspx)所在使用者相信安裝惡意程式碼的攻擊。  

-   惡意程式碼會安裝在系統管理帳戶的內容，該電腦可以現在用來擷取按鍵輸入、 剪貼簿內容、 螢幕擷取畫面和常駐記憶體的認證，任何一項都可能會導致公開的功能強大的網域認證帳戶。  

在此案例中的問題有兩個。 首先，雖然本機和網域系統管理使用不同的帳戶，但電腦不安全，並不會保護防止帳戶。 第二，一般使用者帳戶和系統管理帳戶已被授與過多的權利與權限。  

### <a name="browsing-the-internet-with-a-highly-privileged-account"></a>瀏覽網際網路具有高特殊權限的帳戶  
登入帳戶屬於本機 Administrators 群組的電腦上，或是 Active Directory 中的特殊權限群組的成員，以及誰然後瀏覽網際網路 （或遭入侵的內部網路） 與電腦的使用者公開 （expose) 的本機電腦，用來危害目錄。  

使用具有系統管理權限執行的瀏覽器存取惡意的網站可以讓攻擊者存款的特殊權限的使用者內容中的本機電腦上的惡意程式碼。 如果使用者在電腦上具有本機系統管理員權限，攻擊者可以欺騙使用者下載惡意程式碼或開啟電子郵件附件，對於利用應用程式弱點及運用使用者的權限，才能擷取在本機快取在電腦上的所有作用中使用者的認證。 如果使用者具有系統管理權限在目錄中被中 Enterprise Admins、 Domain Admins 或 Active Directory 中的系統管理員群組的成員資格，攻擊者可以擷取網域認證，並使用它們來危害整個 AD DS 網域或樹系中，而不需入侵任何樹系中的其他電腦。  

### <a name="configuring-local-privileged-accounts-with-the-same-credentials-across-systems"></a>設定本機的特殊權限的帳戶，在系統中相同的認證  
設定從一部電腦，用來危害所有使用相同的認證之其他電腦的 SAM 資料庫竊取的多個或所有電腦啟用認證相同的本機系統管理員帳戶名稱和密碼。 至少，您應該使用不同的密碼本機系統管理員帳戶的每個已加入網域的系統。 本機系統管理員帳戶也可能唯一名稱，但不足以確保認證無法使用於其他系統上每個系統的特殊權限的本機帳戶使用不同的密碼。  

### <a name="overpopulation-and-overuse-of-privileged-domain-groups"></a>Overpopulation 和過度使用的特殊權限的網域群組  
在 EA、 DA 或 BA 網域中群組的成員資格授與攻擊者建立目標。 大量的這些群組的成員，較大的可能性，特殊權限的使用者可能會不小心誤用認證和公開 （expose） 這些認證竊取攻擊。 每個工作站或伺服器登入的特殊權限的網域使用者提供的特殊權限的使用者的認證可能是蒐集到，且用來危害 AD DS 網域和樹系可能的機制。  

### <a name="poorly-secured-domain-controllers"></a>不安全的網域控制站  
網域控制站裝載網域的 AD DS 資料庫的複本。 唯讀網域控制站，如果資料庫的本機複本會包含只有部分在目錄中，都不會是預設的特殊權限的網域帳戶的帳戶的認證。 讀寫網域控制站上，每個網域控制站維護 AD DS 資料庫、 包含不只適用於特殊權限的使用者，例如 Domain Admins，但具有特殊權限的帳戶，例如網域控制站帳戶或網域的 Krbtgt 認證的完整複本帳戶，是在網域控制站的 KDC 服務相關聯的帳戶。 如果不是必要的網域控制站功能的其他應用程式安裝在網域控制站上，或是網域控制站不會嚴格修補並受到保護，攻擊者可能會透過未修補的弱點，或者危害可能會利用其他直接是在其上安裝惡意軟體的攻擊媒介。  

## <a name="privilege-elevation-and-propagation"></a>權限提升和傳用  
不論使用的攻擊方法，Active Directory 一律為目標的 Windows 環境遭受攻擊時，此，因為它最終還是會控制存取權以任何攻擊者想。 這不表示針對整個目錄，不過。 通常是 Active Directory 的攻擊的主要目標的特定帳戶、 伺服器和基礎結構元件。 這些帳戶會如下所述。  

### <a name="permanent-privileged-accounts"></a>永久特殊權限的帳戶  
因為 Active Directory 的引進，它已經可以使用具有高度權限的帳戶，以建置 Active Directory 樹系並再將委派權限和執行較低權限帳戶的日常管理工作所需的權限。 在 Active Directory 中 Enterprise Admins、 Domain Admins 或 Administrators 群組的成員資格才需要暫時及不常實作最低權限的方法，可每日管理的環境中。  

永久特殊權限的帳戶是放置於特殊權限的群組和有保留每日的帳戶。 如果您的組織會將五個帳戶放入網域的 Domain Admins 群組，這些五個帳戶可以是目標的 24 小時一天、 一週七天。 不過，實際需要使用以網域系統管理員權限的帳戶是通常僅適用於特定的全網域的組態，和短的時間。  

### <a name="vip-accounts"></a>VIP 帳戶  
在 Active Directory 缺口經常被忽略的目標是 「 非常重要的 person 」 （或 Vip） 的帳戶組織中。 特殊權限的帳戶為目標，因為這些帳戶可以存取權授與攻擊者，可讓它們危害或甚至摧毀目標的系統，如先前所述，這一節。  

### <a name="privilege-attached-active-directory-accounts"></a>「 特殊權限連結 」 的 Active Directory 帳戶  
「 特殊權限連結 」 的 Active Directory 帳戶是未進行任何最高層級的權限，在 Active Directory，但改為已授與權限，在許多伺服器上的高層級的群組成員的網域帳戶和在環境中的工作站。 這些帳戶會設定已加入網域的系統，通常執行的應用程式的基礎結構的大型區段上執行服務的大部分通常以網域為基礎的帳戶。 雖然這些帳戶在 Active Directory 中，有沒有權限，如果他們所獲得大量的系統上的高特殊權限，他們可以用來危害或甚至摧毀基礎結構，達到相同的效果遭到入侵的大型區段特殊權限的 Active Directory 帳戶。  
