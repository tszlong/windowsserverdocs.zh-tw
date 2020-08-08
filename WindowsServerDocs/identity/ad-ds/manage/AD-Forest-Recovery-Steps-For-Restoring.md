---
title: AD 樹系復原-還原樹系的步驟
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.openlocfilehash: e6c758c6ad7f4754c0c3105ab9ffa4ace38ddfaf
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953217"
---
# <a name="ad-forest-recovery---steps-for-restoring-the-forest"></a>AD 樹系復原-還原樹系的步驟

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

本節提供復原樹系的建議路徑總覽。 樹系復原步驟稍後會詳細說明。

下列清單摘要說明在高階的復原步驟：

1. [識別問題](AD-Forest-Recovery-Identify-the-Problem.md)

   使用它和 Microsoft 支援服務來判斷問題的範圍和可能的原因，並評估所有商務專案關係人可能的因應措施。 在許多情況下，總樹系復原應該是最後一個選項。

2. [決定如何復原樹系](AD-Forest-Recovery-Determine-how-to-Recover.md)

   在您判斷是否需要進行樹系復原之後，請完成預備步驟來準備它：判斷目前的樹系結構、識別每個 DC 執行的功能、決定每個網域要還原的 DC，並確保所有可寫入的 Dc 都已離線。

3. [執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)

   在 [隔離] 中，為每個網域復原一個 DC、將其清除，然後重新連接網域。 重設特殊許可權帳戶，並修正此階段中因安全性缺口而造成的問題。

4. [重新部署剩餘的 Dc](AD-Forest-Recovery-Restore-Additional-DCs.md)

   重新部署樹系，使其回到失敗前的狀態。 此步驟必須針對您的特定設計和需求進行調整。 虛擬網域控制站複製有助於加速此程式。

5. [清除](AD-Forest-Recovery-Cleanup.md)

   還原功能之後，請視需要重新設定名稱解析，讓 LOB 應用程式運作。

本指南中的步驟的設計，是為了將危險資料重新介紹基於到復原的樹系中的可能性降至最低。 您可能必須修改這些步驟，以將這類因素列入考慮：

- 延展性
- 遠端系統管理能力
- 復原速度
