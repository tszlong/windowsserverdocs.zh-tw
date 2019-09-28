---
ms.assetid: 680e05ac-f9be-4b07-a9f4-cd6da5835952
title: Active Directory 樹系復原指南
description: 本指南提供備份、還原和 active directory 嚴重損壞修復的指引。
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ee31076669351a92caf79c6b8972ccdd68b347aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390445"
---
# <a name="active-directory-forest-recovery-guide"></a>Active Directory 樹系復原指南

>適用於：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2、Windows Server 2003

本指南包含復原 Active Directory®樹系的最佳做法建議，如果全樹系失敗，則會將樹系中的所有網域控制站（Dc）轉譯為無法正常運作。 它所包含的步驟可作為您樹系復原計畫的範本，您可以針對特定環境進行自訂。 這些步驟適用于執行 Microsoft® Windows Server 2016、2012 R2、2012、2008 R2、2008和2003作業系統的 Dc。  
  
> [!NOTE]
> 執行 Windows Server 2003 的 Dc 獨有的程式會合並在[AD 樹系復原 Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)中。  
  
## <a name="steps-outlined-in-this-guide"></a>本指南中所述的步驟
  
- [AD 樹系復原 - 先決條件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 樹系復原-設計自訂樹系復原計畫](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 樹系復原 - 復原步驟](AD-Forest-Recovery-Steps-For-Restoring.md)
- [AD 樹系復原-識別問題](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 樹系復原-決定如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 樹系復原-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)  
- [AD 樹系復原-常見問題](AD-Forest-Recovery-FAQ.md)  
- [AD 樹系復原-修復多網域樹系中的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 樹系復原 - 虛擬化](AD-Forest-Recovery-Virtualization.md)
- [AD 樹系復原-具有 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)  
