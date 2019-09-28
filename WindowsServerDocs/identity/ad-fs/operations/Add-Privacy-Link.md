---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: 新增隱私權連結
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cd559e38c38e96d1417257fe7d6ff8ccfa180c6b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358417"
---
# <a name="add-privacy-link"></a>新增隱私權連結 


若要新增 [正負號 @ no__t-0in] 頁面上顯示的隱私權連結，請使用下列 Windows PowerShell Cmdlet 和語法。  

![新增隱私權連結](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> 此 Cmdlet 的 `linkText` 並非必要參數，除非您使用其他值，而不是預設的 *Privacy*。 使用預設的優點是頁面會根據所有用戶端地區設定來當地語系化。 自訂 [正負號 @ no__t-0in] 頁面之後，自訂的優先順序會較高;因此，您應該針對您想要支援的所有語言進行自訂。 所有的自訂內容皆接受地區設定參數。 當您設定當地語系化的內容時，應先設定一個國家/地區 @ no__t-0less locale （例如 "en"），然後再設定 country 和 region @ no__t-1specific locale，例如 "en @ no__t-2us"。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
