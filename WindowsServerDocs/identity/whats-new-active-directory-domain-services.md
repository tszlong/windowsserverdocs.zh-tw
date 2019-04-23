---
ms.assetid: 6a852428-c1ec-4703-b3b3-a4bfdf8cbb9d
title: 什麼&#39;的新功能 Windows Server 2016 中的 Active Directory 網域服務
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ffdeedfc2d818fe223c033aeecfe29d18365db7a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865539"
---
# <a name="whats-new-in-active-directory-domain-services-for-windows-server-2016"></a>Active Directory 網域服務的 Windows Server 2016 中的新功能

>適用於：Windows Server 2016

Active Directory 網域服務 (AD DS) 中的下列新功能改善的功能，讓組織保護 Active Directory 環境，並協助他們移轉至僅限雲端部署和混合式部署，其中某些應用程式和服務是裝載在雲端和其他裝載在內部部署環境。 增強功能包括：  
  
- [特殊權限的存取管理](https://docs.microsoft.com/microsoft-identity-manager/pam/privileged-identity-management-for-active-directory-domain-services)  
  
- [Windows 10 裝置透過 Azure Active Directory Join 擴充的雲端功能](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/)
  
- [連線到 Azure AD 已加入網域的裝置，適用於 Windows 10 體驗](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)
  
- [在組織中啟用 Microsoft Passport for Work](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)
  
- [檔案複寫服務 (FRS) 和 Windows Server 2003 的功能等級已被取代](ad-ds/active-directory-functional-levels.md)  
  
## <a name="privileged-access-management"></a>特殊權限的存取管理

特殊權限存取管理 (PAM) 可協助減少安全性問題所造成的 Active Directory 環境的認證竊取技術，這類 pass pass-the-hash、 魚叉式網路釣魚，以及類似類型的攻擊。 它提供新的系統管理存取解決方案，使用 Microsoft Identity Manager (MIM) 所設定。 PAM 導入了：  
  
- 新的防禦 Active Directory 樹系，由 MIM。 防禦樹系有特殊的 PAM 信任與現有的樹系。 它提供新的 Active Directory 環境，即是免費的任何惡意活動，並從現有的樹系用於特殊權限帳戶的隔離。  
  
- 要求系統管理權限，以及新的工作流程以核准的要求在 MIM 中的新處理程序。  
  
- 新陰影的安全性主體 （群組） 中佈建在防禦樹系中的 MIM 系統管理員權限要求的回應。 陰影安全性主體有參考現有的樹系中系統管理群組的 SID 屬性。 這可讓存取現有的樹系中的資源陰影群組，而不需要變更任何存取控制清單 (Acl)。  
  
- 即將過期的連結功能，讓時間繫結中陰影群組的成員資格。 使用者可以加入群組，剛好足夠執行系統管理工作所需的時間。 時限成員資格是由存留時間 (TTL) 值會傳播到 Kerberos 票證存留期，以表示。  
  
    > [!NOTE]  
    > 即將過期的連結位於連結的所有屬性。 但成員/memberOf 連結的屬性群組和使用者之間的關聯性是唯一的範例，完整的解決方案，例如 PAM 預先設定為使用 「 即將過期的連結 」 功能。  
  
- Active Directory 網域控制站的最低可能存留時間 (TTL) 值在使用者擁有多個時間繫結的成員資格系統管理群組中的情況下限制 Kerberos 票證存留期內建 KDC 的增強功能。 比方說，如果您新增至時間界限群組 A，則當您登入時，Kerberos 票證授權票證 (TGT) 存留期等於時您有剩餘群組 a 中如果您還有另一個時間界限群組 b，而具有較低的 TTL，比群組 A，B 的成員然後 TGT 存留期相當於在群組 b 中所剩餘的時間  
  
- 新的監視功能，可幫助您輕鬆識別誰要求存取、 何種存取權授與，以及執行了哪些活動。  

### <a name="requirements-for-privileged-access-management"></a>需求的特殊權限存取管理
  
- Microsoft Identity Manager  
  
- Active Directory 樹系功能等級的 Windows Server 2012 R2 或更高版本。  
  
## <a name="azure-ad-join"></a>加入 Azure AD

Azure Active Directory Join 增強企業版、 商務及教育客戶-適用於公司和個人裝置的改良功能的身分識別的體驗。  
  
優點：  
  
- **最新設定的可用性**公司擁有的 Windows 裝置上。 氧氣服務不再需要個人的 Microsoft 帳戶： 現在關閉以確保符合使用者的現有工作帳戶執行。 氧氣服務將努力加入內部部署 Windows 網域的電腦和電腦和裝置 「 加入 」 至 Azure AD 租用戶 （「 雲端網域 」）。 這些設定包括︰  

   - 漫遊或個人化、 協助工具設定和認證  
   - 備份與還原  
   - 使用公司帳戶存取 Microsoft Store  
   - 動態磚和通知  
  
- **存取組織資源**BYOD 或無法加入 Windows 網域，無論是公司擁有的行動裝置 （手機、 平板手機） 上  
- **單一登入**Office 365 和其他組織的應用程式、 網站和資源。  
- **在 BYOD 裝置上**，（從內部部署網域或 Azure AD） 中加入個人擁有的裝置的工作帳戶，並享受要讓 SSO 運作方式，可協助確保符合新的功能，例如條件式帳戶的資源，透過應用程式和網頁，控制 」 和 「 裝置健康情況證明。  
- **MDM 整合**可讓您自動註冊到您的 MDM （Intune 或協力廠商） 的裝置  
- **設定"kiosk 」 模式和共用裝置**您組織中的多個使用者  
- **開發人員體驗**可讓您建置符合企業和個人的內容與共用的程式設計堆疊的應用程式。  
- **映像**選項可讓您直接在初次執行體驗期間設定公司擁有的裝置選擇適當的映像，並讓您的使用者。  
  
如需詳細資訊，請參閱[Azure Active Directory 中的裝置管理簡介](https://docs.microsoft.com/azure/active-directory/devices/overview)。  
  
## <a name="windows-hello-for-business"></a>Windows Hello 企業版

Windows hello 企業版是索引鍵為基礎的驗證方法的組織和超越密碼的客戶。 這種形式的驗證依賴漏洞、 竊取和 phish 防的認證。  
  
使用者的登入裝置使用生物識別技術或 PIN 登入連結到憑證或非對稱金鑰組的資訊。 身分識別提供者 (Idp) IDLocker 來對應使用者的公開金鑰來驗證使用者，並提供 透過一個單次密碼 (OTP)、 電話或不同的通知機制的資訊中的 記錄檔。  
  
如需詳細資訊，請參閱[Windows hello 企業版](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)  
  
## <a name="deprecation-of-file-replication-service-frs-and-windows-server-2003-functional-levels"></a>檔案複寫服務 (FRS) 和 Windows Server 2003 的功能等級已被取代

雖然在舊版的 Windows Server 的檔案複寫服務 (FRS) 和 Windows Server 2003 功能等級已被取代，它具有重複已不再支援 Windows Server 2003 作業系統。 如此一來，應該從網域移除任何執行 Windows Server 2003 的網域控制站。 網域和樹系功能等級必須提高至至少以防止網域控制站新增至環境中執行舊版 Windows Server 的 Windows Server 2008。

在 Windows Server 2008 和更新版本的網域功能等級，分散式檔案服務 」 (DFS) 複寫來複寫網域控制站之間的 SYSVOL 資料夾的內容。 如果在 Windows Server 2008 網域功能層級或更高版本，您會建立新的網域，則 DFS 複寫會自動使用來複寫 SYSVOL 中。 如果您在建立定義域較低功能層級，您必須從使用 FRS 到 DFS 複寫的 SYSVOL 移轉。 移轉步驟中，您可以其中一個後續[這些步驟](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640019\(v=ws.10\))或您可以參考[簡化一組儲存體小組的檔案櫃部落格上的步驟](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx)。  
  
Windows Server 2003 網域和樹系功能等級會繼續受到支援，但是組織應該提高功能等級為 Windows Server 2008 （或更高版本，如果可能） 來確保 SYSVOL 複寫相容性，並在未來支援。 此外，有許多其他的優點及功能更高版本的功能等級更高版本上。 請參閱下列資源以了解詳細資訊：  

- [了解 Active Directory 網域服務 (AD DS) 功能等級](ad-ds/active-directory-functional-levels.md)  
- [提高網域功能等級](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753104\(v=ws.11\))  
- [提高樹系功能等級](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730985\(v=ws.11\))  
