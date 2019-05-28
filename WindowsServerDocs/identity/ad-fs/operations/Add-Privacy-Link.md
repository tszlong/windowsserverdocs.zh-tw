---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: 新增隱私權連結
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3617335a179ab419982ab57343999ad4fcaf522a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190159"
---
# <a name="add-privacy-link"></a>新增隱私權連結 


若要新增隱私權連結會顯示在登\-在頁面上，使用下列 Windows PowerShell cmdlet 和語法。  

![新增隱私權連結](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> 此 Cmdlet 的 `linkText` 並非必要參數，除非您使用其他值，而不是預設的 *Privacy*。 使用預設的優點是頁面會根據所有用戶端地區設定來當地語系化。 號後面\-頁面中為自訂，自訂會有優先順序; 因此，您應該自訂您想要支援的所有語言。 所有的自訂內容皆接受地區設定參數。 當您設定當地語系化的內容時，您應該設定國家/地區\-較少的地區設定第一個，例如，"en"，然後再設定 country 和 region\-特定地區設定，例如"en\-我們"。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
