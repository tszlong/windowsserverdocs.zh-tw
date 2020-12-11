---
description: 深入瞭解：在內部部署規劃以裝置為基礎的條件式存取
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: 規劃裝置型條件式存取內部部署
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 8301cb1396785d56d81e08e2ad3d52ee4d66d887
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043306"
---
# <a name="plan-device-based-conditional-access-on-premises"></a>規劃裝置型條件式存取內部部署


本檔說明以混合式案例中的裝置為基礎的條件式存取原則，其中內部部署目錄會使用 Azure AD Connect 連接到 Azure AD。

## <a name="ad-fs-and-hybrid-conditional-access"></a>AD FS 和混合式條件式存取

AD FS 在混合式案例中提供條件式存取原則的內部部署元件。  當您向 Azure AD 登錄裝置以進行雲端資源的條件式存取時，Azure AD Connect 裝置回寫功能會使裝置註冊資訊可供內部部署使用，以供 AD FS 原則取用和強制執行。  如此一來，您就可以使用一致的方式來存取內部部署和雲端資源的控制原則。

![條件式存取](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)

### <a name="types-of-registered-devices"></a>已註冊裝置的類型
有三種已註冊的裝置，這些裝置全都以 Azure AD 中的裝置物件表示，也可用於內部部署 AD FS 的條件式存取。

| 描述 |新增公司或學校帳戶  |Azure AD Join  |Windows 10 加入網域 |
| --- | --- |--- | --- |
|描述    |  使用者以互動方式將其工作或學校帳戶新增至其 BYOD 裝置。  **注意：** 新增公司或學校帳戶是 Windows 8/8.1 的 Workplace Join 取代 | 使用者將其 Windows 10 工作裝置加入 Azure AD。|Windows 10 已加入網域的裝置會自動向 Azure AD 註冊。|
|使用者登入裝置的方式     |  不以工作或學校帳戶登入 Windows。  使用 Microsoft 帳戶登入。 |      | 以註冊裝置的 (公司或學校) 帳戶登入 Windows。 | 使用 AD 帳戶登入。|
|裝置的管理方式 | 使用其他 Intune 註冊 (MDM 原則)  | 使用其他 Intune 註冊 (MDM 原則)  | 群組原則，Configuration Manager |
|Azure AD 信任類型|加入工作場所|已聯結的 Azure AD|加入網域 |
|W10 設定位置 | 設定 > 帳戶 > 帳戶 > 新增公司或學校帳戶 | > 聯結 > 系統 > 的設定 Azure AD |   > 加入網域 > 系統 > 的設定 |
|也適用于 iOS 和 Android 裝置？ | 是 | 否 | 否 |

如需註冊裝置之不同方式的詳細資訊，請參閱：
* [在您的工作場所中使用 Windows 10 裝置](/azure/active-directory/devices/overview)
* [設定工作](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/) 
 的 Windows 10 裝置[將 Windows 10 行動裝置版加入 Azure Active Directory](/windows/client-management/join-windows-10-mobile-to-azure-active-directory)

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Windows 10 的使用者和裝置登入與舊版有何不同
針對 Windows 10 和 AD FS 2016，您應該瞭解一些新的裝置註冊和驗證方面的 (資訊，特別是如果您非常熟悉裝置註冊和先前版本) 中的「工作場所聯結」。

首先，在 Windows Server 2016 Windows 10 和 AD FS 中，裝置註冊和驗證不再以 X509 使用者憑證為基礎。  有一個新的、更健全的通訊協定，可提供更佳的安全性和更順暢的使用者體驗。  主要的差異在於，在 Windows 10 加入網域和 Azure AD 加入時，有 X509 電腦憑證和稱為 PRT 的新認證。  您可以在 [這裡](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/) 和 [這裡](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/)閱讀相關資訊。

其次，Windows 10 和 AD FS 2016 使用 Microsoft Passport for Work 來支援使用者驗證，您可以在[這裡](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/)[閱讀。](/windows/security/identity-protection/hello-for-business/hello-identity-verification)

AD FS 2016 以 PRT 和 Passport 認證為基礎，提供順暢的裝置和使用者 SSO。  您可以使用本檔中的步驟來啟用這些功能，並查看其運作方式。

### <a name="device-access-control-policies"></a>裝置存取控制原則
裝置可用於簡單的 AD FS 存取控制規則，例如：

- 只允許從已註冊的裝置存取
- 裝置未註冊時需要多重要素驗證

然後，這些規則可以與其他因素（例如網路存取位置和多重要素驗證）結合，以建立豐富的條件式存取原則，例如：


- 針對從公司網路外部存取的未註冊裝置要求進行多重要素驗證，但特定群組或群組的成員除外

使用 AD FS 2016，可以特別設定這些原則，以要求特定的裝置信任層級： **已驗證**、 **受管理** 或 **符合規範**。

如需設定 AD FS 存取控制原則的詳細資訊，請參閱 [AD FS 中的存取控制原則](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)。

#### <a name="authenticated-devices"></a>已驗證的裝置
經過驗證的裝置是未在 MDM 中註冊的裝置 (Intune 和協力廠商 MDMs，適用于 Windows 10、Intune 僅適用于 iOS 和 Android) 。

經過驗證的裝置會有 **isManaged** AD FS 宣告值 **為 FALSE**。  (，而未註冊的裝置則不會有此宣告。 ) 已驗證的裝置 (和所有已註冊的裝置) 都會有值 **為 TRUE** 的 isKnown AD FS 宣告。

#### <a name="managed-devices"></a>受管理的裝置：

受管理的裝置是已向 MDM 註冊的已註冊裝置。

受管理的裝置會有 isManaged AD FS 宣告值 **為 TRUE**。

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>與 MDM 或群組原則 (的裝置相容) 
符合規範的裝置是註冊的裝置，這些裝置不只會向 MDM 註冊，而是符合 MDM 原則。  (的合規性資訊是由 MDM 所產生，並會寫入 Azure AD。 ) 

符合規範的裝置會有 **>iscompliant** AD FS 宣告值 **為 TRUE**。

如需 AD FS 2016 裝置和條件式存取宣告的完整清單，請參閱 [參考](#reference)。


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
