---
ms.assetid: ceb9ce18-5a94-4166-9edd-2685b81fc15f
title: "跨樹系部署宣告"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7d78258d8f1db9889b6d2db8c497780940ed35a1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="deploy-claims-across-forests"></a>跨樹系部署宣告

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 Windows Server 2012，宣告類型是用它已相關的物件聲明。 每個樹系的 Active Directory 定義宣告類型。 有許多案例安全性主體可能需要往返信任邊界，存取受信任的樹系的資源。 Windows Server 2012 中的跨樹系宣告轉換可讓您轉換輸出並輸入，以便宣告的音效卡且其接受信任和受信任的樹系往返樹系的宣告。 實際案例轉換索賠項目包括：  
  
-   信任的樹系可以使用理賠要求轉換防範權限提高為篩選宣告傳入的特定的值。  
  
    信任的樹系也可以發行索賠主體來自信任邊界受信任的樹系不支援或發出任何主張。  
  
-   信任的樹系可用來防止特定理賠要求類型與特定值宣告前往信任的樹系宣告轉換。  
  
-   您也可以使用理賠要求地圖不同轉換宣告信任和受信任的樹系之間的類型。 這可要將宣告類型、宣告值，或兩者。 不，您需要標準化樹系之前，您可以使用宣告間的資料。 一般化宣告信任和受信任的樹系之間減少 IT 成本。  
  
## <a name="claim-transformation-rules"></a>取得轉換規則  
轉換規則語言語法分為兩個主要部分單一的規則：條件聲明問題聲明一系列。 每個條件聲明有兩個子：理賠要求識別碼和條件。 問題隱私權聲明包含關鍵字、分隔字元，以及運算式的問題。 宣告識別碼變數，表示相符輸入的宣告選擇性開始條件聲明。 檢查運算式條件。 如果輸入的宣告不符合的條件，轉換引擎會忽略的問題聲明，並評估轉換規則針對下一步輸入的宣告。 如果所有的條件符合輸入的宣告，它會處理問題聲明。  
  
宣告規則語言的詳細資訊，請查看[宣告轉換規則語言](Claims-Transformation-Rules-Language.md)。  
  
## <a name="linking-claim-transformation-policies-to-forests"></a>宣告轉換原則連結到森林  
有兩個元件參與設定宣告轉換原則：取得轉換原則物件和轉換連結。 原則物件居住設定命名關聯的樹系，並包含對應宣告資訊。 連結指定哪些信任並受信任的樹系對應適用於。  
  
請務必以了解樹系是否信任或受信任的樹系因為這是基準連結轉換原則物件。 例如，受信任的樹系是樹系包含帳號需要存取。 信任的樹系是樹系包含您想要讓使用者存取權的資源。 宣告相同的安全性原則需要存取權的方向移動。 例如，adatum.com 樹系單向信任的樹系 contoso.com 時，宣告將會從流向 adatum.com contoso.com，可讓使用者從 adatum.com 存取 contoso.com 中的資源。  
  
根據預設，受信任的樹系允許通過，所有傳出宣告並信任的樹系卸除收到的所有傳入宣告。  
  
## <a name="in-this-scenario"></a>本案例中  
本案例可下列指導方針：  
  
-   [跨樹系與 #40; 示範步驟和 #41; 部署宣告](Deploy-Claims-Across-Forests--Demonstration-Steps-.md)  
  
-   [宣告轉換規則語言](Claims-Transformation-Rules-Language.md)  
  
## <a name="BKMK_NEW"></a>角色與包含在本案例中的功能  
下表列出的角色與本案例的功能，並告訴他們支援的方式。  
  
|角色/功能|它如何支援此案例|  
|-----------------|---------------------------------|  
|Active Directory Domain Services|在本案例中，您需要兩個 Active Directory 樹系雙向信任的設定。 您有兩個森林中的宣告。 您也可以設定中央存取原則上信任的樹系資源所在的位置。|  
|檔案與儲存空間服務的角色|在本案例中，資料分類會套用到檔案伺服器上的資源。 中央存取原則會套用到您想要權限授與使用者的資料夾。 轉換後宣告授與使用者存取資源根據會套用至該檔案伺服器上的資料夾的中央存取原則。|  
  


