---
ms.assetid: 7a7ab95c-9cb3-4a7b-985a-3fc08334cf4f
title: "實作最低權限管理模型"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9716d442446b2705dfd2803d061cb884a5e72fbe
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="implementing-least-privilege-administrative-models"></a>實作最低權限管理模型

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

下列摘要是從[系統管理員帳號安全性規劃指南](https://technet.microsoft.com/library/cc162797.aspx)，第一次在 1999 年 4 月 1 日發行：  
  
> 「 最安全性相關訓練課程和文件討論實作原則權限，但組織少依照。 原則很簡單，並正確大幅套用的影響提升您的安全性，降低您的風險。 原則會指出所有使用者應該已經最低限度權限才能完成目前的工作而不需使用者 account 的都登入。 如此一來提供其他攻擊之間的惡意程式碼防護。 這個原則適用於電腦和電腦的使用者。   
> 「 其中一個原因，此原則，也可以為它強制您進行內部研究。 例如，您必須判斷的存取權限的電腦或使用者真正需要並加以執行。 許多組織，這項工作最初似乎變得更好的使用者張貼的工作。不過，它是成功保護您的網路環境基本步驟。   
> 「 您應該會授與所有網域系統管理員的使用者權限的概念其網域權限。 例如，如果系統管理員的身分登入的特殊權限帳號，並不小心執行一個防毒程式，病毒擁有管理及存取權本機電腦的完整網域。 如果系統管理員而必須以無 （非） account 登入，病毒的範圍損壞只會在本機電腦因為它會執行本機電腦的使用者。   
> 「 另一個例子，帳號，您授與網域層級系統管理員權限必須不有提高權限在另一部樹系，即使有信任關係的樹系之間。 這項策略有助於避免廣泛的損害，若有危害一個受管理的樹系攻擊。 組織應該定期稽核他們的網路，以防止未經授權的權限提升。 」  
  
下列摘要是從[Microsoft Windows 安全性資源套件](https://www.microsoft.com/learning/en/us/book.aspx?ID=6815&locale=en-us)、 第一個在發行 2005年:  
  
> 「 永遠的安全性，以授予權限，才能執行工作的最低的想法。 如果應該入侵太多權限的應用程式，攻擊者可能無法以展開超過項目會應用程式已經在最少可能權限的攻擊。 例如，檢查無意打開的電子郵件附件，時限病毒的網路系統管理員的結果。 系統管理員身分登入使用網域管理員，若病毒將會有網域中的所有電腦上的系統管理員權限，因此不受限制存取網路上幾乎所有資料。 如果系統管理員身分登入使用本機系統管理員帳號，病毒將會在本機電腦上有系統管理員權限，因此想無法存取電腦上的任何資料，並在電腦上安裝軟體鍵-筆觸登入惡意軟體。 如果系統管理員身分登入使用一般帳號，病毒將可以存取只系統管理員的資料並不能安裝惡意軟體。 透過小必要朗讀在此範例中，電子郵件權限可能危害的範圍就會大幅降低。 」  
  
## <a name="the-privilege-problem"></a>權限的問題  
上述摘要中所述的原則維持不變，但在評估 Active Directory 安裝，我們側面找到的已被授與權限超越所需執行日常工作，帳號過數字。 環境大小影響原始的權限過於帳號，數字，但不是 proportionmidsized 可能會有許多帳號最高特殊權限的群組時可能成千上萬更大安裝。 有幾個例外，無論的攻擊者的技術與集合，攻擊通常會依照最低抗拒的路徑。 它們基於這個工具和方法如果和時才簡單機制失敗或會阻止這類攻擊防禦者。  
  
很抱歉，您的環境中最抗拒的路徑證明帳號 broad 和深度的權限的超額使用。 Broad 權限的權利和帳號，例如執行特定活動的環境-大型寬廣上的權限，幫助的支援人員可能會授與重設密碼，許多使用者帳號他們的權限。  
  
深的權限，可套用至擴展窄區段強大權限，這類提供工程系統管理員權限的伺服器上，才能執行修復。 Broad 權限和深度的權限都不一定危險，但時網域中的許多帳號永久授與 broad 和深度的權限，如果只有受到危害的其中一個帳號，它可以快速用於重新環境目的攻擊者的設定，或甚至破壞基礎結構大量區段。  
  
Pass hash 攻擊，是一種認證竊取攻擊，而普遍因為執行這些工具且自由地提供簡單易用，因為許多環境容易受到攻擊。 不過，pass hash 攻擊，不是真正的問題。 問題的關鍵有雙重：  
  
1.  它很容易攻擊者取得一部電腦上的深度權限，並傳送到其他電腦廣泛的權限。  
  
2.  在電腦地標通常太多永久帳號，高等級的權限。  
  
即使排除 pass hash 攻擊，攻擊只會使用不同的策略，不不同的方法。 除了播種惡意程式碼，其中包含認證竊取工具，它們可能會植物惡意程式碼登的按鍵輸入，或利用任意數量的擷取認證跨環境 」 功能的其他方法。 如何將，無論目標保持不變，： 帳號 broad 和深度的權限。  
  
太多權限授與不只找到 Active Directory 中危害的環境中。 當組織所開發超過需要更多的權限授與習慣時，通常是透過下列小節中所述基礎結構找到。  
  
## <a name="in-active-directory"></a>在 Active Directory  
在 Active Directory，通常會尋找 EA、 DA 和 BA 群組包含的帳號過數字。 最常，組織 EA 群組包含少成員、 DA 群組通常會包含加成數目的使用者，在 EA 群組中，及系統管理員群組通常會包含更多的成員，比其他群組結合擴展。 這通常是因為系統管理員的日子相信 」 較少比 DAs 或 EAs 的權限]。 會與的權利和權限授與每個群組，同時它們有效考慮同樣強大群組成員，都可以讓他或自己的其他這兩個成員因為。  
  
## <a name="on-member-servers"></a>伺服器成員  
當我們擷取的成員伺服器，您的環境中的系統管理員的本機群組成員資格時，我們發現範圍從本機和網域帳號，少數到許多巢群組成員資格，展開時，顯示數百種，甚至是數千使用本機系統管理員權限帳號，在伺服器上。 很多時候，網域群組成員資格大的屬於成員伺服器本機系統管理員群組，而不是任何使用者都可以修改的網域中的群組成員資格可以控制權系統上所有系統群組已本機系統管理員群組中巢考慮。  
  
## <a name="on-workstations"></a>在 [工作站  
雖然工作站通常會有大幅較少成員他們本機系統管理員群組比成員伺服器，您的環境中，使用者會授與他們個人的電腦上系統管理員本機群組成員資格。 這發生時，即使支援 UAC，那些使用者提供整合的工作站提升權限的風險。  
  
> [!IMPORTANT]  
> 您應該會仔細考慮是否使用者需要系統管理員權限工作站，以及成員的系統管理員群組的電腦上建立不同的本機 account 如此，可能會更好的方式。 當使用者需要權限時，他們可以提供該本機提高權限的認證，但因為本機帳號，無法使用危害的其他電腦或存取網域資源。 如同任何本機帳號，但是，本機特殊權限 account 認證應唯一;如果您使用多個工作站相同的認證建立本機帳號，您會公開 pass hash 攻擊的電腦。  
  
## <a name="in-applications"></a>在應用程式  
目標是診斷組織的作業的攻擊，以授與應用程式中的強大權限帳號鎖定允許 exfiltration 的資料。 存取敏感資料帳號可能會被授與網域或作業系統不提高權限，但帳號，可以管理設定的應用程式或應用程式資訊的存取權提供有風險。  
  
## <a name="in-data-repositories"></a>在 [資料存放庫  
攻擊者搜尋的存取權的文件及其他檔案中的其他目標一樣，可以針對帳號，控制項的檔案存放區的存取，帳號，就能直接存取檔案，甚至群組或存取檔案的角色。 例如如果檔案伺服器用來儲存合約文件，存取受文件所使用的 Active Directory 群組攻擊者可以修改群組成員資格可以新增到群組危害的帳號，並存取合約文件。 萬一 SharePoint 應用程式中提供的存取權的文件，如上文攻擊者時，可以針對應用程式。  
  
## <a name="reducing-privilege"></a>減少權限  
更大、 更複雜環境，更難管理及保護。 在組織中小，審查和降低權限可能會非常簡單的方法，但每個其他伺服器、 工作站、 帳號，並在組織中使用的應用程式新增另一個要保護的物件。 很難或甚至無法正常運作，因為安全每個層面公司的 IT 基礎結構，您應該將焦點放努力第一次上的權限建立最大風險帳號，通常特殊權限建帳號群組 Active Directory 中及特殊權限的本機帳號工作站成員伺服器上。  
  
### <a name="securing-local-administrator-accounts-on-workstations-and-member-servers"></a>本機系統管理員帳號工作站成員伺服器上的保護  
本文件焦某保護 Active Directory，如之前所述，雖然大多數攻擊 directory 開始為個人主機攻擊。 無法提供完整的本機群組成員系統的保護指導方針，但下列建議可以用來協助您保護本機系統管理員帳號工作站成員伺服器上。  
  
#### <a name="securing-local-administrator-accounts"></a>保護帳號本機系統管理員  
在所有 Windows 版本中目前的主要支援，本機停用根據預設，這會讓 account pass hash 和其他認證竊取攻擊無法使用。 但是，在包含的舊版作業系統的網域或已支援的本機系統管理員帳號，這些帳號可成員伺服器和工作站傳播危害上文所述。 基於這個原因，下列控制項是針對所有加入網域的系統區域的系統管理員帳號建議。  
  
適用於執行這些控制項詳細的指示中提供[附錄 H:WINDOWS 保護本機系統管理員帳號及群組](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)。 之前實作這些設定，但確保本機系統管理員帳號不目前用於環境中在電腦上執行的服務，或是執行其他活動的這些帳號不應該使用。 測試完全之前在 production 環境中執行這些設定。  
  
#### <a name="controls-for-local-administrator-accounts"></a>本機系統管理員帳號控制  
建系統管理員帳號永遠不能當做成員伺服器上的服務帳號，也不應該其將用來登入本機電腦 （除非在安全模式下，即使 account 已停用允許）。 實作本文中的設定是以防止每部電腦的本機正在可用，除非它們控制項第一次還原。 實作這些控制或監視變更系統管理員帳號，您可以大幅降低的可能性成功的攻擊本機系統管理員帳號該目標。  
  
##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-joined-systems"></a>設定限制加入網域的系統管理員的身分帳號 Gpo  
在您建立和連結工作站和成員伺服器 Ou 每個網域中的一或多個 Gpo，將管理員新增至下列使用者權限在**電腦設定 \ 原則 \windows 安全性設定本機 Settings\User 權限指派**:  
  
-   拒絕從網路存取此電腦  
  
-   拒絕以分批登入  
  
-   拒絕登入即服務  
  
-   透過遠端桌面服務拒絕登入  
  
當您新增這些使用者權限管理員帳號時，指定您要新增是否本機或網域中的系統管理員帳號順帶一提您標籤 account。 例如，新增這些 NWTRADERS 網域中的系統管理員 account 拒絕權限，您會輸入 account 為**NWTRADERS\Administrator**，或瀏覽以系統管理員負責 NWTRADERS 網域。 為確保您的本機限制，請輸入：**系統管理員**這些使用者權限的群組原則物件編輯器設定。  
  
> [!NOTE]  
> 即使重新命名本機系統管理員帳號，仍會套用原則。  
  
這些設定將可確保您的電腦的系統管理員 account 無法用來連接到其他電腦，即使這不小心或惡意支援。 無法完全停用本機登入使用本機系統管理員帳號，也不應該您嘗試這樣做，因為電腦的本機的設計目的是要用於損壞修復案例。  
  
應該成員伺服器或工作站成為退出網域與任何其他本機帳號授與系統管理員權限、 電腦開機進入安全模式、 可支援的系統管理員帳號，並 account 然後可用來影響電腦上的修復。 當替換完成後時，管理員應該一次停用。  
  
### <a name="securing-local-privileged-accounts-and-groups-in-active-directory"></a>保護本機特殊權限的帳號，並 Active Directory 中的群組  
*法律第 6 號： 電腦只有在安全的系統管理員是可信任。* - [安全性 （2.0 版） 的 10 定律](https://technet.microsoft.com/security/hh278941.aspx)  
  
以下提供的資訊被要讓一般指導方針保護的最高的權限建帳號和 Active Directory 中的群組。 也會提供詳細逐步指示在[附錄 d 保護建系統管理員帳號 Active Directory 中](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)，[附錄 e 保護企業管理員群組 Active Directory 中](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)、[附錄 f︰ 保護網域管理員群組 Active Directory 中](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)，並在[附錄 g： 保護系統管理員群組 Active Directory 中](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)。  
  
在這些設定實作之前，您也應該測試完全若要判斷是否有適用於您的環境所有的設定。 並非所有組織都無法執行這些設定。  
  
#### <a name="securing-built-in-administrator-accounts-in-active-directory"></a>在 Active Directory 保障建系統管理員帳號  
在 Active Directory 中每個網域中建立管理員建立網域中的一部分。 預設為這個 account 網域系統管理員 」 及網域中的系統管理員群組成員和如果網域森林根網域，account 也是您的企業系統管理員群組成員。 使用本機系統管理員 account 網域的應該保留僅適用於初始建置活動和可能損壞修復案例。 為了確保建可用於影響替換，可以使用任何其他帳號，您應該不會變更管理員森林中的任何網域中的預設成員資格。 而是，您應該下列以協助保護森林中的每個網域中管理員指導方針。 適用於執行這些控制項詳細的指示中提供[附錄 d 保護建系統管理員帳號 Active Directory 在](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
  
#### <a name="controls-for-built-in-administrator-accounts"></a>控制帳號建系統管理員  
實作本文設定的目標是每個網域中的系統管理員 account （不是群組） 除非還原控制項的數字會防止可用。 藉由實作這些控制及監視變更的系統管理員帳號，您可以利用網域管理員大幅降低攻擊的可能性。 針對每個您樹系網域中的系統管理員帳號，您應該進行下列設定。  
  
##### <a name="enable-the-account-is-sensitive-and-cannot-be-delegated-flag-on-the-account"></a>讓 「 Account 是機密，無法委派 」 上的旗標 account  
根據預設，您可以委派 Active Directory 中的所有帳號。 委派讓電腦或服務提供的認證的電腦已驗證 account 服務來取得代表 account 服務的其他電腦。 您可以在**機密帳號，無法委派**屬性網域型帳號，無法獲得 account 的認證的電腦或其他服務網路限制利用其他系統上使用 account 的認證委派的攻擊。  
  
##### <a name="enable-the-smart-card-is-required-for-interactive-logon-flag-on-the-account"></a>讓 「 智慧卡，才互動式登入 「 旗標帳號  
您可以在**智慧卡，才互動式登入**屬性帳號，Windows 將密碼重設為 120 字元隨機的值。 設定此旗標建管理員帳號，您可以確定 account 的密碼不只長且複雜，但不是知道的所有使用者。 不技術需要建立智慧卡帳號之前讓這個屬性，但如果可能，請應該針對每個管理員前設定 account 限制建立智慧卡和智慧卡應存放在安全的位置。  
  
雖然設定**智慧卡，才互動式登入**旗標重設密碼、 防止使用者權限來重設密碼的 account 設為 [已知的值，並在網路上使用 account 的名稱及存取資源的新密碼。 因此，您應該實作 account 下列其他控制項。  
  
##### <a name="disable-the-account"></a>停用 Account  
如果未已停用管理員，停用它當您完成設定 account 的屬性。 這可防止 account 除非第一次可以使用適用於任何用途。 在損壞復原案例中將可執行的 AD DS 環境修復無帳號，您可以網域控制站開機進入安全模式、 在本機建 （這不會封鎖來自登入本機），以重新登入，讓網域的管理員，若有必要。  
  
##### <a name="configuring-gpos-to-restrict-domains-administrator-accounts-on-domain-joined-systems"></a>設定限制加入網域的系統上網域中的系統管理員帳號 Gpo  
雖然停用網域中的系統管理員 account 使 account 有效無法使用，您應該其他限制上實作 account 以防 account 不小心或惡意支援。 雖然這些控制項最終可以透過管理員回復，目標是建立 slow 攻擊者進行，控制和可能會損壞 account 限制。  
  
在您建立和連結工作站和成員伺服器 Ou 每個網域中的一或多個 Gpo，將每個網域的管理員新增至下列使用者權限在**電腦設定 \ 原則 \windows 安全性設定本機 Settings\User 權限指派**:  
  
-   拒絕從網路存取此電腦  
  
-   拒絕以分批登入  
  
-   拒絕登入即服務  
  
-   透過遠端桌面服務拒絕登入  
  
> [!NOTE]  
> 當您新增本機系統管理員帳號此設定時，您必須指定您要設定本機系統管理員帳號或網域系統管理員帳號。 例如，新增拒絕 NWTRADERS 網域的本機這些權限，您必須輸入 account 為**NWTRADERS\Administrator**，或瀏覽到本機系統管理員負責 NWTRADERS 網域。 如果您輸入**系統管理員**中的這些使用者權限設定群組原則物件編輯器] 中，您將會限制本機管理員 GPO 所套用的每一部電腦上。  
>   
> 我們建議以相同的方式為網域型系統管理員帳號限制本機系統管理員帳號工作站成員伺服器上。 因此，您應該通常加入每個網域森林中的系統管理員負責和本機電腦的系統管理員負責這些使用者權限設定。 下圖顯示設定封鎖本機系統管理員帳號，並網域的管理員，執行下列帳號的應該不需要登入的這些使用者權限的範例。  

>   
> ![最小的權限管理員模型](media/Implementing-Least-Privilege-Administrative-Models/SAD_20.gif)  
  
##### <a name="configuring-gpos-to-restrict-administrator-accounts-on-domain-controllers"></a>設定限制的系統管理員帳號網域控制站 Gpo  
在森林中的每個網域中的預設網域控制站的原則或原則連結到網域控制站組織單位應修改將每個網域的管理員新增至下列使用者權限在**電腦設定 \ 原則 \windows 安全性設定本機 Settings\User 權限指派**:  
  
-   拒絕從網路存取此電腦  
  
-   拒絕以分批登入  
  
-   拒絕登入即服務  
  
-   透過遠端桌面服務拒絕登入  
  
> [!NOTE]  
> 這些設定將可確保您的本機無法用來連接網域控制站，雖然帳號，如果功能，可以登入本機網域控制站。 應該僅支援並損壞修復案例中使用此帳號，因為它被預期實體存取至少網域控制站將或從遠端存取網域控制站的權限的其他帳號，可以使用。  
  
##### <a name="configure-auditing-of-built-in-administrator-accounts"></a>設定的建系統管理員帳號稽核  
當您有保護每個網域中的系統管理員帳號，並將它關閉時，您應該設定稽核監視 account 變更。 如果尚未 account、 重設密碼，或任何其他修改過去，應該會收到通知的使用者或的小組負責 AD ds，除了事件回應團隊，在組織中管理。  
  
### <a name="securing-administrators-domain-admins-and-enterprise-admins-groups"></a>保障系統管理員、 網域系統管理員 」 及企業管理員群組  
  
#### <a name="securing-enterprise-admin-groups"></a>保護企業管理員群組  
企業系統管理員群組中，位於森林根網域中，應包含在日常，可能的網域的本機，除了不使用者提供如之前所述方式保護和[附錄 d 保護建系統管理員帳號 Active Directory 在](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
  
EA 存取必要時，其帳號需要 EA 權利與權限，使用者應該暫時置於企業系統管理員 」 群組。 雖然使用者使用高度授權的帳號，應該稽核，最好是執行的一位使用者執行所做的變更，並觀察所做的變更的其他使用者最小化非故意濫用或設定錯誤的可能性他們的活動。 完成後活動，帳號應該會從 EA 群組。 這可以實現手動程序，記載處理程序，第三方特殊權限的身分日存取管理 (PIM 日 PAM) 軟體或兩者。 指導方針建立帳號，可以用來控制的 Active Directory 中有特殊權限群組成員資格的中所提供[的認證竊取吸引帳號](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)中提供詳細的指示及[附錄 i： 建立管理帳號保護帳號及群組 Active Directory 中的](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
企業系統管理員是，預設建森林中的每個網域中的系統管理員群組成員。 因為發生的樹系損壞修復案例中，EA 權限很可能就需要從系統管理員群組中的每個網域移除企業系統管理員群組是不適當的修改。 如果企業系統管理員群組已移除從森林中的系統管理員群組，應該加入每個網域中的系統管理員群組，下列其他控制項應：  
  
-   企業系統管理員群組如上文所述，應包含在日常，可能安全中所述的樹系根網域管理員帳號，除了不使用者[附錄 d 保護建系統管理員帳號 Active Directory 在](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
  
-   EA 群組應該 gpo 連結到 Ou 包含成員伺服器與每個網域中的工作站，新增至下列使用者權限︰  
  
    -   拒絕從網路存取此電腦  
  
    -   拒絕以分批登入  
  
    -   拒絕登入即服務  
  
    -   在本機拒絕登入  
  
    -   透過遠端桌面服務，拒絕登入。  
  
這會阻止 EA 群組成員登入成員伺服器以及工作站。 如果捷徑伺服器可用來管理網域控制站和 Active Directory，確定捷徑伺服器位於不連結的嚴格 Gpo 組織單位。  
  
-   稽核應該會傳送任何修改 EA 群組成員資格或屬性警示設定。 這些應該會收到通知，至少的使用者或的小組負責 Active Directory 管理及意外回應。 您也應該定義處理程序以及暫時填入 EA 群組，包括通知程序群組合法人口執行時的程序。  
  
#### <a name="securing-domain-admins-groups"></a>保護網域管理員群組  
企業系統管理員群組一樣，應該只在組建或損壞修復案例中需要網域系統管理員群組成員資格。 應該有不日常帳號本機系統管理員負責網域中，除了 DA 群組中所述保護[附錄 d 保護建系統管理員帳號 Active Directory 在](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
  
需要 DA 存取權時，需要這個層級的存取權限帳號應該暫時放 DA 群組中的問題網域。 雖然使用者使用高度授權的帳號，應該稽核活動，以及最好是執行最小化非故意濫用或設定錯誤的可能性執行所做的變更的一位使用者和另一位使用者觀察所做的變更。 完成後活動，帳號應該移除網域系統管理員 」 群組。 這可以實現手動程序，記載處理程序，透過協力廠商特殊權限的身分日存取管理 (PIM 日 PAM) 軟體或兩者。 指導方針建立帳號，可以用來控制的 Active Directory 中有特殊權限群組成員資格的提供可在[附錄 i： 建立管理帳號 Active Directory 中的群組保護帳號，](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)。  
  
網域系統管理員是，預設的所有成員伺服器與各自網域中的工作站本機系統管理員群組成員。 此預設巢應該修改，因為它會影響性和損壞的復原選項。 如果從本機系統管理員群組成員伺服器上已移除網域管理員群組，他們應該加入每個成員 server 和工作站透過 [設定限制的群組 Gpo 連結網域中的系統管理員群組。 下列一般控制項，深度中所述[附錄 f︰ 保護網域管理員群組 Active Directory 在](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)也應執行。  
  
森林中的每個網域中的網域管理員群組：  
  
1.  移除所有成員 DA 群組中，可能的建網域中，除了所述的保護提供[附錄 d 保護建系統管理員帳號 Active Directory 在](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
  
2.  Gpo 連結到 Ou 包含成員伺服器與每個網域中的工作站，應該 DA 群組新增至下列使用者權限︰  
  
    -   拒絕從網路存取此電腦  
  
    -   拒絕以分批登入  
  
    -   拒絕登入即服務  
  
    -   在本機拒絕登入  
  
    -   透過遠端桌面服務拒絕登入  
  
    這會阻止 DA 群組成員登入成員伺服器以及工作站。 如果捷徑伺服器可用來管理網域控制站和 Active Directory，確定捷徑伺服器位於不連結的嚴格 Gpo 組織單位。  
  
3.  稽核應該會傳送任何修改 DA 群組成員資格或屬性警示設定。 這些應該會收到通知，至少的使用者或的小組負責 AD DS 管理及意外回應。 您也應該定義處理程序以及暫時填入 DA 群組，包括通知程序群組合法人口執行時的程序。  
  
#### <a name="securing-administrators-groups-in-active-directory"></a>保護 Active Directory 中的系統管理員群組  
EA 和 DA 群組一樣，應該只在組建或損壞修復案例中需要系統管理員 (BA) 群組成員資格。 應該有不日常帳號系統管理員除外本機系統管理員負責網域群組中所述保護[附錄 d 保護建系統管理員帳號 Active Directory 在](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
  
所需的系統管理員的存取權時，需要這個層級的存取權限帳號應該暫時放網域有問題的系統管理員 」 群組。 使用者使用高度授權的帳號，雖然活動應該會稽核，最好使用使用者執行所做的變更，並觀察所做的變更的其他使用者最小化非故意濫用或設定錯誤的可能性執行。 活動完成後，從系統管理員群組應該會立即移除帳號。 這可以實現手動程序，記載處理程序，透過協力廠商特殊權限的身分日存取管理 (PIM 日 PAM) 軟體或兩者。  
  
系統管理員是，大部分各自網域中 AD DS 物件的擁有者預設。 此群組成員資格可能需要在組建和損壞擁有權或拍攝物件的擁有權的功能是在需要復原案例中。 此外，DAs 和 EAs 繼承他們的權限和一定他們預設群組中的成員系統管理員權限的數字。 預設群組巢不應修改特殊權限的群組 Active Directory 中，針對和安全每個網域中的系統管理員群組中所述[附錄 g： 保護系統管理員群組 Active Directory 中](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)，以及一般下方的指示操作。  
  
1.  移除所有成員的系統管理員群組中，可能的本機系統管理員負責網域中，除了所述的保護提供[附錄 d 保護建系統管理員帳號 Active Directory 在](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)。  
  
2.  登入成員伺服器或工作站應該完全不需要的網域中的系統管理員群組成員。 一或多個 gpo 連結到工作站和成員伺服器 Ou 每個網域中，系統管理員群組應該加入下列使用者權限：  
  
    -   拒絕從網路存取此電腦  
  
    -   拒絕分批，以登入  
  
    -   拒絕登入即服務  
  
    -   這會阻止的系統管理員群組成員用來登入或連接到成員伺服器或工作站 （除非多控制項的第一次破壞） 位置他們的認證可能會快取，藉此危害。 有特殊權限的 account 應該永遠不會用來登入的權限較低系統，並執行這些控制項可以提供的防護一些攻擊。  
  
3.  在每一個網域中的樹系的系統管理員群組組織單位應被授與下列使用者的網域控制站權利 （如果它們已無權這些），這會讓來執行所需的樹系損壞復原案例功能的系統管理員群組成員：  
  
    -   從網路存取此電腦  
  
    -   在本機允許登入  
  
    -   允許登入透過遠端桌面服務  
  
4.  稽核應該設定的系統管理員群組成員資格或屬性任何修改傳送通知。 這些應該會收到通知，至少負責 AD DS 管理小組的成員。 也會收到通知安全性小組的成員，並修改的系統管理員群組成員資格應定義程序。 具體而言，這些處理程序應包含安全性小組的通知時時不會收到通知，, 它們會如預期般並不會引發鬧鐘修改即將系統管理員群組程序。 此外，應該實作通知安全性小組的系統管理員群組使用已經完成之帳號已從群組時的處理程序。  
  
> [!NOTE]  
> 當您的系統管理員群組 Gpo 上實作限制時，Windows 會套用的設定，除了網域中的系統管理員群組的電腦本機系統管理員群組成員。 因此，您應該時小心上系統管理員群組實作限制。 系統管理員群組成員制訂網路、 批次及服務登入，建議您不論是可行實作，但不會限制登入本機或透過遠端桌面服務登入。 封鎖這些登入類型，可以封鎖合法管理某部電腦的系統管理員本機群組成員。 下圖顯示本機封鎖不當建設定，並網域系統管理員帳號，除了不當建本機或網域系統管理員 」 群組。 請注意，**透過遠端桌面服務拒絕登入**使用者權限，不包含管理員群組，因為包含在此設定也會封鎖這些登入，必須在本機電腦的系統管理員群組成員。 電腦上的服務執行操作有特殊權限的群組本節所述的設定，如果實作這些設定可能會造成服務和應用程式失敗。 因此，與的所有建議在本區段中，您應該會完全都測試適用於設定您的環境中。  

>   
> ![最小的權限管理員模型](media/Implementing-Least-Privilege-Administrative-Models/SAD_3.gif)  
  
### <a name="role-based-access-controls-rbac-for-active-directory"></a>Active Directory 角色為基礎的存取控制 (RBAC)  
一般而言，以角色為基礎存取控制 (RBAC) 是群組的使用者，並提供資源商業規則的存取權的機制。 在 Active Directory AD ds 實作 RBAC 是建立的權限與權限的委派給角色允許不太多權限授與他們執行日常的系統管理工作角色的成員。 Active Directory 中為 RBAC 可以設計和實作透過原生工具和介面，利用您可能已經擁有，你第三方或任何組合種方法購買的軟體。 本章節不提供逐步指示實作 RBAC 的 Active Directory，但改為討論的因素考慮中選擇實作 RBAC AD DS 安裝您的方法。  
  
#### <a name="native-approaches-to-rbac-for-active-directory"></a>原生種 RBAC 的 Active Directory  
最簡單的 RBAC 實作，您可以實作角色為 AD DS 群組並代理人的權利和權限的群組，允許執行日常的系統管理指定的範圍中的角色。  
  
有時候，現有在 Active Directory 安全性群組可以用於授與的權利和工作函式的適當權限。 例如，如果 IT 組織中的特定員工負責管理與維護 DNS 區域和記錄，委派那些責任可以簡單 account 建立的每個 DNS 系統管理員，並將它新增到群組 DNS 系統管理員 Active Directory 中為。 DNS 管理群組，然而更高特殊權限的群組在 Active Directory，有幾個強大的權限，雖然這群組成員已委派權限，讓它們來管理 DNS。  
  
有時候，您可能需要建立安全性群組和代理人的權利和允許群組成員 Active Directory 物件、 檔案系統物件，以及登錄物件的權限來執行指定管理工作。 例如，負責遺失的密碼重設您的技術支援電信業者時，協助使用者的連接的問題，以及疑難排解應用程式設定中，您可能需要結合委派設定使用者 Active Directory 物件的權限，支援服務的使用者來連接遠端使用者的電腦，以檢視或修改使用者的設定可讓。 針對每個您定義的角色，您應該找出：  
  
1.  日常和頻率較低執行哪些工作執行哪些工作角色的成員。  
  
2.  系統上，以及哪些應用程式中的角色成員應該會授與權限。  
  
3.  哪些使用者應被授與的角色成員資格。  
  
4.  如何將會執行管理角色成員資格。  
  
您的環境中，手動建立角色為基礎的存取 Active Directory 環境管理控制可能會很困難與維護。 如果清楚定義角色及系統管理 IT 基礎架構的責任，您可能想要利用其他工具，可協助您建立可以管理原生 RBAC 部署。 例如，如果 Forefront 身分管理員 (FIM-A) 中使用您的環境中，您可以使用 FIM 來建立和管理的角色，可以輕鬆地執行管理人口自動化。 如果您使用 System Center Configuration Manager (SCCM) 和 System Center Operations Manager (SCOM)，您可以使用特定應用程式的角色委派管理及監視功能，並也執行設定的一致性與稽核跨網域中的系統。 如果您有實作公用基礎結構 (PKI)，您可以問題，並要求負責管理環境 IT 人員智慧卡。 FIM Credential 管理 （FIM-A 公分），您甚至可以管理的角色與認證結合為您管理的人員。  
  
有時候，它可能組織考慮部署第三方 RBAC 軟體提供 「 的全新 」 功能的建議。 Active Directory、 Windows 及非 Windows 目錄作業系統的 RBAC commercial、 現有 (COTS) 方案提供的供應商的數字。 在選擇之間原生方案和第三方你時，您應該考慮下列：  
  
1.  高預算： 投資開發 RBAC 使用軟體，以及您已經擁有的工具，可減少軟體相關的成本部署方案。 不過，除非您有豐富的經驗，在 [建立及部署原生 RBAC 方案的人員，您可能需要參與顧問資源開發方案。 您應該會仔細衡量預期的費用自訂開發方案的成本部署 」 的全新 「 方案，尤其是預算會限制。  
  
2.  IT 環境組成： 如果您的環境主要組成 Windows 系統中，或如果您已經運用 Active Directory 非 Windows 系統管理帳號，自訂的原生方案可能會為您的需求提供最佳方案。 如果您的基礎結構包含許多系統，不執行 Windows 不會由 Active Directory，您可能要考慮選項分開 Active Directory 環境非 Windows 系統管理。  
  
3.  方案中的雲端模型： 如果 product 依賴位置高特殊權限的群組 Active Directory 中為其服務帳號，並不提供不需要太多權限的選項會授與 RBAC 軟體，則您真的不降低您的 Active Directory 攻擊 surfaceyou 已只變更組成 directory 中最有特殊權限的群組。 除非應用程式廠商可以提供服務帳號，最小化帳號危害及惡意使用的可能性控制項，您可能要考慮其他選項。  

  
### <a name="privileged-identity-management"></a>有特殊權限的身分管理  
特殊權限的身分管理 (PIM)，有時到特殊權限 account 為管理 (PAM) 或 credential 特殊權限的管理 (PCM) 會建築設計和實作的方法來管理特殊權限在您的基礎結構帳號。 一般而言，PIM 提供的機制來帳號會授與暫時權限，並權限，才能執行建置或中斷修復功能，而不是離開永久連接到帳號權限。 是否 PIM 功能手動建立，或透過實作可能提供部署協力廠商軟體一或多個下列功能：  
  

-   認證 」 保存庫、 [位置] 核取 [並受指派的初始密碼，然後 「 簽入 」 時活動已完成，此時會再試一次重設密碼帳號特殊權限的密碼。  
  
-   使用的權限的認證繫結的時間限制  
  
-   其中之一單次使用認證  
  
-   工作流程自訂監視與報告執行之活動和自動移除權限的活動正在完成，或分配時間的權限授與已經過期  
  
-   更換固定認證，例如使用者名稱和密碼的應用程式開發介面 (Api) 的指令碼，讓從保存庫視擷取認證  
  
-   自動服務 account 認證管理  
  
### <a name="creating-unprivileged-accounts-to-manage-privileged-accounts"></a>若要管理特殊權限的帳號建立授權的帳號  
管理特殊權限的帳號挑戰，根據預設，可以管理已授權和受保護的帳號帳號的權限的群組和帳號受保護。 如果您安裝您 Active Directory 實作適當 RBAC 和 PIM 方案，方案可能包含方式，可讓您有效 depopulate 的最有特殊權限的群組 directory，在填入只會暫時和時所需的群組成員資格。  
  
如果您實作原生 RBAC 和 PIM，但是，您應該時所需的 Active Directory 中建立帳號，已經不權限的功能只有填入與 depopulating 權限的群組。 [附錄 i： 帳號保護帳號和 Active Directory 中的群組的建立管理](../../../ad-ds/manage/component-updates/../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)提供逐步指示為這個項目的建立帳號，您可以使用。  
  
### <a name="implementing-robust-authentication-controls"></a>實作穩定驗證控制  
*法律第 6 號： 確實是其他人查看可用嘗試猜到您的密碼。* - [10 變的法律的安全性管理](https://technet.microsoft.com/library/cc722488.aspx)  
  
Pass hash 和其他認證竊取攻擊並非特定 Windows 作業系統，即使它們新。 在 1997年建立第一次 hash 攻擊。 在過去，不過，這些攻擊自訂的工具，已在他們的成功、 hit-or-miss 而必須具有較高的技能攻擊。 免費且輕鬆使用的工具的原生擷取認證導入會導致指數增加認證竊取攻擊成功率與數量最近幾年。 不過，認證竊取攻擊是不是用認證是針對和危害的僅限機制。  
  
雖然您應該先執行控制項，以協助您防範認證竊取攻擊，您也應該找出您的環境中最常攻擊者做目標帳號，並實作穩定驗證控制那些帳號。 如果您的最有特殊權限的帳號使用單一因數驗證，例如使用者名稱和密碼 （都 「 您知道，」 是一個驗證因素），那些帳號弱受保護。 攻擊需要就是知道的使用者名稱和密碼帳號，並 pass hash 攻擊不 requiredthe 攻擊者可以使用者任何系統接受單一規格認證，以驗證。  
  
雖然實作要素保護不您攻擊 pass hash、 受保護的系統可以搭配實作要素。 執行系統受保護的相關詳細資訊中提供[實作安全的系統管理主機](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)，和驗證選項討論下列區段中。  
  
#### <a name="general-authentication-controls"></a>一般驗證控制項  
如果您未實作要素例如智慧卡，請考慮將這樣做。 智慧卡在公開私密金鑰按鍵組，導致無法存取或使用除非使用者提出的正確的 PIN、 密碼或智慧卡人體使用者的私密金鑰實作私密金鑰硬體執行保護。 即使使用者的 pin 碼或密碼攔截按鍵記錄器危害的電腦上，攻擊重複使用 pin 碼或密碼，也必須實際卡。  
  
萬一長且複雜密碼已經證明難以因為使用者抗拒實作，智慧卡提供的使用者可能會執行簡單的 pin 碼或密碼不會受到影響暴力或彩虹表格攻擊認證機制。 智慧卡的 Pin 是不會儲存在 Active Directory 中，或在本機坡資料庫中，雖然認證 hashes 可能仍然在電腦使用智慧卡有已驗證的 LSASS 保護記憶體中儲存。  
  
#### <a name="additional-controls-for-vip-accounts"></a>適用於 VIP 帳號其他控制項  
其他優點實作智慧卡或其他憑證式驗證機制是利用驗證機制保證保護的機密資料的存取 VIP 使用者的能力。 驗證機制保證位於的網域中的功能層級設定為 Windows Server 2012 或 Windows Server 2008 R2。 停用它，是時驗證機制保證加入系統管理員指定的全域群組成員資格使用者的 Kerberos 權杖時使用的憑證登入方法登入期間驗證使用者的認證。  
  
這可讓資源控制資源，例如的檔案、 資料夾及根據是否使用者登入時使用的憑證登入方法，除了使用憑證的類型印表機的系統管理員。 例如，使用者登入時使用智慧卡，使用者的存取權的網路上的資源可以指定為不同的存取權時，使用者不會使用智慧卡 （也就是，當使用者登入時輸入使用者名稱和密碼）。 如需驗證機制保證的詳細資訊，請查看[適用於在 Windows Server 2008 R2 的指示 AD DS 驗證機制保證](https://technet.microsoft.com/library/dd378897.aspx)。  
  
#### <a name="configuring-privileged-account-authentication"></a>設定特殊權限的 Account 驗證  
中的所有系統帳號 Active Directory，讓**要求智慧卡互動式登入**屬性，並變更 （最小） 稽核屬性的任何**Account**索引標籤上的 （例如，data-cn、 名稱、 sAMAccountName、 userPrincipalName 和 userAccountControl） account 管理使用者物件。  
  
雖然設定**要求智慧卡互動式登入**上帳號重設密碼 120 字元隨機值，並需要互動式登入，屬性智慧卡仍被覆的權限，它們變更密碼帳號，讓使用者和帳號然後可以用來建立非互動式只有使用者名稱和密碼的登入。  
  
有時候的設定而定帳號 Active Directory 和憑證] 設定中 Active Directory 憑證 Services (AD CS) 或第三方 PKI、 使用者主體名稱 (UPN) 屬性管理或 VIP 帳號可以會針對特定類型的攻擊，如下所述。  
  
##### <a name="upn-hijacking-for-certificate-spoofing"></a>適用於憑證詐騙 UPN 劫持  
雖然這份文件的範圍完整的攻擊公用基礎結構 (Pki) 討論，已指數自 2008年增加公開和私人 Pki 攻擊。 已針對公開的公用 Pki 漏洞，但可能更大規模組織內部 PKI 攻擊。 這類一個攻擊運用 Active Directory 和憑證允許的攻擊，以詐騙方式很難偵測其他帳號的認證。  
  
當憑證將會提供個加入網域的系統驗證時，主題或主旨另一種方式名稱 （舊） 中的屬性憑證用來地圖使用者在 Active Directory 物件的憑證。 根據的憑證，以及如何建構類型，主題中的屬性憑證通常會包含使用者的一般的名稱 (DATA-CN)，下列螢幕擷取畫面中所示。  
  
![最小的權限管理員模型](media/Implementing-Least-Privilege-Administrative-Models/SAD_4.gif)  
  
預設 Active Directory 建構使用者的 DATA-CN 串連 account 的第一個名稱 + 「 」 + 姓氏。 不過，不需要 DATA-CN 元件使用者 Active Directory 物件的或保證唯一，且帳號移動到不同 directory 位置中變更 account 的分辨的名稱，也就是完整設定中，directory 物件先前的螢幕擷取畫面的底部窗格中所示。  
  
憑證主體名稱並不保證靜態或唯一，因為到另一種方式主體名稱常用 Active Directory 中找出使用者物件。 （Active Directory 整合 Ca） 的企業憑證授權單位使用者發行憑證的舊屬性通常會包含使用者的 UPN 或電子郵件地址。 因為 Upn 保證為唯一 AD DS 森林中，尋找使用者物件 UPN，通常執行驗證，或不憑證參與驗證程序的一部分。  
  
攻擊者以取得詐騙憑證運用 Upn 用於中驗證憑證的舊屬性。 如果攻擊已入侵可以讀取和寫入 Upn 使用者物件帳號，攻擊實作方式如下：  
  
在使用者 （例如 VIP 使用者） 物件 UPN 屬性暫時變更為不同的值。 薩姆 account 名稱屬性和 DATA-CN 也可以變更在此階段，雖然這通常不需要如之前所述的原因。  
  
當已變更 UPN 屬性目標帳號時，過時，讓使用者 account 或剛建立的使用者 account 的 UPN 屬性變更為原始已指派給目標 account 值。 過時，讓使用者帳號是帳號不登入長一段時間，但不會被關閉。 他們的目標攻擊者會想要 」 隱藏一般看見 」 原因如下：  
  
1.  因為 account 支援，但尚未最近使用，使用 account 不太可能觸發通知的方式讓使用者停用的 account 可能。  
  
2.  使用現有的帳號，不需要建立新的使用者帳號，可能會注意到的系統管理員的員工。  
  
3.  過時帳號，仍都支援通常各種不同的安全性群組成員，並會授與資源簡化的存取，「 混合 「 現有的使用者擴展到網路上的存取權。  
  
目標 UPN 現在已帳號用於要求 Active Directory 憑證服務的一或多個憑證。  
  
當憑證取得攻擊者帳號時，upn，請在 [新的 「 account 和目標 account 會傳回至其原始值。  
  
攻擊者現在有一或多個憑證如果為使用者其 account 暫時已修改 VIP 使用者可以顯示驗證資源和應用程式。 雖然完整的所有方式可以透過攻擊目標憑證和 PKI 討論本文件的範圍，此攻擊機制提供闡述為何，您應該在監視權限和 VIP 帳號 AD DS，尤其是對屬性的任何變更，在**Account**索引標籤上的 account （例如data-cn，名稱、 sAMAccountName、 userPrincipalName，以及 userAccountControl)。 除了監視帳號，您應該只誰可以修改以較小的帳號一組盡可能管理使用者。 同樣地，帳號的系統管理員使用者應該保護與監視未經授權的變更。  
  


