---
title: Active Directory 樹系復原計劃的必要條件
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: c8945dd5ccccb27826dd96413b56a070a7452789
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842169"
---
# <a name="active-directory-forest-recovery-prerequisites"></a>Active Directory 樹系復原必要條件

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

下列文件會討論您應知悉如何設計樹系復原計劃，或嘗試復原前的必要條件。

## <a name="assumptions-for-using-this-guide"></a>使用本指南假設

1. 您已使用 Microsoft 支援專業人員和：
   - 判斷失敗原因的全樹系。 本指南不會建議失敗的原因或建議任何程序，以避免發生失敗。
   - 評估任何可能的補救方法。  
   - 結束時，在未諮詢 Microsoft 支援服務，該還原整個樹系到其狀態失敗發生之前是從失敗中復原的最佳方式。 在許多情況下，樹系復原應該最後一個選項。

2. 您已遵循 Microsoft 最佳作法建議，使用整合 Active Directory 的網域名稱系統 (DNS)。 具體來說，應該針對每個 Active Directory 網域的 Active Directory 整合 DNS 區域。 
   - 如果這不是這樣，則您仍然可以使用本指南的基本原則執行樹系復原。 不過，您必須採取特定的量值，根據您自己環境的 DNS 復原。 如需使用 Active Directory 整合 DNS 的詳細資訊，請參閱[建立 DNS 基礎結構設計](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)。

3. 雖然本指南旨在做為樹系復原的一般指南未涵蓋所有可能的案例。 比方說，從 Windows Server 2008 開始，沒有 Server Core 版本，也就是完整版的 Windows Server，但不含完整 GUI。 雖然它當然有可能復原只是執行 Server Core 的 Dc 所組成的樹系，本指南中的任何詳細的指示。 不過，根據此處所討論的指引您將能夠自行設計所需的命令列動作。  

> ![!NOTE]
> 雖然本指南的目標復原樹系和維護或還原完整的 DNS 功能，但是復原可能會導致從失敗前的組態變更的 DNS 組態。 樹系復原之後，您可以還原成原始的 DNS 設定。 本指南中的建議不會說明如何設定執行的公司命名空間的其他部分的名稱解析的 DNS 伺服器有未儲存在 AD DS 的 DNS 區域。  

## <a name="concepts-for-using-this-guide"></a>使用本指南的概念

規劃 Active Directory 樹系復原之前，您應該熟悉下列：  
  
- Active Directory 的基本概念  
- 操作主機角色 （也稱為彈性單一主機操作或 FSMO） 的重要性。 這些角色包括下列各項：  
   - 架構主機
   - 網域命名主機
   - 相對識別碼 (RID) 主機
   - 主要網域控制站 (PDC) 模擬器主機
   - 基礎結構主機

此外，您應該先備份和還原 AD DS 與 SYSVOL 上定期執行實驗室環境中。 如需詳細資訊，請參閱 <<c0> [ 備份系統狀態資料](AD-Forest-Recovery-Procedures.md)並[執行的 Active Directory 網域服務的非系統授權還原](AD-Forest-Recovery-Procedures.md)。

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
