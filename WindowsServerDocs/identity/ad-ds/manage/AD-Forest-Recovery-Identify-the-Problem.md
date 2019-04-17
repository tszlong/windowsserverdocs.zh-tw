---
title: "廣告樹系復原-找出問題"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 1e9ede12dade0b5f1149eae784620520a8866e30
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="identify-the-problem"></a>找出問題

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2
  
 樹系失敗的問題出現時，例如事件登或其他監視方案，使用 Microsoft 支援服務來判斷失敗的原因，評估任何可能的救濟權利。  
 
## <a name="examples-of-forest-wide-failures"></a>樹系失敗的範例 
  
-   所有網域控制站已邏輯損壞或實體損壞的業務持續性不; 點例如，所有 AD DS 而定，應用程式都也將無法運作。  
  
-   系統管理員已洩露 Active Directory 環境。  
  
-   攻擊者刻意 — 或系統管理員的身分不小心-執行散播資料損壞跨樹系的指令碼。  
  
-   攻擊者刻意 — 或系統管理員的身分不小心-延伸包含惡意或衝突變更 Active Directory 架構。  
  
-   攻擊已安裝 Dc，惡意軟體所管理，以及您已從備份還原樹系建議由 Microsoft 支援服務。  
  
    > [!IMPORTANT]
    >  本文件不包含了解如何復原駭客入侵或受到危害的樹系安全性建議。 一般而言，最好是依照 Pass--Hash 降低技術強化環境。 如需詳細資訊，請查看[Mitigating Pass--Hash (PtH) 攻擊和其他認證竊取技術](https://www.microsoft.com/download/details.aspx?id=36036)。  
  
-   網域控制站皆可以複製他們複寫合作夥伴。  
  
-   找不到任何網域控制站 AD DS 進行的變更。  
  
-   無法安裝新的網域控制站在任何網域。  
  
## <a name="next-steps"></a>後續步驟
-   [廣告樹系修復必要條件](AD-Forest-Recovery-Prerequisties.md)  
-   [廣告樹系修復-設計自訂樹系復原計劃](AD-Forest-Recovery-Devising-a-Plan.md)  
- [廣告樹系修復-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
-   [廣告樹系修復-判斷如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [廣告樹系修復-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)  
-   [廣告樹系修復-常見問題集](AD-Forest-Recovery-FAQ.md)  
-   [廣告樹系修復-復原 Multidomain 樹系單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [廣告樹系復原與 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md) 
