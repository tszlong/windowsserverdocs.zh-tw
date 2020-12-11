---
description: 深入瞭解：使用 Active Directory 同盟服務控制組織資料的存取權
title: AD FS 中的用戶端存取控制原則
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 669069b905dcd73a184abf51905894d6f5236afb
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046636"
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>使用 Active Directory 同盟服務控制組織資料的存取權

本檔概述跨內部部署、混合式和雲端案例的 AD FS 的存取控制。

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>內部部署資源的 AD FS 和條件式存取
從 Active Directory 同盟服務引進之後，授權原則已可根據要求和資源的屬性，限制或允許使用者存取資源。  當 AD FS 已從版本移至版本時，這些原則的實施方式已有所變更。  如需依版本的存取控制功能的詳細資訊，請參閱：
- [Windows Server 2016 AD FS 中的存取控制原則](Access-Control-Policies-in-AD-FS.md)
- [Windows Server 2012 R2 中 AD FS 的存取控制](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>混合式組織中的 AD FS 和條件式存取

AD FS 在混合式案例中提供條件式存取原則的內部部署元件。 以 AD FS 為基礎的授權規則應用於非 Azure AD 資源，例如直接與 AD FS 同盟的內部部署應用程式。  雲端元件是由 [Azure AD 條件式存取](/azure/active-directory/active-directory-conditional-access)提供。  Azure AD Connect 提供連接這兩者的控制平面。

例如，當您向 Azure AD 登錄裝置以進行雲端資源的條件式存取時，Azure AD Connect 裝置回寫功能會使裝置註冊資訊可供內部部署使用，以供 AD FS 原則取用和強制執行。  如此一來，您就可以使用一致的方式來存取內部部署和雲端資源的控制原則。

![條件式存取](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>Office 365 用戶端存取原則的演進
許多人都使用用戶端存取原則搭配 AD FS，以根據因素（例如用戶端的位置和所使用的用戶端應用程式的類型）來限制對 Office 365 和其他 Microsoft 線上服務的存取。
- [Windows Server 2012 R2 AD FS 中的用戶端存取原則](Access-Control-Policies-W2K12.md)
- [AD FS 2.0 中的用戶端存取原則](Access-Control-Policies-in-AD-FS-2.md)

這些原則的一些範例包括：
- 封鎖所有外部網路用戶端存取 Office 365
- 封鎖所有外部網路用戶端對 Office 365 的存取，但存取 Exchange Online 以進行 Exchange Active Sync 的裝置除外

這些原則的基礎需求通常是藉由確保只有授權的用戶端、不快取資料的應用程式，或可從遠端停用的裝置存取資源，來降低資料洩漏的風險。

雖然上述 AD FS 記載的原則可在記載的特定案例中運作，但是它們有一些限制，因為它們依存于未一致提供的用戶端資料。  例如，用戶端應用程式的身分識別僅適用于 Exchange Online 服務，而不是 SharePoint Online 之類的資源，也就是可以透過瀏覽器或「豐富型用戶端」（例如 Word 或 Excel）來存取相同的資料。  此外 AD FS 也不知道存取 Office 365 中的資源，例如 SharePoint Online 或 Exchange Online。

為了解決這些限制，並提供更健全的方式來使用原則來管理 Office 365 或其他 Azure AD 資源中商務資料的存取權，Microsoft 引進了 Azure AD 條件式存取。  您可以針對特定資源或 Azure AD 中的 Office 365、SaaS 或自訂應用程式內的任何或所有資源設定 Azure AD 條件式存取原則。  這些原則會以裝置信任、位置和其他因素為軸。

若要瞭解 Azure AD 條件式存取的詳細資訊，請參閱 [Azure Active Directory 中的條件式存取](/azure/active-directory/active-directory-conditional-access)

啟用這些案例的一項重要變更是 [新式驗證](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/)，這是一種驗證使用者和裝置的新方式，可在 Office 用戶端、Skype、Outlook 和瀏覽器中以相同的方式運作。

## <a name="next-steps"></a>後續步驟
如需控制跨雲端和內部部署存取的詳細資訊，請參閱：

- [Azure Active Directory 中的條件式存取](/azure/active-directory/active-directory-conditional-access)
- [AD FS 2016 中的存取控制原則](Access-Control-Policies-in-AD-FS.md)
