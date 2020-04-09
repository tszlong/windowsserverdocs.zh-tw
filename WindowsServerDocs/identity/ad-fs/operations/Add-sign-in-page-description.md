---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: 在頁面描述中新增 sign\-
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8d3cc69bde1c9126f97926802b53d049ed1ef501
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859991"
---
# <a name="add-sign-in-page-description"></a>在頁面描述中新增 sign\-


## <a name="to-add-sign-in-page-description"></a>若要在頁面描述中新增 sign\-  
若要將頁面描述中的登\-新增至頁面的 [登入]\-，請使用下列 Windows PowerShell Cmdlet 和語法。  

![新增登入描述](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> `SignInPageDescriptionText` 參數的字串支援包含標記與不包含標記的純 HTML。 因此，您也可以執行下列 Cmdlet，而不使用 &lt;p&gt; 標記。  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

自訂頁面中的 登入\-之後，自訂的優先順序會較高;因此，您應該針對您想要支援的所有語言進行自訂。 所有的自訂內容皆接受地區設定參數。 當您設定當地語系化的內容時，應該先使用國家/地區\-較少的地區設定，例如 "en"，然後才能設定國家和地區\-特定地區設定，例如 "en\-us"。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
