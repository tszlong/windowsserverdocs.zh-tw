---
description: 深入瞭解：移除 Microsoft 著作權
ms.assetid: c89a977c-b09f-44ec-be42-41e76a6cf3ad
title: 移除 Microsoft 著作權
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: e602ae1f188de72ae974e45d8a5f341185216eb5
ms.sourcegitcommit: 6a62d736e4d9989515c6df85e2577662deb042b6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2021
ms.locfileid: "98103740"
---
# <a name="remove-the-microsoft-copyright"></a>移除 Microsoft 著作權



根據預設，AD FS 頁面會包含 Microsoft 著作權。 若要從您的自訂頁面移除此著作權，可以使用下列程序。

![顯示在底部有著作權之登入頁面的螢幕擷取畫面。](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)

## <a name="to-remove-the-microsoft-copyright"></a>移除 Microsoft 著作權

1. 根據預設值建立自訂的佈景主題。

   ```powershell
   New-AdfsWebTheme –Name custom –SourceName default
   ```

2. 指定輸出資料夾以匯出佈景主題。

   ```powershell
   Export-AdfsWebTheme -Name custom -DirectoryPath C:\CustomWebTheme
   ```

3. 找出 `Style.css` 位於輸出檔案夾中的檔案。 使用上述範例，路徑會是 `C:\CustomWebTheme\Css\Style.css.`

4. `Style.css`使用編輯器（例如 [記事本]）開啟檔案。

5. 找出 `#copyright` 部分，並將它變更如下：

   ```css
   #copyright {color:#696969; display:none;}
   ```

6. 建立以新檔案為基礎的自訂主題 `Style.css` 。

   ```powershell
   Set-AdfsWebTheme -TargetName custom -StyleSheet @{locale="";path="C:\customWebTheme\css\style.css"}
   ```

7. 啟動新的佈景主題。

   ```powershell
   Set-AdfsWebConfig -ActiveThemeName custom
   ```

現在，您應該不會再于登入頁面底部看到著作權。

![移除著作權](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1a.png)

## <a name="additional-references"></a>其他參考資料
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)
