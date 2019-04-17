---
title: "Azure AD UX Web 主題中 AD FS"
description: "下列文件告訴您如何變更 AD FS 表單登入，以便看起來像 Azure AD 的使用者體驗。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/24/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e7bac1db17eb4facc7643fe0db0ccf00c119a45d
ms.sourcegitcommit: 9278435cbfa8dbeb30d0557ed0d27832b154edd2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/09/2018
---
# <a name="using-an-azure-ad-ux-web-theme-in-active-directory-federation-services"></a>使用 Azure AD UX Web 主題 Active Directory 同盟服務
AD FS 表單登入目前不鏡射體驗 Azure 日 O365 登入。  若要為使用者提供更統一且順暢的體驗，我們推出依照階層樣式表 web 主題，可以用於 AD FS 伺服器。  目前表單登入 AD fs 在 Windows Server 2016 的外觀如下：

![目前登入](media/Azure-UX-Web-Theme-in-AD-FS/one.png)


新樣式，使用的使用者體驗看起來更像 Azure 與 Office 365 登入的體驗。

## <a name="download-the-css-style-sheet"></a>下載客服支援樣式
您可以從下列 Github 下載的 web 主題[位置](https://github.com/Microsoft/adfsWebCustomization/tree/master/centeredUi)。


## <a name="enabling-the-new-web-theme"></a>讓新的網路主題
若要讓新的網路主題使用下列程序：

### <a name="to-enable-the-new-azure-ad-ux-web-theme-in-ad-fs"></a>若要讓 AD FS 中的新 Azure AD UX web 主題
1.  [開始] 畫面 PowerShell 系統管理員的身分
2.  建立新的網路主題使用 PowerShell:  `New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"}`
3.  做為作用中的主題使用 PowerShell 設定新的主題：<ph x="1">'設定 AdfsWebConfig-ActiveThemeName 自訂'
![</ph>PowerShell](media/Azure-UX-Web-Theme-in-AD-FS/two.png)
4.  移至 [https:// 測試登入<AD FS name.domain>/adfs/ls/idpinitiatedsignon.htm![登入](media/Azure-UX-Web-Theme-in-AD-FS/three.png)

>![筆記]您需要確定該 idpinitiatedsignon 已支援。  這不被支援預設。  若要在 idpinitiatedsignon 使用下列命令 PowerShell:  `Set-AdfsProperties –EnableIdpInitiatedSignonPage $True`

## <a name="image-recommendations"></a>建議的影像
以下是在背景影像和商標映像大小的建議：

### <a name="logo"></a>商標
- 調整 24px 高度 256px 最大的寬度
- 不要將任何填補周圍中資產商標。  請確定透明資產背景。

### <a name="background"></a>背景
- 大小 1024 x 1080 像素的不超過 200 KB 檔案的大小。  您應該使用的最高解析度可能的外觀比例 16:9 月 16:10 保持在限制影像大小。

## <a name="next-steps"></a>後續步驟
- [AD FS 在 Windows Server 2016 中的自訂項目](AD-FS-Customization-in-Windows-Server-2016.md)
- [進階的自訂項目](Advanced-Customization-of-AD-FS-Sign-in-Pages.md)
- [自訂 web 主題](Custom-Web-Themes-in-AD-FS.md)
