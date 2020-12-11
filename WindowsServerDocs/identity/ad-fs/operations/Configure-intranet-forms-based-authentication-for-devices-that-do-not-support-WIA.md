---
description: 深入瞭解：為不支援 WIA 的裝置設定內部網路表單型驗證
ms.assetid: d562ef46-f240-48be-bbd4-fd88fc6bbbdc
title: 針對不支援 WIA 的裝置設定內部網路表單型驗證
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 1c3058663c01a7e90f4c67241a927923d659512a
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048796"
---
# <a name="configuring-intranet-forms-based-authentication-for-devices-that-do-not-support-wia"></a>針對不支援 WIA 的裝置設定內部網路表單型驗證

根據預設，windows Server 2012 R2 中的 Windows 整合式驗證 (WIA) 已在 Windows Server R2 中的 Active Directory 同盟服務 (AD FS) 中啟用，以供使用瀏覽器進行驗證之任何應用程式的內部網路 (內部網路) 。 例如，這些可以是以瀏覽器為基礎的應用程式，這些應用程式使用 WS-Federation 或 SAML 通訊協定，以及使用 OAuth 通訊協定的豐富應用程式。 WIA 可讓使用者無縫登入應用程式，而不需要手動輸入其認證。 不過，某些裝置和瀏覽器無法支援 WIA，因此來自這些裝置的驗證要求將會失敗。 此外，您也不需要在某些與 NTLM 協商的瀏覽器上體驗。 建議的方法是，針對這類裝置和瀏覽器，改回表單型驗證。

Windows Server 2016 和 Windows Server 2012 R2 中的 AD FS，讓系統管理員能夠設定支援切換至表單型驗證的使用者代理程式清單。 您可以透過兩個設定來執行回復：

- Commandlet 的 **WIASupportedUserAgentStrings** 屬性 `Set-ADFSProperties`
- Commandlet 的 **WindowsIntegratedFallbackEnabled** 屬性 `Set-AdfsGlobalAuthenticationPolicy`

**WIASupportedUserAgentStrings** 會定義支援 WIA 的使用者代理程式。 AD FS 在瀏覽器或瀏覽器控制項中執行登入時，會分析使用者代理字串。 如果使用者代理字串的元件不符合 **WIASupportedUserAgentStrings** 屬性中所設定之使用者代理程式字串的任何元件，AD FS 將會切換回提供表單型驗證，前提是 **WindowsIntegratedFallbackEnabled** 旗標設定為 True。

根據預設，新的 AD FS 安裝會建立一組使用者代理程式字串相符專案。 不過，這些可能會根據瀏覽器和裝置的變更而過期。 尤其是，Windows 裝置的使用者代理程式字串類似權杖中的細微變化。 下列 Windows PowerShell 範例會針對目前市場上支援無縫 WIA 的目前裝置集合，提供最佳的指導方針：

```powershell
Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")
```

上述命令可確保 AD FS 僅涵蓋下列 WIA 使用案例：

使用者代理程式|使用案例|
-----|-----|
MSIE 6。0|IE 6。0|
MSIE 7.0;Windows NT|IE 7，IE 位於內部網路區域。 "Windows NT" 片段是由桌上型電腦作業系統所傳送。|
MSIE 8。0|IE 8.0 (沒有任何裝置傳送此資訊，因此需要進行更明確的) |
MSIE 9。0|IE 9.0 (沒有任何裝置傳送此資訊，因此不需要讓此更明確的) |
MSIE 10.0;Windows NT 6|適用于 Windows XP 的 IE 10.0 及較新版本的桌面作業系統</br></br>將喜好設定設為行動) 的 Windows Phone 8.0 裝置 (會被排除，因為它們傳送</br></br>使用者代理程式： Mozilla/5.0 (相容;MSIE 10.0;Windows Phone 8.0;Trident/6.0;IEMobile/10.0;分支觸及NOKIALumia 920) |
Windows NT 6.3;Trident/7。0</br></br>Windows NT 6.3;Win6464Trident/7。0</br></br>Windows NT 6.3;WOW64Trident/7。0| Windows 8.1 桌面作業系統，不同的平臺|
Windows NT 6.2;Trident/7。0</br></br>Windows NT 6.2;Win6464Trident/7。0</br></br>Windows NT 6.2;WOW64Trident/7。0|Windows 8 桌面作業系統，不同的平臺|
Windows NT 6.1;Trident/7。0</br></br>Windows NT 6.1;Win6464Trident/7。0</br></br>Windows NT 6.1;WOW64Trident/7。0|Windows 7 桌面作業系統，不同的平臺|
[MSIPC]| Microsoft Information Protection and Control 用戶端|
Windows Rights Management 用戶端|Windows Rights Management 用戶端|

若要針對 WIASupportedUserAgents 字串中提及的使用者代理程式，讓其改為以表單為基礎的驗證，請將 WindowsIntegratedFallbackEnabled 旗標設定為 true。

```powershell
Set-AdfsGlobalAuthenticationPolicy -WindowsIntegratedFallbackEnabled $true
```

此外，請確定已針對內部網路啟用表單驗證。

## <a name="configuring-wia-for-chrome"></a>設定 Chrome 的 WIA
您可以將 Chrome 或其他使用者代理程式新增至支援 WIA 的 AD FS 設定。 這可讓您順暢地登入應用程式，而不需要在您存取受 AD FS 保護的資源時，手動輸入認證。 遵循下列步驟來啟用 Chrome 上的 WIA：

在 AD FS 設定中，在 Windows 平臺上新增 Chrome 的使用者代理程式字串：

```powershell
Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + "Mozilla/5.0 (Windows NT)")
```

同樣地，在 Apple macOS 上的 Chrome 中，將下列使用者代理程式字串新增至 AD FS 設定：

```powershell
Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + "Mozilla/5.0 (Macintosh; Intel Mac OS X)")
```

確認 AD FS 屬性中已設定 Chrome 的使用者代理程式字串：

```powershell
Get-AdfsProperties | Select -ExpandProperty WIASupportedUserAgents
```

 (您需要新的螢幕擷取畫面，) ![ 設定驗證](media/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA/chrome1.png)

>[!NOTE]
> 當新的瀏覽器和裝置發行時，建議您協調這些使用者代理程式的功能，並據此更新 AD FS 設定，以在使用瀏覽器和裝置時將使用者的驗證體驗優化。 更具體來說，建議您在將新的裝置或瀏覽器類型新增至 WIA 的支援矩陣時，重新評估 AD FS 中的 **WIASupportedUserAgents** 設定。
