---
ms.assetid: 7a7ab95c-9cb3-4a7b-985a-3fc08334cf4f
title: 實作最低權限管理模型
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 609ea0bd796a4d3696e14c7499be047ebdd83183
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868849"
---
# <a name="implementing-least-privilege-administrative-models"></a>實作最低權限管理模型

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

下列摘要是從[系統管理員帳戶安全性規劃指南](https://technet.microsoft.com/library/cc162797.aspx)，第一次發行在 1999 年 4 月 1 日：

> 「 最安全性相關訓練課程和文件討論的實作最低權限原則，但組織很少遵循。 這個原則很簡單，並將它套用到正確大幅的影響會增加您的安全性並降低風險。 原則狀態的所有使用者應該具有絕對最低權限完成目前的工作而不需要其他所需的使用者帳戶都登入。 這樣能夠防範惡意程式碼，在其他攻擊。 此原則適用於電腦和這些電腦的使用者。   
> 「 此原則的運作得很好的其中一個原因是它會強制您進行一些內部的參考資料。 比方說，您必須判斷真的需要電腦或使用者，存取權限，並接著實作它們。 對許多組織來說，這項工作一開始看起來像是大量工作;不過，它是必要的步驟，來成功地保護您的網路環境。
> 「 您應該授與所有的網域系統管理員使用者的最小權限概念其網域權限。 比方說，如果系統管理員特殊權限的帳戶登入，且不小心執行的防毒程式，病毒會有 本機電腦以及整個網域的系統管理存取權。 如果未具權限的 （非系統管理） 帳戶，必須改為系統管理員登入，病毒的損害範圍只會在本機電腦因為它執行的本機電腦使用者。
> 「 在另一個範例中，您授與網域層級系統管理員權限的帳戶必須不有提高的權限在另一個樹系，即使在樹系之間沒有信任關係。 這種做法有助於避免損害擴大，如果攻擊者企圖入侵一個受管理的樹系。 組織應該定期稽核以防止未經授權的權限的提升其網路。 」  

下列摘要是從[Microsoft Windows Security Resource Kit](https://www.microsoftpressstore.com/store/microsoft-windows-security-resource-kit-9780735621749)、 第一個在已發行 2005年:  

> 「 一律將以授與最少的權限來執行工作所需的安全性。 如果有太多的權限的應用程式應該遭到入侵，攻擊者可能可以在擴充功能會應用程式已在最少的權限可能超過攻擊。 例如，檢查網路系統管理員，因而不自覺地開啟啟動病毒的電子郵件附件的後果。 如果系統管理員登入使用網域系統管理員帳戶，病毒將網域中的所有電腦上具備系統管理員權限，並因此不受限制地存取網路上幾乎所有的資料。 如果系統管理員登入使用本機系統管理員帳戶，病毒將會在本機電腦上具有系統管理員權限，並因此就能夠存取的電腦上的任何資料，並在安裝惡意軟體，例如金鑰筆劃記錄軟體在電腦中。 如果系統管理員登入使用一般使用者帳戶，病毒會有系統管理員的資料存取，並不能安裝惡意軟體。 使用讀取電子郵件，在此範例中，所需的最低權限可能遭到入侵的範圍就會大幅降低。 」  

## <a name="the-privilege-problem"></a>權限問題

在上述摘要中所述的原則未變更，但評估 Active Directory 安裝，因此我們無可避免地找到大量具有已授與存取權來執行日常遠超過所需的權限的帳戶工作。 環境的大小會影響原始的數字，過度特殊權限的帳戶，但非 proportionmidsized 目錄中可能有數十個帳戶的最高特殊權限的群組，而大型的安裝可能會有數百或甚至數千個。 少數的例外狀況，不論精密複雜的攻擊技巧和利器，攻擊者通常會遵循最低抗拒的路徑。 只有時更簡單的機制失敗或會阻撓防禦者，它們會增加其工具和方法的複雜性。  

不幸的是，在許多環境中的最低抗拒的路徑已證明具有深度與廣度的權限的帳戶的過度使用。 廣泛的權限是權限和權限可讓帳戶跨大型的交叉區段的 [環境]-執行特定活動，例如，技術支援人員可能會授與權限，讓他們能夠重設許多使用者帳戶的密碼。  

深入的權限會套用至窄區段的母體擴展的高權限，這類讓工程師系統管理員權限的伺服器上，以便他們可以執行修復。 廣泛的權限和深度的權限都不一定具有危險性，但如果只有其中一個帳戶遭到入侵，深度與廣度權限，會永久授與網域中的許多帳戶，它快速可用來重新設定環境攻擊者的用途，或甚至摧毀基礎結構的大型區段。  

Pass pass-the-hash 攻擊，這是一種認證竊取攻擊，是非常普遍，因為執行這些工具是免費取得，且簡單易用，而且許多環境會很容易遭受攻擊。 不過，傳遞-雜湊攻擊中，不是真正的問題。 問題的癥結所在有兩個：  

1. 它並不難的攻擊者取得深入的權限在單一電腦上，然後傳播到其他電腦中廣泛的權限。  
2. 運算的範疇通常有太多永久的帳戶，具有高的層級的權限。

即使會消除 pass pass-the-hash 攻擊，攻擊者只會使用不同的策略，而不是不同的策略。 而不是植包含工具的認證竊取的惡意程式碼，它們可能計劃記錄按鍵動作的惡意程式碼，或利用任何數目的其他方法來擷取整個環境是功能強大的認證。 策略，不論目標保持不變： 深度與廣度的特殊權限的帳戶。  

授與過多的權限的不只找到 Active Directory 中遭洩露的環境中。 當組織所開發授與比所需的權限的習慣時，它通常位於整個基礎結構，如下列各節所述。  

## <a name="in-active-directory"></a>在 Active Directory 中

在 Active Directory，常會發現 EA、 DA 和 BA 群組包含過多的帳戶數目。 大多數情況下，組織的 EA 群組中包含的最少的成員，、 DA 群組通常包含的 EA 群組中的使用者數目的倍數，且系統管理員群組通常包含比結合的其他群組的母體擴展的更多成員。 這通常是因為系統管理員以某種方式是一個信念"較不具權限 」 比 DAs 或 EAs。 雖然權利和權限授與給這些群組各有不同，但其有效地視為同樣功能強大的群組因為値的其他兩個成員，可以進行的其中一個成員。  

## <a name="on-member-servers"></a>在成員伺服器上

當我們擷取在許多環境中的成員伺服器上的本機系統管理員群組的成員資格時，我們找到少數幾個本機和網域帳戶，從範圍到數十種巢狀群組的成員資格，展開時，會顯示數百甚至數千的在伺服器上具有本機系統管理員權限帳戶。 在許多情況下，具有大型的成員資格的網域群組是巢狀在成員伺服器的本機系統管理員群組，而不需要任何使用者都可以修改這些網域中的群組成員資格，可取得的所有系統管理控制權的事實的考量在其上本機系統管理員群組中有已巢狀群組。  

## <a name="on-workstations"></a>在工作站上

雖然工作站通常會有明顯較少的成員在其本機系統管理員群組相較於成員伺服器，在許多環境中，使用者會授與在其個人電腦上本機 Administrators 群組的成員資格。 當發生這種情況，即使已啟用 UAC 時，這些使用者會呈現較高的風險，他們的工作站的完整性。  

> [!IMPORTANT]  
> 您應該仔細考慮是否使用者需要在其工作站上的系統管理權限，而且如果沒有的話，更好的方法可能是系統管理員群組成員的電腦上建立不同的本機帳戶。 當使用者需要提高權限時，它們可以呈現在提高權限，該本機帳戶的認證，但該帳戶是本機的因為它不能危及其他電腦或存取網域資源。 如同任何本機帳戶，不過，本機的特殊權限帳戶的認證應該是唯一的;如果您使用相同的認證，在多個工作站上建立本機帳戶，您會公開傳遞 pass-the-hash 攻擊的電腦。  

## <a name="in-applications"></a>在應用程式

在攻擊中所在的目標是組織的智慧財產權，可以允許資料外洩的鎖定已被授與應用程式內功能強大的權限的帳戶。 雖然有權存取敏感性資料的帳戶可能已被授與位於網域或作業系統，可以操作的應用程式設定的帳戶沒有提高權限，或提供資訊的應用程式的存取權有風險。  

## <a name="in-data-repositories"></a>在 資料存放庫

在此情況下，與其他目標，請求存取權之智慧財產權的文件和其他檔案的形式的攻擊者可以鎖定帳戶，控制檔案存放區的存取，可直接存取的檔案，或甚至是群組或角色的帳戶存取檔案。 比方說，如果檔案伺服器用來儲存的合約文件，而且授與存取權的文件使用的 Active Directory 群組，攻擊者可以修改群組的成員資格遭入侵的帳戶新增到群組即可存取合約文件。 在文件存取權由 SharePoint 等應用程式的情況下，攻擊者可以針對應用程式，如先前所述。  

## <a name="reducing-privilege"></a>減少權限

較大和更多複雜環境中，更困難，則管理及保護。 在小型組織中，檢閱並減少權限可能會相當簡單的主張，但每個額外的伺服器、 工作站、 使用者帳戶和組織中的使用中的應用程式新增另一個物件，必須受到保護。 由於很難或甚至無法正確保護每個層面的組織的 IT 基礎結構，您應該先專注的努力，於其權限建立的最大的風險，通常是內建的特殊權限的帳戶的帳戶和在 Active Directory 和特殊權限的本機帳戶，在工作站和成員伺服器上的群組。  

### <a name="securing-local-administrator-accounts-on-workstations-and-member-servers"></a>保護在工作站和成員伺服器上的本機系統管理員帳戶

雖然本文件著重於保護 Active Directory 中，如先前所述，大部分的攻擊，對目錄便會開始為個別主機的攻擊。 無法提供完整的指導方針來保護在成員的系統上的本機群組，但下列建議可用來協助您保護在工作站和成員伺服器上的本機系統管理員帳戶。  

#### <a name="securing-local-administrator-accounts"></a>保護本機系統管理員帳戶

所有版本的 Windows 目前在主流支援，在本機系統管理員帳戶已停用根據預設，可以讓帳戶無法用於傳遞-雜湊和其他認證竊取攻擊。 不過，在包含舊版作業系統的網域或已啟用的本機系統管理員帳戶，這些帳戶可以用於如先前所述散佈成員伺服器和工作站的危害。 基於這個理由，下列控制項建議所有加入網域的系統上的本機系統管理員帳戶。  

提供實作這些控制項的詳細的指示[附錄 h:保護本機系統管理員帳戶和群組](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)。 之前實作這些設定，不過，請確定，本機系統管理員帳戶是不目前環境中使用的電腦上執行的服務，或執行，這些帳戶不應該使用其他活動。 在生產環境中實作前徹底測試這些設定。  

#### <a name="controls-for-local-administrator-accounts"></a>本機系統管理員帳戶的控制項

請勿為成員伺服器上的服務帳戶使用內建的 Administrator 帳戶，也不應該它們用來登入本機電腦 （除非在安全模式下，就允許即使帳戶已停用）。 實作此處所述設定的目標是防止每台電腦的本機系統管理員帳戶正在使用，除非保護控制項第一次會反轉。 藉由實作這些控制項，並監視變更的系統管理員帳戶，您可以大幅減少成功攻擊的可能性該目標的本機系統管理員帳戶。  

##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-joined-systems"></a>設定 Gpo，來限制在已加入網域的系統上的系統管理員帳戶

在您建立並連結至工作站和成員伺服器 Ou，每個網域中的一或多個 Gpo，系統管理員帳戶新增到下列中的使用者權限**電腦設定 \ 原則 \windows 設定 \ 安全性設定 \ 本機 Policies\使用者權限指派**:  

- 拒絕從網路存取這台電腦
- 拒絕以批次工作登入
- 拒絕以服務方式登入
- 拒絕透過遠端桌面服務登入

當您將系統管理員帳戶新增至這些使用者權限時，指定是否要加入本機系統管理員帳戶或網域的系統管理員帳戶了加上標籤的帳戶。 比方說，將拒絕 NWTRADERS 網域的系統管理員帳戶，這些權限，您會輸入為帳戶**NWTRADERS\Administrator**，或瀏覽至 NWTRADERS 網域的系統管理員帳戶。 若要確保您限制本機 Administrator 帳戶，請輸入**系統管理員**這些使用者權限設定群組原則物件編輯器 」 中。  

> [!NOTE]  
> 即使已重新命名本機系統管理員帳戶，仍然會套用原則。  

這些設定可確保連線到其他電腦，無法使用，電腦的系統管理員帳戶，即使它不小心或惡意地啟用。 無法完全停用使用本機系統管理員帳戶的本機登入，或如果您嘗試這樣做，因為電腦的本機系統管理員帳戶設計用於災害復原案例。  

應該在成員伺服器或工作站成為退出網域使用任何其他本機帳戶授與系統管理權限、 電腦可以開機進入安全模式，可以啟用的系統管理員帳戶，和效果可以再使用帳戶在電腦上的修復。 修復完成後，應該再次停用的系統管理員帳戶。  

### <a name="securing-local-privileged-accounts-and-groups-in-active-directory"></a>保護特殊權限的本機帳戶與 Active Directory 中的群組

*法律編號第六：只有系統管理員值不值得信任的安全電腦。* - [十大不變法則安全性 （2.0 版）](https://technet.microsoft.com/security/hh278941.aspx)  

此處提供的資訊被要提供一般指導方針來保護最高的權限內建的帳戶與 Active Directory 中的群組。 中也會提供詳細的逐步指示[附錄 d:保護 Active Directory 中的內建的 Administrator 帳戶的安全](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)，[附錄 e:保護 Active Directory 中的 Enterprise Admins 群組](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)，[附錄 f:保護 Active Directory 中的 Domain Admins 群組](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)，然後在[附錄 g:保護 Active Directory 中的系統管理員群組](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)。  

在實作這些設定之前，您也應該測試徹底地判斷它們是否適合您環境的所有設定。 並非所有組織都都能夠實作這些設定。  

#### <a name="securing-built-in-administrator-accounts-in-active-directory"></a>保護 Active Directory 中的內建的 Administrator 帳戶

在 Active Directory 中的每個網域，系統管理員帳戶會建立為網域的一部分。 此帳戶是預設網域系統管理員和網域中的系統管理員群組的成員而且如果樹系根網域的網域，帳戶也是 Enterprise Admins 群組的成員。 使用本機系統管理員帳戶的網域應該只會針對初始建置活動，而且可能嚴重損壞修復案例保留。 若要確保的內建的系統管理員帳戶，可用來影響修復，可以使用任何其他帳戶，您不應該變更預設的成員資格的樹系中任何網域中的系統管理員帳戶。 相反地，您應該遵循指導方針來協助保護系統管理員帳戶的樹系中每個網域中。 提供實作這些控制項的詳細的指示[附錄 d:保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

#### <a name="controls-for-built-in-administrator-accounts"></a>控制項的內建的 Administrator 帳戶

實作此處所述設定的目標是防止每個網域系統管理員帳戶 （而非群組） 從正在使用，除非相反的控制項數目。 藉由實作這些控制項，並監視的系統管理員帳戶以進行變更，您可以利用網域系統管理員帳戶，大幅減少成功攻擊的可能性。 在您的樹系中每個網域中的系統管理員帳戶，您應該設定下列設定。  

##### <a name="enable-the-account-is-sensitive-and-cannot-be-delegated-flag-on-the-account"></a>啟用帳戶 」 是機密帳戶，無法委派 」 旗標

根據預設，可以委派在 Active Directory 中的所有帳戶。 委派可讓電腦或出示已向電腦進行驗證的帳戶認證的服務或其他電腦，以取得代表帳戶的服務的服務。 當您啟用**是機密帳戶，無法委派**屬性網域帳戶，帳戶的認證無法提供給其他電腦或服務在網路上，這會限制利用的攻擊在其他系統上使用的帳戶認證的委派。  

##### <a name="enable-the-smart-card-is-required-for-interactive-logon-flag-on-the-account"></a>啟用帳戶的 「 智慧卡 」 需要互動式登入旗標

當您啟用**智慧卡是互動式登入必須**屬性上的帳戶，Windows 將帳戶的密碼重設為 120 個字元的隨機值。 藉由設定這個旗標上內建的系統管理員帳戶，您必須確定帳戶的密碼不只是費時又複雜，但並不知道的任何使用者。 不就技術上而言，不必再啟用這個屬性，建立帳戶的智慧卡，但可能的話，應該為每個系統管理員帳戶，然後再設定帳戶限制建立智慧卡和智慧卡應該儲存在安全的位置。  

雖然設定**智慧卡是互動式登入必須**旗標會重設帳戶的密碼，它無法防止具有重設帳戶的密碼設為已知的值中的帳戶，並使用的權限的使用者帳戶的名稱和新的密碼以存取網路上的資源。 基於這個原因，您應該實作在帳戶上的下列其他控制項。  

##### <a name="configuring-gpos-to-restrict-domains-administrator-accounts-on-domain-joined-systems"></a>設定 Gpo，來限制在已加入網域的系統上的網域的系統管理員帳戶

雖然停用在網域中的系統管理員帳戶，會讓帳戶有效地無法使用，您應該實作額外的限制，帳戶如果已啟用的帳戶不小心或惡意即可。 雖然最終可反轉這些控制項由系統管理員帳戶，目標是要建立降低攻擊者所進行的控制項，而且可能會限制帳戶的損害。  

在您建立並連結至工作站和成員伺服器 Ou，每個網域中的一或多個 Gpo，加入每個網域系統管理員帳戶中的下列使用者權限**電腦設定 \ 原則 \windows 設定 \ 安全性設定 \ 本機原則 \ 使用者權限指派**:  

- 拒絕從網路存取這台電腦  
- 拒絕以批次工作登入  
- 拒絕以服務方式登入  
- 拒絕透過遠端桌面服務登入  

> [!NOTE]  
> 當您將本機系統管理員帳戶加入此設定時，您必須指定您要設定本機系統管理員帳戶或網域系統管理員帳戶。 例如，將這些 NWTRADERS 網域的本機系統管理員帳戶拒絕權限，您必須輸入的帳戶**NWTRADERS\Administrator**，或瀏覽至 NWTRADERS 網域的本機系統管理員帳戶。 如果您鍵入**系統管理員**中這些使用者的權限設定群組原則物件編輯器 」 中，您將會限制這個 GPO 會套用每一部電腦上的本機系統管理員帳戶。  
>
> 我們建議在網域系統管理員帳戶相同的方式來限制在成員伺服器和工作站上的本機系統管理員帳戶。 因此，您應該通常新增樹系中的每個網域的系統管理員帳戶和本機電腦的系統管理員帳戶對這些使用者的權限設定。 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-controllers"></a>設定 Gpo，來限制在網域控制站上的系統管理員帳戶

在樹系中每個網域中，預設網域控制站原則] 或 [連結到網域控制站 OU 的原則應該修改每個網域系統管理員帳戶新增到下列中的使用者權限**電腦設定 \ 原則 \Windows 設定 \ 安全性設定 \ 本機原則 \ 使用者權限指派**:  

- 拒絕從網路存取這台電腦  
- 拒絕以批次工作登入  
- 拒絕以服務方式登入  
- 拒絕透過遠端桌面服務登入  

> [!NOTE]  
> 這些設定可確保連線到網域控制站無法使用，本機系統管理員帳戶，雖然帳戶，如果啟用，可以登入本機網域控制站。 此帳戶應該僅啟用並用於災害復原案例，因為它預期會提供至少一個網域控制站的實體存取，或其他遠端存取網域控制站的權限的帳戶可以是使用此項目。  

##### <a name="configure-auditing-of-built-in-administrator-accounts"></a>設定的內建的 Administrator 帳戶稽核

當您有保護每個網域系統管理員帳戶，並將它停用時，您應該設定稽核來監視變更的帳戶。 如果已啟用的帳戶、 重設其密碼時，或任何其他修改的帳戶，應該會收到通知的使用者或小組負責管理 AD DS，除了您組織中的事件回應小組。  

### <a name="securing-administrators-domain-admins-and-enterprise-admins-groups"></a>保護系統管理員、 Domain Admins 和 Enterprise Admins 群組

#### <a name="securing-enterprise-admin-groups"></a>保護企業系統管理員群組

Enterprise Admins 群組，該表格位於樹系根網域中，應該包含在日常，與可能的例外狀況的本機系統管理員帳戶的網域，沒有任何使用者，提供稍早所述受到保護，並在[附錄 d:保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

需要 EA 存取時，其帳戶需要 EA 權利與權限的使用者應該暫時放入 Enterprise Admins 群組。 雖然使用者將具有高度權限的帳戶，其活動應該進行稽核和最好是利用一位使用者執行變更並觀察變更的另一位使用者不小心誤用或設定錯誤的可能性降到最低. 當活動完成時，應該從 EA 群組中移除帳戶。 這可以透過手動程序，並記錄處理程序、 第三方特殊權限的身分識別/存取權管理 (PIM/PAM) 軟體或兩者的組合。 指導方針建立帳戶，可以用來控制在 Active Directory 中的特殊權限群組的成員資格中提供[吸引人的帳戶認證竊取](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)中提供詳細的指示和[附錄 i:建立管理帳戶，受保護的帳戶和 Active Directory 中群組](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)。  

企業系統管理員 」 是，根據預設，樹系中每個網域中的內建系統管理員群組的成員。 從每個網域中的系統管理員群組移除 Enterprise Admins 群組是不適當的修改，因為樹系的災害復原案例中，萬一 EA 權限可能需要使用中。 如果已從樹系中的系統管理員群組移除 Enterprise Admins 群組，它應新增至每個網域中的系統管理員群組，並應該實作下列額外控制項：  

- 如先前所述，Enterprise Admins 群組應該包含在日常，與可能的例外狀況，樹系根網域的系統管理員帳戶，應受到保護，如中所述的任何使用者[附錄 d:保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
- 在 Gpo 連結到 Ou 包含成員伺服器和每個網域中的工作站，EA 群組應該加入下列的使用者權限：  
   - 拒絕從網路存取這台電腦  
   - 拒絕以批次工作登入  
   - 拒絕以服務方式登入  
   - 拒絕本機登入  
   - 拒絕透過遠端桌面服務登入。  

這會防止 EA 群組的成員登入的成員伺服器和工作站。 如果跳躍伺服器用來管理網域控制站和 Active Directory，請確定跳躍伺服器位於的嚴格 Gpo 未連結的 OU。  

- 稽核應該設定為傳送警示，如果對屬性或 EA 群組的成員資格進行任何修改。 這些警示應該傳送，至少到使用者或團隊負責 Active Directory 系統管理和事件回應。 您也應該定義處理程序和暫時填入的 EA 群組中，執行合法的母體擴展的群組時，包含通知程序的程序。  
  
#### <a name="securing-domain-admins-groups"></a>保護網域系統管理員群組

在此與 Enterprise Admins 群組的情況下，應該只有在建置或災害復原的情況下需要 Domain Admins 群組的成員資格。 中應該會有任何日常規律的使用者帳戶資料群組，除了本機系統管理員帳戶的網域中所述保護[附錄 d:保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
  
需要資料存取時，需要此層級的存取權限的帳戶應該暫時放 DA 群組中的網域有問題。 雖然使用者將具有高度權限的帳戶，應該稽核，最好是以一位使用者執行變更並觀察變更的另一位使用者不小心誤用或設定錯誤的可能性降到最低執行活動。 當活動完成時，應該從 Domain Admins 群組中移除帳戶。 這可以透過手動程序，並記錄處理程序，透過第三方特殊權限的身分識別/存取權管理 (PIM/PAM) 軟體或兩者的組合。 指導方針建立帳戶，可以用來控制在 Active Directory 中的特殊權限群組的成員資格中提供[附錄 i:建立管理帳戶，受保護的帳戶和 Active Directory 中群組](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
網域系統管理員 」 是，根據預設，所有的成員伺服器和其個別網域中的工作站上本機 Administrators 群組的成員。 不應該修改這個預設巢狀結構，因為它會影響可支援性和災害復原選項。 如果已從本機 Administrators 群組的成員伺服器上移除 Domain Admins 群組，它們應該將每個成員伺服器和透過受限的群組設定連結的 Gpo 在網域中的工作站上的 Administrators 群組。 下列的一般控制項，詳述於深度[附錄 f:保護 Active Directory 中的 Domain Admins 群組](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)也應執行。  

對於每個樹系中的網域中 Domain Admins 群組：  

1. 移除所有成員在 DA 群組，可能的例外狀況的內建的 Administrator 帳戶的網域，提供如中所述保護[附錄 d:保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
2. 在 Gpo 連結到 Ou 包含成員伺服器和每個網域中的工作站，DA 群組應該加入下列的使用者權限：  
   - 拒絕從網路存取這台電腦  
   - 拒絕以批次工作登入  
   - 拒絕以服務方式登入  
   - 拒絕本機登入  
   - 拒絕透過遠端桌面服務登入  
  
   如此可防止資料群組的成員登入的成員伺服器和工作站。 如果跳躍伺服器用來管理網域控制站和 Active Directory，請確定跳躍伺服器位於的嚴格 Gpo 未連結的 OU。  

3. 稽核應該設定為傳送警示，如果對屬性或資料群組的成員資格進行任何修改。 這些警示應該傳送，至少到使用者或小組負責 AD DS 系統管理和事件回應。 您也應該定義處理程序和暫時填入 DA 群組中，執行合法的母體擴展的群組時，包含通知程序的程序。  

#### <a name="securing-administrators-groups-in-active-directory"></a>保護 Active Directory 中的系統管理員群組

在此情況下，使用 EA 和 DA 群組，應該只在建置或災害復原案例中需要系統管理員 (BA) 群組的成員資格。 中應該會有任何日常規律的使用者帳戶本機系統管理員帳戶的網域，除了系統管理員群組中所述保護[附錄 d:保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  

需要系統管理員存取時，需要此層級的存取權限的帳戶應該暫時放在有問題的網域系統管理員群組中。 雖然使用者將具有高度權限的帳戶，活動應進行稽核和，最好是執行與使用者執行變更並觀察變更的另一位使用者不小心誤用或設定錯誤的可能性降到最低。 當活動完成時，帳戶應該立即從系統管理員群組移除。 這可以透過手動程序，並記錄處理程序，透過第三方特殊權限的身分識別/存取權管理 (PIM/PAM) 軟體或兩者的組合。  

系統管理員是，根據預設，大部分在其各自的定義域中的 AD DS 物件的擁有者。 此群組成員資格可能必須在擁有權或取得物件的擁有權的能力是在需要的組建和災害復原案例。 此外，DAs 和 EAs 會繼承其權限和透過其預設群組的成員資格系統管理員權限的數目。 預設為不應該修改 Active Directory 中的特殊權限的群組，建立巢狀群組，而且應該保護每個網域的系統管理員群組，如中所述[附錄 g:保護 Active Directory 中的系統管理員群組](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)，並在下列的一般指示。  

1. 移除所有成員在系統管理員群組，可能的例外狀況的本機系統管理員帳戶的網域，提供如中所述保護[附錄 d:保護 Active Directory 中的內建的 Administrator 帳戶](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
2. 網域的系統管理員群組的成員應該永遠不需要登入的成員伺服器或工作站。 在一或多個 Gpo 連結到工作站和成員伺服器的每個網域中的 Ou，系統管理員群組應該加入下列的使用者權限：  
   - 拒絕從網路存取這台電腦  
   - 拒絕以批次作業登入  
   - 拒絕以服務方式登入  
   - 這會讓系統管理員群組的成員無法用來登入或連接到成員伺服器或工作站 （除非有多個控制項第一次違反服務等級），其中其認證無法快取，並進而入侵。 特殊權限的帳戶應該永遠不會用來登入較低權限系統，並強制執行這些控制項可提供多個攻擊的防護。  

3. 在網域控制站 OU，每個網域、 樹系的系統管理員群組中應該被授與下列使用者權限 （如果它們還沒有這些權限），這將允許 Administrators 群組的成員來執行所需的功能全樹系的災害復原案例：  
   - 從網路存取這台電腦  
   - 允許本機登入  
   - 允許透過遠端桌面服務登入  

4. 稽核應該設定為傳送警示，如果對屬性或系統管理員群組的成員資格進行任何修改。 這些警示應該傳送，至少必須負責 AD DS 管理團隊的成員。 安全性小組的成員也應傳送警示和程序應定義修改系統管理員群組的成員資格。 具體來說，這些程序應該包括 「 管理員 」 群組進行修改以便時傳送警示，如預期，並不會引發警示時，安全性小組通知用的程序。 此外，您應該實作程序，以系統管理員群組的使用已完成，並使用的帳戶具有從群組中移除時，通知安全性小組。  

> [!NOTE]  
> 當您實作在 Gpo 中的 Administrators 群組的限制時，Windows 會套用的設定，除了網域的系統管理員群組的電腦本機 Administrators 群組的成員。 因此，您應該小心實作上的 Administrators 群組的限制時。 本機登入或透過遠端桌面服務的登入，雖然系統管理員群組的成員禁止網路、 批次和服務的登入建議只要是可行的方法是實作，並不會限制。 封鎖這些登入類型，可以封鎖合法的系統管理的電腦的本機 Administrators 群組成員。 下列螢幕擷取畫面顯示封鎖不當使用內建的組態設定本機和網域系統管理員帳戶，除了內建的本機或網域系統管理員群組的誤用。 請注意，**拒絕透過遠端桌面服務登入**使用者權限不包括系統管理員群組，因為它包含在此設定也會封鎖這些成員的本機電腦的帳戶登入系統管理員群組。 如果電腦上的服務設定為執行任何特殊權限的群組，這一節所述的內容中，實作這些設定可能導致服務和應用程式失敗。 因此，如同所有的這一節中的建議，您應該徹底測試適用性的設定您的環境中。  
>
> ![最低權限管理模型](media/Implementing-Least-Privilege-Administrative-Models/SAD_3.gif)  

### <a name="role-based-access-controls-rbac-for-active-directory"></a>Active directory 角色型存取控制 (RBAC)

一般而言，角色型存取控制 (RBAC) 是一種機制分組使用者，以及提供根據商務規則的資源的存取權。 在 Active Directory 中，適用於 AD DS 中實作 RBAC 是建立的權利與權限委派給允許角色的成員來執行日常系統管理工作，而過多的權限授與的角色的程序。 可以設計和實作透過原生工具和介面，您可能已經擁有，購買第三方產品或這些方法的任何組合的軟體，利用 Active directory 的 RBAC。 本節不會提供逐步指示，針對 Active Directory，實作 RBAC，但改為討論中選擇您的 AD DS 安裝中實作 RBAC 方法，您應該考慮的因素。  

#### <a name="native-approaches-to-rbac-for-active-directory"></a>RBAC 的 Active Directory 的原生方法

在最簡單的 RBAC 實作中，您可以實作做為 AD DS 群組的角色，並委派權限和群組，讓它們執行指定的範圍內的每日管理角色的權限。  

在某些情況下，在 Active Directory 中的現有安全性群組可用來授與權限的工作函式的適當權限。 比方說，如果您的 IT 組織中的特定員工負責管理和維護的 DNS 區域和記錄，委派這些責任，可以是簡單，只要建立每個 DNS 系統管理員帳戶，並將它新增至 DNSActive Directory 中的系統管理員群組。 DNS Admins 群組中的，不同於更高權限群組，在 Active Directory 中有幾個功能強大的權限，雖然這個群組的成員已經被委派的權限，讓他們管理的 DNS，並為仍受限於洩露和不當內容可能會導致提高權限。

在其他情況下，您可能需要建立安全性群組和委派的權利和權限給 Active Directory 物件、 檔案系統物件，與登錄物件，讓群組的成員來執行指定的系統管理工作。 比方說，如果您 「 服務台 」 操作員負責重設遺忘的密碼，協助使用者連線問題和疑難排解應用程式設定，您可能需要將 Active Directory 中使用者物件的委派設定並允許技術支援使用者從遠端連線至使用者的電腦，來檢視或修改使用者的組態設定的權限。 針對您所定義的每個角色，您應該識別：  

1. 哪些工作角色的成員執行日常的行動和較不常執行工作。  
2. 哪些系統和應用程式角色的成員應該授與權限。  
3. 哪些使用者應該被授與角色的成員資格。  
4. 如何將執行的角色成員資格的管理。  

在許多環境中，以手動方式建立的 Active Directory 環境的系統管理角色型存取控制不容易實作和維護。 如果您已清楚定義的 IT 基礎結構的系統管理角色和責任，您可能想要利用其他工具，可協助您建立可管理的原生 RBAC 部署。 比方說，Forefront Identity Manager (FIM) 是否在使用您的環境中，您可以使用 FIM 來自動建立及擴展的系統管理角色，可以簡化進行中的管理。 如果您使用 System Center Configuration Manager (SCCM) 和 System Center Operations Manager (SCOM)，您可以使用應用程式特定角色來委派管理和監視功能，並也強制執行一致的組態和稽核內跨系統網域中。 如果您已實作公開金鑰基礎結構 (PKI)，您可以發出，並且負責管理環境的 IT 人員需要智慧卡。 使用 FIM 憑證管理 (FIM CM)，您甚至可以為您的系統管理人員結合管理角色和認證。  

在其他情況下，它可能會考慮部署提供 「 外的箱 」 功能的第三方 RBAC 軟體的組織偏好程式碼。 Rbac 的 Active Directory、 Windows、 和非 Windows 目錄和作業系統的商業、 現成 (COTS) 解決方案是由許多廠商提供。 原生的解決方案和協力廠商產品之間選擇時，您應該考慮下列因素：  

1. 預算：藉由投資開發的軟體和工具，您可能已經擁有使用 RBAC，您可以減少部署的解決方案所牽涉到的軟體成本。 不過，除非您有豐富經驗中建立及部署原生的 RBAC 解決方案的人員，您可能需要進行諮詢的資源，以開發您的解決方案。 您應該仔細衡量預期的成本，以自訂開發的解決方案來部署 「 外的箱 」 方案，成本，特別是如果您的預算有限。  
2. IT 環境的構成要素：如果您的環境包含主要是由 Windows 系統，或如果您已經運用 Active Directory 管理非 Windows 系統和帳戶，自訂原生解決方案可能提供最好的解決方案為您的需求。 如果您的基礎結構會包含未在執行 Windows，而且不由 Active Directory 管理的許多系統，您可能需要考慮的非 Windows 系統與 Active Directory 環境分開的管理選項。  
3. 在方案中的權限模型：如果產品所仰賴其服務帳戶的 Active Directory 中的高特殊權限分組的位置，而且不會不包含供應項目選項不需要過多的權限授與 RBAC 軟體，您有不真正縮減您的 Active Directory 攻擊介面只變更目錄中的最高權限群組的組合。 除非應用程式廠商可以提供控制項的服務帳戶的帳戶被入侵和惡意使用的可能性降到最低，您可能要考慮的其他選項。  

### <a name="privileged-identity-management"></a>權限身分管理

Privileged identity management (PIM)，有時稱為特殊權限的帳戶管理 (PAM)] 或 [特殊權限的認證管理 (PCM) 是設計，建構，以及實作的方法來管理特殊權限的帳戶，在您基礎結構。 一般而言，PIM 提供的機制所授與帳戶暫時的權限，並執行建置或中斷所需的權限修正函式，而不是離開永久附加至帳戶的權限。 PIM 功能以手動方式建立，或透過實作一或多個下列功能的協力廠商軟體的部署可能提供：  
  
- 認證 保存庫，"，"簽出 」 並指派一個初始密碼，然後 「 簽入 「 當活動已完成，此時會再次重設密碼的帳戶上特殊權限帳戶的密碼。  
- 使用特殊權限認證的時間繫結限制  
- 一次使用的認證  
- 工作流程產生授與的權限監視和報告執行的活動以及自動移除權限，當活動完成或分配的時間已過期  
- 取代為硬式編碼的認證，例如使用者名稱和密碼的指令碼的應用程式開發介面 (Api)，讓擷取從保存庫所需的認證  
- 自動管理的服務帳戶認證  

### <a name="creating-unprivileged-accounts-to-manage-privileged-accounts"></a>建立無特殊權限的帳戶來管理特殊權限的帳戶

在 管理特殊權限的帳戶的挑戰之一是，根據預設，可以管理特殊權限和受保護的帳戶的帳戶和群組特殊權限和受保護帳戶。 如果您實作適當的 RBAC 和 PIM 解決方案為您的 Active Directory 安裝時，解決方案可能包括方法可讓您有效地 depopulate 最高權限群組的目錄中，填入僅限群組的成員資格暫時並在必要時。  

如果您實作原生的 RBAC 和 PIM，不過，您就應該考慮建立時所需的 Active Directory 中的帳戶沒有權限，並使用唯一的函式填入和 depopulating 特殊權限的群組。 [附錄 i:建立受保護的帳戶和 Active Directory 中的群組的管理帳戶](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)提供可用來針對此用途建立帳戶的逐步指示。  

### <a name="implementing-robust-authentication-controls"></a>實作強固的驗證控制項

*法律編號第六：這種人出有嘗試猜出您的密碼。* - [10 不變法則安全性管理](https://technet.microsoft.com/library/cc722488.aspx)  

Pass-雜湊和其他認證竊取攻擊並不專屬於 Windows 作業系統，也新增。 在 1997年中建立第一個階段-雜湊攻擊。 在過去，不過，這些攻擊所需的自訂的工具，hit-or-miss 其順利完成，而所需技能相對較高程度的攻擊者。 引進的原生擷取認證的免費、 簡單易用的工具已經產生呈指數增加的數目和認證竊取攻擊成功近幾年來的。 不過，認證竊取攻擊也絕對不是認證會用為目標，並洩露的唯一機制。  

雖然您應該實作控制項，以協助保護您防範認證竊取攻擊，您也應該識別最有可能設為目標的攻擊者，您的環境中的帳戶及這些實作強固的驗證控制項帳戶。 如果您最多權限的帳戶使用單一要素驗證，例如使用者名稱和密碼 （兩者都是 「 您知道的東西，「 這是一項驗證因素），這些帳戶弱式保護。 所有這些攻擊者需要的只是知識的使用者名稱和密碼與帳戶相關聯的知識和傳遞 pass-the-hash 攻擊就不需要攻擊者可以接受的一項因素認證的任何系統使用者身分進行驗證。  

多重要素驗證的實作，雖然無法防止您傳遞-雜湊攻擊中，搭配受保護的系統可以實作多因素驗證。 中提供實作保護的系統的詳細資訊[實作安全的系統管理主機](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)，下列各節中討論的驗證選項。  

#### <a name="general-authentication-controls"></a>一般驗證控制項

如果您未實作多重要素驗證，例如智慧卡，請考慮這種方式。 智慧卡實作公開-私密金鑰組，防止使用者的私密金鑰遭到存取或使用，除非該使用者提供正確的 PIN、 密碼或生物識別元給智慧卡硬體強制執行保護私密金鑰。 即使在遭入侵的電腦，重複使用這個 PIN 或密碼，攻擊者上的按鍵輸入記錄器攔截使用者的 PIN 或密碼也必須實際出現在卡片。

在冗長且複雜的密碼已證明難實施，因為使用者抗拒的情況下，智慧卡提供的機制，供使用者可以實作相當簡單的 Pin 或密碼不需要認證不容易遭受暴力密碼破解或彩虹資料表等攻擊。 智慧卡 Pin 不會儲存在 Active Directory 或本機 SAM 資料庫中，雖然仍然可能在其智慧卡已用於驗證的電腦上的 LSASS 受保護記憶體中儲存認證雜湊。  

#### <a name="additional-controls-for-vip-accounts"></a>VIP 帳戶的其他控制項

實作智慧卡或其他憑證型驗證機制的另一個優點是能夠利用來保護機密資料的存取給 VIP 使用者的驗證機制保證。 驗證機制保證可在其中的功能等級設定為 Windows Server 2012 或 Windows Server 2008 R2 的網域中。 啟用時，驗證機制保證將系統管理員指定的全域群組成員資格使用者的 Kerberos 權杖時使用的憑證為基礎的登入方法登入時驗證使用者的認證。  

這可讓資源管理員來控制存取資源，例如檔案、 資料夾和印表機，根據是否在使用者登入時使用的憑證為基礎的登入方法，除了使用憑證的類型。 例如，當使用者登入時使用智慧卡，使用者的存取權的網路上的資源可以指定為不同的存取權時，使用者不會使用智慧卡 （也就是當使用者登入時輸入使用者名稱和密碼）。 如需有關驗證機制保證的詳細資訊，請參閱[適用於 Windows Server 2008 R2 逐步指南中的 AD DS 的驗證機制保證](https://technet.microsoft.com/library/dd378897.aspx)。  

#### <a name="configuring-privileged-account-authentication"></a>設定特殊權限的帳戶驗證

Active Directory 中的所有系統管理帳戶，啟用**互動式登入需要智慧卡**屬性，且 （最小），若要變更的稽核的任何屬性上**帳戶**索引標籤帳戶 （例如 cn、 名稱、 sAMAccountName、 userPrincipalName 及 userAccountControl） 系統管理使用者的物件。  

雖然設定**互動式登入需要智慧卡**帳戶的帳戶密碼重設為 120 個字元的隨機值，而且需要智慧卡，為互動式登入，屬性仍會覆寫可讓使用者變更密碼的帳戶和帳戶的權限的使用者可用來建立非互動式登入，只有使用者名稱和密碼。  

在其他情況下，取決於組態中的帳戶在 Active Directory 憑證服務 (AD CS) 或第三方 PKI、 使用者主體名稱 (UPN) 的 Active Directory 與憑證設定的屬性系統管理，或設為目標 VIP 帳戶針對特定種類的攻擊，如下所述。  

##### <a name="upn-hijacking-for-certificate-spoofing"></a>憑證偽造的 UPN 攔截

雖然公開金鑰基礎結構 (Pki) 的攻擊完整討論已超出本文的範圍，對公開和私密金鑰的 Pki 攻擊已自 2008年起以指數方式增加。 已廣泛缺憾公用 Pki 的缺口，但對組織的內部 PKI 攻擊很可能是更豐富。 一種這類攻擊會利用 Active Directory 與憑證讓攻擊者偽造的方式，可能難以偵測的其他帳戶的認證。  

已加入網域的系統驗證憑證時，主體或主體別名 (SAN) 屬性，憑證中的內容用來將憑證對應至 Active Directory 中使用者物件。 根據憑證和其建構方式的類型，在憑證中的主旨屬性通常包含使用者的一般名稱 (CN)，如下列螢幕擷取畫面所示。  

![最低權限管理模型](media/Implementing-Least-Privilege-Administrative-Models/SAD_4.gif)  

根據預設，Active Directory，請建構藉由串連帳戶的第一個名稱的使用者的 CN +""+ 姓氏。 在 Active Directory 中的使用者物件的 CN 元件不需要或保證是唯一的但將移至不同的位置，在目錄中的使用者帳戶變更帳戶的分辨的名稱 (DN)，這是在物件的完整路徑目錄，如先前的螢幕擷取畫面的下方窗格中所示。  

因為憑證主體名稱不一定是靜態或唯一，主體別名的內容通常可在 Active Directory 中找出使用者物件。 憑證發行給使用者，從企業憑證授權單位 （Active Directory 整合式 Ca） 的 SAN 屬性通常會包含使用者的 UPN 或電子郵件地址。 因為 Upn 一定是唯一的 AD DS 樹系中，尋找使用者物件的 UPN 通常做為執行驗證，有或沒有憑證驗證程序所涉及的一部分。  

在 驗證憑證的 SAN 屬性中的 upn，請使用可利用攻擊者取得詐騙憑證。 如果攻擊者已盜用的帳戶能夠讀取及寫入使用者物件上的 Upn，攻擊是無限期地實作，如下所示：  

（例如 VIP 使用者） 的使用者物件上的 UPN 屬性暫時變更為不同的值。 SAM 帳戶名稱屬性和 CN 也可以變更在此階段中，雖然這通常是不必要稍早所述的理由。  

當目標帳戶的 UPN 屬性已變更時，是過時，已啟用的使用者帳戶或為剛建立的使用者帳戶的 UPN 屬性變更為原先已指派給目標帳戶的值。 過時，已啟用的使用者帳戶是不登入的長一段時間，但尚未停用的帳戶。 它們的目標攻擊者想要 「 隱藏在顯而易見的 」，原因如下：  

1. 因為帳戶已啟用，但沒有最近使用過，使用的帳戶不太可能觸發警示的方式來啟用已停用的使用者帳戶可能。  
2. 使用現有的帳戶不需要建立新的使用者帳戶，可能會注意到系統管理人員。  
3. 仍舊處於啟用狀態的過時的使用者帳戶通常是各種安全性群組的成員，並簡化存取和 「 漸變"現有使用者母體擴展在網路上資源的存取權授與。  

在其目標 UPN 現在已設定的使用者帳戶用來從 Active Directory 憑證服務要求一或多個憑證。  

當有攻擊者的帳戶取得憑證時，「 新 」 的帳戶和目標帳戶 Upn 會回到其原始值。  

攻擊者現在擁有一或多個可提供資源與應用程式的驗證中，如同使用者是暫時修改其帳戶的 VIP 使用者的憑證。 雖然所有可由攻擊者目標憑證和 PKI 的方式完整討論已超出本文的範圍，提供此攻擊機制是為了說明為什麼您應該監視特殊權限和 VIP 帳戶在 AD DS 變更特別是針對任何屬性上的變更**帳戶**帳戶 （例如 cn、 名稱、 sAMAccountName、 userPrincipalName 及 userAccountControl） 索引標籤。 除了監控帳戶，您應該限制誰可以修改帳戶，以較小一組系統管理使用者越好。 同樣地，系統管理使用者的帳戶應該受到保護，並監視未經授權的變更。  
