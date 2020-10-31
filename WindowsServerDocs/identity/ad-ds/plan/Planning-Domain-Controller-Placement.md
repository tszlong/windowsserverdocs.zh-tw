---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: 規劃設置網域控制站
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: ea98ff3dd41e7280acec36432010b6b73156621c
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93071080"
---
# <a name="planning-domain-controller-placement"></a>規劃設置網域控制站

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在您收集所有將用來設計網站拓撲的網路資訊之後，請規劃要放置網域控制站的位置，包括樹系根網域控制站、區域網域控制站、操作主機角色持有者，以及通用類別目錄伺服器。

在 Windows Server 2008 中，您也可以利用 (Rodc) 的唯讀網域控制站。 RODC 是新型的網域控制站，裝載 Active Directory 資料庫的唯讀分割。 除了帳戶密碼，RODC 還會保有可寫入網域控制站所保有的所有 Active Directory 物件及屬性。 不過，您無法對儲存於 RODC 上的資料庫進行變更。 變更必須在可寫入的網域控制站上進行，然後複寫回 RODC。

RODC 的設計主要是要部署在遠端或分公司環境中，這通常會有相對較少的使用者、不佳的實體安全性、相對較差的中樞網站網路頻寬，以及) 的資訊技術知識 (人員。 部署 Rodc 會產生更高的安全性，並更有效率地存取網路資源。 如需 RODC 功能的詳細資訊，請參閱 [AD DS： Read-Only 網域控制站](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc732801(v=ws.10))。 如需有關如何部署 RODC 的詳細資訊，請參閱《[唯讀網域控制站的逐步指南》](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc772234(v=ws.10)) 。

> [!NOTE]
> 本指南不會說明如何判斷每個網站中所代表每個網域的適當網域控制站數目和網域控制站硬體需求。

## <a name="in-this-section"></a>本節內容

- [規劃設置樹系根網域控制站](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)

- [規劃區域網域控制站放置](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)

- [規劃設置通用類別目錄伺服器](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)

- [規劃設置操作主機角色](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)
