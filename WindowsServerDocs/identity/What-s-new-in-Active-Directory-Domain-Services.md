---
ms.assetid: 9a06cd41-426f-4cb9-89cf-f5be730e0b79
title: Active Directory Domain Services&#39;的新功能
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: ''
ms.suite: na
ms.technology: active-directory-domain-services
ms.tgt_pltfrm: na
ms.topic: article
author: Femila
ms.author: billmath
ms.date: 05/31/2017
ms.openlocfilehash: 3fc45782af7aaaccf87569c59f936abd8cbc5d08
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868264"
---
# <a name="what39s-new-in-active-directory-domain-services"></a>Active Directory Domain Services&#39;的新功能 

>適用於：Windows Server 2016

Active Directory Domain Services （AD DS）中的下列新功能可改善組織保護 Active Directory 環境的能力，並協助他們遷移至僅限雲端的部署和混合式部署，其中有些應用程式和服務託管于雲端，其他則裝載于內部部署。 這些改良功能包括：  
  
-   [特殊許可權存取管理](https://technet.microsoft.com/library/mt150258.aspx   
)  
  
- [透過 Azure Active Directory Join 將雲端功能延伸到 Windows 10 裝置](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/)   
  
- [將已加入網域的裝置連接到 Azure AD 以進行 Windows 10 體驗](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)   
  
- [在您的組織中啟用 Microsoft Passport for Work](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)    
  
-  [檔案複寫服務（FRS）和 Windows Server 2003 功能等級的淘汰](ad-ds/active-directory-functional-levels.md)  
  
  
## <a name="BKMK_PAM"></a>特殊許可權存取管理  
特殊許可權存取管理（PAM）有助於減輕因認證竊取技術（例如傳遞雜湊、魚叉式網路釣魚和類似的攻擊類型）而造成的 Active Directory 環境的安全性疑慮。 它提供了使用 Microsoft Identity Manager （MIM）設定的新系統管理存取解決方案。 PAM 引進：  
  
-   由 MIM 布建的新防禦 Active Directory 樹系。 防禦樹系與現有樹系具有特別的 PAM 信任。 它提供一個新的 Active Directory 環境，已知這是任何惡意活動，而且可以從現有的樹系隔離，以使用特殊許可權帳戶。  
  
-   MIM 中的新程式會要求系統管理許可權，以及根據要求核准的新工作流程。  
  
-   由 MIM 在防禦樹系中布建的新陰影安全性主體（群組），以回應系統管理許可權要求。 陰影安全性主體的屬性會參考現有樹系中系統管理群組的 SID。 這可讓陰影群組存取現有樹系中的資源，而不需要變更任何存取控制清單（Acl）。  
  
-   過期連結功能，可啟用陰影群組中的時間限制成員資格。 只要有足夠的時間執行系統管理工作，就可以將使用者新增至群組。 時間系結的成員資格會以存留時間（TTL）值（傳播至 Kerberos 票證存留期）來表示。  
  
    > [!NOTE]  
    > 所有連結屬性都有過期的連結。 但是，群組和使用者之間的成員/memberOf 連結屬性關聯性是唯一的範例，例如 PAM 的完整解決方案已預先設定為使用 [過期連結] 功能。  
  
-   KDC 增強功能內建于 Active Directory 網域控制站，以在使用者于系統管理群組中有多個時間系結成員資格的情況下，將 Kerberos 票證存留期限制為最低可能的存留時間（TTL）值。 例如，如果您加入到時間界限群組 A，則當您登入時，Kerberos 票證授權票證（TGT）存留期會等於您在群組 A 中剩餘的時間。如果您也是另一個時間界限群組 B 的成員，其 TTL 低於群組 A，則 TGT 存留期會等於您在群組 B 中剩餘的時間。  
  
-   新的監視功能，可協助您輕鬆地找出要求存取的物件、授與的存取權，以及執行的活動。  
  
**滿足**  
  
-   Microsoft Identity Manager  
  
-   Active Directory Windows Server 2012 R2 或更高版本的樹系功能等級。  
  
## <a name="BKMK_AzureADJoin"></a>Azure AD 聯結  
Azure Active Directory Join 可增強企業和 EDU 客戶的身分識別體驗，並改善公司和個人裝置的功能。  
  
優點：  
  
-   公司擁有的 Windows 裝置上**的現代化設定可用性**。 氧氣 Services 不再需要個人 Microsoft 帳戶：他們現在會從使用者現有的工作帳戶執行，以確保合規性。 氧氣服務將適用于加入內部部署 Windows 網域的電腦，以及已加入 Azure AD 租使用者（「雲端網域」）的電腦和裝置。 這些設定包括︰  
  
    -   漫遊或個人化、協助工具設定和認證  
  
    -   備份與還原  
  
    -   使用工作帳戶存取 Microsoft Store  
  
    -   動態磚和通知  
  
-   存取無法加入 Windows 網域之行動裝置（手機、phablets）上的**組織資源**，無論他們是公司擁有或 BYOD  
  
-   **單一登入**Office 365 和其他組織應用程式、網站和資源。  
  
-   **在 BYOD 裝置上**，將工作帳戶（從內部部署網域或 Azure AD）新增至個人擁有的裝置，並透過應用程式和 web 來享用 SSO，以協助確保符合條件帳戶控制等新功能和裝置健康情況證明。  
  
-   **Mdm 整合**可讓您將裝置自動註冊到您的 MDM （Intune 或協力廠商）  
  
-   為組織中的多個使用者**設定「kiosk」模式和共用的裝置**  
  
-   **開發人員體驗**可讓您使用共用的程式設計堆疊，建立可滿足企業和個人內容的應用程式。  
  
-   **影像處理**選項可讓您選擇映射處理，並允許使用者在第一次執行體驗期間直接設定公司擁有的裝置。  
  
如需詳細資訊， [請參閱適用于企業的 Windows 10：使用裝置進行工作](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-windows10-devices-overview/?rnd=1)的方式。  
  
## <a name="BKMK_IDLocker"></a>Microsoft Passport  
Microsoft Passport 是以新的金鑰為基礎的驗證方法，也就是組織和取用者，不僅是密碼。 這種形式的驗證會依賴缺口、遭竊和網路釣魚認證。  
  
使用者以已連結至憑證或非對稱金鑰組之資訊的生物識別或 PIN 碼登入裝置。 身分識別提供者（Idp）會藉由將使用者的公開金鑰對應至 IDLocker 來驗證使用者，並透過單次密碼（OTP）、Phonefactor 或不同的通知機制來提供登入資訊。  
  
如需詳細資訊，請參閱[透過 Microsoft Passport 驗證不含密碼](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport/)的身分識別  
  
## <a name="BKMK_FRSDeprecation"></a>檔案複寫服務（FRS）和 Windows Server 2003 功能等級的淘汰  
雖然檔案複寫服務（FRS）和 Windows Server 2003 功能等級在舊版的 Windows Server 中已淘汰，但仍有重複的 Windows Server 2003 作業系統不再受到支援。 因此，執行 Windows Server 2003 的任何網域控制站都應該從網域中移除。 網域和樹系功能等級應至少提高至 Windows Server 2008，以防止執行舊版 Windows Server 的網域控制站新增至環境。  
  
在 Windows Server 2008 及更高版本的網域功能等級上，分散式檔案服務（DFS）複寫是用來複寫網域控制站之間的 SYSVOL 資料夾內容。 如果您在 Windows Server 2008 網域功能等級或更新版本建立新的網域，DFS 複寫會自動用來複寫 SYSVOL。 如果您已在較低的功能層級建立網域，則需要從使用 FRS 遷移至 SYSVOL 的 DFS 複寫。 針對遷移步驟，您可以遵循[TechNet 上的程式](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx)，或參考[存放裝置小組檔案封包 blog 上的一組簡化的步驟](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx)。  
  
Windows Server 2003 網域和樹系功能等級會繼續受到支援，但組織應將功能等級提升為 Windows Server 2008 （或更高版本），以確保未來的 SYSVOL 複寫相容性和支援。 此外，更高的功能層級也提供許多其他優點和功能。 請參閱下列資源以了解詳細資訊：  
  
-   [瞭解 Active Directory Domain Services （AD DS）功能等級](ad-ds/active-directory-functional-levels.md)  
  
-   [提高網域功能等級](https://technet.microsoft.com/library/cc753104.aspx)  
  
-   [提高樹系功能等級](https://technet.microsoft.com/library/cc730985.aspx)  
  
