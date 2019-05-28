---
ms.assetid: c89a977c-b09f-44ec-be42-41e76a6cf3ad
title: 移除 Microsoft 著作權
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6e15f9d1490ad62f1458cd32da6e78a6febec58d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189026"
---
# <a name="remove-the-microsoft-copyright"></a>移除 Microsoft 著作權 


 
根據預設，AD FS 頁面包含 Microsoft 著作權。 若要從您的自訂頁面移除此著作權，可以使用下列程序。 

![移除著作權](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png) 
  
## <a name="to-remove-the-microsoft-copyright"></a>移除 Microsoft 著作權  
  
1. 根據預設值建立自訂的佈景主題。

   ```powershell
   New-AdfsWebTheme –Name custom –SourceName default
   ```

2. 指定輸出資料夾以匯出佈景主題。  

   ```powershell
   Export-AdfsWebTheme -Name custom -DirectoryPath C:\CustomWebTheme
   ```

3. 找出`Style.css`位於輸出資料夾中的檔案。 藉由使用上述的範例，路徑就是 `C:\CustomWebTheme\Css\Style.css.`
  
4. 開啟`Style.css`編輯器，例如 [記事本] 檔案。  
  
5. 找出 `#copyright` 部分，並將它變更如下：  

   ```css
   #copyright {color:#696969; display:none;}
   ```

6. 建立自訂佈景主題為基礎的新`Style.css`檔案。  

   ```powershell
   Set-AdfsWebTheme -TargetName custom -StyleSheet @{locale="";path="C:\customWebTheme\css\style.css"}
   ```

7. 啟動新的佈景主題。  

   ```powershell
   Set-AdfsWebConfig -ActiveThemeName custom
   ```

現在，您應該不會再看到著作權底部的 [登入] 頁面。

![移除著作權](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1a.png) 

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md) 
