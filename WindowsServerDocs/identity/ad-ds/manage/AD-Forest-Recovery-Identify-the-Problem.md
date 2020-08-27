---
title: AD 樹系復原 - 找出問題
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.openlocfilehash: fc285fb355b6539aac56c7c410de1e1f7da48f00
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939658"
---
# <a name="identify-the-problem"></a>識別問題

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

當樹系失敗的徵兆出現時（例如在事件記錄檔或其他監視解決方案中），請使用 Microsoft 支援服務來判斷失敗的原因，並評估任何可能的補救方式。

## <a name="examples-of-forest-wide-failures"></a>全樹系失敗的範例

- 所有的 Dc 都有邏輯損毀，或實體損毀到不可能發生商務持續性的點;例如，所有相依于 AD DS 的商務應用程式都無法正常操作。
- Rogue 系統管理員已入侵 Active Directory 環境。
- 攻擊者刻意（或系統管理員不小心）執行可將資料損毀分散到整個樹系的腳本。
- 攻擊者刻意（或系統管理員）不小心將 Active Directory 架構延伸至惡意或衝突的變更。
- 攻擊者的管理是在 Dc 上安裝惡意軟體，而且您 Microsoft 支援服務被建議從備份復原樹系。

   > [!IMPORTANT]
   >  這份檔並未涵蓋有關如何復原遭到駭客入侵或遭入侵的樹系的安全性建議。 一般情況下，建議遵循傳遞雜湊緩和技術來強化環境。 如需詳細資訊，請參閱 [減輕傳遞雜湊 (PtH) 攻擊和其他認證竊取技術](https://www.microsoft.com/download/details.aspx?id=36036)。

- 沒有任何 Dc 可以與其複寫協力電腦複寫。
- 無法在任何網域控制站上進行 AD DS 的變更。
- 新的 Dc 無法安裝在任何網域中。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原 - 先決條件](AD-Forest-Recovery-Prerequisties.md)
- [AD 樹系復原-設計自訂樹系復原方案](AD-Forest-Recovery-Devising-a-Plan.md)
- [AD 樹系修復-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 樹系復原-決定復原方式](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 樹系復原-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
- [AD 樹系復原-常見問題](AD-Forest-Recovery-FAQ.md)
- [AD 樹系復原-復原多網域樹系中的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)
- [AD 樹系復原-含 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)
