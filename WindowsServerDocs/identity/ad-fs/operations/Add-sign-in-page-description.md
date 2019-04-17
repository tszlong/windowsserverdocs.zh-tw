---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: "新增 sign\\ 在頁面上的描述"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 128a4ffd8d4b9dfcfe5f0e8e2df8a0e1505cbb24
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="add-sign-in-page-description"></a>新增 sign\ 在頁面上的描述

>適用於：Windows Server 2016、Windows Server 2012 R2

## <a name="to-add-sign-in-page-description"></a>若要新增 sign\ 在頁面上的描述  
若要 sign\ 中頁面需要新增描述 sign\ 在頁面上，使用下列的 Windows PowerShell PowerShell cmdlet 和語法。  

![在 [描述新增登入](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> 適用於字串`SignInPageDescriptionText`參數支援兩個純真 HTML 標記與不用。 因此，您可以也執行下列 cmdlet 不使用&lt;p&gt;標籤。  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

自訂 sign\ 頁面會之後，自訂的優先順序。因此，您應該自訂為所有要支援的語言。 所有自訂的 content 參數的地區設定。 當您設定當地語系化的 content 時，它應該設定無 country\ 的地區設定第一次，，例如「en-us」，例如設定國家/地區和 region\ 特定地區之前「en\-我們」。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
