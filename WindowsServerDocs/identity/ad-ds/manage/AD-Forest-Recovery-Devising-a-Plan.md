---
title: AD 樹系復原-設計 AD 樹系復原計畫
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 17381f30-55f2-4e00-977a-b701675fa4ff
ms.openlocfilehash: d6637f92dff1542837b42a1406a17555a753bf86
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972315"
---
# <a name="ad-forest-recovery---devising-an-ad-forest-recovery-plan"></a>AD 樹系復原-設計 AD 樹系復原計畫

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

視您的環境和商務需求而定，您可能需要執行本指南中所述的所有步驟，才能執行成功的樹系復原。 假設本指南僅作為樹系復原的範本，請務必設計符合您環境的自訂樹系復原計畫，並符合您的業務需求。

例如，在您的樹系復原計畫中，您應該會有樹系的詳細拓撲圖。 對應應該會列出 Dc 的所有資訊，例如其名稱、角色和備份狀態，以及它們之間的信任關係。 如需可用來建立拓撲圖的工具，請參閱[Microsoft Active Directory 拓朴 Diagrammer](https://www.microsoft.com/download/details.aspx?id=13380)。

您應該一年至少進行一次您的樹系復原計畫。 此外，在有 Enterprise Admins 或 Domain Admins 群組的成員資格變更時，也最好執行樹系復原演練。 這有助於確保您的資訊技術 (IT) 人員完全瞭解樹系復原計畫。

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
