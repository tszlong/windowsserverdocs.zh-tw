---
title: 將瀏覽器設定為使用 Windows 整合式驗證 (WIA) 搭配 AD FS
description: 本檔說明如何設定瀏覽器以搭配使用 WIA 與 AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/20/2020
ms.topic: article
ms.openlocfilehash: c7779522e874fb484f3801780495f96ddcead990
ms.sourcegitcommit: 4165d4a9198228d4ec809ccd7d791f8de2aeb159
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97091272"
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>將瀏覽器設定為使用 Windows 整合式驗證 (WIA) 搭配 AD FS

根據預設，windows Server 2012 R2 中的 Windows 整合式驗證 (WIA) 已在 Windows Server R2 中的 Active Directory 同盟服務 (AD FS) 中啟用，以供使用瀏覽器進行驗證之任何應用程式的內部網路 (內部網路) 。

AD FS 2016 現在具有改良的預設設定，可讓 Edge 瀏覽器執行 WIA，同時也不會 (錯誤地) 捕捉 Windows Phone：

```
=~Windows\s*NT.*Edg.*
```

上述表示您不再需要設定個別的使用者代理程式字串來支援常見的邊緣案例，即使它們經常更新也是一樣。

若為其他瀏覽器，請設定 AD FS 屬性 **WiaSupportedUserAgents** ，以根據您所使用的瀏覽器新增必要的值。  您可以使用下列程式。

### <a name="view-wiasupporteduseragent-settings"></a>查看 WIASupportedUserAgent 設定

**WIASupportedUserAgents** 會定義支援 WIA 的使用者代理程式。 AD FS 在瀏覽器或瀏覽器控制項中執行登入時，會分析使用者代理字串。

您可以使用下列 PowerShell 範例來查看目前的設定：

```powershell
Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![WIA 支援](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>變更 WIASupportedUserAgent 設定
根據預設，新的 AD FS 安裝會建立一組使用者代理程式字串相符專案。 不過，這些可能會根據瀏覽器和裝置的變更而過期。 尤其是，Windows 裝置的使用者代理程式字串類似權杖中的細微變化。 下列 Windows PowerShell 範例會針對目前市場上支援無縫 WIA 的目前裝置集合，提供最佳的指導方針：

如果您有 Windows Server 2012 R2 或更早版本的 AD FS：

```powershell
Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0","Windows NT 10.0; WOW64; Trident/7.0","MSIPC", "Windows Rights Management Client", "Edg/","Edge/")
```

如果您在 Windows Server 2016 或更新版本上有 AD FS：

```powershell
Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0","Windows NT 10.0; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client", "=~Windows\s*NT.*Edg.*")
```

上述命令可確保 AD FS 僅涵蓋下列 WIA 使用案例：

|使用者代理程式|使用案例|
|-----|-----|
|MSIE 6。0|IE 6。0|
|MSIE 7.0;Windows NT|IE 7，IE 位於內部網路區域。 "Windows NT" 片段是由桌上型電腦作業系統所傳送。|
|MSIE 8。0|IE 8.0 (沒有任何裝置傳送此資訊，因此需要進行更明確的) |
|MSIE 9。0|IE 9.0 (沒有任何裝置傳送此資訊，因此不需要讓此更明確的) |
|MSIE 10.0;Windows NT 6|適用于 Windows XP 的 IE 10.0 及較新版本的桌面作業系統</br></br>將喜好設定設為行動) 的 Windows Phone 8.0 裝置 (會被排除，因為它們傳送</br></br>使用者代理程式： Mozilla/5.0 (相容;MSIE 10.0;Windows Phone 8.0;Trident/6.0;IEMobile/10.0;分支觸及NOKIALumia 920) |
|Windows NT 6.3;Trident/7。0</br></br>Windows NT 6.3;Win6464Trident/7。0</br></br>Windows NT 6.3;WOW64Trident/7。0| Windows 8.1 桌面作業系統，不同的平臺|
|Windows NT 6.2;Trident/7。0</br></br>Windows NT 6.2;Win6464Trident/7。0</br></br>Windows NT 6.2;WOW64Trident/7。0|Windows 8 桌面作業系統，不同的平臺|
|Windows NT 6.1;Trident/7。0</br></br>Windows NT 6.1;Win6464Trident/7。0</br></br>Windows NT 6.1;WOW64Trident/7。0|Windows 7 桌面作業系統，不同的平臺|
|Edg/79.0.309.43 | 適用于 Windows Server 2012 R2 或更早版本的 Microsoft Edge (Chromium)  |
|Edg/*| 適用于 Windows Server 2016 或更新版本的 Microsoft Edge (Chromium) |
|[MSIPC]| Microsoft Information Protection and Control 用戶端|
|Windows Rights Management 用戶端|Windows Rights Management 用戶端|

### <a name="additional-links"></a>其他連結

[Microsoft Edge 文件](/microsoft-edge/web-platform/user-agent-string)
