---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: 新增隱私權連結
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cf111fbfc14665d489201f7d88419bdeffb737f9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859391"
---
# <a name="add-privacy-link"></a>新增隱私權連結 


若要新增 [簽署\-] 頁面上顯示的隱私權連結，請使用下列 Windows PowerShell Cmdlet 和語法。  

![新增隱私權連結](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> 此 Cmdlet 的 `linkText` 並非必要參數，除非您使用其他值，而不是預設的 *Privacy*。 使用預設的優點是頁面會根據所有用戶端地區設定來當地語系化。 自訂頁面中的 登入\-之後，自訂的優先順序會較高;因此，您應該針對您想要支援的所有語言進行自訂。 所有的自訂內容皆接受地區設定參數。 當您設定當地語系化的內容時，您應該先使用國家/地區\-較少的地區設定，例如 "en"，然後再設定國家和地區\-特定地區設定，例如 "en\-us"。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
