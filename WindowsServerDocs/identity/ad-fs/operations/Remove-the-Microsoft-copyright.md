---
ms.assetid: c89a977c-b09f-44ec-be42-41e76a6cf3ad
title: "移除的 Microsoft 的著作權權益"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c2e6f9445e53a5b5867a763d58ad4a6ca3600cbe
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="remove-the-microsoft-copyright"></a>移除的 Microsoft 的著作權權益 

>適用於：Windows Server 2016、Windows Server 2012 R2
 
根據預設，AD FS 之包含 Microsoft 著作權。 若要移除此著作權從 [自訂頁面，您可以使用下列程序。 

![移除著作權](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png) 
  
## <a name="to-remove-the-microsoft-copyright"></a>若要移除的 Microsoft 的著作權權益  
  
1.  建立自訂預設值為基礎的主題。  
  

    `New-AdfsWebTheme –Name custom –SourceName default ` 
 
  
2.  藉由輸出資料夾匯出主題。  

    `Export-AdfsWebTheme -Name custom -DirectoryPath C:\customWebTheme ` 

  
3.  找出 Style.css 檔案所在位置輸出資料夾中。 使用先前的範例，為 C:\\CustomWebTheme\\Css\\Style.css 路徑。  
  
4.  使用編輯器] 中，例如記事本開放 Style.css 檔案。  
  
5.  找出`#copyright`部分，以及然後變更下列：  
  `#copyright {color:#696969; display:none;} ` 
 
6.  建立自訂主題為基礎的新 Style.css 檔案。  
  
    `Set-AdfsWebTheme -TargetName custom -StyleSheet @{locale="";path="C:\customWebTheme\css\style.css"}  `

7.  啟動新的主題。  
  

    `Set-AdfsWebConfig -ActiveThemeName custom ` 


現在，您應該不會再看到登入頁面底部的著作權權益。

![移除著作權](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1a.png) 

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md) 
