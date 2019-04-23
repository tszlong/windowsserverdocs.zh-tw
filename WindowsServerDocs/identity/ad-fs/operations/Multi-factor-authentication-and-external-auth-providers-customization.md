---
title: 多重要素驗證及外部驗證提供者自訂
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 6d06c017601003e3b93df32f5fa50190ce54541d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864799"
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>多重要素驗證及外部驗證提供者自訂 

>適用於：Windows Server 2016, Windows Server 2012 R2

在 AD FS 中，支援多重要素驗證提供\-的\-\- 方塊中。 例如，您可以設定 AD FS，以使用內建\-中作為第二個因素驗證的憑證驗證。 您也可以使用外部驗證提供者。 這種方法可以讓 AD FS，以整合其他服務，例如 Azure multi-factor Authentication，或您可以開發自己的提供者。 請參閱[解決方案指南：使用多個管理風險\-因素存取控制](https://technet.microsoft.com/library/dn280937.aspx)如需有關如何註冊使用 AD FS 的外部驗證提供者。  
  
建議外部驗證提供者使用 AD FS 提供以編寫驗證 UI.css 檔案中所定義的類別。 您可以使用下列 Cmdlet 來匯出預設網頁佈景主題，並查看使用者介面類別和.css 檔案中定義的元素。 .Css 檔案可以用於開發的正負號\-外部驗證提供者的使用者介面中。  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
以下是範例的正負號\-在使用者介面，這以紅色醒目提示，由外部驗證提供者。 使用者介面會使用 AD FS.css 檔案中的 UI 類別。  
  
![AD FS 和 MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
您撰寫新的自訂驗證方法之前，我們建議您先研究 AD FS 佈景主題和樣式定義，以了解編寫內容的需求。  
  
-   自訂驗證方法只會編寫在 AD FS 登上的 HTML 片段\-頁面，並不是整個頁面中。 您應該使用 AD FS 的樣式定義，以取得一致的外觀和行為。  
  
![AD FS 和 MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   請注意，AD FS 系統管理員可以自訂 AD FS 樣式。 . 不建議以硬式編碼的方式自行編寫樣式。 相反地，我們建議使用 AD FS 盡可能的樣式。  
  
-   Out\-的\- 方塊中，AD FS 樣式由編寫與一個從左\-要\-右\(LTR\)樣式與一個權限\-來\-左\(RTL\). 系統管理員可以自訂兩者，而且可以提供語言\-透過網頁佈景主題定義特定的樣式。 每個樣式表有三個區段，並有各自的註解：  
  
    -   **佈景主題樣式**\-不應該也不能使用這些樣式。 這些樣式是用來定義所有頁面的佈景主題。 它們是刻意由元素 ID 所定義，以避免被重複使用。  
  
    -   **常用樣式**\-這些是應用於內容的樣式。  
  
    -   **尺寸樣式**\-這些是供不同尺寸的樣式。 您應該了解此區段，以確定內容可搭配不同的尺寸，例如手機和平板電腦。  
  
如需詳細資訊，請參閱[解決方案指南：使用多個管理風險\-因素存取控制](https://technet.microsoft.com/library/dn280937.aspx)和[解決方案指南：透過其他多管理風險\-因素 Authentication for Sensitive Applications](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx)。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md) 
