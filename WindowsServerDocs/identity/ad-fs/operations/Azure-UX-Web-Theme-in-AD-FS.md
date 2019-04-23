---
title: 在 AD FS 的 azure AD UX 網頁佈景主題
description: 下列文件說明如何變更 AD FS 表單登入，使它類似於 Azure AD 使用者體驗。
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/24/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8d6afd7829c92382815e95b8c43a054b000359e2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887879"
---
# <a name="using-an-azure-ad-ux-web-theme-in-active-directory-federation-services"></a>在 Active Directory Federation Services 中使用 Azure AD UX 網頁佈景主題
AD FS form 符號中目前不會鏡像 Azure/O365 登入體驗。  若要為使用者提供更統一且順暢的體驗，我們發行了遵循階層式樣式表網頁佈景主題並可套用至您的 AD FS 伺服器。  目前，表單登入適用於 AD FS，在 Windows Server 2016 看起來如下：

![目前登入](media/Azure-UX-Web-Theme-in-AD-FS/one.png)


與新的樣式表，看起來更像是 Azure 和 Office 365 登入體驗的使用者體驗。

## <a name="download-the-css-style-sheet"></a>下載的 CSS 樣式表
您可以從下列 Github 中的網頁佈景主題[位置](https://github.com/Microsoft/adfsWebCustomization/tree/master/centeredUi)。


## <a name="enabling-the-new-web-theme"></a>啟用新的網頁佈景主題
若要啟用新的網頁佈景主題，請使用下列程序：

### <a name="to-enable-the-new-azure-ad-ux-web-theme-in-ad-fs"></a>若要啟用新的 Azure AD UX web 佈景主題在 AD FS
1.  系統管理員身分啟動 PowerShell
2.  建立新的網頁佈景主題，使用 PowerShell:  `New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"}`
3.  將新的佈景主題設定為 使用 PowerShell 的現用主題：`Set-AdfsWebConfig -ActiveThemeName custom`
![PowerShell](media/Azure-UX-Web-Theme-in-AD-FS/two.png)
4.  測試登入，方法是前往 https://<AD FS name.domain>/adfs/ls/idpinitiatedsignon.htm![登入](media/Azure-UX-Web-Theme-in-AD-FS/three.png)

> ![注意]您必須確定已啟用該 idpinitiatedsignon。  它不是預設啟用。  若要啟用 idpinitiatedsignon 會使用下列 PowerShell 命令：  `Set-AdfsProperties –EnableIdpInitiatedSignonPage $True`

## <a name="image-recommendations"></a>映像的建議
啟用集中的 UI，可讓您使用相同的映像進行背景和標誌，您可能已擁有 Azure Active Directory 公司商標。 一般而言，並套用相同大小、 比例、 及格式的建議。

### <a name="logo"></a>標誌
描述 | 限制式 | 建議
------- | ------- | ----------
標誌會顯示在 登入面板上。 | 透明 JPG 或 PNG<br>最大高度：36 像素<br>最大寬度：245 像素 | 在此使用貴組織的標誌。<br>使用透明的映像。 請勿假設背景為白色。<br>請勿將您的標誌周圍填補新增映像中，或您的標誌會看起來不成比例的小型。

### <a name="background"></a>背景
描述 | 限制式 | 建議
------- | ------- | ----------
此選項會出現在 [登入] 頁面中，背景錨定至可檢視的空間，並且會調整並裁切以填滿瀏覽器視窗的中央。    <br>在窄畫面，例如行動電話，不會顯示此映像。<br>頁面載入時透過這個影像套用 0.55 不透明度的黑色遮罩。 | JPG 或 PNG<br>影像尺寸：1920 x 1080 像素<br>檔案大小：&lt; 300 KB | <br>使用映像在沒有強烈主題焦點。 不透明的登入表單會顯示此映像的中央，並可以涵蓋的任何一部分的映像，視瀏覽器視窗的大小而定。<br>檔案大小保持小，才能確保快速載入時間。

## <a name="next-steps"></a>後續步驟
- [Windows Server 2016 中的 AD FS 自訂](AD-FS-Customization-in-Windows-Server-2016.md)
- [進階的自訂](Advanced-Customization-of-AD-FS-Sign-in-Pages.md)
- [自訂網頁佈景主題](Custom-Web-Themes-in-AD-FS.md)
