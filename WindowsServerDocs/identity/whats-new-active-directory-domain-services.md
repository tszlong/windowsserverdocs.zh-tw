---
ms.assetid: 6a852428-c1ec-4703-b3b3-a4bfdf8cbb9d
title: "功能與 #39; s Active Directory Domain Services 在 Windows Server 2016 中的新功能"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1fdd012af6f025d58340f63f98da0d8dc11df0e0
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="what39s-new-in-active-directory-domain-services-for-windows-server-2016"></a>功能與 #39; s Active Directory Domain Services 適用於 Windows Server 2016 中的新功能

>適用於：Windows Server 2016

Active Directory Domain Services (AD DS) 下列新功能改善安全 Active Directory 環境並協助他們移轉至雲端僅部署，部署混合，其中某些應用程式和服務會在雲端中及其他位於場所組織的功能。 改進包括：  
  
-   [存取特殊權限的管理](https://technet.microsoft.com/library/mt150258.aspx   
)  
  
- [將它擴展至 Azure Active Directory 加入群組，透過 Windows 10 裝置的雲端功能](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-overview/)   
  
- [Azure AD 網域裝置連接適用於 Windows 10 體驗](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-devices-group-policy/)   
  
- [Microsoft Passport 工作讓您在組織中](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)    
  
-  [取代了檔案複寫服務 (FRS) 與 Windows Server 2003 功能層級](ad-ds/active-directory-functional-levels.md)  
  
  
## <a name="BKMK_PAM"></a>存取特殊權限的管理  
權限存取管理 (PAM) 可協助降低安全性考量，對於所造成的 Active Directory 環境認證竊取技術，這類 pass hash、 矛網路釣魚或類似類型的攻擊。 提供新的系統管理員存取方案使用 Microsoft 的身分管理員 (MIM) 設定。 PAM 引進了：  
  
-   新堡壘 Active Directory 森林，MIM 來提供。 堡壘樹系有特殊 PAM 信任的現有的樹系。 提供新的 Active Directory 環境已知的任何惡意的活動，以及特殊權限帳號使用現有的樹系的隔離免費的。  
  
-   要求系統管理員權限，以及根據 \ [核准要求的新工作流程 MIM 中的新處理程序。  
  
-   新陰影安全性原則 （群組） 來 MIM 堡壘森林中提供，以回應系統管理員權限要求。 陰影安全性主體有屬性，參考系統群組現有的樹系的 SID。 這樣陰影在現有的樹系存取資源群組而無須更動任何存取控制清單 (Acl)。  
  
-   逾期連結功能，可讓時間繫結陰影群組成員資格。 使用者可以會新增到群組不足，就無法執行管理工作所需的時間。 時間繫結成員資格會傳送至 Kerberos 票證期間 live 時間 (TTL) 值來表示。  
  
    > [!NOTE]  
    > 適用於所有連結屬性的過期的連結。 但的成員隸屬日連結的屬性群組使用者的關係是只範例位置完整的方案，例如 PAM 已經預先設定為使用過期功能的連結。  
  
-   \ [KDC 調節中之後建置 Active Directory 網域控制站限制 Kerberos 票證期間最低 live 時間 (TTL) 值於使用者有多個時間繫結成員資格管理群組中的位置。 例如，如果您新增到群組時間繫結 A，當您登入，Kerberos 票證授與票證 (TGT) 期間等於與的時間，然後您有剩餘 a 群組中如果您是也群組成員的另一個時間繫結 B 的群組 A 比較低 TTL，然後 TGT 期間等於群組 B.中剩餘的時間  
  
-   新的監視功能，可協助您輕鬆地找出人員要求存取、 哪些存取被授與，以及執行何種活動。  
  
**需求**  
  
-   Microsoft 的身分管理員  
  
-   Active Directory 樹系功能層級的 Windows Server 2012 R2 或更高版本。  
  
## <a name="BKMK_AzureADJoin"></a>Azure AD Join  
Azure Active Directory 加入美化的身分體驗企業版、 企業和 EDU 針對-改良功能的企業和個人裝置。  
  
優點：  
  
-   **現代化設定的可用性**在 corp 擁有的 Windows 裝置上。 氧氣服務不會再要求個人的 Microsoft account： 立即關閉要確保使用者現有工作帳號執行。 氧氣服務將繼續運作加入先 Windows 網域的電腦和電腦和裝置 「 加入 「 您 Azure AD 承租人 （」 雲端網域 」）。 這些設定包括︰  
  
    -   漫遊或個人化、 協助工具設定和認證  
  
    -   備份與還原  
  
    -   Microsoft 網上商店的工作帳號與存取  
  
    -   動態磚和通知  
  
-   **存取組織資源**在行動裝置版的裝置 （手機版，phablets） 無法加入網域 Windows，是否有 corp 擁有的或 BYOD  
  
-   **單一登入**到 Office 365 其他組織應用程式、 網站及資源。  
  
-   **BYOD 裝置上**、 工作 account （從先網域或 Azure AD） 加入個人所擁有的裝置，並享受 SSO 資源，透過 [app 與網頁上運作的方式，可協助確保遵守新功能，例如條件 Account 控制和裝置健康證明。  
  
-   **MDM 整合**可讓您自動註冊您的 MDM （Intune 或協力廠商） 裝置  
  
-   **設定 「 kiosk 」 模式，並分享的裝置**適用於您組織中的多個使用者  
  
-   **開發人員體驗**可讓您建置的應用程式迎合企業和個人內容與共用 programing 堆疊。  
  
-   **影像]**選項可讓您選擇之間映像處理與可讓使用者直接在初次執行體驗期間設定 corp 擁有的裝置。  
  
適用於更多的資訊查看，[適用於企業的 Windows 10： 使用裝置的工作方式](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-windows10-devices-overview/?rnd=1)。  
  
## <a name="BKMK_IDLocker"></a>Microsoft Passport  
Microsoft Passport 是新的驗證方法組織和對消費者而言，超過密碼。 這種驗證依賴違約、 遭竊和 phish 竄改認證。  
  
使用者登入的裝置使用生物特徵辨識或釘選登入資訊的非對稱金鑰憑證或連結。 身分提供者 (IDPs) 驗證使用者 IDLocker 到對應的使用者，並提供上一次密碼 (OTP)、 Phonefactor 或不同通知機制透過資訊的登入。  
  
適用於更多的資訊查看，[驗證身分而不需要密碼，透過 Microsoft Passport](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport/)  
  
## <a name="BKMK_FRSDeprecation"></a>取代了檔案複寫服務 (FRS) 與 Windows Server 2003 功能層級  
在舊版的 Windows Server 而被取代檔案複寫服務 (FRS) 和 Windows Server 2003 功能層級，但它天堂重複已不再支援 Windows Server 2003 作業系統。 如此一來，應該會執行 Windows Server 2003 的任何網域控制站從網域。 網域和樹系提高功能等級應該要至少為防止新增的環境中執行較舊版本的 Windows Server 的網域控制站的 Windows Server 2008。  
  
Windows Server 2008 及較高的網域功能層級，散發檔案服務 (DFS) 複寫用來網域控制站之間複製 SYSVOL 資料夾內容。 如果您建立新的網域網域層級 Windows Server 2008 功能或更高版本，DFS 複寫自動用於複寫 SYSVOL。 如果您建立網域層級會正常運作，您必須從使用 SYSVOL DFS 複寫 FRS 移轉。 移轉的步驟，您可以依照任一個[參考 TechNet 上的程序](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx)或您可以參考[簡化步驟儲存小組檔案櫃部落格上的](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx)。  
  
Windows Server 2003 網域及森林功能等級繼續支援，但組織應該提高以 Windows Server 2008（或更高版本，如果可能的話）功能等級確保 SYSVOL 複寫相容性，並在未來的支援。 除此之外，有許多其他優點和功能提供較高的功能等級更高版本。 查看下列的詳細資訊的資源：  
  
-   [了解 Active Directory Domain 服務 (AD DS) 功能的層級](ad-ds/active-directory-functional-levels.md)  
  
-   [提升網域功能](https://technet.microsoft.com/library/cc753104.aspx)  
  
-   [提升森林功能](https://technet.microsoft.com/library/cc730985.aspx)  
  
