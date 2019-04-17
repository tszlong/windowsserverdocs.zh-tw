---
title: "廣告樹系修復模擬"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adfs
ms.openlocfilehash: 08b0c4a4c5ed230f91241fcfb67db1749935c5e2
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2017
---
# <a name="active-directory-forest-recovery-virtualization"></a>Active Directory 樹系修復模擬

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

本主題描述 Windows Server 2016，2012 R2、 2012年模擬的網域控制站複製功能。  
 
## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>使用模擬的網域控制站複製促進樹系復原  
 簡化模擬的網域控制站 DC 複製，而且可以加速安裝其他模擬的 Dc，尤其是幾個網域控制站 hypervisors 執行的位置資料中心例如打造位置網域中的程序。 從備份還原每個網域中的某個 virtual DC 之後，每個網域中的其他 Dc 可快速依照 online 使用模擬的 DC 複製程序。 您可以製作復原，將它關閉，然後再複製的第一個模擬的 DC virtual 硬碟為很多時候，以視需要建立複製擬化檔案網域控制站建置網域。  
  
 模擬俠複製需求︰  
  
-   Hypervisor 必須支援 VM-GenerationID。 HYPER-V 中 2012年、 Windows Server 2016 和 Windows 8 是支援 VM-GenerationID hypervisor 的範例。 如果 VM-GenerationID 支援，請洽詢您 hypervisor 廠商。  
  
-   適用於複製做為來源模擬的俠必須執行 Windows Server 2016 或 2012年，且可複製網域控制站群組成員。  
  
-   肯定必須執行 Windows Server 2016 或 2012年。 您可以複製肯定，如果擬化檔案。  
  
 了解如何執行逐步指示擬化檔案複製俠的查看[Active Directory Domain Services (AD DS) 模擬 (層級 100) 簡介](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md)。 如需詳細資訊，了解如何擬化檔案俠複製運作，請查看[擬化檔案網域控制站技術參考 (層級 300)](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md)。  

-   [廣告樹系修復必要條件](AD-Forest-Recovery-Prerequisties.md)  
-   [廣告樹系修復-設計自訂樹系復原計劃](AD-Forest-Recovery-Devising-a-Plan.md)  
- [廣告樹系修復-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
-   [廣告樹系修復-判斷如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [廣告樹系修復-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)  
-   [廣告樹系修復-常見問題集](AD-Forest-Recovery-FAQ.md)  
-   [廣告樹系修復-復原 Multidomain 樹系單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [廣告樹系復原與 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md) 

  
