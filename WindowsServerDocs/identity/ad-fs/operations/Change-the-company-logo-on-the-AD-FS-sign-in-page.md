---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: "變更 AD FS 登入頁面上的公司商標"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ca54abe7fe852b22f2f4d9a717e38d219fa50694
ms.sourcegitcommit: a00fc4426dc4ad0098257f01f0124d6c733d1aef
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/09/2018
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>變更 AD FS 登入頁面上的公司商標

>適用於：Windows Server 2016、Windows Server 2012 R2

#### <a name="change-company-logo"></a>變更公司商標  
若要變更顯示 sign\ 在頁面上的公司商標，使用下列 PowerShell Windows PowerShell cmdlet 和語法。  

![變更商標](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> 我們建議為 260 x 350 @ 96 dpi 檔案的大小與不超過 10 KB 標誌尺寸。  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}  

  
> [!NOTE]  
> `TargetName`是必要的參數。 放開 AD FS 使用的預設主題名為*預設*。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
