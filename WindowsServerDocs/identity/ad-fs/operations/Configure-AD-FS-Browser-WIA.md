---
title: 設定與 AD FS 搭配使用 Windows 整合式驗證 (WIA) 的瀏覽器
description: 本文件說明如何設定與 AD FS 搭配使用 WIA 的瀏覽器
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f71680bb721635bd37197dca9d3ae4726099525f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845479"
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>設定與 AD FS 搭配使用 Windows 整合式驗證 (WIA) 的瀏覽器

根據預設，Active Directory Federation Services (AD FS) 在 Windows Server 2012 R2 中使用任何應用程式內組織的內部網路 （內部網路） 發生的驗證要求中啟用 Windows 整合式驗證 (WIA)其驗證的瀏覽器。

AD FS 2016 現在已改善的預設設定，以啟用 microsoft Edge 瀏覽器，而不也 （不正確） 攔截的 Windows Phone 以及執行 WIA:

    =~Windows\s*NT.*Edge

上面的內容表示您不再需要設定個別的使用者代理字串，以支援常見的邊緣案例中，即使會經常更新。

其他瀏覽器中，設定 AD FS 屬性**WiaSupportedUserAgents**將根據您使用的瀏覽器的必要的值。  您可以使用下列程序。



### <a name="view-wiasupporteduseragent-settings"></a>檢視 WIASupportedUserAgent 設定
**WIASupportedUserAgents**定義支援 WIA 的使用者代理程式。 執行登入瀏覽器或瀏覽器控制項中時，AD FS 會分析使用者代理字串。

您可以檢視目前的設定，使用下列 PowerShell 範例：

```powershell
    $strings = Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![WIA 支援](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>變更 WIASupportedUserAgent 設定
根據預設，新的 AD FS 安裝會有一組的使用者代理程式字串建立的相符項目。 不過，這些可能是過期根據瀏覽器和裝置的變更。 特別是，Windows 裝置會在權杖中，有微小差異與相似的使用者代理程式字串。 下列 Windows PowerShell 範例提供最新的集合支援的裝置目前市場上無縫 WIA 的最佳指引：

```powershell
    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")
```

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
