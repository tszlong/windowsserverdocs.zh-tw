---
description: 深入瞭解：新增技術支援中心連結
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: 新增服務台連結
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c806390687d5293feacd2575b10569504334914f
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044366"
---
# <a name="add-help-desk-link"></a>新增服務台連結


## <a name="to-add-a-help-desk-link"></a>新增技術支援中心連結
若要新增登入頁面上顯示的服務台連結 \- ，請使用下列 Windows PowerShell Cmdlet 和語法。

![新增技術支援中心](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)


`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`


> [!IMPORTANT]
> 此 Cmdlet 的 `linkText` 並非必要參數，除非您使用其他值，而不是預設的 *Help*。 使用預設的優點是會根據所有用戶端地區設定來當地語系化。 在自訂頁面之後，自訂會有較高的優先順序；因此，您應該自訂您想要支援的所有語言。


## <a name="additional-references"></a>其他參考資料
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)
