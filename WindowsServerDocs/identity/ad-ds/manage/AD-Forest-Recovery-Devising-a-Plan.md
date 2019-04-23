---
title: AD 樹系復原-設計的 AD 樹系復原計劃
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 17381f30-55f2-4e00-977a-b701675fa4ff
ms.technology: identity-adds
ms.openlocfilehash: edfc874fe030c6394bc8bda95c49e61951e78f43
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881779"
---
# <a name="ad-forest-recovery---devising-an-ad-forest-recovery-plan"></a>AD 樹系復原-設計的 AD 樹系復原計劃

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

根據您的環境和商務需求，您可能會或可能不需要執行執行成功的樹系復原此指南中所述的所有步驟。 假設本指南只做為樹系復原的範本，就很重要，您可以設計自訂的樹系的復原方案適合您的環境，並符合您的業務需求。  
  
例如，在樹系復原計劃，您應該有樹系的詳細的拓撲地圖。 對應應該會列出網域控制站，例如其名稱、 其角色和備份狀態，以及它們之間的信任關係的所有資訊。 這種工具可用來建立拓撲地圖，請參閱[Microsoft Active Directory 拓樸 Diagrammer](https://www.microsoft.com/download/details.aspx?id=13380)。  
  
您應該將練習您的樹系復原計劃每年至少一次。 此外，它是個不錯的主意，以執行樹系復原演練，Enterprise Admins 或 Domain Admins 群組成員資格變更時。 這有助於確保您的資訊技術 (IT) 人員完全了解樹系復原計劃。

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
