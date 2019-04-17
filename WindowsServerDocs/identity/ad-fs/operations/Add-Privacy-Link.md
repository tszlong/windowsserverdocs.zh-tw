---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: "新增連結隱私權"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 81a453b45693b8222bdfc0231885b506fdfcd2fc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="add-privacy-link"></a>新增連結隱私權 

>適用於：Windows Server 2016、Windows Server 2012 R2

若要新增的隱私權連結，會顯示在 sign\ 在頁面上，使用下列的 Windows PowerShell cmdlet 和語法。  

![新增連結隱私權](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> `linkText`在這個 cmdlet 參數不需要，除非您要使用預設值，也就是比另一個值*隱私權*。 使用預設的優點是之當地語系化 client 所有的地區設定。 自訂 sign\ 頁面會之後，自訂的優先順序。因此，您應該自訂為所有要支援的語言。 所有自訂的 content 參數的地區設定。 當您設定當地語系化的 content 時，您應該設定 country\ 無地區設定的第一次，，例如「en-us」，您在設定之前國家與地區 region\ 特定的設定，例如「en\-我們」。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
