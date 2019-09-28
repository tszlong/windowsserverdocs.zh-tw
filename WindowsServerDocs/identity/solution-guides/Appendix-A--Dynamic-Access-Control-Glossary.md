---
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: 附錄 A 動態存取控制詞彙
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 5508c3397039a1a70c07f1dc5f29e06bd02234a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357615"
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>附錄 A：動態存取控制詞彙

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

以下是動態存取控制案例中包含的詞彙和定義清單。  
  
|詞彙|定義|  
|--------|--------------|  
|自動分類|根據系統管理員設定的分類規則所決定的分類屬性所發生的分類。|  
|CAPID|集中存取原則識別碼。 此識別碼會參照特定的集中存取原則，並用來從檔案和資料夾的安全描述項參考原則。|  
|集中存取規則|包含條件和存取運算式的規則。|  
|集中存取原則|在 Active Directory 中撰寫及主控的原則。|  
|以宣告為基礎的存取控制|利用宣告來對資源進行存取控制決策的範例。|  
|分類|判斷資源分類屬性的程式，並將這些屬性指派給與資源相關聯的中繼資料。 另請參閱 REF AutomaticClassification \h \\ * MERGEFORMAT 自動分類、REF InheritedClassification \h \\ @ no__t-2 MERGEFORMAT 繼承的分類和 REF ManualClassification \h \\ @ no__t-4 MERGEFORMAT Manual分類.|  
|裝置宣告|與系統相關聯的宣告。  使用使用者宣告時，它會包含在嘗試存取資源的使用者權杖中。|  
|任意存取控制清單（DACL）|一種存取控制清單，可識別允許或拒絕存取安全性實體資源的信任者。 您可以自行修改資源擁有者。|  
|資源屬性|描述檔案的屬性（例如標籤），並使用自動分類或手動分類將檔案指派給檔案。 範例包含：敏感度、專案和保留期限。|  
|檔案伺服器資源管理員|Windows Server 作業系統中的一項功能，可讓您管理檔案伺服器上的資料夾配額、檔案檢測、存放裝置報告、檔案分類和檔案管理工作。|  
|資料夾屬性和標籤|描述資料夾並由系統管理員和資料夾擁有者手動指派的屬性和標籤。 這些屬性會將預設屬性值指派給這些資料夾內的檔案，例如，秘密或部門。|  
|群組原則|一組規則和原則，可控制 Active Directory 環境中使用者和電腦的工作環境。|  
|近乎即時的分類|在建立或修改檔案之後，很快就會執行的自動分類。|  
|近乎即時的檔案管理工作|很快就會執行的檔案管理工作（建立或修改檔案。 這些工作是由近乎即時的分類所觸發。|  
|組織單位 (OU)|代表組織內階層式邏輯結構的 Active Directory 容器。 這是套用群組原則設定的最小範圍。|  
|Secure 屬性|在特定時間點，授權執行時間可以信任的分類屬性是關於資源的有效判斷提示。 在宣告型存取控制中，指派給資源的安全屬性會被視為資源宣告。|  
|安全描述項|資料結構，包含與安全性實體資源（例如存取控制清單）相關聯的安全性資訊。|  
|安全描述項定義語言|描述安全描述項中的資訊做為文字字串的規格。|  
|預備原則|尚未生效的集中存取原則。|  
|系統存取控制清單（SACL）|存取控制清單，指定特定信任者在需要產生哪些審核記錄時的存取嘗試類型。|  
|使用者宣告|使用者安全性權杖中所提供使用者的屬性。 範例包含：部門、公司、專案和安全性淨空。  來自 Windows Server 2012 之前系統的使用者權杖中的資訊（例如使用者所屬的安全性群組）也可以被視為使用者宣告。 有些使用者宣告是透過 Active Directory 提供的，而有些則是以動態方式計算，例如使用者是否使用智慧卡登入。|  
|使用者 token|識別使用者的資料物件，以及與該使用者相關聯的使用者宣告和裝置宣告。 它是用來授權使用者存取資源。|  
  
## <a name="see-also"></a>另請參閱  
[動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  


