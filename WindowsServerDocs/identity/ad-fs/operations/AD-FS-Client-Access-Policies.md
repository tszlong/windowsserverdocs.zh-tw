---
ms.assetid: 
title: "AD FS client 存取控制原則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5e1e1b907e6fccbf2b9906106d3360bd9a6fd69d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>控制與 Active Directory 同盟服務組織的資料的存取權

本文件概述的所有 AD FS 進行存取控制提供場所，混合和雲端案例。  

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>AD FS 和條件存取場所資源 
Active Directory 同盟服務推出之後, 已可限制或允許要求的屬性資源和資源使用者存取授權原則。  AD FS 已經版本，因為已變更實作這些原則的方式。  如版本存取控制功能的詳細資訊：
- [在 Windows Server 2016 中 AD FS 存取控制原則](Access-Control-Policies-in-AD-FS.md)
- [在 Windows Server 2012 R2 AD FS 中存取控制](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>AD FS 與在組織中混合條件存取  

AD FS 提供條件存取原則在混合案例中的上場所元件。 AD FS 根據授權規則適用於非 Azure AD 的資源，例如場所上的應用程式聯盟直接與 AD FS。  雲端元件提供[Azure AD 條件存取](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access)。  Azure AD 連接提供連接兩個控制項平面。

例如，當您使用雲端資源條件存取 Azure AD 登記裝置，Azure AD 連接裝置重新寫入項功能可以讓裝置登記資訊可在場所 AD FS 使用並執行的原則。  如此一來，您可以存取控制原則場所在兩個和雲端資源一致的方式。  

![條件存取](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>進化版的 Client 存取原則的 Office 365
許多您正在使用 AD FS 進行 client 存取原則限制 Office 365，以及其他因素例如 client 及 client 所使用的應用程式類型的位置，包括的 Microsoft Online 服務的存取。  
- [在 Windows Server 2012 R2 AD FS client 存取原則](Access-Control-Policies-W2K12.md)
- [AD FS 2.0 中 client 存取原則](Access-Control-Policies-in-AD-FS-2.md)

這些原則的一些範例包括：
- 封鎖所有的外部 client 存取 Office 365
- 封鎖所有外部 client 存取 Office 365，除了裝置存取換貨 Online Exchange 使用同步

這些原則背後的基礎需要通常是確保只會在授權的戶端，應用程式，不會快取的資料，來降低資料洩露的風險或裝置，您可以停用從遠端可以取得資源。

AD FS 上述記載的原則工作記載特定案例中，當他們使用的是限制因為它們無法使用一致的 client 資料而定。  例如 client 應用程式的身分只已經可 Exchange Online 型服務的資源，例如 SharePoint Online 位置相同的資料可能會存取透過瀏覽器或粗 client，例如 Word 或 Excel 不。  AD FS 也不知道的資源，例如 SharePoint Online 或 Online 換貨正在存取 Office 365 中。

地址這些限制及提供更穩定的方式使用原則，來管理商務用 Office 365 或其他根據 Azure AD 資源的資料的存取權，Microsoft 已導入 Azure AD 條件的存取。  Azure AD 條件存取原則可以 Azure AD 特定的資源，或是任何或所有資源 Office 365、SaaS 或自訂應用程式中的設定。  這些原則樞紐上信任的裝置，位置，以及其他因素而有所不同。

若要深入了解條件 Azure AD 的存取，請查看[在 Azure Active Directory 中條件存取](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access)

主要變更，讓這些案例中為[現代化驗證](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/)、驗證使用者與裝置的 Office 戶端、Skype、Outlook、和瀏覽器上運作的方式相同的新方式。

## <a name="next-steps"></a>後續步驟
如需詳細資訊，前提和控制跨雲端存取上看到：

- [在 Azure Active Directory 中條件存取](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access)
- [AD FS 2016 中存取控制原則](Access-Control-Policies-in-AD-FS.md)
