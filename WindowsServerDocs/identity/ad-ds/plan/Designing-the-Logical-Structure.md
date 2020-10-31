---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: 設計邏輯結構
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 09a26589d2301e4b4d2e206c4953e9ae7810f3cc
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93069300"
---
# <a name="designing-the-logical-structure"></a>設計邏輯結構

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory Domain Services (AD DS) 可讓組織為使用者與資源管理建立可擴充、安全且可管理的基礎結構。 它也可讓它們支援啟用目錄的應用程式。

設計完善的 Active Directory 邏輯結構提供下列優點：

-   簡化包含大量物件之 Microsoft Windows 網路的管理

-   合併網域結構和降低系統管理成本

-   適當地委派資源的系統管理控制能力

-   降低對網路頻寬的影響

-   簡化的資源分享

-   最佳搜尋效能

-   低擁有權總成本

設計完善的 Active Directory 邏輯結構有助於有效率地整合這類功能，例如群組原則;桌面鎖定;軟體發佈;以及使用者、群組、工作站和伺服器的系統管理。 此外，經過精心設計的邏輯結構有助於將 Microsoft 與非 Microsoft 應用程式和服務（例如 Microsoft Exchange Server、公開金鑰基礎結構 (PKI) ，以及以網域為基礎的分散式檔案系統 (DFS) ）整合。

當您在部署 AD DS 之前設計 Active Directory 邏輯結構時，可以將部署程式優化，以充分利用 Active Directory 功能。 為了設計 Active Directory 邏輯結構，您的設計小組會先找出組織的需求，並根據此資訊，決定樹系和網域界限的放置位置。 然後，設計小組決定如何設定網域名稱系統 (DNS) 環境，以符合樹系的需求。 最後，設計小組會識別在組織中委派資源管理所需的組織單位 (OU) 結構。

## <a name="in-this-guide"></a>本指南內容

-   [瞭解 Active Directory 的邏輯模型](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)

-   [識別部署專案參與者](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)

-   [建立樹系設計](../../ad-ds/plan/Creating-a-Forest-Design.md)

-   [建立網域設計](../../ad-ds/plan/Creating-a-Domain-Design.md)

-   [建立 DNS 基礎結構設計](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)

-   [建立組織單位設計](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)

-   [附錄 A：DNS 清查](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)



