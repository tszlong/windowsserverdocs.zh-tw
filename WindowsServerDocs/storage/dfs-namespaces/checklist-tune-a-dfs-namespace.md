---
title: "檢查清單：調整 DFS 命名空間"
description: "本文說明如何最佳化 DFS 命名空間為更新的命名空間資料處理轉介和輪詢 AD DS 的方式"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 827484c2ffcbb2617626129011a8b510473fe669
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="checklist-tune-a-dfs-namespace"></a>檢查清單：調整 DFS 命名空間

> 適用於：Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

建立命名空間以及新增資料夾和目標後，使用下列檢查清單來調整或最佳化 DFS 命名空間為更新的命名空間資料處理轉介和輪詢 Active Directory Domain Services (AD DS) 的方式。

-   防止使用者查看命名空間中他們不具有存取權限的資料夾。 [在命名空間上啟用存取型列舉](enable-access-based-enumeration-on-a-namespace.md) 
-   啟用或防止使用者在存取命名空間中的資料夾時，被轉介至命名空間或資料夾目標。 [啟用或停用轉介和用戶端容錯回復](enable-or-disable-referrals-and-client-failback.md) 
-   調整用戶端快取轉介多久之後可要求新的轉介。 [變更用戶端快取轉介的時間量](change-the-amount-of-time-that-clients-cache-referrals.md)
-   最佳化命名空間伺服器輪詢 AD DS 以取得最新命名空間資料的方式。 [最佳化命名空間輪詢](optimize-namespace-polling.md)
-   使用繼承的權限來控制哪些使用者可以檢視已啟用存取型列舉之命名空間中的資料夾。 [使用繼承的權限搭配存取型列舉](using-inherited-permissions-with-access-based-enumeration.md)

此外，透過使用稱為目標優先順序的 DFS 命名空間增強功能，您可以指定伺服器的優先順序，讓特定伺服器一律位於伺服器清單的第一個或最後一個 (稱為轉介)，當用戶端存取命名空間中的含目標資料夾時，會收到此伺服器清單。

-   指定使用者應轉介的資料夾目標順序。 [設定轉介中目標的排序方法](set-the-ordering-method-for-targets-in-referrals.md)
-   覆寫特定命名空間伺服器或資料夾目標的轉介順序。 [設定目標優先順序以覆寫轉介順序](set-target-priority-to-override-referral-ordering.md)

## <a name="see-also"></a>請參閱

-   [命名空間](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [檢查清單：部署 DFS 命名空間](checklist-deploy-dfs-namespaces.md)
-   [調整 DFS 命名空間](tuning-dfs-namespaces.md)


