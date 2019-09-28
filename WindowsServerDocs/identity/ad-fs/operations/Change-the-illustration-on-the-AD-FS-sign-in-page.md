---
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: 變更 AD FS 登入頁面上的圖例
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3da7726ca625c32728fb0ae64d291ae599b6cd8d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358286"
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>變更 AD FS 登入頁面上的圖例

## <a name="change-the-illustration"></a>變更圖例  


若要變更圖例（顯示在 @ no__t-0in 頁面上的左側圖形），請使用下列 Windows PowerShell Cmdlet 和語法。  

![變更圖例](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> 建議插圖尺寸為 1420x1080 pixels @ 96 DPI，檔案大小不超過 200 KB。  
  
 
    Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
  
  
