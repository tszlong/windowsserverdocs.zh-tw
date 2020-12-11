---
description: 深入瞭解：新增隱私權連結
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: 新增隱私權連結
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c15491ed9540c68c76e9a6ddfbcdf830690c9dc1
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044306"
---
# <a name="add-privacy-link"></a>新增隱私權連結


若要新增登入頁面上顯示的隱私權連結 \- ，請使用下列 Windows PowerShell Cmdlet 和語法。

![新增隱私權連結](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)


`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`


> [!IMPORTANT]
> 此 Cmdlet 的 `linkText` 並非必要參數，除非您使用其他值，而不是預設的 *Privacy*。 使用預設的優點是頁面會根據所有用戶端地區設定來當地語系化。 在 \- 自訂登入頁面之後，自訂會優先使用; 因此，您應該針對您想要支援的所有語言進行自訂。 所有的自訂內容皆接受地區設定參數。 當您設定當地語系化內容時，您應該先使用較小的地區設定（ \- 例如 "en"）進行設定，然後再設定國家/地區特定地區設定 \- ，例如 "en-us" \- 。

## <a name="additional-references"></a>其他參考資料
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)
