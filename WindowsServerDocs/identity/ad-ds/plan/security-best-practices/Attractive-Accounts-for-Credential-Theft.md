---
ms.assetid: 34244b53-1206-4f5b-8c4d-3ebf574d8e24
title: "認證竊取帳號吸引"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ae1bfe017e8b21c3abfcdc137153b0fd379053fa
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="attractive-accounts-for-credential-theft"></a>認證竊取帳號吸引

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

認證竊取攻擊就是在其中一開始攻擊最高權限 （根，系統管理員或根據使用中的作業系統系統，） 的存取權的網路，然後使用免費工具從其他登入帳號的工作階段解壓縮認證的電腦。 系統設定，根據這些認證可以 hashes、 門票或甚至純文字密碼的形式解壓縮。 如果任何收割後的認證區域 （例如，在 Windows 中，系統管理員帳號或根帳號 OSX、 UNIX，或 Linux） 的網路上的其他電腦上有可能是帳號，攻擊者提供的認證到其他電腦傳播危害到其他電腦，並取得帳號兩種特定類型的憑證嘗試在網路:  

1.  有特殊權限的網域帳號 broad 和深度的權限 （也就是帳號有多部電腦上與 Active Directory 中的系統管理員等級權限）。 這些帳號不可能的 Active Directory 中的最高權限群組成員，但它們可能會被授與系統管理員等級權限許多伺服器和工作站網域或森林，讓它們在 Active Directory 有效地權力特殊權限群組成員。 在大部分案例中，帳號授與權限等級高 broad 負責 Windows 基礎結構所有的服務帳號，因此隨時可評估服務帳號幅度和深度的權限。  

2.  「 非常重要的人員] (VIP) 網域帳號。 本文件處在 VIP account 是攻擊想 （診斷作業和其他重要資訊），資訊的存取權的任何帳號或任何帳號，可以用來完全掌控存取該資訊。 這些帳號的範例包括：  

    1.  高階主管其帳號存取敏感公司資訊  

    2.  帳號，有義務維護主管所使用的應用程式與電腦之人員協助 Desk 人員  

    3.  帳號法律員工是否可以存取組織超出定約和合約文件自己公司或組織 client 的文件  

    4.  管線 product 規劃存取計劃與規格你有公司的開發人員，無論你會公司的類型  

    5.  其帳號用於存取研究資料、 product 遭遇或任何其他研究攻擊感興趣的研究人員  

因為在 Active Directory 高度授權的帳號傳播危害和操作 VIP 帳號或他們可以存取的資料，認證竊取攻擊的最適合帳號是帳號，在 Active Directory 中的企業系統管理員，網域系統管理員及系統管理員群組成員。  

因為網域控制站的 AD DS 資料庫存放庫中，網域控制站在 Active Directory 擁有完整存取權的所有資料網域控制站的也針對洩露是否同時認證竊取攻擊，或一或多個高特殊權限的 Active Directory 之後已遭入侵帳號。 雖然許多發行 （和許多攻擊） 專注於網域系統管理員群組成員資格描述 pass hash 和其他認證竊取攻擊時 (中所述[減少 Active Directory 攻擊](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md))，可用於的列在此處群組成員 account 危害整個 AD DS 安裝。  

> [!NOTE]  
> 完整 pass hash 和其他認證竊取攻擊相關資訊，請查看[Mitigating Pass--Hash (PTH) 攻擊和其他認證竊取技術](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf)中列出的白皮書[附錄 m： 文件連結，並建議朗讀](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)。 如需來判斷對手攻擊的有時稱為 「 進階持續威脅 」 (APTs)，請查看[判斷對手和目標攻擊](https://www.microsoft.com/download/details.aspx?id=34793)。  

## <a name="activities-that-increase-the-likelihood-of-compromise"></a>增加危害的可能性的活動  
因為的認證竊取目標通常是非常有特殊權限的網域帳號，VIP 帳號，很重要的系統管理員會了解增加成功憑證竊取攻擊的機率的活動。 攻擊者也針對 VIP 帳號，如果 Vip 不提供高等級的系統上或網域中的權限，但其認證竊取需要其他類型的攻擊，例如上工程 VIP 提供機密資訊。 或攻擊者必須先取得哪一個 VIP 的快取認證系統有特殊權限的存取。 因此，增加認證竊取以下所述的可能性的活動的著重於防止取得高特殊權限管理的認證。 這些活動的攻擊者會危害取得權限的認證系統的常見的機制。  

### <a name="logging-on-to-unsecured-computers-with-privileged-accounts"></a>登入不安全的帳號特殊權限的電腦  
讓成功認證竊取攻擊核心弱點是登入電腦，而不安全帳號，是針對整個環境深特殊權限的行為。 這些登入可以錯誤以下所述的各種組態的結果。  

#### <a name="not-maintaining-separate-administrative-credentials"></a>不維護不同管理認證  
雖然這較少，評定各種 AD DS 安裝，但我們發現 IT 員工單一帳號使用所有他們的作品。 Account 的至少一個 Active Directory 中的最高特殊權限群組成員，且相同的員工使用登入他們的工作站在早上、 查看他們的電子郵件帳號，瀏覽網際網路網站和下載 content 到他們的電腦。 當使用本機系統管理員權限與權限授與帳號執行時的使用者時，他們公開本機電腦，以完成危害。 當那些帳號也 Active Directory 中最有特殊權限群組成員，他們將公開整個樹系危害，讓您很輕鬆攻擊者的 Active Directory 和 Windows 環境完整控制權。  

同樣地，在某些環境中，我們發現的相同使用者名稱和密碼用於根帳號非 Windows 電腦上在可用的 Windows 環境，可從 Windows 系統反之亦然 UNIX 或 Linux 系統擴充危害攻擊。

#### <a name="logons-to-compromised-workstations-or-member-servers-with-privileged-accounts"></a>登入危害的工作站或成員伺服器帳號特殊權限  
高度授權的核對危害的工作站或成員伺服器互動方式登入以使用時，該危害的電腦可能蒐認證登入系統任何 account 從集。  

#### <a name="unsecured-administrative-workstations"></a>不安全的系統管理工作站  
許多組織中的 IT 人員都使用多個帳號。 一個 account 用來登入員工的工作站，而且這些都是 IT 人員，因為它們通常本機系統管理員權限上工作站。 有時候，UAC 會向左功能，讓使用者在至少會收到分割存取權杖在登入時，並必須時所需的權限提高。 時維護活動執行這些使用者，他們通常使用本機安裝的管理工具，並提供認證網域權限帳號，選取 [**系統管理員身分執行**選項，或是地提供認證出現提示時。 看起來適當此設定，但它將會公開危害因為環境：  

-   員工用來登入他們工作站 [一般] 帳號本機系統管理員權限，電腦很容易受到[下載磁碟機，](https://www.microsoft.com/security/sir/glossary/drive-by-download-sites.aspx)中的使用者相信安裝惡意程式碼的攻擊。  

-   惡意程式碼已安裝的部分管理帳號，現在可以使用電腦擷取按鍵，剪貼、 螢幕擷取畫面，以及存放於記憶體認證，任何，可能會導致曝光強大核對的憑證。  

本案例中的問題將有兩個。 首先，本機和網域管理用於不同帳號，雖然電腦為不安全，並不保護針對竊取帳號。 第二，定期帳號和管理 account 獲得過權利與權限。  

### <a name="browsing-the-internet-with-a-highly-privileged-account"></a>瀏覽高度授權 Account 與網際網路  
使用者登入電腦的本機系統管理員群組的電腦上，或是的 Active Directory，以及人特殊權限群組成員帳號，然後瀏覽網際網路 （或危害的內部） 公開本機電腦和 directory 危害。  

存取惡意的網站與執行系統管理員權限的瀏覽器，可以允許存款中操作有特殊權限使用者的本機電腦上的惡意程式碼的攻擊。 如果使用者在電腦上有本機系統管理員權限，攻擊可能欺騙使用者下載惡意程式碼或打開電子郵件附件，利用應用程式的安全漏洞利用使用者的權限來擷取本機快取使用電腦所有使用者的認證。 如果使用者透過企業系統管理員，網域系統管理員 」，或在 Active Directory 中的系統管理員群組成員資格 directory 中有系統管理員權限，可以擷取網域認證攻擊者，並使用它們來降低整個 AD DS 網域或森林，而不需要危害森林中的任何其他電腦。  

### <a name="configuring-local-privileged-accounts-with-the-same-credentials-across-systems"></a>使用所有系統相同的認證本機特殊權限的帳號設定  
設定遭竊從坡資料庫危害所有其他使用相同的認證的電腦使用一部電腦上的許多或所有電腦讓認證的相同本機系統管理員 account 名稱與密碼。 最小，您應該使用不同的密碼本機系統管理員帳號在每個加入網域的系統。 可能也唯一名稱本機系統管理員帳號，但足以確保認證不能用於其他系統使用各系統有特殊權限本機帳號不同的密碼。  

### <a name="overpopulation-and-overuse-of-privileged-domain-groups"></a>Overpopulation 和的網域特殊權限的群組超額使用  
攻擊者會授與網域中 EA、 DA 或 BA 群組成員資格建立目標。 越大成員數個群組，較大的可能性授權的使用者可能不小心濫用認證，並將它們認證竊取公開攻擊。 每個工作站或伺服器特殊權限的網域使用者登入時提供的特殊權限的使用者的認證可能會被收集和使用危害的網域 AD DS 和樹系可能機制。  

### <a name="poorly-secured-domain-controllers"></a>不安全的網域控制站  
網域控制站儲存的網域 AD DS 資料庫的複本。 唯讀模式網域控制站在本機資料庫的複本包含 directory，這都有特殊權限的網域帳號，預設的帳號子集的認證。 讀取寫入網域控制站，每個網域控制站維護完整 AD DS 資料庫，包括不僅等網域系統管理員權限使用者的認證的複本，但有特殊權限的帳號，例如網域控制站帳號或的網域 Krbtgt 帳號，也就是 KDC 相關聯的那個服務網域控制站上。 如果不必要的網域控制站功能的其他應用程式的網域控制站上安裝或網域控制站並未嚴格修補及安全攻擊可能危害它們透過未弱點，或他們可能會利用直接在安裝惡意軟體的其他攻擊。  

## <a name="privilege-elevation-and-propagation"></a>提高權限和傳  
使用攻擊方法，無論 Active Directory 永遠為目標，當 Windows 環境受到攻擊，因為它最終控制存取至任何攻擊。 這不 」 表示針對整個 directory，但是。 特定帳號，伺服器，並基礎結構元件通常攻擊 Active Directory 的主要目標。 這些帳號所述方式，如下所示。  

### <a name="permanent-privileged-accounts"></a>永久特殊權限的帳號  
由於引入 Active directory 之後，這可能使用高度特殊權限帳號建置 Active Directory 樹系然後代理人的權利和權限，才能執行日常的系統管理帳號權限較低。 在 Active Directory 中企業系統管理員，網域系統管理員 」 或系統管理員群組成員資格只有需要暫時並不常實作日常管理最低權限的方法環境中。  

永久特殊權限的帳號是放在 [權限的群組並左有每天從帳號。 如果您的組織到網域系統管理員 」 的網域群組地點五個帳號，這些五個帳號可以目標的 24 小時星期 7 天、。 不過，帳號使用網域系統管理員權限的實際需要是通常只會針對特定全網域設定，以及簡短一段時間。  

### <a name="vip-accounts"></a>VIP 帳號  
在 Active Directory 破壞常被忽略的目標是 「 非常重要的人員] （或 Vip） 帳號是在組織中。 有特殊權限的帳號為目標，因為這些帳號可以權限授與對攻擊者，讓他們危害，或甚至破壞目標的系統上，為先前在本區段中所述。  

### <a name="privilege-attached-active-directory-accounts"></a>[權限連接 「 Active Directory 帳號  
[權限連接 「 Active Directory 帳號的網域帳號不所做的任何最高的 Active Directory 中權限，但改為獲得高階上許多伺服器工作站環境中的權限的群組成員。 這些帳號是最常網域型加入網域的系統上，為基礎結構大型區段上執行的應用程式通常會執行服務設定帳號。 雖然這些帳號 Active Directory 中有不權限，如果高權限在系統大量授權，他們可以用於危害，或甚至破壞大量區段的基礎結構，要達到有特殊權限的 Active Directory account 危害相同的效果。  
