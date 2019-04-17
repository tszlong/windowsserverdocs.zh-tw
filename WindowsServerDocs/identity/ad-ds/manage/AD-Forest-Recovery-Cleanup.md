---
title: "廣告樹系修復清理"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 2f652d08304a17ecbfde51bbb6f35e4666cd9eca
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---cleanup"></a>廣告樹系修復清理 

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

 視需要請執行下列文章復原步驟：  
  
-   復原整個樹系之後，您可以還原成原始 DNS 設定，包括組態的每個網域控制站的慣用及其他 DNS 伺服器。 設定故障之前一樣的 DNS 伺服器之後，將會還原他們先前的名稱解析功能。 Delete 任何 DNS 記錄的網域控制站的無法復原。  
  
-   適用於所有網域控制站無法復原 delete 記錄 Windows 網際網路名稱服務」(WINS)。  
  
-   您可以操作主機角色傳輸到其他 Dc 網域或森林中，並新增更多全球 catalog 伺服器為失敗之前的設定。  
  
-   整個樹系還原到先前的狀態，因為（例如，使用者與電腦）中的新增任何物件和所有更新（例如密碼變更）現有的物件這個時間點之後所做的都會遺失。 因此，您應該會重新建立這些遺失的物件，並重新套用遺漏適當的更新。  
  
-   您可能也需要還原出信任外部網域與森林，因為這些外部信任關係的未自動從備份還原。

## <a name="next-steps"></a>後續步驟

- [廣告樹系復原指南](AD-Forest-Recovery-Guide.md)
- [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)  



