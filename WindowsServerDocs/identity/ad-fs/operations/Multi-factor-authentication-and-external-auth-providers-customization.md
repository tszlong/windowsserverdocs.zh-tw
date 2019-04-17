---
title: "多因素驗證和外部驗證提供者的自訂項目"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 6d06c017601003e3b93df32f5fa50190ce54541d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>多因素驗證和外部驗證提供者的自訂項目 

>適用於：Windows Server 2016、Windows Server 2012 R2

AD FS 中, 提供 out\ of\-the\ 方塊要素支援。 例如，您可以設定 AD FS 使用的憑證驗證 built\ 中的第二個因數驗證以。 您也可以使用外部驗證提供者。 這種方法可以讓整合與其他服務，例如 Azure 多因素驗證，AD FS 或開發自己的提供者。 查看[方案快速入門：管理的風險 Multi\ 雙因素存取控制與](https://technet.microsoft.com/library/dn280937.aspx)如需詳細資訊，了解如何使用 AD FS 登記外部驗證提供者。  
  
我們建議外部驗證提供者使用 AD FS 提供撰寫驗證 UI 著定義類別。 您可以使用下列 cmdlet 匯出預設網頁主題，並檢查的使用者介面類別和定義著中的項目。 著可用的外部驗證提供者 sign\ 在使用者介面的開發中。  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
紅色，在外部驗證提供者反白顯示的 sign\ 的使用者介面的範例如下。 在使用者介面中 AD FS 著使用 UI 類別。  
  
![AD FS 和 MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
您撰寫新的自訂的驗證方法之前，我們建議您研究以了解製作需求 content AD FS 主題] 和 [樣式定義。  
  
-   自訂的驗證方法只撰寫頁面 sign\ 中 AD FS 不是完整頁面上的 HTML 區段。 您應該使用 AD FS 樣式定義的一致的外觀和的行為。  
  
![AD FS 和 MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   請注意，AD FS 管理員可以來自訂 AD FS 樣式。 . 我們不建議以硬您自己的樣式。 而是，我們建議使用 AD FS 樣式盡可能。  
  
-   Out\ of\-的方塊中，AD FS 樣式一種 left\ to\ 右下 \(LTR\) 樣式和一個 right\ to\ 左 \(RTL\) 撰寫。 系統管理員可以自訂兩，並提供透過 web 主題定義特定 language\ 樣式。 每個樣式已意見各自的三個區段︰  
  
    -   **主題樣式**\-這些樣式不應該無法使用。 這些樣式來定義主題所有網頁上的適用於對短片。 它們是由定義項目 ID 故意，因此不重複使用。  
  
    -   **常用的樣式**\-這些都是適用於您的樣式。  
  
    -   **表單規格樣式**\-這些都是針對不同尺寸規格樣式。 您應該會了解此一節，以確保您運作使用不同的尺寸規格，例如、手機與平板電腦。  
  
如需詳細資訊，請查看[方案快速入門：Multi\ 雙因素存取控制與管理的風險](https://technet.microsoft.com/library/dn280937.aspx)和[方案指南：管理與其他 Multi\ 雙因素驗證敏感的應用程式的風險](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx)。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md) 
