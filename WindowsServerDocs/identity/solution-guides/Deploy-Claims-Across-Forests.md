---
ms.assetid: ceb9ce18-5a94-4166-9edd-2685b81fc15f
title: 跨樹系部署宣告
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7d78258d8f1db9889b6d2db8c497780940ed35a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890649"
---
# <a name="deploy-claims-across-forests"></a>跨樹系部署宣告

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在 Windows Server 2012 中，宣告類型會是與它相關聯的物件相關的判斷提示。 宣告類型會依據 Active Directory 中的每個樹系而定義。 在許多情況下，安全性主體都可能需要周遊信任邊界，以存取信任樹系中的資源。 Windows Server 2012 中的跨樹系宣告轉換可讓您轉換周遊樹系，以便宣告會辨識和接受信任和受信任的樹系中的輸出和輸入宣告。 以下是宣告轉換的部分實際案例：  
  
-   信任樹系可藉由篩選含有特定值的傳入宣告，使用宣告轉換來防止權限提高。  
  
    如果受信任的樹系不支援或未發出任何宣告，信任樹系也可以針對跨信任邊界的主體發出宣告。  
  
-   受信任樹系可以使用宣告轉換，防止某些宣告類型和含有特定值的宣告送出到信任樹系中。  
  
-   您也可以使用宣告轉換，對應信任和受信任的樹系之間的不同宣告類型。 這可以用來將宣告類型、宣告值或兩者一般化。 若未這麼做，您將需要標準化樹系之間的資料，才能使用這些宣告。 將信任和受信任的樹系之間的宣告一般化，可以降低 IT 成本。  
  
## <a name="claim-transformation-rules"></a>宣告轉換規則  
轉換規則語言語法將單一規則分成兩個主要部分：一系列的條件陳述式和問題陳述式。 每個條件陳述式有兩個子元件：宣告識別碼和條件。 問題陳述式包含關鍵字、分隔符號和問題運算式。 條件陳述式可選擇性地以宣告識別碼變數開頭，用以表示相符的輸入宣告。 條件會檢查運算式。 如果輸入宣告不符合條件，轉換引擎將會忽略問題陳述式，並根據轉換規則評估下一個輸入宣告。 輸入宣告若符合所有條件，則會處理問題陳述式。  
  
如需宣告規則語言的詳細資訊，請參閱 [Claims Transformation Rules Language](Claims-Transformation-Rules-Language.md)。  
  
## <a name="linking-claim-transformation-policies-to-forests"></a>將宣告轉換原則連結至樹系  
設定宣告轉換原則時牽涉到兩個元件：宣告轉換原則物件和轉換連結。 原則物件存在於樹系中的設定命名內容，且包含宣告的對應資訊。 連結會指定對應適用於哪些信任和受信任的樹系。  
  
請務必了解樹系是信任樹系還是或受信任的樹系，因為這是連結轉換原則物件的基礎。 例如，假設受信任的樹系是包含需要存取權之使用者帳戶的樹系。 信任的樹系則是包含您要將其存取權給予使用者之資源的樹系。 宣告周遊的方向會與需要存取權的安全性主體相同。 例如，若有從 contoso.com 樹系到 adatum.com 樹系的單向信任存在，宣告將會從 adatum.com 流向 contoso.com，讓 adatum.com 中的使用者能夠存取 contoso.com 中的資源。  
  
根據預設，受信任的樹系會讓所有的傳出宣告都可傳遞，而信任的樹系會捨棄它所接收的所有傳入宣告。  
  
## <a name="in-this-scenario"></a>在這個案例中  
以下是可供此案例使用的指導方針：  
  
-   [跨樹系部署宣告&#40;示範步驟&#41;](Deploy-Claims-Across-Forests--Demonstration-Steps-.md)  
  
-   [宣告轉換規則語言](Claims-Transformation-Rules-Language.md)  
  
## <a name="BKMK_NEW"></a>在此案例包含角色與功能  
下表列出這個案例中的角色與功能，並說明它們如何支援這個案例。  
  
|角色/功能|如何支援本案例|  
|-----------------|---------------------------------|  
|Active Directory Domain Services|在此案例中，您必須設定兩個具有雙向信任的 Active Directory 樹系。 您在兩個樹系中都有宣告。 您也必須在資源所在的信任樹系上設定集中存取原則。|  
|檔案和存放服務角色|在此案例中，資料分類會套用到檔案伺服器上的資源。 集中存取原則會套用到您要授與使用者存取權的資料夾。 在轉換之後，宣告會根據檔案伺服器上的資料夾所套用的集中存取原則，為使用者授與資源的存取權。|  
  


