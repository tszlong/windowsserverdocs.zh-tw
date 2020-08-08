---
title: AD FS 中的 Azure AD UX Web 主題
description: 下列檔說明如何變更 AD FS 表單登入，使其類似 Azure AD 的使用者體驗。
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/24/2017
ms.topic: article
ms.openlocfilehash: a7cb3a037d074fc4a61e6c805bca181316643bb3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956675"
---
# <a name="using-an-azure-ad-ux-web-theme-in-active-directory-federation-services"></a>在 Active Directory 同盟服務中使用 Azure AD UX Web 主題
AD FS 表單登入目前不會反映 Azure/O365 登入體驗。  為了為使用者提供更一致且流暢的體驗，我們發行了遵循的級聯樣式表 web 主題，可套用至您的 AD FS 伺服器。  目前，Windows Server 2016 上 AD FS 的表單登入如下所示：

![目前的登入](media/Azure-UX-Web-Theme-in-AD-FS/one.png)


使用新的樣式表單，使用者體驗看起來會更像 Azure 和 Office 365 登入體驗。

## <a name="download-the-css-style-sheet"></a>下載 CSS 樣式表單
您可以從下列 Github[位置](https://github.com/Microsoft/adfsWebCustomization/tree/master/centeredUi)下載 web 主題。


## <a name="enabling-the-new-web-theme"></a>啟用新的 web 主題
若要啟用新的 web 主題，請使用下列程式：

### <a name="to-enable-the-new-azure-ad-ux-web-theme-in-ad-fs"></a>若要在 AD FS 中啟用新的 Azure AD UX web 主題
1. 以系統管理員身分啟動 PowerShell
2. 使用 PowerShell 建立新的 web 主題：`New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"}`
3. 使用 powershell 將新主題設定為作用中主題： `Set-AdfsWebConfig -ActiveThemeName custom` 
    ![ powershell](media/Azure-UX-Web-Theme-in-AD-FS/two.png)
4. 前往 HTTPs:///adfs/ls/idpinitiatedsignon.htm 登入來測試登入 <AD FS name.domain> ![](media/Azure-UX-Web-Theme-in-AD-FS/three.png)

> !下您必須確定已啟用 iDPInitiatedsignon.aspx) 。  預設不會啟用此記錄。  若要啟用 iDPInitiatedsignon.aspx) ，請使用下列 PowerShell 命令：`Set-AdfsProperties –EnableIdpInitiatedSignonPage $True`

## <a name="image-recommendations"></a>映射建議
啟用中央 UI 可讓您將相同的影像用於背景和標誌，而您可能已經有 Azure Active Directory 公司商標。 一般來說，大小、比例和格式的相同建議也適用。

### <a name="logo"></a>標誌

描述 | 條件約束 | 建議
------- | ------- | ----------
標誌會顯示在 [登入] 面板頂端。 | 透明 JPG 或 PNG<br>最大高度：36 像素<br>最大寬度：245 像素 | 請在這裡使用您組織的標誌。<br>使用透明的映像。 請不要假設背景是白色。<br>請勿在映像中的標誌周圍加上邊框間距，否則您的標誌會看起來不成比例。

### <a name="background"></a>背景

描述 | 條件約束 | 建議
------- | ------- | ----------
此選項會出現在登入頁面的背景中，錨定至可檢視空間的中心，並且會調整並裁切以填滿瀏覽器視窗。    <br>在手機這類螢幕狹窄的裝置上，不會顯示此映像。<br>載入頁面時，會在這整個影像套用 0.55 不透明度的黑色遮罩。 | JPG 或 PNG<br>映像尺寸：1920 x 1080 像素<br>檔案大小：&lt; 300 KB | <br>使用沒有強烈主題焦點的映像。 不透明的登入表單會出現在此影像的中央，並且可根據瀏覽器視窗的大小來覆蓋此影像的任何部分。<br>請將檔案大小維持在小型，以確保載入時間快速。

## <a name="next-steps"></a>後續步驟
- [Windows Server 2016 中的 AD FS 自訂](./ad-fs-customization-in-windows-server.md)
- [進階自訂](Advanced-Customization-of-AD-FS-Sign-in-Pages.md)
- [自訂 web 主題](Custom-Web-Themes-in-AD-FS.md)
