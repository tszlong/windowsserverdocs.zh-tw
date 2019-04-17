---
title: "必要條件規劃 Active Directory 森林復原"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adfs
ms.openlocfilehash: 672e1f4d0de9bfb2cbe291c5ed715814c8acacd0
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2017
---
#<a name="active-directory-forest-recovery-prerequisites"></a>Active Directory 樹系修復必要條件

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

下列文件商討您應該先設計的樹系修復計畫，或嘗試復原熟悉的必要條件。

## <a name="assumptions-for-using-this-guide"></a>使用本指南假設 

1.  使用 Microsoft 支援人員過和：

    - 判斷樹系失敗的原因。 本指南不建議失敗的原因或建議任何程序，以避免失敗。  
    - 評估任何可能的救濟權利。  
    - 太平洋、 與 Microsoft 支援服務，諮詢在該還原整個樹系狀態失敗之前是復原從失敗的最佳方式。 很多時候，森林修復應該最後一個選項。  </br></br>

2. 您有依照 Microsoft 最佳建議使用 Active Directory – 整合網域名稱系統 」 (DNS)。 具體而言，應該是每個 Active Directory domain Active Directory – 整合 DNS 區域。 如果這不是如此，您仍然可以使用基本本指南原則來執行復原樹系。 不過，您將需要需要特定措施 DNS 復原根據您自己的環境。 如需有關如何使用 Active Directory – 整合 DNS 的詳細資訊，請查看[建立設計 DNS 基礎架構](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)。
3. 本指南做樹系復原一般的輔助，雖然涵蓋可能不是所有的案例。 例如，開始使用 Windows Server 2008，還有 Server Core 版本，也就是完整版本的 Windows Server，但不完整 GUI。 雖然它自然也是可以復原組成只執行 Server Core 網域控制站的樹系，本指南有任何詳細的指示。 不過，依據以下討論指導方針您將無法自行設計所需的命令列動作。  
 
>![!NOTE]
> 本指南的目標是復原樹系和維護或還原完整 DNS 功能，但修復可能會導致變更的組態失敗之前 DNS 設定。 樹系復原之後，您可以回復到原始 DNS 設定。 本指南建議事項執行告訴您如何設定執行公司命名空間的其他部分的名稱解析 DNS 伺服器，其中有不會儲存在 AD DS DNS 區域。  

## <a name="concepts-for-using-this-guide"></a>使用本指南概念
 規劃區域的 Active Directory 樹系復原您開始之前，您應該熟悉動作：  
  
-   Active Directory 的基本概念  
  
-   （也稱為彈性的單一主機操作或 FSMO） 操作主機角色的重要性。 這些角色包含下列類型：  
  
    -   架構主機  
  
    -   網域命名主機  
  
    -   相關 ID (RID) 主機  
  
    -   主要網域控制站 (PDC) 模擬器主機  
  
    -   基礎結構主機  
  
 此外，您應該已經備份與還原 AD DS 和 SYSVOL 定期測試環境中。 如需詳細資訊，請查看[「 資料備份系統狀態](AD-Forest-Recovery-Procedures.md)和[執行未授權的 Active Directory Domain Services 還原](AD-Forest-Recovery-Procedures.md)。

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
