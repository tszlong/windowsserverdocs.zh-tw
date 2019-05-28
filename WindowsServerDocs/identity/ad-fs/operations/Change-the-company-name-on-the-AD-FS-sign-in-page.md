---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: 變更 AD FS 登入頁面上的公司名稱
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e2b5c7228094305759344d5094cffa7f24a0da7a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190018"
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>變更 AD FS 登入頁面上的公司名稱
 
若要變更登上顯示的公司名稱\-在頁面上，使用下列 Windows PowerShell cmdlet 和語法。 這個值預設使用安裝期間輸入的 Federation Service 顯示名稱的值來設定。  

![變更名稱](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> 您也可以使用 Windows PowerShell 整合式指令碼環境\(ISE\)來變更公司名稱。 藉由使用 Windows PowerShell ISE 中，您就可以顯示內容，在 Unicode\-相容的環境。 如需詳細資訊，請參閱 [Windows PowerShell ISE 簡介](https://technet.microsoft.com/library/dd315244.aspx)。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
  
