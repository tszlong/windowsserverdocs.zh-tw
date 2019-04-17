---
ms.assetid: 3ea48a72-20a2-4da4-84e4-26b5728513ce
title: "檔案計劃存取稽核"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 65e941f6741298da54186be10bd9c0add115ea14
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="plan-for-file-access-auditing"></a>檔案計劃存取稽核

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

此主題中的資訊解釋的安全性稽核引進 Windows Server 2012 和新的改進稽核考慮將動態存取控制您的企業中部署設定。 您要部署的實際稽核原則設定會在您的目標，其中可能包括法規、監視、法庭分析和疑難排解而定。  
  
> [!NOTE]  
> 詳細資訊，了解如何規劃及部署整體安全性稽核策略企業所述的[規劃和部署進階安全性稽核原則](https://go.microsoft.com/fwlink/?LinkID=191139)。 如需有關設定和部署安全性稽核原則的詳細資訊，請[進階安全性稽核原則 Step-by-Step 指南](https://go.microsoft.com/fwlink/?LinkID=191141)。  
  
下列安全性稽核功能在 Windows Server 2012 中的可以搭配動態存取控制延長整體安全性稽核策略。  
  
-   **運算式為基礎的稽核原則 」**。 動態存取控制可讓您使用運算式根據使用者、 電腦及資源宣告建立目標的稽核原則。 例如，您可以建立追蹤歸類為高商務影響員工高安全性距離不需要的檔案上所有讀取和寫入作業稽核原則。 直接的檔案或資料夾或透過群組原則集中製作運算式型稽核原則。 如需詳細資訊，請查看[群組原則，使用通用物件存取稽核](https://go.microsoft.com/fwlink/?LinkId=241498)。  
  
-   **從物件的稽核存取的其他資訊**。 檔案存取稽核不是以 Windows Server 2012 的新功能。 就地正確稽核原則，使用 Windows 和 Windows Server 作業系統產生稽核事件每次使用者存取檔案。 現有的檔案存取事件 4656 (4663） 包含屬性之檔案的存取的資訊。 事件登入篩選工具可以使用此資訊來協助您找出最相關稽核事件。 如需詳細資訊，請查看[稽核處理操作](https://technet.microsoft.com//library/dd772626(WS.10).aspx)和[稽核安全性帳號管理員]](https://go.microsoft.com/fwlink/?LinkId=241501)。  
  
-   **從使用者登入事件的詳細資訊**。 Windows 作業系統的正確稽核原則的位置產生稽核事件每次使用者登入電腦在本機或遠端。 在 Windows Server 2012 或 Windows 8，您也可以監視使用者的安全性權杖相關聯的使用者與裝置宣告。 範例包含 clearances 部門、 公司、 專案和安全性。事件 4626 包含這些使用者宣告和裝置宣告，可以利用相互關聯使用者登入以便事件篩選依據檔案屬性和使用者屬性物件存取事件事件稽核登入管理工具的相關資訊。 如需有關使用者登入稽核資訊，請查看[稽核登入](https://go.microsoft.com/fwlink/?LinkId=241502)。  
  
-   **修訂新類型的安全物件**。 修訂安全物件很重要下列案例中：  
  
    -   **修訂的中央存取原則和中央存取規則**。 中央存取原則和中央存取規則定義中央的原則，可用來控制重要的資源。 這些的任何變更可以直接影響的檔案存取權限授與對使用者在多部電腦上。 因此，修訂的中央存取原則和中央存取規則很重要的組織。 因為的中央存取原則和中央存取規則會儲存在 Active Directory Domain Services (AD DS)，您可以嘗試修改，例如變更 AD DS 任何其他安全物件的稽核稽核。 如需詳細資訊，請查看[稽核 Directory 服務存取]](https://technet.microsoft.com/library/dd941618(WS.10).aspx)。  
  
    -   **修訂定義理賠要求字典中的**。 宣告定義包含宣告名稱、 描述，以及可能值。 宣告定義的任何變更可能影響重要的資源的存取權限。 因此，以取得定義修訂很重要您的組織。 例如中央存取原則，中央存取規則宣告定義會儲存在 AD DS;因此，它們可以稽核等 AD DS 中的任何其他安全物件。 如需詳細資訊，請查看[稽核 Directory 服務存取]](https://technet.microsoft.com/library/dd941618(WS.10).aspx)。  
  
    -   **修訂檔案屬性**。 檔案屬性判斷的中央存取規則會套用至該檔案。 變更檔案屬性可以可能會影響檔案的存取限制。 因此，這很重要修訂檔案屬性。 您可以變更檔案屬性的任何電腦上設定的授權原則變更稽核原則。 如需詳細資訊，請查看[授權原則變更稽核](https://go.microsoft.com/fwlink/?LinkId=241504)和[檔案系統存取物件的稽核](https://go.microsoft.com/fwlink/?LinkId=241505)。 在 Windows Server 2012，事件 4911 會從其他授權原則變更事件處檔案屬性原則變更。  
  
    -   **Chang 追蹤與檔案關聯的中央存取原則。** 事件 4913 顯示安全性識別碼 (Sid) 的舊和新的中央存取原則。 每個中央存取原則也有可供使用此安全性識別字的使用者易記名稱。 如需詳細資訊，請查看[授權原則變更稽核](https://go.microsoft.com/fwlink/?LinkId=241504)。  
  
    -   **使用者和電腦屬性修訂**。 檔案，例如使用者與電腦物件的屬性，可能與這些屬性變更可能影響的使用者的能力來存取檔案。 因此，可以寶貴修訂使用者或電腦的屬性。 使用者與電腦物件會儲存在 AD DS;因此，您可以稽核變更他們屬性。 如需詳細資訊，請查看[DS 存取](https://go.microsoft.com/fwlink/?LinkId=241508)。  
  
-   **原則變更臨時**。 中央存取原則變更可能影響存取控制決策上所有的電腦執行的原則的位置。 鬆散原則無法權限授與其他比，並過於限制原則可能會產生協助工程師過多。 如此一來，它可以是非常寶貴之前，請先執行變更驗證變更的中央存取原則。 Windows Server 2012 目的，介紹 「 臨時。 」 的概念 臨時可讓使用者驗證其建議的原則變更之前，請先執行它們。 建議的原則部署以執行的原則，使用臨時原則，但分段的原則不確實授與或拒絕權限。 改為、 Windows Server 2012 登稽核事件 (4818) 的隨時存取檢查使用分段的原則的結果是不同的存取檢查使用執行的原則，結果。  
  


