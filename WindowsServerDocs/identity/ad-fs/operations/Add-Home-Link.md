---
description: 深入瞭解：新增首頁連結
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: 新增首頁連結
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 5f684ae3e68459721678a50c91cc04d58c89d81c
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039346"
---
# <a name="add-home-link"></a>新增首頁連結

若要新增登入頁面上顯示的首頁連結 \- ，請使用下列 Windows PowerShell Cmdlet 和語法。


![新增首頁連結](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)


`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home `


> [!IMPORTANT]
> 此 Cmdlet 的 `linkText` 並非必要參數，除非您使用其他值，而不是預設的 *Home*。 使用預設的優點是會根據所有用戶端地區設定來當地語系化。 在 \- 自訂登入頁面之後，自訂會優先使用; 因此，您應該針對您想要支援的所有語言進行自訂。

## <a name="additional-references"></a>其他參考資料
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)
