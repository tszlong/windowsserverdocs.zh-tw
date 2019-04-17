---
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: "變更 AD FS 登入頁面上的圖例"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ac5e60aaad864248b58a3908e7aa9622165fbc14
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>變更 AD FS 登入頁面上的圖例

>適用於：Windows Server 2016、Windows Server 2012 R2

## <a name="change-the-illustration"></a>變更圖示  


若要變更圖示，圖形左，會顯示在 sign\ 在頁面上，使用下列的 Windows PowerShell PowerShell cmdlet 和語法。  

![變更圖示](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> 我們建議的尺寸圖將 1420 x 1080 像素，96 DPI @ 不超過 200 KB 檔案的大小。  
  
 
    Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
  
  
