---
ms.assetid: ''
title: 在 AD FS 中的用戶端存取控制原則
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cc91cd9a446c8ca30471b65374ca99a7bd49d369
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882369"
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>使用 Active Directory 同盟服務的組織資料的存取控制

本文件提供與跨 AD FS 存取控制的概觀，在內部部署、 混合式部署和雲端案例。  

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>AD FS 和條件式存取內部部署資源 
自 Active Directory Federation Services 引進，授權原則已可用來限制或允許使用者存取要求的屬性為基礎的資源和資源。  因為 AD FS 已移動版本，這些原則的實作方式已變更。  如需版本的存取控制功能的詳細資訊，請參閱：
- [在 Windows Server 2016 中的 AD FS 存取控制原則](Access-Control-Policies-in-AD-FS.md)
- [Windows Server 2012 R2 中的 AD FS 中的存取控制](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>AD FS 與在混合式組織中的條件式存取  

AD FS 提供在內部部署元件的混合式案例中的條件式存取原則。 AD FS 為基礎的授權規則適用於非 Azure AD 資源，例如在內部部署環境直接與 AD FS 同盟的應用程式。  所提供之雲端元件[Azure AD 條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)。  Azure AD Connect 會提供連接兩個控制平面。

例如，當您註冊的裝置與 Azure AD 條件式存取雲端資源時，Azure AD Connect 的裝置寫回功能會建立裝置註冊資訊可在內部部署 AD FS 使用，並強制執行的原則。  如此一來，您會有一致的方法來存取控制原則，同時在內部部署和雲端資源。  

![條件式存取](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>適用於 Office 365 用戶端存取原則的發展
許多人會使用與 AD FS 用戶端存取原則來限制存取 Office 365 和其他因素，例如用戶端和用戶端應用程式所使用的類型的位置為基礎的 Microsoft Online services。  
- [在 Windows Server 2012 R2 AD FS 中的用戶端存取原則](Access-Control-Policies-W2K12.md)
- [AD FS 2.0 中的用戶端存取原則](Access-Control-Policies-in-AD-FS-2.md)

這些原則的一些範例包括：
- 封鎖所有對 Office 365 的外部網路用戶端存取
- 封鎖所有外部網路用戶端存取 Office 365 的 Exchange Active Sync 存取 Exchange Online 的裝置除外

通常這些原則背後的基礎需求就是確保只有已獲授權的用戶端，不會快取資料的應用程式來降低資料外洩的風險，或可以從遠端停用的裝置可以取得資源的存取權。

雖然上述所記錄的原則適用於 AD FS 運作中所述的特定案例，因為它們取決於用戶端不是一致可用的資料有限制。  例如，用戶端應用程式的身分識別，只是適用於 Exchange Online 基礎服務，也不適用於資源，例如 SharePoint Online，透過瀏覽器或豐富型用戶端，例如 Word 或 Excel 可能會存取相同的資料。  也 AD FS 會察覺有 Office 365 存取，例如 SharePoint Online 或 Exchange Online 中的資源。

若要解決這些限制，並提供更穩固的方式使用原則來管理 Office 365 或其他 Azure AD 為基礎的資源中的商務資料的存取權，Microsoft 已推出 Azure AD 條件式存取。  Azure AD 條件式存取原則可以針對特定的資源，或任何或所有的資源，在 Office 365、 SaaS 或自訂應用程式設定 Azure AD 中。  這些原則會依據裝置信任、 位置和其他因素。

若要深入了解 Azure AD 條件式存取，請參閱[中 Azure Active Directory 條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)

索引鍵變更為啟用這些案例[新式驗證](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/)、 驗證使用者和裝置的相同方式適用於 Office 用戶端、 Skype、 Outlook 及瀏覽器的新方式。

## <a name="next-steps"></a>後續步驟
如需詳細資訊，控制存取跨雲端和內部部署上，請參閱：

- [Azure Active Directory 中的條件式存取](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)
- [在 AD FS 2016 中的存取控制原則](Access-Control-Policies-in-AD-FS.md)
