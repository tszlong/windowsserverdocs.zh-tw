---
ms.assetid: 680e05ac-f9be-4b07-a9f4-cd6da5835952
title: Active Directory 樹系修復指南
description: 本指南提供有關備份、還原及 active directory 嚴重損壞修復的指引。
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.openlocfilehash: 68acf07288ffb444dca9ca4e0edb661d41f2065c
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93071310"
---
# <a name="active-directory-forest-recovery-guide"></a>Active Directory 樹系修復指南

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2、Windows Server 2003

本指南包含最佳做法建議，以在整個樹系的樹系中將所有網域控制站) 在樹系中無法正常運作的域 (控制器時，復原 Active Directory®樹系。 其所包含的步驟可作為樹系復原方案的範本，您可以針對特定環境自訂此範本。 這些步驟適用于執行 Microsoft® Windows Server 2016、2012 R2、2012、2008 R2、2008和2003作業系統的 Dc。

> [!NOTE]
> 執行 Windows Server 2003 的 Dc 唯一的程式會合並在 [AD 樹系復原 Windows server 2003](AD-Forest-Recovery-Windows-Server-2003.md)中。

## <a name="steps-outlined-in-this-guide"></a>本指南概述的步驟

- [AD 樹系復原 - 先決條件](AD-Forest-Recovery-Prerequisties.md)
- [AD 樹系復原-設計自訂樹系復原方案](AD-Forest-Recovery-Devising-a-Plan.md)
- [AD 樹系復原 - 復原步驟](AD-Forest-Recovery-Steps-For-Restoring.md)
- [AD 樹系修復-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 樹系復原-決定復原方式](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 樹系復原-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
- [AD 樹系復原-常見問題](AD-Forest-Recovery-FAQ.md)
- [AD 樹系復原-復原多網域樹系中的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)
- [AD 樹系復原 - 虛擬化](AD-Forest-Recovery-Virtualization.md)
- [AD 樹系復原-含 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)
