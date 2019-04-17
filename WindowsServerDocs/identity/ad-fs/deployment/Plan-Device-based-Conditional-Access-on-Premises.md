---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: "裝置為基礎的條件存取先計劃"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6303bba92ce4f4f99ae8e5095060b06885e427d6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="plan-device-based-conditional-access-on-premises"></a>裝置為基礎的條件存取先計劃

>適用於：Windows Server 2016

本文件描述根據在混合案例中，先目錄連接到使用 Azure AD 連接 Azure AD 的裝置條件存取原則。     

## <a name="ad-fs-and-hybrid-conditional-access"></a>AD FS 和混合條件存取  

AD FS 提供條件存取原則在混合案例中的上場所元件。  當您使用雲端資源條件存取 Azure AD 登記裝置時，Azure AD 連接裝置重新寫入項功能可以讓裝置登記資訊可在場所 AD FS 使用並執行的原則。  如此一來，您可以存取控制原則場所在兩個和雲端資源一致的方式。  

![條件存取](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

### <a name="types-of-registered-devices"></a>且已裝置類型  
有三種類型的且已全部以在 Azure AD 的裝置物件，可用於條件存取上，以及場所 AD FS 使用的裝置。  

| |新增工作或學校 Account  |Azure AD Join  |Windows 10 Domian 加入    
| --- | --- |--- | --- |
|描述    |  使用者加入他們的作品或學校 account 他們 BYOD 裝置互動。  **注意：**新增公司或學校 Account 是替換的工作地點加入 Windows 8/8.1       | 使用者加入 Azure ad 的 Windows 10 裝置的工作。|使用 Azure AD 自動登記加入網域的 Windows 10 裝置。|           
|如何使用者登入的裝置     |  Windows 即公司或學校 account 不登入。  使用 Microsoft account 登入。       |   Windows 即登記裝置 （公司或學校） account 登入。      |     請使用 AD account 登入。|      
|如何管理的裝置    |      MDM 原則 （以其他 Intune 註冊）   | MDM 原則 （以其他 Intune 註冊）        |   群組原則、 System Center Configuration Manager (SCCM) |
|Azure AD 信任類型|加入的地點|加入 azure AD|加入網域  |     
|W10 設定的位置    | 設定 > 帳號 > 您 > [新增公司或學校 account        | 設定 > 系統 > 有關 > 加入 Azure AD       |   設定 > 系統 > 有關 > 加入網域 |       
|也適用於 iOS 和 Android 裝置嗎？   |    [是]     |       否]  |   否]   |   

  

適用於登記裝置的不同方式的詳細資訊，請查看也：  
* [在您的工作地點使用 Windows 10 裝置](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-windows10-devices/)  
* [設定 Windows 10 裝置的工作](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/)  
[Windows 10 行動裝置版加入 Azure Active Directory](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)  

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Windows 10 使用者和裝置登入是從先前版本不同  
適用於 Windows 10 和 「 AD FS 2016 有一些新的裝置登記和驗證您必須知道 （尤其是您熟悉非常裝置登記和 」 的工作地點加入 「 先前發行的版本中）。  

首先，在 Windows 10 和 Windows Server 2016 中的 AD FS，裝置登記和驗證不會再完全根據 X509 使用者憑證。  還有新的和更穩定的通訊協定，可提供更佳的安全性，更順暢的使用者體驗。  主要不同的適用於 Windows 10 網域加入 Azure AD Join，還有 X509 呼叫 PRT 電腦憑證和新的憑證。  您可以朗讀所有關於[在此](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/)和[以下](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/)。  

第二，Windows 10 和 「 AD FS 2016 支援使用者驗證使用 Microsoft Passport 工作，您可以了解[在此](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/)和[以下](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)。  

AD FS 2016 提供順暢的裝置 」 和使用者 SSO 基礎護照和 PRT 認證。  您可以使用本文件中的步驟，讓這些功能並查看其運作。  

### <a name="device-access-control-policies"></a>裝置存取控制原則  
裝置可用於簡單 AD FS 存取控制規則例如：  

- 可讓存取只從且已裝置   
- 當裝置不登記需要使用多監視器因素驗證  

本規則再加其他因素而有所不同，例如網路存取您的位置和多因素驗證，建立豐富的條件存取原則，例如：  


- 從以外的公司網路時，除了的特定群組成員存取解除裝置需要多因素驗證  

使用 AD FS 2016，可以這些原則設定專為需要特定裝置信任層級，以及： 任一個**驗證**，**管理**，或**相容**。  

如需有關設定 AD FS 存取控制原則、 查看[存取控制原則 AD FS 在](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md)。  

#### <a name="authenticated-devices"></a>已驗證的裝置  
已驗證的裝置會不會參與 MDM （Intune 和 3 廠商 MDMs 適用於 Windows 10 中，Intune 僅適用於 iOS 和 Android） 的且已的裝置。   

已驗證的裝置將會有**isManaged** AD FS 取得值的**\ [false\]**。 （而不會完全登記裝置缺乏此宣告。）已驗證的裝置 （和所有且已的裝置） 將會有 isKnown AD FS 取得值的**TRUE**。  

#### <a name="managed-devices"></a>受管理的裝置：   

受管理的裝置是且已退出 MDM.使用的裝置  

受管理的裝置將會有 isManaged AD FS 取得值的**TRUE**。  

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>（使用 MDM 或群組原則） 相容的裝置  
相容的裝置的且已的裝置會不只退出使用 MDM 但相容的 MDM 原則。 （compliance 資訊來自 MDM 和寫入 Azure AD）。  

相容的裝置將會有**isCompliant** AD FS 取得值的**，則為 TRUE**。    

AD FS 2016 的裝置」和「宣告條件存取的完整清單，請查看[參考](#reference)。  


## <a name="reference"></a>參考資料  
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>新 AD FS 2016 與裝置宣告的完整清單  

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
