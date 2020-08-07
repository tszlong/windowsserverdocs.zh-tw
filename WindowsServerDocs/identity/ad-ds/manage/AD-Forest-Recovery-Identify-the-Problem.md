---
title: AD 樹系復原 - 找出問題
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.openlocfilehash: 33a1febbdbe564873f8f7c0a4df474e933f09b92
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969895"
---
# <a name="identify-the-problem"></a>識別問題

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

當全樹系失敗的徵兆出現時（例如事件記錄檔或其他監視解決方案），請使用 Microsoft 支援服務來判斷失敗的原因，並評估任何可能的補救方式。

## <a name="examples-of-forest-wide-failures"></a>全樹系失敗的範例

- 所有 Dc 的邏輯都已損毀或實體損毀，而無法實現商務持續性;例如，相依于 AD DS 的所有商務應用程式都沒有功能性。
- Rogue 系統管理員已危害 Active Directory 環境。
- 攻擊者刻意（或系統管理員）不小心，會執行將資料損毀散佈到整個樹系的腳本。
- 攻擊者刻意（或系統管理員）不小心地擴充 Active Directory 架構，其中包含惡意或衝突的變更。
- 攻擊者已管理，以在 Dc 上安裝惡意軟體，而且您已 Microsoft 支援服務從備份復原樹系。

   > [!IMPORTANT]
   >  本檔並未涵蓋如何復原遭到駭客入侵或遭盜用之樹系的安全性建議。 一般來說，建議遵循傳遞雜湊緩和技術來強化環境。 如需詳細資訊，請參閱[降低傳遞雜湊 (PtH) 攻擊和其他認證竊取技術](https://www.microsoft.com/download/details.aspx?id=36036)。

- 所有 Dc 都無法與複寫協力電腦複寫。
- 無法在任何網域控制站上進行變更 AD DS。
- 新的 Dc 無法安裝在任何網域中。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原 - 先決條件](AD-Forest-Recovery-Prerequisties.md)
- [AD 樹系復原-設計自訂樹系復原計畫](AD-Forest-Recovery-Devising-a-Plan.md)
- [AD 樹系復原-識別問題](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 樹系復原-決定如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 樹系復原-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
- [AD 樹系復原-常見問題](AD-Forest-Recovery-FAQ.md)
- [AD 樹系復原-修復多網域樹系中的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)
- [AD 樹系復原-具有 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)
