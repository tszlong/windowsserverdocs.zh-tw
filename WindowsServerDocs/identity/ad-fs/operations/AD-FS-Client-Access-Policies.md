---
title: AD FS 中的用戶端存取控制原則
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4d7d45b91e866b9df927620f2e214ced248b3361
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947484"
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>使用 Active Directory 同盟服務控制組織資料的存取

本檔概述跨內部部署、混合式和雲端案例的 AD FS 的存取控制。

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>內部部署資源的 AD FS 和條件式存取
自 Active Directory 同盟服務引進之後，授權原則就可以根據要求和資源的屬性，限制或允許使用者存取資源。  隨著 AD FS 從版本移至版本，這些原則的執行方式也已改變。  如需依版本存取控制功能的詳細資訊，請參閱：
- [Windows Server 2016 中 AD FS 的存取控制原則](Access-Control-Policies-in-AD-FS.md)
- [Windows Server 2012 R2 AD FS 中的存取控制](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>混合式組織中的 AD FS 和條件式存取

AD FS 在混合式案例中提供條件式存取原則的內部部署元件。 以 AD FS 為基礎的授權規則應用於非 Azure AD 資源，例如直接同盟至 AD FS 的內部部署應用程式。  雲端元件是由 Azure AD 的[條件式存取](/azure/active-directory/active-directory-conditional-access)所提供。  Azure AD Connect 提供連接這兩者的控制平面。

例如，當您使用 Azure AD 對雲端資源的條件式存取註冊裝置時，Azure AD Connect 裝置回寫功能會讓裝置註冊資訊可供內部部署使用，以供 AD FS 原則使用並強制執行。  如此一來，您就可以使用一致的方法來存取內部部署和雲端資源的控制原則。

![條件式存取](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>適用于 Office 365 的用戶端存取原則演進
許多人都使用用戶端存取原則搭配 AD FS，以根據因素（例如用戶端位置和所使用的用戶端應用程式類型）來限制存取 Office 365 和其他 Microsoft 線上服務。
- [Windows Server 2012 R2 中的用戶端存取原則 AD FS](Access-Control-Policies-W2K12.md)
- [AD FS 2.0 中的用戶端存取原則](Access-Control-Policies-in-AD-FS-2.md)

這些原則的一些範例包括：
- 封鎖所有外部網路用戶端存取 Office 365
- 封鎖所有外部網路用戶端存取 Office 365，但存取 Exchange Online 以進行 Exchange Active Sync 的裝置除外

這些原則背後的基礎需求通常是藉由確保只有獲得授權的用戶端、未快取資料的應用程式，或可從遠端停用的裝置存取資源，來降低資料流程失的風險。

上述 AD FS 的記載原則在記載的特定案例中運作，因為它們相依于不一致提供的用戶端資料，所以有其限制。  例如，用戶端應用程式的身分識別僅適用于 Exchange Online 服務，而不適用於 SharePoint Online 等資源，在此情況下，可能會透過瀏覽器或「胖用戶端」（例如 Word 或 Excel）存取相同的資料。  此外 AD FS 不知道存取 Office 365 中的資源，例如 SharePoint Online 或 Exchange Online。

為了解決這些限制，並提供更健全的方式來使用原則來管理 Office 365 中商務資料的存取權，或其他以 Azure AD 為基礎的資源，Microsoft 引進 Azure AD 條件式存取。  您可以針對特定資源或 Azure AD 中 Office 365、SaaS 或自訂應用程式內的任何或所有資源，設定條件式存取原則 Azure AD。  這些原則會在裝置信任、位置和其他因素上轉動。

若要深入瞭解 Azure AD 條件式存取，請參閱[Azure Active Directory 中的條件式存取](/azure/active-directory/active-directory-conditional-access)

啟用這些案例的一項重要變更是[新式驗證](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/)，這是一種新方法，用來驗證在 Office 用戶端、Skype、Outlook 和瀏覽器上運作方式相同的使用者和裝置。

## <a name="next-steps"></a>後續步驟
如需控制跨雲端和內部部署存取的詳細資訊，請參閱：

- [Azure Active Directory 中的條件式存取](/azure/active-directory/active-directory-conditional-access)
- [AD FS 2016 中的存取控制原則](Access-Control-Policies-in-AD-FS.md)
