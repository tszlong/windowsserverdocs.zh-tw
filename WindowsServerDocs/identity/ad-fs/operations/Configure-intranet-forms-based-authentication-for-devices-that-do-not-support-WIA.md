---
ms.assetid: d562ef46-f240-48be-bbd4-fd88fc6bbbdc
title: 設定內部網路表單型驗證不支援 WIA 的裝置
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cddc5d890114dec7e0053b16701db6f03c3cbbdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889849"
---
# <a name="configuring-intranet-forms-based-authentication-for-devices-that-do-not-support-wia"></a>設定內部網路表單型驗證不支援 WIA 的裝置

>適用於：Windows Server 2016, Windows Server 2012 R2

根據預設，Active Directory Federation Services (AD FS) 在 Windows Server 2012 R2 中使用任何應用程式內組織的內部網路 （內部網路） 發生的驗證要求中啟用 Windows 整合式驗證 (WIA)其驗證的瀏覽器。 比方說，這些可以是瀏覽器為基礎的應用程式，使用 WS-同盟或 SAML 通訊協定和豐富的應用程式使用 OAuth 通訊協定。 WIA 提供順暢的登入的應用程式的使用者，而不必手動輸入其認證。 不過，某些裝置和瀏覽器不支援 WIA 的而且從這些裝置的驗證要求失敗的結果。 此外，交涉到 NTLM 某些瀏覽器上的體驗並不理想。 建議的方法是針對這類裝置和瀏覽器的表單型驗證的遞補。

在 Windows Server 2016 和 Windows Server 2012 R2 的 AD FS 會提供系統管理員能夠設定支援回到使用表單型驗證的使用者代理程式的清單。 後援可由兩個組態：


- **WIASupportedUserAgentStrings**屬性`Set-ADFSProperties`commandlet
- **WindowsIntegratedFallbackEnabled**屬性`Set-AdfsGlobalAuthenticationPolicy`commandlet

**WIASupportedUserAgentStrings**定義支援 WIA 的使用者代理程式。 執行登入瀏覽器或瀏覽器控制項中時，AD FS 會分析使用者代理字串。 如果使用者代理字串元件不符合使用者代理程式字串中所設定的元件之一**WIASupportedUserAgentStrings**屬性中，AD FS 會改為使用提供的表單型驗證，前提**WindowsIntegratedFallbackEnabled**旗標設定為 True。

根據預設，新的 AD FS 安裝會有一組的使用者代理程式字串建立的相符項目。 不過，這些可能是過期根據瀏覽器和裝置的變更。 特別是，Windows 裝置會在權杖中，有微小差異與相似的使用者代理程式字串。 下列 Windows PowerShell 範例提供最新的集合支援的裝置目前市場上無縫 WIA 的最佳指引：

    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")

上述命令可確保 AD FS 僅針對 WIA 涵蓋下列使用案例：

使用者代理程式|使用案例|
-----|-----|
MSIE 6.0|IE 6.0|
MSIE 7.0; Windows NT|IE 7、 IE 內部網路區域中。 桌面作業系統時，會傳送到 「 Windows NT"片段。|
MSIE 8.0|IE 8.0 （沒有任何裝置傳送，因此需要進行更特定）|
MSIE 9.0|IE 9.0 （沒有任何裝置傳送，因此您不需要進行這更特定）|
MSIE 10.0; Windows NT 6|適用於 Windows XP 和較新版本的桌面作業系統的 IE 10.0</br></br>Windows Phone 8.0 裝置 （設定為行動裝置喜好設定） 會排除，因為他們傳送</br></br>使用者代理程式：Mozilla/5.0 （相容;MSIE 10.0;Windows Phone 8.0;Trident/6.0;之 IEMobile/10.0;ARM;觸控;NOKIA;Lumia 920)|
Windows NT 6.3;Trident/7.0</br></br>Windows NT 6.3;Win64;x64;Trident/7.0</br></br>Windows NT 6.3;WOW64;Trident/7.0| Windows 8.1 桌面作業系統、 不同平台|
Windows NT 6.2;Trident/7.0</br></br>Windows NT 6.2;Win64;x64;Trident/7.0</br></br>Windows NT 6.2;WOW64;Trident/7.0|Windows 8 桌面作業系統、 不同平台|
Windows NT 6.1; Trident/7.0</br></br>Windows NT 6.1;Win64;x64;Trident/7.0</br></br>Windows NT 6.1;WOW64;Trident/7.0|Windows 7 桌面作業系統、 不同平台|
[MSIPC]| Microsoft 資訊保護與控制用戶端|
Windows Rights Management 用戶端|Windows Rights Management 用戶端|

若要讓以外 WIASupportedUserAgents 字串中所提及的使用者代理程式的表單型驗證的遞補，WindowsIntegratedFallbackEnabled 旗標設定為 true

    Set-AdfsGlobalAuthenticationPolicy -WindowsIntegratedFallbackEnabled $true

也請確定內部網路，會啟用表單型驗證。

## <a name="configuring-wia-for-chrome"></a>為 Chrome 設定 WIA
您可以將 Chrome 或其他使用者代理程式新增至支援 WIA 的 AD FS 設定。 如此應用程式的無縫式登入而不需要手動輸入認證，當您存取 AD FS 保護的資源。 請遵循下列步驟來啟用 WIA Chrome 上：

在 AD FS 組態中，新增 chrome 的使用者代理字串

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + “Chrome”)
    
確認使用者代理字串 chrome 現在已設定在 AD FS 屬性

    Get-AdfsProperties | Select -ExpandProperty WIASupportedUserAgents

![設定驗證](media/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA/chrome1.png) 

>[!NOTE]   
> 發行新的瀏覽器和裝置，建議您協調這些使用者代理程式的功能，並據以更新 AD FS 設定，使用稱為瀏覽器和裝置時，最佳化使用者的驗證體驗。 更具體來說，建議您重新評估**WIASupportedUserAgents** WIA 新裝置或瀏覽器類型加入至您的支援矩陣時，AD FS 中設定。


