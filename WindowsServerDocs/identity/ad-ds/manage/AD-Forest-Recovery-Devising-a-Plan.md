---
description: 深入瞭解： AD 樹系復原-設計 AD 樹系復原方案
title: AD 樹系復原-設計 AD 樹系復原方案
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 17381f30-55f2-4e00-977a-b701675fa4ff
ms.openlocfilehash: a1f78d060287bedd412e63231f0a9830d61afd56
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97042926"
---
# <a name="ad-forest-recovery---devising-an-ad-forest-recovery-plan"></a>AD 樹系復原-設計 AD 樹系復原方案

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

根據您的環境和商務需求，您可能或可能不需要執行本指南中所述的所有步驟來執行成功的樹系復原。 假設本指南僅做為樹系復原的範本，您必須設計符合您環境的自訂樹系復原方案，並符合您的商務需求。

例如，在您的樹系復原方案中，您應該會有樹系的詳細拓撲圖。 對應應該會列出 Dc 的所有資訊，例如其名稱、其角色和備份狀態，以及兩者之間的信任關係。 如需可用來建立拓撲圖的工具，請參閱 [Microsoft Active Directory 拓撲 Diagrammer](https://www.microsoft.com/download/details.aspx?id=13380)。

您應在一年中至少練習一次樹系復原方案。 此外，當 Enterprise Admins 或 Domain Admins 群組的成員資格有所變更時，最好執行樹系復原演練。 這有助於確保您的資訊技術 (IT) 的員工完全瞭解樹系復原方案。

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
