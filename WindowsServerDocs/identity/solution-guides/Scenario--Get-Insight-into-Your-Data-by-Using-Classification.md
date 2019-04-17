---
ms.assetid: ad3f0480-99f7-428a-ab33-6d165a440840
title: "案例取得深入了解您使用分類的資料"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2e5248b5fd5e20b7436de9f796367c018d8ef16e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-get-insight-into-your-data-by-using-classification"></a>案例︰ 使用分類取得深入了解您的資料

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

傳送的資料與儲存空間資源仍持續增加中重要性大部分的組織。 IT 系統管理員所面臨越來越要求的作業更大、更複雜的儲存空間基礎結構，同時負責確保責任使用該總成本擁有維護合理的層級。 管理儲存空間資源是專業音量或可用性資料。它也是關於執行公司原則，並了解如何為了讓有效率使用量和降低風險 compliance 耗用儲存空間。 檔案分類基礎結構提供深入了解您的資料，自動執行分類程序，以便您可以更有效率地管理您的資料。 使用檔案分類基礎結構分類下列方法可：手動，以程式設計方式，並自動。 本主題焦某自動檔案分類方法。  
  
## <a name="BKMK_OVER"></a>案例描述  
檔案分類基礎結構會自動掃描的檔案，並根據檔案到它們分類使用分類規則。 分類屬性定義集中 Active Directory 中，這些定義可以在組織中共用檔案伺服器上。 您可以建立分類規則掃描標準字串或字串符合模式（運算式）的檔案。 當您找不到檔案中的設定的分類參數時，該檔案會被歸類為分類規則中設定。 分類規則的一些範例包括：  
  
-   為高企業影響可包含「Contoso 機密「字串的檔案  
  
-   可包含個人資訊，以最少 10 身分證號碼的檔案  
  
當歸類檔案時，您可以使用檔案來執行動作的任何檔案管理工作歸類特定的方式。 管理工作檔案中的動作，包括保護過期檔案，並執行自訂動作（例如張貼 web 服務資訊）的檔案相關聯的權限。  
  
您可以找到計劃的設定中的自動檔案分類資訊[計劃自動檔案分類的](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955)。  
  
您可以找到步驟以了解如何自動可中的檔案[部署自動檔案分類與 #40; 示範步驟和 #41;](Deploy-Automatic-File-Classification--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>本案例中  
本案例是動態存取控制案例的一部分。 如需關於動態存取控制的詳細資訊，請查看：  
  
-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_APP"></a>實用的應用程式  
Windows Server 2012 中的檔案分類基礎結構提供動態存取控制讓輕鬆地可與標籤資料企業資料擁有者。 儲存在中央存取原則分類資訊可讓您定義的關鍵企業資料類別存取原則。  
  
## <a name="BKMK_NEW"></a>本案例中所包含的功能  
下表列出的功能是此案例，並告訴他們支援的方式。  
  
|功能|它如何支援此案例|  
|-----------|---------------------------------|  
|[檔案伺服器資源管理員概觀](https://technet.microsoft.com/library/hh831701.aspx)|檔案分類基礎結構是隨附檔案伺服器資源管理員] 中的功能。|  
|[檔案和儲存服務概觀](https://technet.microsoft.com/library/hh831487.aspx)|檔案伺服器資源管理員是隨附檔案的服務伺服器角色功能。|  
  


