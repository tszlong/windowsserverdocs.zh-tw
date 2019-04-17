---
ms.assetid: 7e804590-6d6c-4cca-ac14-02d4dff06cec
title: "更新密碼的自訂項目"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4b06992bfb398b66988ad4882217a8a83738365e
ms.sourcegitcommit: 78d8839ccafa9530784cb9e38c3127ed2c215423
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/09/2018
---
# <a name="update-password-customization"></a>更新密碼的自訂項目 

>適用於：Windows Server 2016、Windows Server 2012 R2

有時候，使用者可能會無法連接到企業網路，以變更其密碼。 這個因素可能會造成困擾，尤其是最從最接近居住可能會遠端員工的公司。 為這些特定案例中，只有連接到網際網路可以使用更新密碼頁面。  
  
您可以藉由提供您自己的頁面描述自訂更新密碼頁面。  
  
> 要使用密碼更新頁面，請移至下端點 AD FS 管理。 更新密碼的端點位於底部在其他-日 adfs 日入口網站日 updatepassword 日。 您有支援端點之後, 您必須重新 AD FS 服務。 此必須手動完成。 您可以再瀏覽至 https://<fqdn>加入日 adfs 日入口網站日 updatepassword 日在工作地點裝置，您應該會看到更新的 [密碼] 頁面。  
  
![更新](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom5.png)  
  
## <a name="customize-the-update-password-page-description"></a>自訂描述更新密碼頁面  
若要自訂更新密碼頁面描述，使用下列 Windows PowerShell cmdlet 和語法。  
  

    Set-AdfsGlobalWebContent -UpdatePasswordPageDescriptionText "This is the Contoso Update Password page."  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
