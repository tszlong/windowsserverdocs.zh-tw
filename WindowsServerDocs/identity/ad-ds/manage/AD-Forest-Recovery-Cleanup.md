---
title: AD 樹系復原-清除
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: fa7193cc800eac0fee6425a66bd5cd82d8c822c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868959"
---
# <a name="ad-forest-recovery---cleanup"></a>AD 樹系復原-清除

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

 視需要請執行下列復原後步驟：  
  
- 復原整個樹系之後，您可以還原成原始的 DNS 設定，包括在每個網域控制站上的慣用和替代 DNS 伺服器的組態。 DNS 伺服器設定之前所發生的問題之後，將會還原其先前的名稱解析功能。 刪除任何 DNS 記錄尚未復原的網域控制站的。  
- 尚未復原的所有網域控制站的刪除 Windows 網際網路名稱服務 (WINS) 的記錄。  
- 您可以轉移操作主機角色到網域或樹系中其他網域控制站，並新增失敗前組態為基礎的較通用類別目錄伺服器。  
- 因為整個樹系會還原到先前的狀態，任何已加入的物件 （例如使用者和電腦） 和 （例如密碼變更） 這個點之後對現有物件所做的所有更新都會遺失。 因此，您應該重新建立這些遺漏的物件，並重新套用適當的遺漏更新。  
- 您可能也需要還原連出的信任外部網域和樹系，因為這些外部信任關聯性都不會自動從備份還原。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)  
