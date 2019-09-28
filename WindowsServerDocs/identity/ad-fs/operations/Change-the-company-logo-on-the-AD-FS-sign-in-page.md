---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: 變更 AD FS 登入頁面上的公司標誌
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b22c969e0113081e1ca8a662ae81a2ee24829835
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358300"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>變更 AD FS 登入頁面上的公司標誌

#### <a name="change-company-logo"></a>變更公司標誌  
若要變更 [正負號 @ no__t-0in] 頁面上顯示的公司標誌，請使用下列 PowerShell Windows PowerShell Cmdlet 和語法。  

![變更標誌](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> 建議標誌尺寸為 260 x 35 @ 96 dpi，檔案大小不超過 10 KB。  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}  

  
> [!NOTE]  
> `TargetName` 是必要參數。 以 AD FS 發行的預設主題名稱為*default*。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
