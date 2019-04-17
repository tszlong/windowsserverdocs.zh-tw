---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: "規劃通用伺服器位置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c32393640e929adf9681f511ed3707b85a3784db
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="planning-global-catalog-server-placement"></a>規劃通用伺服器位置

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

通用位置需要規劃以外，如果您有單一網域樹系。 在單一網域樹系設定所有網域控制站伺服器通用為。 每個網域控制站森林中儲存的唯一的網域 directory 磁碟分割，因為設定為通用伺服器的每個網域控制站不需要任何額外的磁碟空間，CPU 使用率或複寫傳輸。 在單一網域樹系所有網域控制站都做為 virtual 通用伺服器。是的他們可以所有回應任何驗證或服務要求。 這個條件特殊單一網域森林是設計。 驗證要求不需要洽詢往來通用伺服器像時有多個網域，使用者可以的不同網域中的通用群組成員。 不過，指定的伺服器通用為網域控制站可以回應通用查詢 3268 通用連接埠。 若要簡化管理在本案例中的，以確保一致回應指定所有網域控制站伺服器通用不相關的網域控制站可以回應通用查詢問題。 具體而言，使用者可以使用 Start\Search\For 連絡人或尋找印表機或展開通用群組，請要求這些前往只通用。  
  
在 [多重網域樹系通用伺服器幫助使用者登入要求樹系搜尋。 下圖顯示如何判斷哪一個位置需要通用伺服器。  
  
![規劃 gc 位置](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)  
  
在大部分案例中，建議您安裝新的網域控制站時包含通用。 適用於下列例外：  
  
-   有限的頻寬：在網站遠端寬區域 (wan) 和之間的連結遠端網站中樞網站限制，如果您可以使用通用群組成員資格快取中遠端網站登入需要，網站中的使用者。  
  
-   基礎結構操作主要相容的角色：不要將通用放網域控制站裝載基礎結構作業主角網域中的，否則所有的網域控制站網域中的通用伺服器，或樹系只有一個網域。  
  
## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>新增通用伺服器根據應用程式需求  
某些應用程式，例如 Microsoft Exchange、訊息 (也稱為 MSMQ)，以及應用程式使用 DCOM 請勿提供適當回應透過潛在 WAN 連結，因此必須可用性通用基礎結構提供查詢低延遲。 判斷位置或的位置是否需要 Microsoft Exchange Server 所執行的任何應用程式，不佳慢速 WAN 連結。 如果您的位置包含 WAN 的連結，不提供回應適當的應用程式，您必須通用伺服器置於的位置，以減少查詢延遲。  
  
> [!NOTE]  
> Read-only 網域控制站 (Rodc) 可以順利升級至通用伺服器狀態。 不過，某些 directory 功能的應用程式不支援 RODC 為通用伺服器。 例如，Microsoft Exchange Server 的任何版本不使用 Rodc。 不過，Microsoft Exchange Server 的運作方式包含 Rodc 的環境中，只要有寫入網域控制站可供使用。 Exchange Server 2007 有效地忽略 Rodc。 Exchange Server 2003 也會 Rodc 預設條件 Exchange 元件自動偵測可用的網域控制站在略過。 讓您知道唯讀 directory 伺服器不做 Exchange Server 2003 任何變更。 因此，試著強迫 Exchange Server 2003 服務與管理工具，可使用 Rodc 可能會造成無法預期的行為。  
  
## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>新增使用者大量通用的伺服器  
通用伺服器置於包含超過 100 使用者降低塞車的網路 WAN 連結，並避免生產力遺失 WAN 連結失敗在所有位置。  
  
## <a name="using-highly-available-bandwidth"></a>使用高頻寬  
您不需要通用將位置需要伺服器通用應用程式，不包含，包括少於 100 的使用者，並連接到另一個位置，包括通用伺服器 WAN 連結，為 100%適用於 Active Directory Domain Services (AD DS)。 若是如此，使用者可以存取通用伺服器 WAN 連結。  
  
使用者漫遊需要每次登入時第一次的任何位置時，請連絡的通用伺服器。 如果無法接受 WAN 連結登入時，將通用大量漫遊使用者所造訪的位置。  
  
## <a name="enabling-universal-group-membership-caching"></a>讓通用群組成員資格快取  
位置，包括小於 100 使用者和不包含大量漫遊使用者或需要伺服器通用應用程式，您可以部署網域控制站的正在執行 Windows Server 2008 以及通用群組成員資格快取。 確保通用伺服器的快取通用群組成員資格支援，可重新整理萬用群組資訊快取中的網域控制站的多個複寫躍點。 如何通用群組快取的運作方式的相關資訊，會看到全球 Catalog 運作 ([https://go.microsoft.com/fwlink/?LinkId=107063](https://go.microsoft.com/fwlink/?LinkId=107063))。  
  
為協助您在擬想要放置通用伺服器及網域控制站的群組通用將支援試算表，查看工作協助工具的 Windows Server 2003 部署套件 ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558))，下載 < DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>, and open Domain 控制器位置 (DSSTOPO_4.doc)。 查看您要部署的樹系根網域和區域網域時，將通用伺服器位置的相關資訊。  
  


