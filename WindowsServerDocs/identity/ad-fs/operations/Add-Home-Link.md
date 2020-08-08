---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: 新增首頁連結
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4bd64d19f5e203086c4a5576f331fde9d1b33cf7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954284"
---
# <a name="add-home-link"></a>新增首頁連結

若要新增登入頁面上顯示的首頁連結 \- ，請使用下列 Windows PowerShell Cmdlet 和語法。


![新增主連結](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)


`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home `


> [!IMPORTANT]
> 此 Cmdlet 的 `linkText` 並非必要參數，除非您使用其他值，而不是預設的 *Home*。 使用預設的優點是會根據所有用戶端地區設定來當地語系化。 \-自訂登入頁面之後，自訂會有較高的優先順序; 因此，您應該自訂您想要支援的所有語言。

## <a name="additional-references"></a>其他參考資料
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)
