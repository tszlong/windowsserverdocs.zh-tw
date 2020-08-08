---
title: AD 樹系復原-清除
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.openlocfilehash: 74f4518ead647aa4a3c8e4f7a8388c8735d466e1
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956705"
---
# <a name="ad-forest-recovery---cleanup"></a>AD 樹系復原-清除

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 視需要執行下列復原後步驟：

- 整個樹系復原之後，您可以還原成原始的 DNS 設定，包括在每個 Dc 上設定慣用和替代 DNS 伺服器。 在 DNS 伺服器設定為故障之前，其先前的名稱解析功能將會還原。 刪除尚未復原之 Dc 的任何 DNS 記錄。
- 針對所有尚未復原的 Dc，刪除 Windows 網際網路名稱服務 (WINS) 記錄。
- 您可以將操作主機角色轉移到網域或樹系中的其他 Dc，並根據失敗前的設定新增更多的通用類別目錄伺服器。
- 因為整個樹系會還原為先前的狀態，所以 (的任何物件，例如新增的使用者和) 電腦，以及在此時間點之後對現有物件所做的所有更新 (如密碼變更) 。 因此，您應該重新建立這些遺漏的物件，並適當地重新套用遺失的更新。
- 您可能也需要還原外部網域和樹系的連出信任，因為這些外部信任關係不會從備份自動還原。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
