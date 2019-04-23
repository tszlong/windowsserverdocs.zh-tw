---
title: 檢查清單：調整 DFS 命名空間
description: 本文說明如何最佳化 DFS 命名空間為更新的命名空間資料處理轉介和輪詢 AD DS 的方式
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5e2d43f75a64a6a7950539c14386c6a037bc3c45
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872639"
---
# <a name="checklist-tune-a-dfs-namespace"></a>檢查清單：調整 DFS 命名空間

> 適用於：Windows Server 2019，Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

建立命名空間及新增資料夾與目標，使用下列檢查清單，來微調或最佳化的方式之後 DFS 命名空間處理轉介與輪詢 Active Directory 網域服務 (AD DS) 更新命名空間資料。

-   防止使用者查看命名空間中他們不具有存取權限的資料夾。 [啟用存取型列舉型別命名空間](enable-access-based-enumeration-on-a-namespace.md) 
-   啟用或防止使用者在存取命名空間中的資料夾時，被轉介至命名空間或資料夾目標。 [啟用或停用轉介和用戶端容錯回復](enable-or-disable-referrals-and-client-failback.md) 
-   調整用戶端快取轉介多久之後可要求新的轉介。 [變更用戶端快取轉介的時間長度](change-the-amount-of-time-that-clients-cache-referrals.md)
-   最佳化命名空間伺服器輪詢 AD DS 以取得最新命名空間資料的方式。 [最佳化命名空間輪詢](optimize-namespace-polling.md)
-   使用繼承的權限來控制哪些使用者可以檢視已啟用存取型列舉之命名空間中的資料夾。 [使用存取型列舉中的繼承的權限](using-inherited-permissions-with-access-based-enumeration.md)

此外，使用 DFS 命名空間的增強功能，稱為目標優先順序，您可以指定伺服器的優先順序，讓特定伺服器一律位於第一次或上次 （又稱為轉介） 的伺服器清單中存取的資料夾時，用戶端會收到具有目標命名空間中。

-   指定使用者應轉介的資料夾目標順序。 [設定轉介中的目標的排序方法](set-the-ordering-method-for-targets-in-referrals.md)
-   覆寫特定命名空間伺服器或資料夾目標的轉介順序。 [設定目標優先順序以覆寫轉介順序](set-target-priority-to-override-referral-ordering.md)

## <a name="see-also"></a>另請參閱

-   [命名空間](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [檢查清單：部署 DFS 命名空間](checklist-deploy-dfs-namespaces.md)
-   [調整 DFS 命名空間](tuning-dfs-namespaces.md)


