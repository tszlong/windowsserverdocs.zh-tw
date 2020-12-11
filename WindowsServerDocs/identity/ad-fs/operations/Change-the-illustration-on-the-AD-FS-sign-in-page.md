---
description: 深入瞭解：變更 AD FS 登入頁面上的圖例
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: 變更 AD FS 登入頁面上的圖例
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 30f6b4cc31869b06f57939123b5feca9371715a3
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040216"
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>變更 AD FS 登入頁面上的圖例

## <a name="change-the-illustration"></a>變更圖例

若要變更 [登入] 頁面上顯示的圖形（顯示在 [登入] 頁面上）， \- 請使用下列 Windows PowerShell Cmdlet 和語法。

![變更圖例](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

> [!IMPORTANT]
> 建議插圖尺寸為 1420x1080 pixels @ 96 DPI，檔案大小不超過 200 KB。

```powershell
Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}
```

## <a name="additional-references"></a>其他參考資料

[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)
