---
title: 規劃 Active Directory 樹系復原的必要條件
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: 6dcd1185ba4d4c847cfe212f78ccc9661fd2aead
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823781"
---
# <a name="active-directory-forest-recovery-prerequisites"></a>Active Directory 樹系復原必要條件

> 適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

下列檔討論在建立樹系復原計畫或嘗試復原之前，您應該先熟悉的必要條件。

## <a name="assumptions-for-using-this-guide"></a>使用本指南的假設

1. 您已經與 Microsoft 支援服務專業人員合作，並且：
   - 判斷整個樹系失敗的原因。 本指南不建議發生失敗的原因，或建議採取任何程式來避免失敗。
   - 評估任何可能的補救方式。  
   - 結束，在與 Microsoft 支援服務的諮詢中，將整個樹系還原到失敗發生之前的狀態，是從失敗中復原的最佳方式。 在許多情況下，樹系復原應該是最後一個選項。

1. 您已遵循使用 Active Directory 整合式網域名稱系統（DNS）的 Microsoft 最佳作法建議。 具體而言，每個 Active Directory 網域都應該有一個 Active Directory 整合的 DNS 區域。
   - 如果不是這種情況，您仍然可以使用本指南的基本原則來執行樹系復原。 不過，您必須根據自己的環境，針對 DNS 復原採取特定的量值。 如需使用 Active Directory 整合式 DNS 的詳細資訊，請參閱[建立 Dns 基礎結構設計](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)。

1. 雖然本指南的目的是做為樹系復原的一般指南，但並非所有可能的案例都涵蓋在內。 比方說，從 Windows Server 2008 開始，有一個 Server Core 版本，這是 Windows Server 的完整版本，但沒有完整的 GUI。 雖然您可以復原的樹系只包含執行 Server Core 的 Dc，但本指南沒有詳細的指示。 不過，根據此處所討論的指導方針，您將能夠自行設計必要的命令列動作。  

> [!NOTE]
> 雖然本指南的目標是要復原樹系，並維護或還原完整的 DNS 功能，但在失敗之前，修復可能會導致 DNS 設定從設定中變更。 在樹系復原之後，您可以還原成原始的 DNS 設定。 本指南中的建議不會說明如何設定 DNS 伺服器，以對公司命名空間的其他部分執行名稱解析，其中有不會儲存在 AD DS 中的 DNS 區域。  

## <a name="concepts-for-using-this-guide"></a>使用本指南的概念

開始規劃 Active Directory 樹系的復原之前，您應該先熟悉下列各項：  
  
- 基本 Active Directory 概念  
- 操作主機角色（也稱為彈性單一主機操作或 FSMO）的重要性。 這些角色包括下列各項：  
  - 架構主機
  - 網域命名主機
  - 相對識別碼（RID）主機
  - 主域控制站（PDC）模擬器主機
  - 基礎結構主機

此外，您應該定期在實驗室環境中備份和還原 AD DS 和 SYSVOL。 如需詳細資訊，請參閱[備份系統狀態資料](AD-Forest-Recovery-Procedures.md)和[執行 Active Directory Domain Services 的非系統授權還原](AD-Forest-Recovery-Procedures.md)。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [AD 樹系復原 - 先決條件](AD-Forest-Recovery-Prerequisties.md)

> [!div class="nextstepaction"]
> [AD 樹系復原-設計自訂樹系復原計畫](AD-Forest-Recovery-Devising-a-Plan.md)

> [!div class="nextstepaction"]
> [AD 樹系復原-識別問題](AD-Forest-Recovery-Identify-the-Problem.md)

> [!div class="nextstepaction"]
> [AD 樹系復原-決定如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)

> [!div class="nextstepaction"]
> [AD 樹系復原-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)

> [!div class="nextstepaction"]
> [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)

> [!div class="nextstepaction"]
> [AD 樹系復原-常見問題](AD-Forest-Recovery-FAQ.md)

> [!div class="nextstepaction"]
> [AD 樹系復原-修復多網域樹系中的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)

> [!div class="nextstepaction"]
> [AD 樹系復原-具有 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)
