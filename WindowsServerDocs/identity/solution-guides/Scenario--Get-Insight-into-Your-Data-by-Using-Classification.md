---
ms.assetid: ad3f0480-99f7-428a-ab33-6d165a440840
title: 案例使用分類取得資料的深入解析
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a92f8791d5ceef3a8dbba4541588da1d48c4fbff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861111"
---
# <a name="scenario-get-insight-into-your-data-by-using-classification"></a>Scenario: Get Insight into Your Data by Using Classification

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

對大部分的組織來說，對資料及存放裝置資源的依賴變得越來越重要。 IT 系統管理員面對日益艱鉅的挑戰，不但要監看不斷擴充且愈加複雜的存放裝置基礎結構，同時還肩負了確保擁有權總成本維持在合理範圍內的責任。 管理存放裝置資源不再只限於資料的數量或可用性，現在還包含了強制執行公司政策，清楚知道存放裝置的使用方式，以便有效地利用它們及符合公司規範以降低風險。 檔案分類基礎結構透過自動化分類程序，讓您深入了解資料，以便更有效地管理資料。 檔案分類基礎結構提供下列分類方法：手動、透過程式設計以及自動。 這個主題著眼於自動檔案分類方法。  
  
## <a name="scenario-description"></a><a name="BKMK_OVER"></a>案例描述  
檔案分類基礎結構使用分類規則自動掃描檔案，並按照檔案內容來分類檔案。 分類內容是在 Active Directory 內集中定義，因此可以在組織中各個檔案伺服器之間共用。 您可以建立分類規則，掃描檔案以尋找標準字串或符合某個模式 (規則運算式) 的字串。 在檔案中找到設定的分類參數時，該檔案會依分類規則中的設定進行分類。 分類規則的一些範例包括：  
  
-   將包含「Contoso 機密」字串的任何檔案分類為具有高商業影響  
  
-   將包含至少 10 個身分證號碼的檔案分類為具有個人識別資訊  
  
當檔案被分類時，您可以使用檔案管理工作，在以特定方式分類的檔案中執行動作。 檔案管理工作的動作包括保護與檔案關聯的權限、讓檔案到期以及執行自訂動作 (例如張貼資訊到 Web 服務)。  
  
您可以在[規劃自動檔案分類](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955)中找到設定自動檔案分類的規劃資訊。  
  
您可以在[部署自動檔案&#40;分類示範步驟&#41;](Deploy-Automatic-File-Classification--Demonstration-Steps-.md)中找到如何自動分類檔案的步驟。  
  
## <a name="in-this-scenario"></a>在這個案例中  
此案例是動態存取控制案例的一部分。 如需動態存取控制的其他資訊，請參閱：  
  
-   [動態存取控制：案例總覽](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="practical-applications"></a><a name="BKMK_APP"></a>實際應用  
Windows Server 2012 中的檔案分類基礎結構可讓商務資料擁有者輕鬆分類和標籤資料，藉此提供動態存取控制。 儲存在集中存取原則中的分類資訊可讓您定義對企業很重要的資料類別存取原則。  
  
## <a name="features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>此案例中包含的功能  
下表列出這個案例中的功能，並說明它們如何支援這個案例。  
  
|功能|如何支援本案例|  
|-----------|---------------------------------|  
|[檔案伺服器 Resource Manager 總覽](https://technet.microsoft.com/library/hh831701.aspx)|檔案分類基礎結構是包含在檔案伺服器資源管理員的功能。|  
|[檔案和存放服務概觀](https://technet.microsoft.com/library/hh831487.aspx)|檔案伺服器資源管理員是包含在檔案服務伺服器角色的功能。|  
  


