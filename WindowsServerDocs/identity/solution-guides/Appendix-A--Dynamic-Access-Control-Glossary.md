---
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: "A 附錄動態存取控制詞彙"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2044355812e95b9a5bfe90e33257f11ce78cf93
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>答附錄動態存取控制詞彙

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

以下是清單中的條款及動態存取控制案例中所包含定義。  
  
|詞彙|解析度|  
|--------|--------------|  
|自動分類|就會發生的分類根據是由系統管理員的身分設定分類規則分類屬性。|  
|CAPID|中央存取原則 id。 這個 ID 參考特定的中央存取原則，並用於的檔案和資料夾的電源參考原則。|  
|中央存取規則|包含條件和存取運算式規則。|  
|中央存取原則|設計與裝載 Active Directory 中的原則。|  
|宣告型存取控制|利用範例宣告做出存取控制資源。|  
|分類|判斷分類的資源屬性，並將這些屬性指派給資源相關聯的中繼資料的程序。 也請參考 AutomaticClassification \h \\ * 原自動分類、 參考 InheritedClassification \h \\\ * 原繼承分類，以及參考 ManualClassification \h \\\ * 原手動分類。|  
|裝置宣告|系統相關聯的理賠要求。  使用者宣告，使用它包含嘗試存取資源的使用者權杖中。|  
|任意存取控制清單 (DACL)|辨識允許或無法存取安全資源信任者存取控制清單。 您可以修改資源擁有者自行。|  
|資源屬性|屬性 （例如標籤），描述檔案，使用自動分類或手動分類指派給檔案。 範例： 敏感度、 專案和保留時間。|  
|檔案伺服器資源管理員|Windows Server 作業系統的提供資料夾配額、 檔案檢測、 報告儲存空間、 檔案分類和檔案管理工作檔案的伺服器上的管理功能。|  
|資料夾屬性和標籤|屬性，並描述資料夾，並手動由系統管理員 」 及 「 資料夾擁有者指派標籤。 這些屬性指派預設屬性的值的檔案，例如加密或部門這些資料夾中。|  
|群組原則|一組規則的原則，控制項的 Active Directory 環境中使用者和電腦正常運作的環境。|  
|即時分類附近|自動分類的檔案會建立或修改之後，很快就會執行。|  
|附近即時檔案管理工作|管理工作稍後後所執行的檔案 （的檔案會建立，或修改。 這些工作是不久即時分類觸發。|  
|單位 （組織單位）|Active Directory 容器代表階層邏輯結構，在組織中。 這是最小領域的群組原則設定的套用。|  
|安全屬性|授權執行階段可以將特定的時間點的有效聲明信任分類屬性。 宣告為基礎的存取控制，請在安全的已指派給資源屬性會被視為資源理賠要求。|  
|電源|資料結構包含相關聯的安全的資源，例如存取控制清單的安全性資訊。|  
|安全性描述定義語言|告訴您的資訊中電源字串規格。|  
|原則階段|尚不作用中的中央存取原則。|  
|系統存取控制清單 (SACL)|存取控制清單指定稽核記錄需要被轉換特定信任者，嘗試存取的類型。|  
|使用者宣告|使用者的使用者的安全性權杖中提供的屬性。 範例： 距離部門、 公司、 專案和安全性。  從系統之前 Windows Server 2012，例如使用者是部分安全性群組使用者權杖中的資訊可也視為使用者主張。 部分使用者宣告提供 Active Directory 透過與其他人計算動態，例如使用者登入智慧卡。|  
|使用者權杖|辨識使用者的使用者宣告和裝置宣告使用者相關聯的資料物件。 它使用授權的使用者存取資源。|  
  
## <a name="see-also"></a>也了  
[動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  


