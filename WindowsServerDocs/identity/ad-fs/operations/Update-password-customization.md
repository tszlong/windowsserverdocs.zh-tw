---
description: 深入瞭解：更新密碼自訂
ms.assetid: 7e804590-6d6c-4cca-ac14-02d4dff06cec
title: 更新密碼自訂
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 58699ca51b1a56296f5ad1618dd2eefca4301ac2
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039676"
---
# <a name="update-password-customization"></a>更新密碼自訂

在某些情況下，使用者可能無法連線到公司網路以變更帳戶密碼。 特別是遠離公司辦公室的員工，這可能會造成問題。 在這些特定的情況下，只有連線到網際網路才能使用更新密碼頁面。

您可以提供更新密碼頁面的描述，以自訂該頁面。

若要啟用密碼更新頁面，請移至 [端點] 下的 [AD FS 管理]。 更新密碼的端點位於 [其他] 底部 - /adfs/portal/updatepassword/。 啟用端點之後，您必須重新啟動 AD FS 服務。 這必須手動完成。 如果您想要在外部使用更新密碼網頁，而且使用 Web 應用程式 Proxy 時，您必須在 proxy 上啟用它， (在 proxy) 上啟用此選項。 然後，您可以 `https://<fqdn>/adfs/portal/updatepassword/` 在已加入工作場所的裝置上流覽至，您應該會看到 [更新密碼] 頁面。

![update](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom5.png)

## <a name="customize-the-update-password-page-description"></a>自訂更新密碼頁面描述

若要自訂 [更新密碼] 頁面描述，請使用下列 Windows PowerShell Cmdlet 和語法。

```powershell
Set-AdfsGlobalWebContent -UpdatePasswordPageDescriptionText "This is the Contoso Update Password page."
```

## <a name="additional-references"></a>其他參考資料

[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)
