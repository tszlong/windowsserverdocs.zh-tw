---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: 規劃裝置型條件式存取內部部署
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ffd7131f7f3772ab47b62c9755008fe3b1c4b274
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192074"
---
# <a name="plan-device-based-conditional-access-on-premises"></a>規劃裝置型條件式存取內部部署


本文件說明在混合式案例中，在內部部署目錄已連線到使用 Azure AD Connect 的 Azure AD 的裝置為基礎的條件式存取原則。     

## <a name="ad-fs-and-hybrid-conditional-access"></a>AD FS 和混合式的條件式存取  

AD FS 提供在內部部署元件的混合式案例中的條件式存取原則。  當您註冊的裝置與 Azure AD 條件式存取雲端資源時，Azure AD Connect 的裝置寫回功能會建立裝置註冊資訊可在內部部署 AD FS 使用，並強制執行的原則。  如此一來，您會有一致的方法來存取控制原則，同時在內部部署和雲端資源。  

![條件式存取](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

### <a name="types-of-registered-devices"></a>類型的已註冊的裝置  
有三種類型的已註冊的裝置，全部都以 Azure AD 中的裝置物件，並可用於使用 AD FS 在內部部署以及條件式存取。  

| |新增工作或學校帳戶  |加入 Azure AD  |Windows 10 Domian 聯結    
| --- | --- |--- | --- |
|描述    |  使用者新增他們的工作或學校帳戶以其 BYOD 裝置以互動方式。  **注意：** 新增工作或學校帳戶會取代 Windows 8/8.1 工作地點       | 使用者將他們的 Windows 10 工作裝置加入 Azure AD。|Windows 10 已加入網域的裝置自動向 Azure AD。|           
|使用者如何登入裝置     |  沒有登入做為工作或學校帳戶的 Windows 中。  使用 Microsoft 帳戶登入。       |   登入 Windows，做為註冊的裝置 （公司或學校） 帳戶。      |     使用 AD 帳戶登入。|      
|如何管理裝置    |      （與其他 Intune 註冊） 的 MDM 原則   | （與其他 Intune 註冊） 的 MDM 原則        |   群組原則，System Center Configuration Manager (SCCM) |
|Azure AD 信任類型|已加入工作場所|已加入 azure AD|加入網域  |     
|W10 設定位置    | 設定 > 帳戶 > 您的帳戶 > 新增工作或學校帳戶        | 設定 > System > 關於 > 加入 Azure AD       |   設定 > System > 關於 > 加入網域 |       
|也適用於 iOS 和 Android 裝置？   |    是     |       否  |   否   |   

  

如需有關註冊裝置的不同方式的詳細資訊，請參閱：  
* [在您的工作場所中使用 Windows 10 裝置](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-windows10-devices/)  
* [設定 Windows 10 裝置進行工作](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/)  
[加入 Azure Active Directory 中的 Windows 10 行動裝置](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)  

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Windows 10 使用者和裝置登入有何不同於舊版  
適用於 Windows 10 和 AD FS 2016 有一些新的層面的裝置註冊和驗證您應該了解 （尤其是如果您已非常熟悉裝置註冊和 「 工作場所聯結 」 在舊版本）。  

首先，在 Windows 10 和 Windows Server 2016 中的 AD FS 中，裝置註冊和驗證不再僅基於 X509 使用者憑證。  沒有新且更穩固的通訊協定，提供更佳的安全性和更順暢的使用者體驗。  主要差異是，對於 Windows 10 網域加入和 Azure AD Join，會有 X509 呼叫 PRT 電腦憑證和新的認證。  您可以閱讀所有它[此處](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/)並[這裡](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/)。  

其次，Windows 10 和 AD FS 2016 支援的使用者驗證使用 Microsoft Passport for Work，您可以閱讀[此處](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/)並[這裡](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)。  

AD FS 2016 提供無縫式裝置和使用者 PRT 和 Passport 認證為基礎的 SSO。  使用本文件中的步驟，您就可以啟用這些功能，並查看其運作。  

### <a name="device-access-control-policies"></a>裝置存取控制原則  
裝置可用於簡單的 AD FS 存取控制規則中這類：  

- 允許只能從已註冊的裝置存取   
- 不註冊裝置時需要多重要素驗證  

這些規則接著可以結合其他因素，例如網路存取的位置和多重要素驗證，建立豐富的條件式存取原則，例如：  


- 未註冊的裝置從特定群組或群組的成員除外的公司網路外部存取需要多重要素驗證  

與 AD FS 2016，這些原則可以設定為需要的特定裝置信任層級也： 任一**驗證**，**受控**，或**相容**。  

如需有關設定 AD FS 存取控制原則，請參閱 < [AD FS 中的存取控制原則](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)。  

#### <a name="authenticated-devices"></a>已驗證的裝置  
已驗證的裝置是已註冊的裝置未註冊 MDM （Intune 和第 3 方 MDMs 適用於 Windows 10 中，僅適用於 iOS 的 Intune 和 Android） 中。   

已驗證的裝置會有**isManaged** AD FS 宣告值**FALSE**。 （而不會完全註冊的裝置會缺少此宣告。）已驗證的裝置 （和所有已註冊的裝置） 將會有 isKnown AD FS 宣告值 **，則為 TRUE**。  

#### <a name="managed-devices"></a>受管理的裝置：   

受管理的裝置是已註冊的裝置已向 MDM 註冊  

受管理的裝置會有 isManaged AD FS 宣告值 **，則為 TRUE**。  

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>裝置相容 （使用 MDM 或群組原則）  
符合規範的裝置是已註冊的裝置，且不只向 MDM 但符合 MDM 原則。 （相容性資訊源自 MDM 的而且會寫入至 Azure AD）。  

符合規範的裝置會有**isCompliant** AD FS 宣告值 **，則為 TRUE**。    

如需完整的 AD FS 2016 的裝置 」 和 「 條件式存取宣告的詳細清單，請參閱[參考](#reference)。  


## <a name="reference"></a>參考資料  
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>新的 AD FS 2016 和裝置宣告的完整清單  

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
