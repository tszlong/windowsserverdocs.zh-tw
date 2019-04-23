---
title: AD 樹系復原虛擬化
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: 23317f55fdce18e78ac3e7e1490f6fc4937fd062
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858449"
---
# <a name="active-directory-forest-recovery-virtualization"></a>Active Directory 樹系復原虛擬化

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

本主題說明 Windows Server 2016、 2012 R2 和 2012年的虛擬的網域控制站複製功能。  

## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>若要加速樹系復原中使用 複製虛擬的網域控制站

複製虛擬的網域控制站 (DC) 可以簡化並加快的網域，尤其是在集中式位置，例如資料中心在 hypervisor 執行數個網域控制站的位置中安裝額外的虛擬網域控制站的程序。 從備份還原每個網域中的一個虛擬 DC 之後，每個網域中的其他網域控制站可以快速上線使用虛擬的 DC 複製程序。 您可以準備第一次虛擬的 DC，您要復原，關閉的狀況下，並將複製該虛擬硬碟許多次，以視需要建立複製虛擬網域控制站以建置出的網域。  
  
虛擬化 DC 複製的需求如下：  
  
- Hypervisor 必須支援 VM 世代識別碼 」。 HYPER-V 的 Windows Server 2016、 2012年和 Windows 8 是支援 Vm-generationid 之 hypervisor 的範例。 如果支援 VM 世代識別碼 」，請洽詢您的 hypervisor 廠商。  
- 做為複製來源的虛擬化的 DC 必須執行 Windows Server 2016 或 2012年，而且是 [Cloneable Domain Controllers] 群組的成員。 
- PDC 模擬器必須執行 Windows Server 2016 或 2012年。 如果它虛擬化，您可以複製 PDC 模擬器。  
  
如需如何執行的逐步指示虛擬化 DC 複製，請參閱 < [Active Directory 網域服務 (AD DS) 虛擬化 (等級 100) 簡介](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md)。 如詳細瞭解虛擬化 DC 複製運作，請參閱 <<c0> [ 虛擬化網域控制站技術參考 (技術等級 300)](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md)。 

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
