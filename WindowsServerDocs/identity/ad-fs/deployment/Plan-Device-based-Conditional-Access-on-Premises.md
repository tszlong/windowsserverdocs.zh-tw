---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: 規劃裝置型條件式存取內部部署
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 7f331aac7b58cc22f696130647a7f5e95ea808c9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87945474"
---
# <a name="plan-device-based-conditional-access-on-premises"></a>規劃裝置型條件式存取內部部署


本檔說明以混合式案例中的裝置為基礎的條件式存取原則，其中內部部署目錄會使用 Azure AD Connect 連接到 Azure AD。

## <a name="ad-fs-and-hybrid-conditional-access"></a>AD FS 和混合式條件式存取

AD FS 在混合式案例中提供條件式存取原則的內部部署元件。  當您使用 Azure AD 對雲端資源的條件式存取註冊裝置時，Azure AD Connect 裝置回寫功能會讓裝置註冊資訊可供內部部署使用，以用於 AD FS 原則來取用和強制執行。  如此一來，您就可以使用一致的方法來存取內部部署和雲端資源的控制原則。

![條件式存取](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)

### <a name="types-of-registered-devices"></a>已註冊裝置的類型
已註冊的裝置有三種類型，全都在 Azure AD 中以裝置物件表示，而且也可以用於內部部署 AD FS 的條件式存取。

| 描述 |新增公司或學校帳戶  |Azure AD Join  |Windows 10 網域加入 |
| --- | --- |--- | --- |
|描述    |  使用者會以互動方式將其公司或學校帳戶新增至其 BYOD 裝置。  **注意：** 新增公司或學校帳戶是 Windows 8/8.1 中的 Workplace Join 取代 | 使用者將其 Windows 10 工作裝置加入 Azure AD。|已加入網域的 Windows 10 裝置會自動向 Azure AD 註冊。|
|使用者如何登入裝置     |  不會以工作或學校帳戶登入 Windows。  使用 Microsoft 帳戶登入。 |      | 以 (工作或學校) 帳戶的身分登入 Windows，以註冊裝置。 | 使用 AD 帳戶登入。|
|如何管理裝置 | 具有其他 Intune 註冊 (的 MDM 原則)  | 具有其他 Intune 註冊 (的 MDM 原則)  | 群組原則，Configuration Manager |
|Azure AD 信任類型|已加入工作場所|已聯結的 Azure AD|加入網域 |
|W10 設定位置 | 設定 > 帳戶 > 帳戶 > 新增工作或學校帳戶 | 有關 > Join 的 > 系統 > 設定 Azure AD |   有關 > 加入網域的系統 > 設定 > |
|也適用于 iOS 和 Android 裝置？ | 是 | 否 | 否 |

如需不同方式來註冊裝置的詳細資訊，請參閱：
* [在您的工作場所中使用 Windows 10 裝置](/azure/active-directory/devices/overview)
* [設定 Windows 10 裝置的工作](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/) 
[將 Windows 10](/windows/client-management/join-windows-10-mobile-to-azure-active-directory)行動裝置版加入 Azure Active Directory

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Windows 10 使用者和裝置登入與先前版本的不同之處
對於 Windows 10 和 AD FS 2016，您應該 (瞭解裝置註冊和驗證的一些新層面，特別是當您非常熟悉裝置註冊和舊版) 中的「加入工作地點」時。

首先，在 windows 10 和 Windows Server 2016 中的 AD FS 中，裝置註冊和驗證不再以 X509 使用者憑證為基礎。  有一個新的、更穩固的通訊協定，可提供更佳的安全性和更順暢的使用者體驗。  主要的差異在於，針對 Windows 10 網域加入和 Azure AD 聯結，有 X509 電腦憑證和名為 PRT 的新認證。  您可以在[這裡](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/)和[這裡](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/)閱讀相關資訊。

第二，Windows 10 和 AD FS 2016 支援使用 Microsoft Passport for Work 的使用者驗證，您可以在[這裡](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/)和[這裡](/windows/security/identity-protection/hello-for-business/hello-identity-verification)閱讀相關資訊。

AD FS 2016 根據 PRT 和 Passport 認證提供無縫裝置和使用者的 SSO。  使用本檔中的步驟，您可以啟用這些功能並查看其工作。

### <a name="device-access-control-policies"></a>裝置存取控制原則
裝置可以用於簡單 AD FS 存取控制規則，例如：

- 僅允許從已註冊的裝置進行存取
- 裝置未註冊時需要多重要素驗證

然後，這些規則可以與其他因素（例如網路存取位置和多重要素驗證）結合，以建立豐富的條件式存取原則，例如：


- 針對從公司網路外部存取的未註冊裝置，除了特定群組或群組的成員以外，需要多重要素驗證

有了 AD FS 2016，這些原則可以特別設定為需要特定的裝置信任層級，也就是**已驗證**、**受管理**或**相容**。

如需設定 AD FS 存取控制原則的詳細資訊，請參閱[AD FS 中的存取控制原則](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)。

#### <a name="authenticated-devices"></a>已驗證的裝置
已驗證的裝置是未在 MDM 中註冊的裝置 (Intune 和適用于 Windows 10 的協力廠商 MDMs、僅適用于 iOS 和 Android) 的 Intune。

已驗證的裝置會有值**為 FALSE**的**isManaged** AD FS 宣告。  (，但未註冊的裝置將缺少此宣告。 ) 已驗證的裝置 (而且所有已註冊的裝置) 都會有值**為 TRUE**的 isKnown AD FS 宣告。

#### <a name="managed-devices"></a>受管理的裝置：

受管理的裝置是向 MDM 註冊的已註冊裝置。

受管理的裝置將具有值**為 TRUE**的 isManaged AD FS 宣告。

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>與 MDM 或群組原則 (的裝置相容) 
相容的裝置是註冊的裝置，不只會向 MDM 註冊，而是與 MDM 原則相容。  (合規性資訊源自于 MDM，並會寫入 Azure AD。 ) 

符合規範的裝置會有**isCompliant** AD FS 宣告值**為 TRUE**。

如需 AD FS 2016 裝置和條件式存取宣告的完整清單，請參閱[參考](#reference)。


## <a name="reference"></a>參考
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>新 AD FS 2016 和裝置宣告的完整清單

* https://schemas.microsoft.com/ws/2014/01/identity/claims/anchorclaimtype
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/implicitupn
* https://schemas.microsoft.com/2014/03/psso
* https://schemas.microsoft.com/2015/09/prt
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
* https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname
* https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid
* https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid
* https://schemas.microsoft.com/2012/01/devicecontext/claims/displayname
* https://schemas.microsoft.com/2012/01/devicecontext/claims/identifier
* https://schemas.microsoft.com/2012/01/devicecontext/claims/ostype
* https://schemas.microsoft.com/2012/01/devicecontext/claims/osversion
* https://schemas.microsoft.com/2012/01/devicecontext/claims/ismanaged
* https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser
* https://schemas.microsoft.com/2014/02/devicecontext/claims/isknown
* https://schemas.microsoft.com/2014/02/deviceusagetime
* https://schemas.microsoft.com/2014/09/devicecontext/claims/iscompliant
* https://schemas.microsoft.com/2014/09/devicecontext/claims/trusttype
* https://schemas.microsoft.com/claims/authnmethodsreferences
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path
* https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork
* https://schemas.microsoft.com/2012/01/requestcontext/claims/client-request-id
* https://schemas.microsoft.com/2012/01/requestcontext/claims/relyingpartytrustid
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-ip
* https://schemas.microsoft.com/2014/09/requestcontext/claims/userip
* https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod
