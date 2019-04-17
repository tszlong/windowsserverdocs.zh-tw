---
ms.assetid: d562ef46-f240-48be-bbd4-fd88fc6bbbdc
title: "設定裝置不支援 WIA 內部表單式驗證"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c78dc702cbff8eab487c6c077dfe57b0f0663342
ms.sourcegitcommit: 5012c078b410f59261edf0cc917393467243a5c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/23/2018
---
# <a name="configuring-intranet-forms-based-authentication-for-devices-that-do-not-support-wia"></a>設定裝置不支援 WIA 內部表單式驗證

>適用於：Windows Server 2016、Windows Server 2012 R2

根據預設，Windows 整合驗證 (WIA) 可以在 Active Directory 同盟 Services (AD FS) 在 Windows Server 2012 R2 進行驗證要求組織連絡 (intranet) 的任何應用程式使用它驗證的瀏覽器中發生的。 例如，這些可能瀏覽器為基礎的應用程式使用 WS 同盟或 SAML 通訊協定和豐富的應用程式使用 OAuth 通訊協定。 WIA 而不需要手動輸入認證他們提供的應用程式順暢登入的使用者。 不過，某些裝置和瀏覽器不支援 WIA，因此從這些裝置驗證要求失敗。 此外，不想在某些瀏覽器交涉 ntlm 的體驗。 建議的方法是後援表單架構的驗證，這類裝置，瀏覽器。

Windows Server 2016 和 Windows Server 2012 R2 AD FS 讓系統管理員可以設定的使用者，以驗證支援代理程式清單的能力。 後援是因為兩種設定：


- **WIASupportedUserAgentStrings**屬性的`Set-ADFSProperties`commandlet
- **WindowsIntegratedFallbackEnabled**屬性的`Set-AdfsGlobalAuthenticationPolicy`commmandlet

**WIASupportedUserAgentStrings**定義支援 WIA 使用者代理程式。 AD FS 分析使用者代理字串時登入執行瀏覽器或瀏覽器的控制項。 如果使用者代理字串的元件不符合元件使用者代理程式字串中所設定的任何**WIASupportedUserAgentStrings**屬性，AD FS 會改為使用提供表單架構的驗證，提供的**WindowsIntegratedFallbackEnabled**設定為 True 旗標。

根據預設，新 AD FS 安裝有建立的使用者專員字串相符項目的設定。 不過，這些可能是最新的根據變更瀏覽器和裝置。 尤其是，Windows 裝置有類似的使用者代理程式字串次要變化權杖中。 下列 Windows PowerShell 範例提供最佳的指導方針目前支援的裝置是目前市面上 WIA 順暢的設定：

    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")

上述命令，將可確保 AD FS 只適用於 WIA 涵蓋使用如下：

使用者代理程式|使用案例|
-----|-----|
MSIE 6.0|IE 6.0|
MSIE 7.0;Windows NT|IE 7、IE 在該處。 桌面作業系統傳送的「Windows NT」片段。|
MSIE 8.0|IE 8.0 不裝置傳送此 (，需要更多特定）|
MSIE 9.0|IE 9.0（不裝置傳送，所以不需要將此詳細特定）|
MSIE 10.0;Windows NT 6|適用於 Windows XP 和桌面作業系統的較新版 IE 10.0</br></br>因為它們傳送排除 （的喜好設定為行動裝置版） 的 Windows Phone 8.0 裝置</br></br>使用者代理： Mozilla 日 5.0 （相容。MSIE 10.0;Windows Phone 8.0;戟日 6.0;IEMobile 日 10.0;ARM;觸控功能。NOKIA;Lumia 920)|
Windows NT 6.3;7.0 戟日</br></br>Windows NT 6.3;Win64;x64;7.0 戟日</br></br>Windows NT 6.3;WOW64;7.0 戟日| Windows 8.1 桌面作業系統，不同平台|
Windows NT 6.2;7.0 戟日</br></br>Windows NT 6.2;Win64;x64;7.0 戟日</br></br>Windows NT 6.2;WOW64;7.0 戟日|Windows 8 桌面作業系統，不同平台|
Windows NT 6.1;7.0 戟日</br></br>Windows NT 6.1;Win64;x64;7.0 戟日</br></br>Windows NT 6.1;WOW64;7.0 戟日|Windows 7 桌面作業系統，不同平台|
MSIPC| 保護 Microsoft 的資訊與控制項 Client|
Windows 的權限管理 Client|Windows 的權限管理 Client|

為了讓後援表單驗證使用者以外的 WIASupportedUserAgents 字串中所提到的代理程式、 設定為 true WindowsIntegratedFallbackEnabled 旗標

    Set-AdfsGlobalAuthenticationPolicy -WindowsIntegratedFallbackEnabled $true

也請確定該功能內部網路的為基礎的驗證。

## <a name="configuring-wia-for-chrome"></a>設定 Chrome WIA
您可以新增支援 WIA AD FS 設定 Chrome 或其他使用者代理程式。 如此完美的登入應用程式而不需要手動輸入認證，當您存取受 AD FS 資源。 請依照下列步驟來讓 WIA Chrome 上：

AD FS 設定中新增 Chrome 使用者代理程式字串

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + “Chrome”)
    
確認 Chrome 的使用者專員字串現在已在 AD FS 屬性設定

    Get-AdfsProperties | Select -ExpandProperty WIASupportedUserAgents

![設定驗證](media/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA/chrome1.png) 

>[!NOTE]   
> 為推出全新的瀏覽器和裝置，建議您協調那些使用者代理程式的功能和時使用的瀏覽器與裝置稱為最佳化使用者的驗證體驗隨之更新 AD FS 設定。 尤其，建議您重新評估**WIASupportedUserAgents**當您的支援矩陣 WIA 新增新的裝置或瀏覽器類型 AD FS 中設定。


