---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: "變更 AD FS 登入頁面上的公司名稱"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8e99f6fed8922ed15bf78a98b207b6f46767763a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>變更 AD FS 登入頁面上的公司名稱

>適用於：Windows Server 2016、Windows Server 2012 R2
 
若要變更顯示在 sign\ 在頁面上的公司名稱，請使用下列 Windows PowerShell PowerShell cmdlet 和語法。 這個值預設設定來使用您在設定期間輸入的同盟服務顯示名稱中的值。  

![名稱變更](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> 您也可以使用 Windows PowerShell 整合指令碼環境 \(ISE\) 公司名稱的變更。 使用 Windows PowerShell ISE，您可以顯示 content Unicode\ 相容的環境中。 如需詳細資訊，請查看[引進 Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx)。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
  
