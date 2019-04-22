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
ms.openlocfilehash: 6e0271ae3d7ac120e510a2fb81fb55c8d10b3c87
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813579"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>變更 AD FS 登入頁面上的公司標誌

>適用於：Windows Server 2016, Windows Server 2012 R2

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
