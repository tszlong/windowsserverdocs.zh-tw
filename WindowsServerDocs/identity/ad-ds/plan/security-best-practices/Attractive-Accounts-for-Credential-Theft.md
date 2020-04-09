---
ms.assetid: 34244b53-1206-4f5b-8c4d-3ebf574d8e24
title: 常成為認證竊取目標的帳戶
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: acaf24feb95cae1d4d99a0f121299368b774e7a4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821321"
---
# <a name="attractive-accounts-for-credential-theft"></a>常成為認證竊取目標的帳戶

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

認證竊取攻擊是攻擊者最初取得最高許可權（root、Administrator 或 SYSTEM，視使用中的作業系統而定）存取網路上的電腦，然後使用自由可用的工具，從其他已登入之帳戶的會話中解壓縮認證。 視系統組態而定，這些認證可以用雜湊、票證或甚至是純文字密碼的形式來解壓縮。 如果有任何搜集到認證適用于可能存在於網路上其他電腦上的本機帳戶（例如，Windows 中的系統管理員帳戶，或 OSX、UNIX 或 Linux 中的根帳號），攻擊者會向網路上的其他電腦出示認證，以將危害傳播到其他電腦，並嘗試取得兩種特定帳戶類型的認證:  

1.  具有廣泛和深層許可權的特殊許可權網域帳戶（也就是在多部電腦上具有系統管理員層級許可權的帳戶，以及 Active Directory）。 這些帳戶可能不是 Active Directory 中任何最高許可權群組的成員，但可能已在網域或樹系中的許多伺服器和工作站上獲得系統管理員層級的許可權，如此一來，就能有效地與 Active Directory 中的特殊許可權群組成員一樣強大。 在大部分情況下，已在 Windows 基礎結構的廣泛大量上授與高層級許可權的帳戶是服務帳戶，因此應該一律評估服務帳戶的廣度和深度。  

2.  「非常重要的人員」（VIP）網域帳戶。 在本檔的內容中，VIP 帳戶是可存取攻擊者所需資訊的任何帳戶（智慧財產和其他機密資訊），或任何可以用來授與攻擊者存取該資訊的帳戶。 這些使用者帳戶的範例包括：  

    1.  其帳戶可存取機密公司資訊的主管  

    2.  負責維護主管所使用之電腦和應用程式的技術支援人員帳戶  

    3.  可存取組織投標和合約檔的合法員工帳戶，不論檔是針對自己的組織或用戶端組織  

    4.  可存取公司開發管線中產品之方案和規格的產品規劃人員，不論公司製作的產品類型為何  

    5.  研究人員的帳戶是用來存取研究資料、產品公式或任何其他對攻擊者感關注的研究  

因為 Active Directory 中的高許可權帳戶可以用來傳播入侵，以及操作 VIP 帳戶或其可存取的資料，所以認證竊取攻擊最有用的帳戶是 Active Directory 中的 Enterprise Admins、Domain Admins 和 Administrators 群組成員的帳戶。  

因為網域控制站是 AD DS 資料庫的存放庫，而網域控制站具有 Active Directory 中所有資料的完整存取權，所以網域控制站的目標也會受到危害，不論是與認證竊取攻擊，還是一或多個高許可權的 Active Directory 帳戶遭到入侵。 雖然許多發行集（以及許多攻擊者）在描述傳遞雜湊和其他認證竊取攻擊（如[減少 Active Directory 攻擊面](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)中所述）時，都著重在 Domain Admins 群組成員資格，但您也可以使用此處所列之任何群組的成員帳戶來危害整個 AD DS 安裝。  

> [!NOTE]  
> 如需有關傳遞雜湊和其他認證竊取攻擊的完整資訊，請參閱[附錄 M：檔連結和建議閱讀](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)中所列的[緩和傳遞雜湊（PTH）攻擊和其他認證竊取技術](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf)白皮書。 如需透過判斷敵人（有時稱為「先進的持續性威脅」（Apt））攻擊的詳細資訊，請參閱[判斷的敵人和目標攻擊](https://www.microsoft.com/download/details.aspx?id=34793)。  

## <a name="activities-that-increase-the-likelihood-of-compromise"></a>增加危害可能性的活動  
由於認證竊取的目標通常是具有高許可權的網域帳戶和 VIP 帳戶，因此系統管理員很重視可提高認證竊取攻擊成功機率的活動。 雖然攻擊者也將目標設為 VIP 帳戶，但如果未在系統或網域中提供較高層級的許可權，則竊取其認證需要其他類型的攻擊，例如社交設計 VIP 以提供秘密資訊。 或者，攻擊者必須先取得快取 VIP 認證之系統的特殊許可權存取權。 因此，提高認證竊取可能性的活動主要著重于防止取得高特殊許可權的系統管理認證。 這些活動是常見的機制，攻擊者可以利用這種方式來危害系統，以取得特殊許可權的認證。  

### <a name="logging-on-to-unsecured-computers-with-privileged-accounts"></a>使用具有特殊許可權的帳戶登入不安全的電腦  
允許認證竊取攻擊成功的核心弱點，是登入不安全的電腦的動作，而這些帳戶在整個環境中廣泛且具有深度許可權。 這些登入可能是此處所述的各種錯誤的結果。  

#### <a name="not-maintaining-separate-administrative-credentials"></a>不維護個別的系統管理認證  
雖然這種情況並不常見，但在評估各種 AD DS 安裝時，我們發現 IT 員工使用單一帳戶來執行其所有工作。 帳戶是 Active Directory 中至少一個最高許可權群組的成員，而且是員工每天用來登入工作站的相同帳戶，檢查其電子郵件、流覽網際網路網站，以及將內容下載到他們的電腦。 當使用者使用已授與本機系統管理員許可權的帳戶來執行時，會公開本機電腦來完成危害。 當那些帳戶同時也是 Active Directory 中最具特殊許可權的群組成員時，他們會公開整個樹系來入侵，讓攻擊者完整輕鬆地取得 Active Directory 和 Windows 環境的完全控制權。  

同樣地，在某些環境中，我們發現，相同的使用者名稱和密碼會用於非 Windows 電腦上的根帳號，如同 Windows 環境中所使用的一樣，這可讓攻擊者將入侵從 UNIX 或 Linux 系統延伸到 Windows 系統，反之亦然。

#### <a name="logons-to-compromised-workstations-or-member-servers-with-privileged-accounts"></a>使用特殊許可權帳戶登入遭到入侵的工作站或成員伺服器  
當使用高許可權的網域帳戶以互動方式登入遭盜用的工作站或成員伺服器時，該遭到入侵的電腦可能會從登入系統的任何帳戶收集認證。  

#### <a name="unsecured-administrative-workstations"></a>不安全的系統管理工作站  
在許多組織中，IT 人員使用多個帳戶。 一個帳戶用來登入員工的工作站，而且因為這些是 IT 人員，他們通常會在工作站上具有本機系統管理員許可權。 在某些情況下，UAC 會保持啟用狀態，讓使用者至少會在登入時收到分割存取權杖，而且必須在需要許可權時提升。 當這些使用者執行維護活動時，他們通常會使用本機安裝的管理工具，並藉由選取 [**以系統管理員身分執行**] 選項或在出現提示時提供認證，來提供其網域許可權帳戶的認證。 雖然這種設定看起來可能很適合，但它會使環境暴露于洩露，因為：  

-   員工用來登入工作站的「一般」使用者帳戶具有本機系統管理員許可權，而電腦容易遭受以[磁片磁碟機為依據的下載](https://www.microsoft.com/security/sir/glossary/drive-by-download-sites.aspx)攻擊，使用者相信要安裝惡意程式碼。  

-   惡意程式碼安裝在系統管理帳戶的內容中，電腦現在可以用來捕獲擊鍵、剪貼簿內容、螢幕擷取畫面和記憶體常駐認證，其中任何一項都可能會導致公開強大網域帳戶的認證。  

此案例中的問題是雙重的。 首先，雖然會使用個別帳戶來進行本機和網域系統管理，但電腦並不安全，而且不會保護帳戶免于遭竊。 第二，已授與一般使用者帳戶和系統管理帳戶過多的權利和許可權。  

### <a name="browsing-the-internet-with-a-highly-privileged-account"></a>使用高許可權帳戶流覽網際網路  
使用電腦上本機 Administrators 群組成員的帳戶登入電腦的使用者，或 Active Directory 中的特殊許可權群組成員，然後流覽網際網路（或遭入侵的內部網路），會公開本機電腦和要洩露的目錄。  

以具有系統管理許可權的瀏覽器來存取惡意製作的網站，可以讓攻擊者將惡意程式碼放在本機電腦的具特殊許可權使用者的內容中。 如果使用者具有電腦上的本機系統管理員許可權，攻擊者可能會欺騙使用者下載惡意程式碼，或開啟利用應用程式弱點的電子郵件附件，並利用使用者的許可權將電腦上所有作用中使用者的本機快取認證解壓縮。 如果使用者在目錄中擁有系統管理許可權，而這些成員是在 Active Directory 的 Enterprise Admins、Domain Admins 或 Administrators 群組中，攻擊者就可以將網域認證解壓縮，並使用它們來危害整個 AD DS 網域或樹系，而不需要危害樹系中的任何其他電腦。  

### <a name="configuring-local-privileged-accounts-with-the-same-credentials-across-systems"></a>以相同的認證在系統間設定本機特殊許可權帳戶  
在許多或所有電腦上設定相同的本機系統管理員帳戶名稱和密碼，可讓您從一部電腦上的 SAM 資料庫竊取認證，以用來危害使用相同認證的其他所有電腦。 您至少應該在每個加入網域的系統上，使用不同的本機系統管理員帳戶密碼。 本機系統管理員帳戶也可以唯一命名，但對每個系統的特殊許可權本機帳戶使用不同的密碼，足以確保認證不能用於其他系統。  

### <a name="overpopulation-and-overuse-of-privileged-domain-groups"></a>Overpopulation 和濫用特殊許可權網域群組  
在網域中授與 EA、DA 或 BA 群組的成員資格，會建立攻擊者的目標。 這些群組的成員數目越大，有許可權的使用者可能會不小心誤用認證，並暴露于認證竊取攻擊的可能性。 具有特殊許可權的網域使用者登入的每個工作站或伺服器，都會顯示可能的機制，讓特殊許可權使用者的認證可以搜集到並用來危害 AD DS 網域和樹系。  

### <a name="poorly-secured-domain-controllers"></a>安全性不佳的網域控制站  
網域控制站會存放網域 AD DS 資料庫的複本。 在唯讀網域控制站的情況下，資料庫的本機複本只包含目錄中帳戶子集的認證，預設為沒有許可權的網域帳戶。 在讀寫網域控制站上，每個網域控制站都會維護 AD DS 資料庫的完整複本，包括網域系統管理員等特殊許可權使用者的認證，但許可權帳戶（例如網域控制站帳戶或網域的 Krbtgt 帳戶，這是與網域控制站上的 KDC 服務相關聯的帳戶）。 如果網域控制站的其他不需要的應用程式是安裝在網域控制站上，或網域控制站未而更加嚴格修補並受到保護，攻擊者可能會透過未修補的弱點來危害它們，或利用其他攻擊媒介直接在其上安裝惡意軟體。  

## <a name="privilege-elevation-and-propagation"></a>許可權提升和傳播  
無論使用的攻擊方法為何，在 Windows 環境遭受攻擊的情況下，Active Directory 一律是目標，因為它最終會控制攻擊者所需的存取權。 不過，這並不表示整個目錄都是以為目標。 特定帳戶、伺服器和基礎結構元件通常是針對 Active Directory 的主要攻擊目標。 這些帳戶的說明如下。  

### <a name="permanent-privileged-accounts"></a>永久的特殊許可權帳戶  
由於引進了 Active Directory，因此您可以使用高許可權帳戶來建立 Active Directory 樹系，然後委派執行日常管理工作所需的許可權給較低許可權的帳戶。 在 Active Directory 中，Enterprise Admins、Domain Admins 或 Administrators 群組的成員資格只需要暫時且不常出現在執行最低許可權方法進行日常管理的環境中。  

永久的特殊許可權帳戶是已放在特殊許可權群組中，並保留在每天的帳戶。 如果您的組織將五個帳戶放入網域的 Domain Admins 群組，這五個帳戶的目標可能是每天24小時，每週七天。 不過，實際需要使用具有網域系統管理員許可權的帳戶，通常僅適用于特定網域的設定，而且短時間內。  

### <a name="vip-accounts"></a>VIP 帳戶  
在 Active Directory 缺口中經常被忽略的目標，是組織中「非常重要的人員」（或 Vip）的帳戶。 許可權帳戶的目標是因為這些帳戶可以授與攻擊者存取權，讓他們能夠危害或甚至終結目標系統，如本節稍早所述。  

### <a name="privilege-attached-active-directory-accounts"></a>「附加許可權」 Active Directory 帳戶  
「附加許可權」 Active Directory 帳戶是網域帳戶，尚未成為在 Active Directory 中具有最高許可權層級之任何群組的成員，而是在環境中的許多伺服器和工作站上被授與高層級的許可權。 這些帳戶通常是以網域為基礎的帳戶，設定為在已加入網域的系統上執行服務，通常適用于在基礎結構的大型區段上執行的應用程式。 雖然這些帳戶在 Active Directory 中沒有許可權，但如果他們被授與大量系統的高許可權，則可以用來危害或甚至終結基礎結構的大型區段，達到與特殊許可權 Active Directory 帳戶的危害相同的效果。  
