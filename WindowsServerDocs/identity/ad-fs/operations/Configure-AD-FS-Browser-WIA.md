---
title: "設定 Windows 整合驗證 (WIA) 使用 AD FS 使用的瀏覽器"
description: "本文件告訴您如何設定 WIA 使用 AD FS 使用的瀏覽器"
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f7d43931d1fe4958a539ff1b728e4cc154d06248
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>設定 Windows 整合驗證 (WIA) 使用 AD FS 使用的瀏覽器

根據預設，Windows 整合驗證 (WIA) 可以在 Active Directory 同盟 Services (AD FS) 在 Windows Server 2012 R2 進行驗證要求組織連絡 (intranet) 的任何應用程式使用它驗證的瀏覽器中發生的。

AD FS 2016 現在已改進的預設設定，讓時不也（不正確）攔截的 Windows Phone，以及執行 WIA 在 Edge 瀏覽器：

    =~Windows\s*NT.*Edge

上述表示，您不再需要設定個人使用者代理程式字串支援常見 Edge 案例中，即使它們會經常更新。

設定其他瀏覽器，[AD FS 屬性**WiaSupportedUserAgents**來新增所需的值根據您所使用的瀏覽器。  您可以使用以下程序。



### <a name="view-wiasupporteduseragent-settings"></a>檢視 WIASupportedUserAgent 設定
**WIASupportedUserAgents**定義支援 WIA 使用者代理程式。 AD FS 分析使用者代理字串時登入執行瀏覽器或瀏覽器的控制項。

您可以檢視使用下列 PowerShell 範例目前的設定：

```powershell
    $strings = Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![WIA 支援](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>變更 WIASupportedUserAgent 設定
根據預設，新 AD FS 安裝有建立的使用者專員字串相符項目的設定。 不過，這些可能是最新的根據變更瀏覽器和裝置。 尤其是，Windows 裝置有類似的使用者代理程式字串次要變化權杖中。 下列 Windows PowerShell 範例提供最佳的指導方針目前支援的裝置是目前市面上 WIA 順暢的設定：

```powershell
    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")
```

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
Windows NT 6.1;7.0 戟日</br></br>Windows NT 6.1;Win64;x64;7.0 戟日</br></br>Windows NT 6.1;WOW64;7.0 戟日|Windows 7 桌面作業系統，不同 platoforms|
MSIPC| 保護 Microsoft 的資訊與控制項 Client|
Windows 的權限管理 Client|Windows 的權限管理 Client|
