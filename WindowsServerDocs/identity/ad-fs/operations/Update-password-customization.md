---
ms.assetid: 7e804590-6d6c-4cca-ac14-02d4dff06cec
title: 更新密碼的自訂
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4b06992bfb398b66988ad4882217a8a83738365e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876569"
---
# <a name="update-password-customization"></a>更新密碼的自訂 

>適用於：Windows Server 2016, Windows Server 2012 R2

在某些情況下，使用者可能無法連線到公司網路以變更帳戶密碼。 特別是遠離公司辦公室的員工，這可能會造成問題。 在這些特定的情況下，只有連線到網際網路才能使用更新密碼頁面。  
  
您可以提供更新密碼頁面的描述，以自訂該頁面。  
  
> 若要啟用密碼更新頁面，請移至 [端點] 下的 [AD FS 管理]。 更新密碼的端點位於 [其他] 底部 - /adfs/portal/updatepassword/。 啟用端點之後，您必須重新啟動 AD FS 服務。 這必須手動完成。 您之後可以在工作地方聯結的裝置上瀏覽 https://<fqdn>/adfs/portal/updatepassword/，應該就會看到更新密碼頁面。  
  
![update](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom5.png)  
  
## <a name="customize-the-update-password-page-description"></a>自訂更新密碼頁面描述  
若要自訂更新密碼頁面描述，請使用下列 Windows PowerShell cmdlet 和語法。  
  

    Set-AdfsGlobalWebContent -UpdatePasswordPageDescriptionText "This is the Contoso Update Password page."  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
