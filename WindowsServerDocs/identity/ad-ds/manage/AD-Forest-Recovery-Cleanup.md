---
description: 深入瞭解： AD 樹系復原-清除
title: AD 樹系復原-清除
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.openlocfilehash: 6059db057af7d7f5164421bc7bdffcb22df24d5a
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97045666"
---
# <a name="ad-forest-recovery---cleanup"></a>AD 樹系復原-清除

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

 視需要執行下列復原後步驟：

- 復原整個樹系之後，您可以還原為原始 DNS 設定，包括在每個 Dc 上設定慣用和替代 DNS 伺服器。 在 DNS 伺服器設定為正常運作之前，將會還原先前的名稱解析功能。 針對未復原的 Dc 刪除任何 DNS 記錄。
- 刪除 Windows 網際網路名稱服務 (WINS) 記錄中所有未復原的 Dc。
- 您可以將操作主機角色轉移到網域或樹系中的其他網域控制站，並根據失敗之前的設定來新增更多的通用類別目錄伺服器。
- 因為整個樹系還原為先前的狀態，所以已新增的任何物件 (例如使用者和電腦) 已新增，而且所有更新 (例如密碼變更) 在此時間點之後對現有物件所做的變更會遺失。 因此，您應該重新建立這些遺漏的物件，並適當地重新套用遺失的更新。
- 您可能還需要還原外部網域和樹系的連出信任，因為這些外部信任關係不會自動從備份還原。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
