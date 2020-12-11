---
description: 深入瞭解：附錄 A：動態存取控制詞彙
ms.assetid: 7f6b27e5-dc55-4ffc-8e76-6d57e65a870b
title: 附錄 A 動態存取控制詞彙
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c8ae5d1cd5f73c095d290e99d4dc9b72babf4d2f
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047656"
---
# <a name="appendix-a-dynamic-access-control-glossary"></a>附錄 A︰動態存取控制詞彙

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

以下是動態存取控制案例中所包含之詞彙和定義的清單。

|詞彙|定義|
|--------|--------------|
|自動分類|根據由系統管理員設定的分類規則所決定的分類屬性所發生的分類。|
|CAPID|集中存取原則識別碼。 此識別碼會參考特定的集中存取原則，並且用來從檔案和資料夾的安全描述項參考原則。|
|集中存取規則|包含條件和存取運算式的規則。|
|集中存取原則|在 Active Directory 中撰寫和主控的原則。|
|宣告型存取控制|利用宣告來對資源進行存取控制決策的典範。|
|分類|判斷資源的分類屬性，並將這些屬性指派給與資源相關聯之中繼資料的程式。 另請參閱 REF AutomaticClassification \h \\ * MERGEFORMAT 自動分類、Ref InheritedClassification \H \\ \* MERGEFORMAT 繼承的分類，以及 ref ManualClassification \h \\ \* MERGEFORMAT 手動分類。|
|裝置宣告|與系統相關聯的宣告。  使用使用者宣告時，會將它包含在嘗試存取資源的使用者權杖中。|
|判別存取控制清單 (DACL)|此存取控制清單會識別被允許或拒絕存取安全性實體資源的信任者。 您可以自行判斷資源擁有者來修改它。|
|資源屬性|屬性 (，例如描述檔案的標籤) ，並使用自動分類或手動分類將其指派給檔案。 範例包括：敏感度、專案和保留期限。|
|File Server Resource Manager|Windows Server 作業系統中的一項功能，可在檔案伺服器上提供資料夾配額、檔案檢測、存放裝置報告、檔案分類和檔案管理作業的管理。|
|資料夾屬性和標籤|描述資料夾並由系統管理員和資料夾擁有者手動指派的屬性和標籤。 這些屬性會將預設屬性值指派給這些資料夾內的檔案，例如，秘密或部門。|
|群組原則|一組規則和原則，可控制 Active Directory 環境中使用者和電腦的工作環境。|
|近乎即時的分類|在檔案建立或修改之後不久就會執行的自動分類。|
|近乎即時的檔案管理工作|檔案管理在 (檔案建立或修改之後不久就會執行的工作。 這些工作是由近乎即時的分類所觸發。|
|組織單位 (OU)|Active Directory 的容器，代表組織內的階層式邏輯結構。 它是套用群組原則設定的最小範圍。|
|安全屬性|授權執行時間可以信任的分類屬性，其為特定時間點上有關資源的有效判斷提示。 在宣告型存取控制中，指派給資源的安全屬性會被視為資源宣告。|
|安全性描述元|一種資料結構，其中包含與安全性實體相關聯的安全性資訊，例如存取控制清單。|
|安全描述項定義語言|描述安全描述項中的資訊做為文字字串的規格。|
|預備原則|尚未生效的集中存取原則。|
|系統存取控制清單 (SACL)|存取控制清單，指定需要產生審核記錄之特定受信者的存取嘗試類型。|
|使用者宣告|使用者安全性權杖中提供的使用者屬性。 範例包括：部門、公司、專案和安全性淨空。  在 Windows Server 2012 之前的系統（例如，使用者所屬的安全性群組）中，使用者權杖中的資訊也可視為使用者宣告。 某些使用者宣告是透過 Active Directory 提供，有些則是以動態方式計算，例如使用者是否以智慧卡登入。|
|使用者權杖|識別使用者的資料物件，以及與該使用者相關聯的使用者宣告和裝置宣告。 它是用來授權使用者存取資源。|

## <a name="see-also"></a>另請參閱
[動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)



