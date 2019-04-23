---
ms.assetid: ad3f0480-99f7-428a-ab33-6d165a440840
title: 使用分類資料的案例中取得深入解析
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2e5248b5fd5e20b7436de9f796367c018d8ef16e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881479"
---
# <a name="scenario-get-insight-into-your-data-by-using-classification"></a>案例：使用分類深入了解您的資料

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

對大部分的組織來說，對資料及存放裝置資源的依賴變得越來越重要。 IT 系統管理員面對日益艱鉅的挑戰，不但要監看不斷擴充且愈加複雜的存放裝置基礎結構，同時還肩負了確保擁有權總成本維持在合理範圍內的責任。 管理存放裝置資源不再只限於資料的數量或可用性，現在還包含了強制執行公司政策，清楚知道存放裝置的使用方式，以便有效地利用它們及符合公司規範以降低風險。 檔案分類基礎結構透過自動化分類程序，讓您深入了解資料，以便更有效地管理資料。 檔案分類基礎結構提供下列分類方法：手動、透過程式設計以及自動。 這個主題著眼於自動檔案分類方法。  
  
## <a name="BKMK_OVER"></a>案例描述  
檔案分類基礎結構使用分類規則自動掃描檔案，並按照檔案內容來分類檔案。 分類內容是在 Active Directory 內集中定義，因此可以在組織中各個檔案伺服器之間共用。 您可以建立分類規則，掃描檔案以尋找標準字串或符合某個模式 (規則運算式) 的字串。 在檔案中找到設定的分類參數時，該檔案會依分類規則中的設定進行分類。 分類規則的一些範例包括：  
  
-   將包含 「 Contoso 公司機密 」 字串的檔案分類為具有高商業影響  
  
-   將包含至少 10 個身分證號碼的檔案分類為具有個人識別資訊  
  
當檔案被分類時，您可以使用檔案管理工作，在以特定方式分類的檔案中執行動作。 檔案管理工作的動作包括保護與檔案關聯的權限、讓檔案到期以及執行自訂動作 (例如張貼資訊到 Web 服務)。  
  
您可以在[規劃自動檔案分類](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955)中找到設定自動檔案分類的規劃資訊。  
  
您可以找到如何自動分類檔案中的步驟[部署自動檔案分類&#40;示範步驟&#41;](Deploy-Automatic-File-Classification--Demonstration-Steps-.md)。  
  
## <a name="in-this-scenario"></a>在這個案例中  
此案例是動態存取控制案例的一部分。 如需動態存取控制的其他資訊，請參閱：  
  
-   [動態存取控制：案例概觀](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_APP"></a>實際的應用程式  
Windows Server 2012 中的檔案分類基礎結構可藉由讓商業資料擁有者輕鬆分類和標籤資料為動態存取控制。 儲存在集中存取原則中的分類資訊可讓您定義對企業很重要的資料類別存取原則。  
  
## <a name="BKMK_NEW"></a>在此案例中所包含的功能  
下表列出這個案例中的功能，並說明它們如何支援這個案例。  
  
|功能|如何支援本案例|  
|-----------|---------------------------------|  
|[檔案伺服器資源管理員概觀](https://technet.microsoft.com/library/hh831701.aspx)|檔案分類基礎結構是包含在檔案伺服器資源管理員的功能。|  
|[檔案和存放服務概觀](https://technet.microsoft.com/library/hh831487.aspx)|檔案伺服器資源管理員是包含在檔案服務伺服器角色的功能。|  
  


