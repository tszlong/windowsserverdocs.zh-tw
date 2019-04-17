---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: "自訂當地語系化"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ac206d5aa8af970b65a014955ac66a8cf2835eb6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="customization-for-localization"></a>自訂當地語系化 

>適用於：Windows Server 2016、Windows Server 2012 R2

可能是為非英文當地語系化網頁。 請留意下列考量當您將當地語系化。  
  
自訂 content 之後，自訂的優先順序。因此，您應該自訂為所有要支援的語言。 所有自訂的 content 參數的地區設定。 當您設定當地語系化的 content 時，設定無 country\ 的地區設定的第一次，，例如「en-us，「您設定的國家與地區特定 region\ 如之前「en\-我們」。  
  
以下範例顯示一些額外的程式碼範例。  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
以下範例顯示一些額外的程式碼範例。  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
如果您想要自訂網頁所需的 Unicode 輸入英文以外的語言，我們建議您使用 Windows PowerShell ISE。 如需詳細資訊請查看[引進 Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx)。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md) 
