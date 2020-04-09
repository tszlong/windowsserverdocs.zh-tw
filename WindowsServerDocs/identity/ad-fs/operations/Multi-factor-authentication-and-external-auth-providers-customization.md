---
title: 多重要素驗證和外部驗證提供者自訂
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 8252244738d59f11a07c3bebadbbf2a5f4818845
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816231"
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>多重要素驗證和外部驗證提供者自訂 



在 AD FS 中，會提供多因素驗證的支援\-\-[\-] 方塊中。 例如，您可以將 AD FS 設定為使用憑證驗證中的內建\-做為第二個要素驗證。 您也可以使用外部驗證提供者。 這種方法可讓 AD FS 與其他服務整合，例如 Azure 多重要素驗證，或者您也可以開發自己的提供者。 如需如何使用 AD FS 來註冊外部驗證提供者的詳細資訊，請參閱[解決方案指南：透過多個\-因素管理風險存取控制](https://technet.microsoft.com/library/dn280937.aspx)。  
  
我們建議外部驗證提供者使用 .css 檔案中所定義的類別，AD FS 提供此檔案來撰寫驗證 UI。 您可以使用下列 Cmdlet 來匯出預設網頁佈景主題，並查看使用者介面類別和.css 檔案中定義的元素。 在外部驗證提供者的使用者介面中，您可以使用 .css 檔案來開發正負號\-。  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
以下是使用者介面中的 sign\-的範例，它會由外部驗證提供者以紅色反白顯示。 使用者介面會使用 AD FS .css 檔案中的 UI 類別。  
  
![AD FS 和 MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
在您撰寫新的自訂驗證方法之前，建議您先研究 AD FS 主題和樣式定義，以瞭解內容撰寫需求。  
  
-   自訂驗證方法只會在頁面的 AD FS sign\-上撰寫 HTML 區段，而不是整個頁面。 您應該使用 AD FS 的樣式定義來取得一致的外觀和行為。  
  
![AD FS 和 MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   請注意，AD FS 系統管理員可以自訂 AD FS 樣式。 。 不建議以硬式編碼的方式自行編寫樣式。 相反地，我們建議盡可能使用 AD FS 的樣式。  
  
-   Out\-方塊的\-，AD FS 樣式是以一個左方\-來撰寫，\-LTR \(的樣式，另一個 right\)\-left\-RTL \(。\) 系統管理員可以自訂這兩者，並可透過 web 主題定義提供語言\-特定的樣式。 每個樣式表有三個區段，並有各自的註解：  
  
    -   \- 這些樣式不應和無法使用的**主題樣式**。 這些樣式是用來定義所有頁面的佈景主題。 它們是刻意由元素 ID 所定義，以避免被重複使用。  
  
    -   **通用樣式**\- 這些是應該用於內容的樣式。  
  
    -   **外型規格樣式**\- 這些是不同外型規格的樣式。 您應該了解此區段，以確定內容可搭配不同的尺寸，例如手機和平板電腦。  
  
如需其他資訊，請參閱[解決方案指南：使用多個\-因素管理風險存取控制](https://technet.microsoft.com/library/dn280937.aspx)和[解決方案指南：透過其他多\-要素驗證管理機密應用程式的風險](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx)。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md) 
