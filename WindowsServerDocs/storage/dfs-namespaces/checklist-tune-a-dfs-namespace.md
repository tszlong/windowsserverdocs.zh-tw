---
title: 檢查清單：調整 DFS 命名空間
description: 本文說明如何最佳化 DFS 命名空間為更新的命名空間資料處理轉介和輪詢 AD DS 的方式
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8a4c581801cbb1dcfe3db2813fb66fb4514d6434
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402187"
---
# <a name="checklist-tune-a-dfs-namespace"></a>檢查清單：調整 DFS 命名空間

> 適用於：Windows Server 2019、Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

建立命名空間並新增資料夾和目標之後，請使用下列檢查清單來微調或優化 DFS 命名空間處理參考和輪詢的方式 Active Directory Domain Services （AD DS），以取得更新的命名空間資料。

-   防止使用者查看命名空間中他們不具有存取權限的資料夾。 [在命名空間上啟用以存取為基礎的列舉](enable-access-based-enumeration-on-a-namespace.md) 
-   啟用或防止使用者在存取命名空間中的資料夾時，被轉介至命名空間或資料夾目標。 [啟用或停用轉介和用戶端容錯回復](enable-or-disable-referrals-and-client-failback.md) 
-   調整用戶端快取轉介多久之後可要求新的轉介。 [變更用戶端快取參考的時間長度](change-the-amount-of-time-that-clients-cache-referrals.md)
-   最佳化命名空間伺服器輪詢 AD DS 以取得最新命名空間資料的方式。 [最佳化命名空間輪詢](optimize-namespace-polling.md)
-   使用繼承的權限來控制哪些使用者可以檢視已啟用存取型列舉之命名空間中的資料夾。 [使用繼承的許可權搭配以存取為基礎的列舉](using-inherited-permissions-with-access-based-enumeration.md)

此外，藉由使用稱為「目標優先順序」的 DFS 命名空間增強功能，您可以指定伺服器的優先順序，讓特定伺服器一律會放在用戶端存取資料夾時所收到的伺服器清單中（稱為「參考」）的第一個或最後一個搭配命名空間中的目標。

-   指定使用者應轉介的資料夾目標順序。 [設定轉介中目標的排序方法](set-the-ordering-method-for-targets-in-referrals.md)
-   覆寫特定命名空間伺服器或資料夾目標的轉介順序。 [設定目標優先順序以覆寫轉介順序](set-target-priority-to-override-referral-ordering.md)

## <a name="see-also"></a>另請參閱

-   [命名空間](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [檢查清單：部署 DFS 命名空間](checklist-deploy-dfs-namespaces.md)
-   [調整 DFS 命名空間](tuning-dfs-namespaces.md)


