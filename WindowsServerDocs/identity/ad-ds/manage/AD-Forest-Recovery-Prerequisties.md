---
title: 規劃 Active Directory 樹系復原的必要條件
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.openlocfilehash: bb640474ca990d7853a35ad684889b389affb69d
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88938098"
---
# <a name="active-directory-forest-recovery-prerequisites"></a>Active Directory 樹系復原必要條件

> 適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

下列檔討論在設計樹系復原計畫或嘗試復原之前，您應該先熟悉的必要條件。

## <a name="assumptions-for-using-this-guide"></a>使用本指南的假設

1. 您已與 Microsoft 支援服務專業人員合作，並且：
   - 判斷整個樹系失敗的原因。 本指南不會建議失敗的原因，也不建議任何程式來防止失敗。
   - 評估任何可能的補救方式。
   - 結論是，在諮詢 Microsoft 支援服務中，將整個樹系還原至失敗之前的狀態，是從失敗中復原的最佳方式。 在許多情況下，樹系復原應該是最後一個選項。

1. 您已遵循 Microsoft 針對使用 Active Directory 整合式網域名稱系統 (DNS) 的最佳作法建議。 具體而言，每個 Active Directory 網域應該會有 Active Directory 整合的 DNS 區域。
   - 如果不是這種情況，您仍然可以使用本指南的基本原則來執行樹系復原。 不過，您將需要根據自己的環境，採取特定的 DNS 復原措施。 如需使用 Active Directory 整合式 DNS 的詳細資訊，請參閱 [建立 Dns 基礎結構設計](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)。

1. 雖然本指南的目的是要做為樹系復原的一般指南，但並非所有可能的案例皆涵蓋在內。 比方說，從 Windows Server 2008 開始，有一個 Server Core 版本，也就是完整版的 Windows Server，但不含完整的 GUI。 雖然您當然可以復原由執行 Server Core 的 Dc 所組成的樹系，但本指南沒有任何詳細的指示。 不過，根據此處所討論的指引，您將能夠自行設計所需的命令列動作。

> [!NOTE]
> 雖然本指南的目標是要復原樹系，以及維護或還原完整的 DNS 功能，但復原可能會導致在失敗前從設定變更的 DNS 設定。 在樹系復原之後，您可以還原為原始的 DNS 設定。 本指南中的建議並不說明如何設定 DNS 伺服器，以執行公司命名空間其他部分的名稱解析，其中有未儲存在 AD DS 中的 DNS 區域。

## <a name="concepts-for-using-this-guide"></a>使用本指南的概念

開始規劃 Active Directory 樹系的復原之前，您應該先熟悉下列各項：

- 基本 Active Directory 概念
- 操作主機角色的重要性 (也稱為彈性單一主機操作或 FSMO) 。 這些角色包括下列各項：
  - 架構主機
  - 網網域命名主機
  -  (RID) 主機的相對識別碼
  - 主域控制站 (PDC) 模擬器主機
  - 基礎結構主機

此外，您應該定期在實驗室環境中備份和還原 AD DS 和 SYSVOL。 如需詳細資訊，請參閱 [備份系統狀態資料](AD-Forest-Recovery-Procedures.md) 和 [執行 Active Directory Domain Services 的非系統授權還原](AD-Forest-Recovery-Procedures.md)。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [AD 樹系復原 - 先決條件](AD-Forest-Recovery-Prerequisties.md)

> [!div class="nextstepaction"]
> [AD 樹系復原-設計自訂樹系復原方案](AD-Forest-Recovery-Devising-a-Plan.md)

> [!div class="nextstepaction"]
> [AD 樹系修復-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)

> [!div class="nextstepaction"]
> [AD 樹系復原-決定復原方式](AD-Forest-Recovery-Determine-how-to-Recover.md)

> [!div class="nextstepaction"]
> [AD 樹系復原-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)

> [!div class="nextstepaction"]
> [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)

> [!div class="nextstepaction"]
> [AD 樹系復原-常見問題](AD-Forest-Recovery-FAQ.md)

> [!div class="nextstepaction"]
> [AD 樹系復原-復原多網域樹系中的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)

> [!div class="nextstepaction"]
> [AD 樹系復原-含 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)
