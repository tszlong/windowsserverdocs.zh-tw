---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: 新增\-在 [描述] 頁面
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 94ad9889ce78ba77f016210ee478a301babdf307
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190046"
---
# <a name="add-sign-in-page-description"></a>新增\-在 [描述] 頁面


## <a name="to-add-sign-in-page-description"></a>若要新增登\-在 [描述] 頁面  
若要新增標誌\-正負號的頁面描述中\-在頁面上，使用下列 Windows PowerShell cmdlet 和語法。  

![新增登入說明](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> `SignInPageDescriptionText` 參數的字串支援包含標記與不包含標記的純 HTML。 因此，您也可以執行下列 cmdlet 而不使用&lt;p&gt;標記。  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

號後面\-頁面中為自訂，自訂會有優先順序; 因此，您應該自訂您想要支援的所有語言。 所有的自訂內容皆接受地區設定參數。 當您設定當地語系化的內容時，它應該設有國家/地區\-較少的地區設定第一個，例如，"en"，然後再設定 country 和 region\-特定地區設定，例如 「 en-us\-我們"。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
