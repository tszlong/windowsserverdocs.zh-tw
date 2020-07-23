---
title: 將瀏覽器設定為使用 Windows 整合式驗證（WIA）搭配 AD FS
description: 本檔說明如何設定瀏覽器以搭配使用 WIA 與 AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/20/2020
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1bcd4268444f49489d3e7a04c55d10cddaf92e00
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966530"
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>將瀏覽器設定為使用 Windows 整合式驗證（WIA）搭配 AD FS

根據預設，windows Server 2012 R2 中的 Active Directory 同盟服務（AD FS）會啟用 Windows 整合式驗證（WIA），以取得針對任何使用瀏覽器進行驗證之應用程式在組織內部網路（內部網路）中發生的驗證要求。

AD FS 2016 現在具有改良的預設設定，可讓 Edge 瀏覽器執行 WIA，同時也不會錯誤地捕捉 Windows Phone：

    =~Windows\s*NT.*Edge

上述的意思是，您不再需要設定個別的使用者代理字串來支援常見的邊緣案例，即使它們經常更新也一樣。

若為其他瀏覽器，請設定 AD FS 屬性**WiaSupportedUserAgents** ，以根據您所使用的瀏覽器來新增必要的值。  您可以使用下列程式。



### <a name="view-wiasupporteduseragent-settings"></a>View WIASupportedUserAgent 設定
**WIASupportedUserAgents**會定義支援 WIA 的使用者代理程式。 AD FS 在瀏覽器或瀏覽器控制項中執行登入時，會分析使用者代理字串。

您可以使用下列 PowerShell 範例來查看目前的設定：

```powershell
    Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![WIA 支援](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>變更 WIASupportedUserAgent 設定
根據預設，新的 AD FS 安裝會建立一組使用者代理程式字串相符專案。 不過，根據瀏覽器和裝置的變更，這些可能會過期。 特別的是，Windows 裝置的使用者代理字串類似于權杖中的次要變化。 下列 Windows PowerShell 範例針對目前在支援無縫 WIA 的市場上，提供目前裝置集合的最佳指引：

如果您在 Windows Server 2012 R2 或更早版本上有 AD FS：

```powershell
   Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client", "Edg/79.0.309.43")
```

如果您在 Windows Server 2016 或更新版本上有 AD FS：

```powershell
   Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client", "Edg/*")
```

上述命令可確保 AD FS 只涵蓋下列適用于 WIA 的使用案例：



|使用者代理程式|使用案例|
|-----|-----|
|MSIE 6。0|IE 6。0|
|MSIE 7.0;Windows NT|IE 7，位於內部網路區域。 「Windows NT」片段是由桌上型電腦作業系統傳送。|
|MSIE 8。0|IE 8.0 （沒有任何裝置傳送此資訊，因此需要進行更明確的動作）|
|MSIE 9。0|IE 9.0 （沒有任何裝置傳送此資訊，因此不需要讓它更明確）|
|MSIE 10.0;Windows NT 6|適用于 Windows XP 和較新版本之桌面作業系統的 IE 10。0</br></br>已排除 Windows Phone 8.0 裝置（喜好設定為 mobile），因為它們會傳送</br></br>使用者代理程式： Mozilla/5.0 （相容;MSIE 10.0;Windows Phone 8.0;Trident/6.0;IEMobile/10.0;ARM按鍵式NETWORKSLumia 920）|
|Windows NT 6.3;Trident/7。0</br></br>Windows NT 6.3;Win64x64Trident/7。0</br></br>Windows NT 6.3;WOW64Trident/7。0| Windows 8.1 桌面作業系統、不同的平臺|
|Windows NT 6.2;Trident/7。0</br></br>Windows NT 6.2;Win64x64Trident/7。0</br></br>Windows NT 6.2;WOW64Trident/7。0|Windows 8 桌面作業系統，不同的平臺|
|Windows NT 6.1;Trident/7。0</br></br>Windows NT 6.1;Win64x64Trident/7。0</br></br>Windows NT 6.1;WOW64Trident/7。0|Windows 7 桌面作業系統，不同的平臺|
|Edg/79.0.309.43 | 適用于 Windows Server 2012 R2 或更早版本的 Microsoft Edge （Chromium） |
|Edg/*| 適用于 Windows Server 2016 或更新版本的 Microsoft Edge （Chromium）|  
|[MSIPC]| Microsoft Information Protection and Control 用戶端|
|Windows Rights Management 用戶端|Windows Rights Management 用戶端|


### <a name="additional-links"></a>其他連結

[Microsoft Edge 文件](/microsoft-edge/web-platform/user-agent-string)
