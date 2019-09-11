---
ms.assetid: d562ef46-f240-48be-bbd4-fd88fc6bbbdc
title: 針對不支援 WIA 的裝置設定內部網路表單架構驗證
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 010315796c6f6328dde661ce79947898f95a80c4
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865823"
---
# <a name="configuring-intranet-forms-based-authentication-for-devices-that-do-not-support-wia"></a>針對不支援 WIA 的裝置設定內部網路表單架構驗證


根據預設，windows Server 2012 R2 中的 Active Directory 同盟服務（AD FS）中已啟用 Windows 整合式驗證（WIA），以取得針對任何使用的應用程式在組織內部網路（內部網路）內發生的驗證要求瀏覽器以進行驗證。 例如，這些可以是以瀏覽器為基礎的應用程式，使用 WS-同盟或 SAML 通訊協定，以及使用 OAuth 通訊協定的豐富應用程式。 WIA 可讓使用者順暢地登入應用程式，而不需要手動輸入其認證。 不過，有些裝置和瀏覽器無法支援 WIA，因此來自這些裝置的驗證要求將會失敗。 此外，與 NTLM 協商的特定瀏覽器的體驗並不理想。 建議的方法是針對這類裝置和瀏覽器，切換到以表單為基礎的驗證。

Windows Server 2016 和 Windows Server 2012 R2 中的 AD FS 可讓系統管理員設定使用者代理程式清單，以支援以表單為基礎的驗證。 您可以透過兩個設定來進行回復：


- `Set-ADFSProperties` Commandlet 的**WIASupportedUserAgentStrings**屬性
- `Set-AdfsGlobalAuthenticationPolicy` Commandlet 的**WindowsIntegratedFallbackEnabled**屬性

**WIASupportedUserAgentStrings**會定義支援 WIA 的使用者代理程式。 AD FS 在瀏覽器或瀏覽器控制項中執行登入時，會分析使用者代理字串。 如果使用者代理字串的元件不符合**WIASupportedUserAgentStrings**屬性中所設定之使用者代理程式字串的任何元件，AD FS 將會切換回提供以表單為基礎的驗證，但前提**是WindowsIntegratedFallbackEnabled**旗標設定為 True。

根據預設，新的 AD FS 安裝會建立一組使用者代理程式字串相符專案。 不過，根據瀏覽器和裝置的變更，這些可能會過期。 特別的是，Windows 裝置的使用者代理字串類似于權杖中的次要變化。 下列 Windows PowerShell 範例針對目前在支援無縫 WIA 的市場上，提供目前裝置集合的最佳指引：

    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")

上述命令可確保 AD FS 只涵蓋下列適用于 WIA 的使用案例：

使用者代理程式|使用案例|
-----|-----|
MSIE 6。0|IE 6。0|
MSIE 7.0;Windows NT|IE 7，位於內部網路區域。 「Windows NT」片段是由桌上型電腦作業系統傳送。|
MSIE 8。0|IE 8.0 （沒有任何裝置傳送此資訊，因此需要進行更明確的動作）|
MSIE 9。0|IE 9.0 （沒有任何裝置傳送此資訊，因此不需要讓它更明確）|
MSIE 10.0;Windows NT 6|適用于 Windows XP 和較新版本之桌面作業系統的 IE 10。0</br></br>已排除 Windows Phone 8.0 裝置（喜好設定為 mobile），因為它們會傳送</br></br>使用者代理程式：Mozilla/5.0 （相容;MSIE 10.0;Windows Phone 8.0;Trident/6.0;IEMobile/10.0;ARM按鍵式NETWORKSLumia 920）|
Windows NT 6.3;Trident/7。0</br></br>Windows NT 6.3;Win64x64Trident/7。0</br></br>Windows NT 6.3;WOW64Trident/7。0| Windows 8.1 桌面作業系統、不同的平臺|
Windows NT 6.2;Trident/7。0</br></br>Windows NT 6.2;Win64x64Trident/7。0</br></br>Windows NT 6.2;WOW64Trident/7。0|Windows 8 桌面作業系統，不同的平臺|
Windows NT 6.1;Trident/7。0</br></br>Windows NT 6.1;Win64x64Trident/7。0</br></br>Windows NT 6.1;WOW64Trident/7。0|Windows 7 桌面作業系統，不同的平臺|
[MSIPC]| Microsoft Information Protection and Control 用戶端|
Windows Rights Management 用戶端|Windows Rights Management 用戶端|

若要針對使用者代理程式啟用 [以表單為基礎的驗證]，而不是 WIASupportedUserAgents 字串中提及的驗證，請將 WindowsIntegratedFallbackEnabled 旗標設定為 true

    Set-AdfsGlobalAuthenticationPolicy -WindowsIntegratedFallbackEnabled $true

也請確定已為內部網路啟用表單驗證。

## <a name="configuring-wia-for-chrome"></a>設定 Chrome 的 WIA
您可以將 Chrome 或其他使用者代理程式新增至支援 WIA 的 AD FS 設定。 這可讓您順暢地登入應用程式，而不需要在存取受 AD FS 保護的資源時，手動輸入認證。 請遵循下列步驟，在 Chrome 上啟用 WIA：

在 AD FS 設定中，在 Windows 架構的平臺上新增 Chrome 的使用者代理字串：

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + "Mozilla/5.0 (Windows NT")

同樣地，針對 Apple macOS 上的 Chrome，請將下列使用者代理程式字串新增至 AD FS 設定：

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + "Mozilla/5.0 (Macintosh; Intel Mac OS X")

確認 AD FS 屬性中現在已設定 Chrome 的使用者代理字串：

    Get-AdfsProperties | Select -ExpandProperty WIASupportedUserAgents

（您在此需要新的螢幕擷取畫面）![設定驗證](media/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA/chrome1.png) 

>[!NOTE]   
> 隨著新的瀏覽器和裝置發行，建議您協調這些使用者代理程式的功能，並據此更新 AD FS 設定，以在使用瀏覽器和裝置時優化使用者的驗證體驗。 更具體來說，建議您在將新裝置或瀏覽器類型新增至您的支援矩陣以使用 WIA 時，重新評估 AD FS 中的**WIASupportedUserAgents**設定。


