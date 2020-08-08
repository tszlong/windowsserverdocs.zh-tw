---
title: AD 樹系復原虛擬化
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.openlocfilehash: e02fbf15ee3f9edc68bbfc479c2ab06aba32a959
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943751"
---
# <a name="active-directory-forest-recovery-virtualization"></a>Active Directory 樹系復原虛擬化

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

本主題說明 Windows Server 2016、2012 R2 和2012中的虛擬網域控制站複製功能。

## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>使用虛擬網域控制站複製來加速樹系復原

虛擬網域控制站 (DC) 複製可簡化並加速在網域中安裝其他虛擬化 Dc 的程式，特別是在集中位置，例如在虛擬機器上執行數個 Dc 的資料中心。 從備份還原每個網域中的一個虛擬 DC 之後，每個網域中的其他 Dc 都可以使用虛擬 DC 複製程式快速地上線。 您可以準備您復原的第一個虛擬化 DC，將它關閉，然後視需要複製該虛擬硬碟，以建立已複製的虛擬化 Dc 來組建網域。

虛擬 DC 複製的需求如下：

- 程式管理人員必須支援 VM-GenerationID。 Windows Server 2016、2012和 Windows 8 中的 hyper-v 是支援 VM GenerationID 的管理元件範例。 如果支援 VM GenerationID，請洽詢您的虛擬機器廠商。
- 當做複製來源使用的虛擬化 DC 必須執行 Windows Server 2016 或2012，而且必須是 Cloneable 網域控制站群組的成員。
- PDC 模擬器必須執行 Windows Server 2016 或2012。 您可以複製 PDC 模擬器（如果它已虛擬化）。

如需有關如何執行虛擬化 DC 複製的逐步指示，請參閱[Active Directory Domain Services (AD DS 簡介) 虛擬化 (層級 100) ](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md)。 如需虛擬化 DC 複製運作方式的詳細資訊，請參閱[虛擬網域控制站技術參考 (層級 300) ](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md)。

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
