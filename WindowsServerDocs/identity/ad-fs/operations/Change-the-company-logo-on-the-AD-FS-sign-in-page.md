---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: 變更 AD FS 登入頁面上的公司標誌
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fe5c138466ea288b5dfb8c7c284603150ab9d874
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190035"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>變更 AD FS 登入頁面上的公司標誌

#### <a name="change-company-logo"></a>變更公司標誌  
若要變更登會顯示公司標誌\-在頁面上，使用下列 PowerShell Windows PowerShell cmdlet 和語法。  

![變更標誌](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> 建議標誌尺寸為 260 x 35 @ 96 dpi，檔案大小不超過 10 KB。  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}  

  
> [!NOTE]  
> `TargetName` 是必要參數。 名為會隨著 AD FS 釋出的預設佈景主題*預設*。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
