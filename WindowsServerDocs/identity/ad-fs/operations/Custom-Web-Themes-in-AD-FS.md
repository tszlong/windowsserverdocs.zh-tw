---
ms.assetid: 0379abc3-25c7-46ab-9a6b-80a5152365b0
title: "AD FS 中自訂 Web 主題"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 300c9fda84285ddfc52a4f47ea0198deb6fd33ef
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="custom-web-themes-in-ad-fs"></a>AD FS 中自訂 Web 主題 

>適用於：Windows Server 2016、Windows Server 2012 R2

預設稱為「已推出的 out\ of\-the\-方塊的主題。 您可以匯出預設的主題，並使用它，以便您可以快速開始。 您可以自訂的外觀及操作，包括配置修改著、匯入並套用這個新的主題，並可以使用的自訂的外觀及操作。 使用著也可讓您輕鬆地使用您的網路設計工具。  
  
下列 cmdlet 建立自訂 web 主題，請重複的預設網頁主題。  
  
  
`New-AdfsWebTheme –Name custom –SourceName default ` 

  
您可以修改著並使用新藉著設定的新網頁主題。 若要匯出 web 主題，請使用下列 cmdlet。  
  

    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  

  
若要將著套用到新的主題，使用下列 cmdlet。  
  

    Set-AdfsWebTheme –TargetName custom –StyleSheet @{path=”c:\NewTheme.css”}  
  
  
下列 cmdlet 會建立新的樣式自訂 web 主題。  
  
  
`New-AdfsWebTheme –Name custom –StyleSheet @{path=”c:\NewTheme.css”} –RTLStyleSheetPath c:\NewRtlTheme.css ` 
  
  
  
AD FS 来套用的自訂 web 主題，請使用下列 cmdlet。  
  

`Set-AdfsWebConfig -ActiveThemeName custom`  

  
若要新增 AD FS JavaScript，使用下列 cmdlet。  
  
 
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’ /adfs/portal/script/onload.js’;path="D:\inetpub\adfsassets\script\onload.js"}  


## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
