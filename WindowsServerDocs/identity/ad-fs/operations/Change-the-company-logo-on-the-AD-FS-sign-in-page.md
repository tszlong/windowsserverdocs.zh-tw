---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: 變更 AD FS 登入頁面上的公司標誌
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b61f7321dc75613a3450998284536673bd790f2b
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/03/2020
ms.locfileid: "87519817"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>變更 AD FS 登入頁面上的公司標誌

## <a name="change-company-logo"></a>變更公司標誌

若要變更登入頁面上顯示的公司標誌 \- ，請使用下列 Powershell Windows powershell Cmdlet 和語法。

![變更標誌](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

> [!IMPORTANT]
> 建議標誌尺寸為 260 x 35 @ 96 dpi，檔案大小不超過 10 KB。

```powershell
Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}
```

> [!NOTE]
> `TargetName` 是必要參數。 以 AD FS 發行的預設主題名稱為*default*。

## <a name="additional-references"></a>其他參考

[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)
