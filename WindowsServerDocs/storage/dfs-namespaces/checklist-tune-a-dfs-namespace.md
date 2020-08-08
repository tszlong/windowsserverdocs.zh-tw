---
title: 檢查清單：調整 DFS 命名空間
description: 本文說明如何最佳化 DFS 命名空間為更新的命名空間資料處理轉介和輪詢 AD DS 的方式
ms.date: 6/5/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 9e5759579b86c2ed7721a31aada5ddaa345fc256
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957686"
---
# <a name="checklist-tune-a-dfs-namespace"></a>檢查清單：調整 DFS 命名空間

> 適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

建立命名空間並新增資料夾和目標之後，請使用下列檢查清單來微調或優化 DFS 命名空間處理參考和輪詢的方式 Active Directory Domain Services (AD DS) 更新的命名空間資料。

-   防止使用者查看命名空間中他們不具有存取權限的資料夾。 [在命名空間上啟用以存取為基礎的列舉](enable-access-based-enumeration-on-a-namespace.md)
-   啟用或防止使用者在存取命名空間中的資料夾時，被轉介至命名空間或資料夾目標。 [啟用或停用轉介和用戶端容錯回復](enable-or-disable-referrals-and-client-failback.md)
-   調整用戶端快取轉介多久之後可要求新的轉介。 [變更用戶端快取參考的時間長度](change-the-amount-of-time-that-clients-cache-referrals.md)
-   最佳化命名空間伺服器輪詢 AD DS 以取得最新命名空間資料的方式。 [最佳化命名空間輪詢](optimize-namespace-polling.md)
-   使用繼承的權限來控制哪些使用者可以檢視已啟用存取型列舉之命名空間中的資料夾。 [使用繼承的許可權搭配以存取為基礎的列舉](using-inherited-permissions-with-access-based-enumeration.md)

此外，您可以使用 DFS 命名空間增強功能 (稱為目標優先順序) 來指定伺服器的優先順序，讓特定伺服器在用戶端存取含有命名空間目標的資料夾時，永遠放在用戶端接收之伺服器清單的第一位或最後一位。

-   指定使用者應轉介的資料夾目標順序。 [設定轉介中目標的排序方法](set-the-ordering-method-for-targets-in-referrals.md)
-   覆寫特定命名空間伺服器或資料夾目標的轉介順序。 [設定目標優先順序以覆寫轉介順序](set-target-priority-to-override-referral-ordering.md)

## <a name="additional-references"></a>其他參考資料

-   [命名空間](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771914(v=ws.11))
-   [檢查清單：部署 DFS 命名空間](checklist-deploy-dfs-namespaces.md)
-   [調整 DFS 命名空間](tuning-dfs-namespaces.md)
