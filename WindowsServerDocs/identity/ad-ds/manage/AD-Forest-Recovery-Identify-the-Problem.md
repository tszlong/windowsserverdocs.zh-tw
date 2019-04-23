---
title: AD 樹系復原 - 找出問題
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: cb3cf45f778ff305c8fe5aad5e23f0257650d6f5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865519"
---
# <a name="identify-the-problem"></a>找出問題

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2
  
全樹系失敗的徵狀出現時，例如在事件記錄檔或其他監視解決方案中，使用 Microsoft 支援服務以判斷失敗的原因，並評估任何可能的補救方法。  

## <a name="examples-of-forest-wide-failures"></a>全樹系失敗的範例

- 所有網域控制站有邏輯損毀或受損的業務續航力是不可能; 的點比方說，依存 AD DS 的所有商務應用程式都都無法作用。  
- 惡意系統管理員已入侵 Active Directory 環境。  
- 攻擊者刻意 — 或系統管理員不小心 — 執行樹系中分配資料損毀的指令碼。  
- 攻擊者刻意 — 或系統管理員不小心 — 延伸 Active Directory 架構的惡意或衝突的變更。  
- 攻擊者有管理惡意軟體安裝於網域控制站，且您已從備份復原樹系建議 Microsoft 支援服務。  
  
   > [!IMPORTANT]
   >  這份文件並未涵蓋如何復原樹系受到駭客攻擊或洩漏的安全性建議。 一般情況下，建議您使用後續階段 Pass-the-hash 防護技術來強化的環境。 如需詳細資訊，請參閱 <<c0> [ 緩解傳遞-雜湊 (PtH) 攻擊和其他認證竊取技術](https://www.microsoft.com/download/details.aspx?id=36036)。
  
- 沒有任何網域控制站可以複寫其複寫協力電腦。  
- 無法對任何網域控制站的 AD DS 的變更。  
- 無法在任何網域中安裝新網域控制站。  
  
## <a name="next-steps"></a>後續步驟

- [AD 樹系復原-必要條件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 樹系復原-設計自訂的樹系復原計劃](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 樹系復原-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 樹系復原-判斷如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 樹系復原-執行初始的復原](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)  
- [AD 樹系復原-常見問題集](AD-Forest-Recovery-FAQ.md)  
- [AD 樹系復原-復原 Multidomain 樹系內的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 樹系復原與 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md) 
